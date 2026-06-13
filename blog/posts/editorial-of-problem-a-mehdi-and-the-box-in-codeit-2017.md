---
title: "Editorial of &#8220;Problem A &#8211; Mehdi and the Box&#8221; in CodeIT 2017"
date: "2017-05-22T15:02:50+00:00"
modified: "2017-05-22T18:07:27+00:00"
slug: "editorial-of-problem-a-mehdi-and-the-box-in-codeit-2017"
author: "Ahmed Lekssays"
featured_image: "../images/editorial-of-problem-a-mehdi-and-the-box-in-codeit-2017/cb2a56ef-18268537_1065466083554757_3503560010645890020_n.png"
categories: ["Computer Science"]
tags: ["programming"]
original_url: "https://lekssays.wordpress.com/2017/05/22/editorial-of-problem-a-mehdi-and-the-box-in-codeit-2017/"
excerpt: "I proposed a problem called &#8220;Problem A &#8211; Mehdi and the Box&#8221; which was on Scorify platform, and I would like to share its simple solution in this post. I am very happy because it was a clear and all the contestants solved it. And the hero was my friend Mehdi Laziri. So, this problem"
---
I proposed a problem called “Problem A – Mehdi and the Box” on the Scorify platform, and I would like to share its simple solution in this post. I am very happy because it was clear and all the contestants solved it.

The hero was my friend Mehdi Laziri, so this problem was a gift for him.

## Problem Statement

Mehdi is a smart guy who wants to put his pens in a 2D box. The problem is that he has N pens of different lengths, and he wants to check whether he can fit each pen Pi in the box.

### Input

- The first line contains T, the number of test cases (1 <= T <= 100).
- The next line contains 3 integers: 1 <= N <= 1000, 1 <= W <= 10000, and 1 <= H <= 10000, the number of pens, the width of the box, and the height of the box, followed by N lines giving the length of each pen Pi.

### Output

For each pen Pi, output “YES” (without quotes) if it can fit in the box. Otherwise, output “NO” (without quotes).

### Sample Input

2 12 17  
21  
20

### Sample Output

NO  
YES

## Idea

The first thing that should come to mind is that the diagonal is the longest segment of a rectangle, since we are dealing with a 2D box. So we should compare the length of each pen with the diagonal of the rectangle. Formally, the following relation should be satisfied: H^2 + W^2 > L^2.

## Time Complexity

We have N queries, and since answering each query is O(1), answering all N queries is O(N).

## A Solution in C++:

This file contains hidden or bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.  
[Learn more about bidirectional Unicode characters](https://github.co/hiddenchars)  

[Show hidden characters]({{ revealButtonHref }})

|  |  |
| --- | --- |
|  | #include <iostream> |
|  |  |
|  | using namespace std; |
|  |  |
|  | int main(void) { |
|  | int T; |
|  |  |
|  | cin >> T; |
|  |  |
|  | while(T–) { |
|  | int N, W, H; |
|  | cin >> N >> W >> H; |
|  | for(int i = 0; i < N; i++){ |
|  | int L; |
|  | cin >> L; |
|  | if(H \* H + W \* W <= L \* L) |
|  | puts("NO"); |
|  | else |
|  | puts("YES"); |
|  | } |
|  | } |
|  | return 0; |
|  | } |

[view raw](https://gist.github.com/Lekssays/df954223447c11c5d46076fcd86745a8/raw/1922c277b0375e1494521dff4bc99900cac85d85/box.cc)  
 [box.cc](https://gist.github.com/Lekssays/df954223447c11c5d46076fcd86745a8#file-box-cc)  
hosted with ❤ by [GitHub](https://github.com)
