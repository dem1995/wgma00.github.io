---
author: admin
comments: true
date: 2016-01-14 15:14:29+00:00
layout: post
slug: ccc10s2p1-barking-dogs-editorial
title: CCC10S2P1 - Barking Dogs! (Editorial)
wordpress_id: 80
categories:
- Competitive Programming
tags:
- Canadian Computing Competition
- CCC
- Implementation
---

<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>
This problem asks us to keep track events that come from $$D$$ dogs, over a time interval $$T$$. Then we must simulate the problem as specified in the statement. One has to be very careful with how they implement this because this problem is very easy to misinterpret or make silly mistakes.

Time complexity: $$O(TD)$$

{% highlight c++ %}
nclude<bits/stdc++.h>
#define pb push_back
#define mp make_pair
#define db 1
#define all(x)(x).begin(),(x).end()
#define x first
#define y second
using namespace std;

const int MAXD = 1000;
const int SLEEPING = -1,WAITING = 1,BARKING = 0;
int D,F,T;
int state[MAXD];
int wait[MAXD];
int timer[MAXD];
int cnt[MAXD];
bool v[MAXD];

vector<int>adj[MAXD];

void update(int dog){
   for(int i = 0;i < (int)adj[dog].size();i++){
       if(state[adj[dog][i]] == SLEEPING){
          state[adj[dog][i]] = WAITING;
          timer[adj[dog][i]] = wait[adj[dog][i]];
       }
   }
}
int main(){
    cin >> D;
    for(int i = 0;i < D;i++){
        cin >> wait[i];
    }
    cin >> F;
    for(int i = 0,a,b;i < F;i++){
       cin >> a >> b;
       adj[--a].pb(--b);
    }
    cin >> T;
    memset(state,SLEEPING,sizeof(state));
    memset(timer,SLEEPING,sizeof(timer));
    state[0] = BARKING;
    timer[0] = 0;
    for(int t = 0;t <= T;t++){
       for(int dog = 0;dog < D;dog++){
           if(timer[dog] == 0){
              cnt[dog]++;
              update(dog);
           }
       }
       for(int dog = 0;dog < D;dog++){
           timer[dog]--;
           if(timer[dog] == 0)state[dog] = BARKING;
           if(timer[dog] <  0)state[dog] = SLEEPING;
           if(timer[dog] >  0)state[dog] = WAITING;
       }
    }
    for(int dog = 0;dog < D;dog++)
        cout << cnt[dog] << endl;
    return 0;
}
{% endhighlight c++ %}
