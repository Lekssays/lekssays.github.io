---
title: "ARM Architecture with a Focus on Cortex-A Family"
date: "2016-12-28T22:40:56+00:00"
modified: "2017-04-19T13:22:08+00:00"
slug: "arm-architecture-with-a-focus-on-cortex-a-family"
author: "Ahmed Lekssays"
featured_image: "../images/arm-architecture-with-a-focus-on-cortex-a-family/16c6b65c-arm-cortex-a15.jpg"
categories: ["Computer Science"]
tags: ["ARM Architecture", "open source"]
original_url: "https://lekssays.wordpress.com/2016/12/28/arm-architecture-with-a-focus-on-cortex-a-family/"
excerpt: "&nbsp; Introduction: ARM Architecture or Advanced RISC Machine has become one of the most used computer architectures in the world due to its low consumption of energy, its high performance in dealing with small and multiple tasks simultaneously, its low cost, and its small size . It is largely used"
---
## Introduction

ARM Architecture, or Advanced RISC Machine, has become one of the most widely used computer architectures in the world thanks to its low energy consumption, its high performance in handling small and multiple tasks simultaneously, its low cost, and its small size. It is used extensively in smartphones, tablets, microcomputers, and embedded systems. It has also become a strong alternative for the supercomputers needed in data centers, because it offers a power-efficient solution.

## Motivation

If we look around us, we find that ARM processors are everywhere; they are always with us. The real motivation, however, is that ARM processors are the future of technology. The world is moving toward green and clean technology: technology that delivers high performance while also respecting the environment. These two qualities are the main goal behind ARM processors. In addition, they are widely used in the Internet of Things, which is growing and changing the world's view of technology. One of the fastest-growing applications of IoT is smart homes. Thus, the motivation to learn about ARM Architecture can be seen as personal, to develop our own projects, or as global, to save the planet.

## Development of the ARM Architecture

### Overview of the History of ARM

ARM Architecture originated at the British technology company Acorn Computers, which developed ARM, or Acorn RISC Machine, in the 1980s. It was the result of a successful collaboration between Acorn Computers and the British Broadcasting Corporation. The first ARM version was ARM1, produced in 1985. ARM Holdings later renamed it to Advanced RISC Machine. ARM Holdings is a British company founded in 1990. It does not make the processors itself; instead, it designs multicore architectures and microprocessors.

### Development Road Map

ARM has several families that depend on the ARM version, from ARMv1 to ARMv8-A. The difference between versions can depend on performance, the field of use, or sometimes the manufacturer, because, as mentioned before, ARM Holdings does not produce processors. Instead, it creates their design and architecture and grants manufacturing licences to companies such as Snapdragon and Qualcomm. For instance, Cortex-M/R/A (32-bit) fall under the umbrella of the ARMv7 family. In this presentation, we will focus on the Cortex-A/A50 family.

## ARM Architecture Profiles

ARM Architecture has three main profiles. They differ in how the architecture is applied in real life:

1. The application profile, implemented in ARMv7-A, for instance, which includes the Cortex-A/A50 family. It has several features, such as MMS (Memory Management Support) and high performance at low power, and relies on multitasking handled by the operating system.
2. The real-time profile, which is needed in embedded systems. It is implemented in ARMv7-R, for instance, which includes the Cortex-R family. It has features such as protected memory, low latency, and the predictability that "real-time" needs require.
3. The microcontroller profile, implemented in ARMv7-M, for instance, which includes the Cortex-M family. It has features such as deeply embedded use, the lowest gate entry point, and deterministic, predictable behavior as a key element.

## Instruction Sets

In ARM Architecture, a halfword means 16 bits (two bytes), a word means 32 bits (four bytes), and a doubleword means 64 bits (eight bytes). Most ARM architectures implement two instruction sets: the 32-bit ARM instruction set and the 16-bit Thumb instruction set. The latest ARM cores, such as ARMv8-A, which includes the Cortex-A family, introduce a new instruction set called Thumb-2. It provides a mixture of 32-bit and 16-bit instructions. In addition, some new ARM cores, like the Cortex-A57, support 64-bit instructions. They maintain code density with increased flexibility. Jazelle-DBX cores can also execute Java bytecode.

In the ARM instruction set, all instructions are 32 bits long, they support many executions in a single cycle, and they are conditionally executed. The Thumb instruction set is a 16-bit instruction set. It is used to optimize code density from C code (65% of ARM code size) in order to improve performance for narrow memory. It is targeted at compiler generation, so it is independent of hand coding. The Thumb-2 instruction set is designed to keep ARM performance and combine it with Thumb code density. In addition to a 16-bit instruction set, it adds a 32-bit instruction set to implement almost all ARM functionalities.

For data processing, the available operations in ARM Architecture on the Cortex-A8 are:

- Arithmetic: ADD ADC SUB SBC RSB RSC
- Logical: AND ORR EOR BIC
- Comparisons: CMP CMN TST TEQ
- Data movement: MOV MVN

They interact only with registers, so they do not deal with memory directly. The second operand in ARM uses an additional register, the Barrel Shifter, before the ALU.

## Processor Modes

The ARM has seven basic operating modes:

- User: unprivileged mode under which most tasks run
- FIQ: entered when a high-priority (fast) interrupt is raised
- IRQ: entered when a low-priority (normal) interrupt is raised
- Supervisor: entered on reset and when a Software Interrupt instruction is executed
- Abort: used to handle memory access violations
- Undef: used to handle undefined instructions
- System: privileged mode using the same registers as user mode
- Monitor: a secure mode for TrustZone

## ARM Register Set

ARM has 37 registers, each 32 bits long. Registers R0 through R7 are the same across all CPU modes; they are never banked. Registers R8 through R12 are the same across all CPU modes except FIQ mode, which has its own distinct R8 through R12 registers.

R13 and R14 are banked across all privileged CPU modes except system mode. That is, each mode that can be entered because of an exception has its own R13 and R14. These registers generally contain the stack pointer and the return address from function calls, respectively.

R13 is also referred to as SP, the Stack Pointer; R14 is also referred to as LR, the Link Register; and R15 is also referred to as PC, the Program Counter.

The Program Status Register has the following 32 bits:

- M (bits 0–4) is the processor mode bits.
- T (bit 5) is the Thumb state bit.
- F (bit 6) is the FIQ disable bit.
- I (bit 7) is the IRQ disable bit.
- A (bit 8) is the imprecise data abort disable bit.
- E (bit 9) is the data endianness bit.
- IT (bits 10–15 and 25–26) is the if-then state bits.
- GE (bits 16–19) is the greater-than-or-equal-to bits.
- DNM (bits 20–23) is the do-not-modify bits.
- J (bit 24) is the Java state bit.
- Q (bit 27) is the sticky overflow bit.
- V (bit 28) is the overflow bit.
- C (bit 29) is the carry/borrow/extend bit.
- Z (bit 30) is the zero bit.
- N (bit 31) is the negative/less-than bit.

## Exception Handling

When an exception occurs, the ARM:

- Copies CPSR into SPSR\_<mode>
- Sets the appropriate CPSR bits
  - Change to ARM state
  - Change to exception mode
  - Disable interrupts (if appropriate)
- Stores the return address in LR\_<mode>
- Sets PC to the vector address

To return, the exception handler needs to:

- Restore CPSR from SPSR\_<mode>
- Restore PC from LR\_<mode>

## Instruction Pipeline

The ARM7TDMI uses a 3-stage pipeline in order to increase the speed of the flow of instructions to the processor, allowing several operations to happen simultaneously. FETCH is the first stage of the pipeline, where the instruction is fetched from memory. Then comes DECODE, which decodes the registers used in the instruction. Finally, EXECUTE reads the registers from the Register Bank, performs the Shift and ALU operations, and writes the registers back to the Register Bank.

## Conclusion

ARM Architecture is one of the most promising technologies that every computer scientist should know about. It is used everywhere around us. In this presentation, we focused on the Cortex-A family, which is an application profile. It is used in smartphones because of its low energy consumption. We chose it because it supports both 32-bit and 64-bit architectures, which is a new step in ARM architecture's history. It is the step that allowed famous companies such as Apple and Samsung to support 64-bit operating systems in their mobile phones, as well as 64-bit operating systems on the Raspberry Pi, in order to increase its use in building green data centers.

## References

ARM – Architecture Reference Manual [http://www.arm.com/](http://www.arm.com/files/pdf/ARM_Arch_A8.pdf).

ARM Architecture A8 Presentation Slides. <http://www.arm.com/files/pdf/ARM_Arch_A8.pdf>

Cortex-A Series Processors. <https://developer.arm.com/products/processors/cortex-a>

P. Dutta, Electrical Engineering Teaching Slides, Electrical Engineering and Computer Science Department, University of Michigan. <https://web.eecs.umich.edu/~prabal/teaching/eecs373-f11/readings/ARM_Architecture_Overview.pdf>

2016. Stallings, Computer Organization and Architecture: Designing for Performance.

Image copyright: <../images/arm-architecture-with-a-focus-on-cortex-a-family/5814da66-ARM-Cortex-A15.jpg>
