---
title: "Editorial of &#8220;Dehbi et ses amis&#8221; &#8211; Moroccan Olympiads of Informatics (Round 2)"
date: "2018-04-21T16:07:38+00:00"
modified: "2018-04-21T16:10:52+00:00"
slug: "editorial-of-dehbi-et-ses-amis-moroccan-olympiads-of-informatics-round-2"
author: "Ahmed Lekssays"
featured_image: "../images/editorial-of-dehbi-et-ses-amis-moroccan-olympiads-of-informatics-round-2/4efa302d-screen-shot-2018-04-21-at-4-08-25-pm.png"
categories: ["Computer Science"]
tags: ["programming"]
original_url: "https://lekssays.wordpress.com/2018/04/21/editorial-of-dehbi-et-ses-amis-moroccan-olympiads-of-informatics-round-2/"
excerpt: "Dehbi is my roommate and one of the best human beings I have met. He is graduating this semester, so he is the hero of this problem. This problem is the last problem in Moroccan Olympiads of Informatics Round 2, and it was solved during the contest once by walux. I would like to congratulate [&helli"
---
Dehbi is my roommate and one of the best human beings I have met. He is graduating this semester, so he is the hero of this problem.

This is the last problem in the Moroccan Olympiads of Informatics (Round 2), and it was solved during the contest only once, by *walux*. I would like to congratulate him for solving every problem in this round.

I wrote the original version in English, but a French translation was prepared for the contest.

## Problem Statement (French)

Dehbi est entrain de jouer à un jeu avec ses amis.

Le jeu peut être décrit de la manière suivante: Les x amis de Dehbi ont tous des positions initiales différentes. Ils veulent le rejoindre en un point d où il les attend avec une récompense. Au temps t = 0, ils se déplacent à partir de leurs positions initiales de manière optimale pour rejoindre la position de Dehbi le plus tôt possible.

Dehbi n’as pas encore préparé la récompense et le jeu commence bientôt. il a besoin de votre aide pour savoir de combien de temps il dispose avant qu’un de ses amis puisse le rejoindre, sachant qu’ils ont tous besoin d’une seconde pour traverser une unité de distance (vitesse de 1 unité / seconde). Il est par ailleurs garanti que le graphe dont les noeuds sont les positions possibles pour les amis de Dehbi et dont les arêtes sont les différentes routes possibles est bien connecté, non orienté et ne contient pas deux arêtes de même poids.

### Input

La première ligne contient un entier T – le nombre de cas de test.Chaque cas est décrit sur plusieurs lignes.  
La première ligne contient deux entiers n et m indiquant respectivement le nombre de positions possibles pour les amis de Dehbi et le nombre de routes existantes (2 ≤ n ≤ , et n-1 ≤ m ≤ min( 10^5,n\*(n-1)/2) ).  
Les m lignes suivantes contiennent 3 entiers chacune, 1 ≤ u, v ≤ n et 1 ≤ w ≤ 500 représentant une route entre u et v de longueur w unités.  
La ligne suivante contient un entier x, le nombre d’amis de Dehbi 1 ≤ x ≤ n.  
La ligne suivante contient x entiers séparés par un espace. La dernière ligne de chaque cas contient un seul entier d indiquant la position de Dehbi.

### Output

Pour chaque cas, afficher un seul entier: le temps que devrait prendre l’ami de Dehbi qui le rejoint en premier.

### Points

20 points : 1 ≤ n ≤ 500.

100 points : 1 ≤ n ≤ .

### Example

**Input**

1  
3 2  
1 3 4  
2 3 10  
2  
1 2  
3

**Output**

4

## Problem Statement (English)

Dehbi and his friends are playing a game.

The game can be described as follows: Dehbi has *x* friends, all located at different starting points, and they want to reach a destination where Dehbi is waiting for them.

Dehbi wants your help. He wants to know the first time at which at least one of his *x* friends reaches him.

### Input

The first line contains two integers *n* and *m*, denoting the number of locations where Dehbi's friends stand and the number of roads, respectively, (2 ≤ *n* ≤ 105), and ![](../images/editorial-of-dehbi-et-ses-amis-moroccan-olympiads-of-informatics-round-2/fd064e53-282597d992f7c5c86afacf98dff2cf85522a9a85.png).

The next *m* lines each contain 3 integers *u*, *v*, and *w*, representing a road (*u*, *v*) and its length.

The next line contains *x*, the number of Dehbi's friends, (1 ≤ *x* ≤ *n*). The following line contains *x* numbers representing the initial starting locations of Dehbi's friends.

The last line contains a single integer: the final destination where Dehbi is waiting for his friends.

### Output

Output one integer: the first time at which one of his *x* friends reaches his place. In other words, the time needed by the fastest friend to reach him.

### Example

**Input**

1  
3 2  
1 3 4  
2 3 10  
2  
1 2  
3

**Output**

4

## Idea

Let's first understand the problem in terms of nodes: we want to know the shortest path from the *n* friends to Dehbi. So instead of running a shortest-path algorithm (Dijkstra, in this case) from each friend, we should start from Dehbi's position and reach out to the friends.

## My Solution (English Version Format) in C++

<https://gist.github.com/Lekssays/d4908780547d5e71c1e2ecef6019556d><https://gist.github.com/Lekssays/d4908780547d5e71c1e2ecef6019556d.js>
