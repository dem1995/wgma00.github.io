---
author: admin
comments: true
date: 2016-01-14 07:04:30+00:00
layout: post
slug: ccc10s4-animal-farm-editorial
title: CCC10S4 - Animal Farm (Editorial)
wordpress_id: 72
categories:
- Competitive Programming
tags:
- Canadian Computing Competition
- CCC
- Minimum Spanning Tree
---

<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>
This problem asks of us to find the minimum spanning tree of the graph where the pens are considered the nodes. First we must construct the graph since we are not given the graph required; instead we are given the corners of the pens. This must be done carefully since we need to ensure that any edge that only connects two pens connects these two pens, and any edge connecting one is actually connected this pen with the outside (which is another node in the graph). It is also important to note that it is not always possible that the graph is connected to the outside node (implying the graph is not connected). This is why it will be relatively trivial to use Prim’s algorithm for finding the minimum spanning since Prim’s can detect if a graph is connected or not automatically. Unlike Kruskal’s Algorithm which requires some more legwork (left as an exercise for the reader).

In the provided implementation we the time complexity for constructing the graph is $$O(N^2)$$ where $$N$$ is the number of pens, and the time complexity for finding the minimum spanning tree is $$O(N^2\log{}N)$$.

Time Complexity: $$O(N^2\log{}N)$$
{% highlight c++ %}
#include<bits/stdc++.h>
#define pb push_back
#define mp make_pair
#define db 0
#define all(x)(x).begin(),(x).end()
#define x first
#define y second
using namespace std;

struct corners{
    int w,pen1,pen2;
};

const int MAXN = 500,INF = 0x3F3F3F3F;
int N,E;

corners c[MAXN][MAXN];
vector<pair<int,int>>adj[MAXN];
bool v[MAXN];

int prim(int n){
    priority_queue<pair<int,pair<int,int>>>pq;
    int MSTNodes = 1, totWeight = 0;
    for(int i = 0; i < adj[1].size();i++){
        if(adj[1][i].x <= n){
            pq.push(mp(-adj[1][i].y,mp(1,adj[1][i].x)));
        }
    }
    memset(v,false,sizeof v);
    v[1] = true;
    while(MSTNodes < n && !pq.empty()){
        int w = -pq.top().x;
        int sn = pq.top().y.x;
        int en = pq.top().y.y;
        pq.pop();
        if(v[sn] && !v[en]){
            MSTNodes++;
            totWeight+=w;
            v[en] = true;
            for(int i = 0; i < adj[en].size();i++){
                if(adj[en][i].x <= n){
                    pq.push(mp(-adj[en][i].y,mp(en,adj[en][i].x)));
                }
            }
        }
    }
    return MSTNodes == n ? totWeight:INF;
}

int main(){
    scanf("%d",&N);
    for(int i = 0; i < MAXN;i++){
        for(int j = 0; j < MAXN;j++){
            c[i][j].w = c[i][j].pen1 = c[i][j].pen2 = INF;
        }
    }
    for(int i = 0; i < N;i++){
        scanf("%d",&E);
        int tc[E],w[E];
        for(int j = 0; j < E;j++)
            scanf("%d",&tc[j]);
        for(int j = 0; j < E;j++)
            scanf("%d",&w[j]);
        for(int j = 0; j < E;j++){
            int corner[] = {tc[j],tc[(j+1)%E]};
            sort(corner,corner+2);
            if(c[corner[0]][corner[1]].pen1 == INF)
                c[corner[0]][corner[1]].pen1 = i+1;
            else
                c[corner[0]][corner[1]].pen2 = i+1;
            c[corner[0]][corner[1]].w = w[j];
        }
    }
    for(int i = 0; i < MAXN;i++){
        for(int j = 0; j < MAXN;j++){
            // not connected to outside because declared twice
            if(c[i][j].pen1 != INF && c[i][j].pen2 != INF){
                adj[c[i][j].pen1].pb(mp(c[i][j].pen2,c[i][j].w));
                adj[c[i][j].pen2].pb(mp(c[i][j].pen1,c[i][j].w));
            }
            // connected to outside because declared once
            else if(c[i][j].pen1 != INF && c[i][j].pen2 == INF){
                adj[c[i][j].pen1].pb(mp(N+1,c[i][j].w));
                adj[N+1].pb(mp(c[i][j].pen1,c[i][j].w));
            }
        }
    }
    int ans1 = prim(N),ans2 = prim(N+1);
    printf("%d\n",min(ans1,ans2));
    return 0;
}
{% endhighlight c++ %}
