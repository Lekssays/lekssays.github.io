---
title: "Editorial of “Problem C – Omar the Bus Driver” in CodeIT 2017"
date: "2017-05-22T15:49:23+00:00"
modified: "2017-05-22T15:52:39+00:00"
slug: "editorial-of-problem-c-omar-the-bus-driver-in-codeit-2017"
author: "Ahmed Lekssays"
featured_image: "../images/editorial-of-problem-c-omar-the-bus-driver-in-codeit-2017/cb2a56ef-18268537_1065466083554757_3503560010645890020_n.png"
categories: ["Computer Science"]
tags: ["programming"]
original_url: "https://lekssays.wordpress.com/2017/05/22/editorial-of-problem-c-omar-the-bus-driver-in-codeit-2017/"
excerpt: "I proposed a problem called “Problem C – Omar the Bus Driver” which was on Scorify platform, and I would like to share its solution in this post. I am not that happy about it because the problem statement was ambiguous, and some cases were not consistent even if they will not change the output. [&he"
---
I proposed a problem called “Problem C – Omar the Bus Driver” on the Scorify platform, and I would like to share its solution in this post. I am not entirely happy with it, because the problem statement was ambiguous and some cases were inconsistent, even though they did not change the output. I noticed this after the requests in the clarifications, since some contestants used *assert* to check it.

The inconsistent case was:

***3** 1 8*

***3** 0 0*

Here the number of nodes is 3, yet the only edge connects a node with index 3 to a node with index 0, which is *NOT* consistent with the problem statement. I am sorry about that.

The hero of the problem was my friend *Omar Salim Moussa*, so this problem was a gift for him.

## I am sorry 😦

I apologize to all the contestants who tried this problem and found it ambiguous. I know some of them spent a lot of time trying to solve it. I deeply apologize in particular to the *Pythonista, Pinky and the Brain, Koding4Khobz* teams. At first glance, the problem can look like a traveling salesman problem. I hope I corrected this ambiguity in the clarifications, and I hope it did not affect the overall standings. I took it very seriously, but these things happen, and I am truly sorry. I hope it will not be repeated next time. This is the first time I have written a problem for a contest, but I am not using that as an excuse.

## Problem Statement

Omar is a bus driver for a famous Moroccan travel company. He travels once per day, visiting N cities. He has a serious problem: he does not know whether the initial volume of gasoline is enough to visit all N cities. Your task is to help Omar determine whether he can traverse all N cities at minimum cost, so that he can tell whether the initial volume of gasoline is sufficient.

### Input
The input consists of several test cases. Each test case starts with a line containing three non-negative integers, 1 ≤ N ≤ 20000, and 0 ≤ M ≤ 30000, and 1 ≤ G ≤ 30000, separated by a single space, where N is the number of cities, M is the number of roads, and G is the initial volume of gasoline. Cities are numbered from 0 to N−1. Then follow M lines, each consisting of three space-separated integers u, v, and g, indicating that there is a road between u and v such that traversing it consumes g liters of gasoline where 0 ≤ g ≤ 20000. Roads are undirected.  
The input is terminated by a line containing -1 -1 -1; this line should not be processed.

### Output
For every test case, if there is no solution, output the word “NULL” (without quotes) on a line of its own. If Omar can visit all the cities with G liters of gasoline, output “POSSIBLE” (without quotes). Otherwise, if he cannot, output “IMPOSSIBLE” (without quotes).

### Sample Input

```
4 4 20
0 1 1
1 2 2
1 3 3
2 3 0
2 1 90
0 1 100
3 0
-1 -1 -1
```

### Sample Output

```
POSSIBLE
IMPOSSIBLE
NULL
```

## Clarifications during the Contest

1. You should connect all the nodes with the minimum cost.
2. You do not need to count an edge again if you have already traversed it.

## Idea

The idea of the problem is to compute the minimum spanning tree (MST) and compare its weight with the initial amount of gasoline. So if `mst <= g`, print “POSSIBLE”; if `mst > g`, print “IMPOSSIBLE”. If `mst = 0`, because the driver cannot go anywhere or the graph is not connected, output “NULL”.

## Time Complexity

In my implementation, I used Kruskal’s algorithm to find the minimum spanning tree, so the time complexity is O(E log V).

## A Solution in C++

This file contains hidden or bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.  
[Learn more about bidirectional Unicode characters](https://github.co/hiddenchars)  

[Show hidden characters]({{ revealButtonHref }})

|  |  |
| --- | --- |
|  | #include <cstdio> |
|  | #include <iostream> |
|  | #include <vector> |
|  | #include <algorithm> |
|  | #include <utility> |
|  | #define MAXN 30023 |
|  | #define eps 1e-7 |
|  |  |
|  | using namespace std; |
|  |  |
|  | struct edge { |
|  | int u, v, w; |
|  |  |
|  | bool operator < (edge o) const { |
|  | if( abs(o.w – w) < eps ) |
|  | return pair<int, int>(u, v) < pair<int, int>(o.u, o.v); |
|  | return w < o.w; |
|  | } |
|  | }; |
|  |  |
|  |  |
|  | int p[MAXN]; |
|  | vector<edge> edgeList; |
|  | vector< pair<int,int> > connectedEdges; |
|  |  |
|  | int find(int u) { |
|  | return p[u] == u ? u : find(p[u]); |
|  | } |
|  |  |
|  | void merge(int u, int v) { |
|  | int pu = find(u), pv = find(v); |
|  | p[pu] = pv; |
|  | } |
|  |  |
|  | int main(void){ |
|  | int u, v, w, n, e, g; |
|  |  |
|  | while(true){ |
|  | cin >> n >> e >> g; |
|  | if(n == -1 && e == -1 && g == -1) return 0; |
|  | edgeList.clear(); |
|  | connectedEdges.clear(); |
|  | for(int i = 0; i < e; i++){ |
|  | cin >> u >> v >> w; |
|  | edgeList.push\_back((edge){u, v, w}); |
|  | } |
|  |  |
|  | sort(edgeList.begin(), edgeList.end()); |
|  | for(int i = 0; i < n; i++) p[i] = i; |
|  |  |
|  | int res = 0; |
|  | for(int i = 0; i < edgeList.size(); i++) { |
|  | int u = edgeList[i].u, v = edgeList[i].v; |
|  | if( find(u) == find(v) ) continue; |
|  | merge(u, v); |
|  | res += edgeList[i].w; |
|  | if(edgeList[i].v > edgeList[i].u){ |
|  | connectedEdges.push\_back(make\_pair(edgeList[i].u, edgeList[i].v)); |
|  | } else { |
|  | connectedEdges.push\_back(make\_pair(edgeList[i].v, edgeList[i].u)); |
|  | } |
|  | } |
|  |  |
|  | if(res == 0 || connectedEdges.size() != n – 1) puts("NULL"); |
|  | else { |
|  | if(res <= g) |
|  | puts("POSSIBLE"); |
|  | else |
|  | puts("IMPOSSIBLE"); |
|  | } |
|  | } |
|  | return 0; |
|  | } |

[view raw](https://gist.github.com/Lekssays/0d37980dc0fb800c4039492c649f2bc4/raw/b26f79fd442bff328f77070a299a67325cb54967/busdriver.cc)  
 [busdriver.cc](https://gist.github.com/Lekssays/0d37980dc0fb800c4039492c649f2bc4#file-busdriver-cc)  
hosted with ❤ by [GitHub](https://github.com)
