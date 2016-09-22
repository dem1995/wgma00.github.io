---
author: admin
comments: true
date: 2016-01-14 20:19:57+00:00
layout: post
slug: ccc10s2p3-wowow-editorial
title: CCC10S2P3 - Wowow (Editorial)
wordpress_id: 92
categories:
- Competitive Programming
tags:
- Canadian Computing Competition
- CCC
- Data Structures
---

<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>
This problem asks us to handle a ranking system where we have to update a person’s score and query for the Kth ranked person.

Although this problem was intended to be solved with an offline solution that incorporates a binary indexed tree, today I will go over the solution which uses an order statistic tree. An order statistic tree is special tree that has the same characteristics of a binary search tree, but with the added functionality of being able to query for the Kth ranked element in the tree. The main idea for handling ranked based queries on an ordered statistic tree is to at every node to keep the number of children in its left and right sub-tree. Then when we search through an element every time we go left in the sub-tree we add the value of the current node’s right sub-tree – since the number of elements in the right sub-tree are the number of elements that are larger than the queried element.

In addition to this we also are required to keep the tree balanced otherwise the tree degenerates into a linked list where the time per query is $$O(N)$$ resulting in an overall complexity of $$O(QN)$$ instead of $$O(Qlog{}N)$$. Luckily the built in Ordered Statistics Tree uses a red black tree for its implementation.



Time Complexity: $$O(Q\log{}N)$$

{% highlight c++ %}
#include<bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
#define pb push_back
#define mp make_pair
#define db 0
#define all(x)(x).begin(),(x).end()
#define x first
#define y second
using namespace std;
using namespace __gnu_pbds;

typedef tree<
  int,
  null_type,
  greater<int>,
  rb_tree_tag,
  tree_order_statistics_node_update>
set_t;

int N,X,R,K,n;
char cmd;
map<int,int>user,score;
set_t s;

int main(){
    scanf("%d",&N);
    for(int i = 0;i < N;++i){
      scanf(" %c",&cmd);
      if(cmd == 'N'){
        scan(X);
        scan(R);
        user[X] = R;
        score[R] = X;
        s.insert(R);
      }
      else if(cmd == 'M'){
        scanf("%d%d",X,R);
        s.erase(user[X]);
        score.erase(user[X]);
        user[X] = R;
        score[R] = X;
        s.insert(R);
      }else{
        scan(K);
        printf("%d\n",score[*s.find_by_order(K-1)]);
      }
    }
    return 0;
}
{% endhighlight c++ %}
