---
author: admin
comments: true
date: 2016-01-14 06:41:39+00:00
layout: post
slug: ccc10s1-computer-purchase-editorial
title: CCC10S1 - Computer Purchase (Editorial)
wordpress_id: 51
categories:
- Competitive Programming
tags:
- Canadian Computing Competition
- CCC
- No Brainers For Speed
---
<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>
This problem asks us to find the most valued computer from a list of computers. The problem description provides a simple ranking structure that rates computers based on the specifications then by their names. An easy way to incorporate this is to define a comparison operator which compares two computers based on their specifications score, then their name. Once this is done we sort the list using C++ built in sorting function then we print out the first two elements.

Time complexity: $$ O(n\log{}n) $$

{% highlight c++ %}
#include<bits/stdc++.h>
#define pb push_back
#define mp make_pair
#define db 0
#define all(x)(x).begin(),(x).end()
#define x first
#define y second
using namespace std;

int N;
int R,S,D;
string nm;
vector<pair<pair<string,int>, pair<int,int>>>arr;

bool compare(pair<pair<string,int>, pair<int,int>>lft,
             pair<pair<string,int>, pair<int,int>> rht){
     int s1 = 2*lft.x.y+3*lft.y.x+lft.y.y;
     int s2 = 2*rht.x.y+3*rht.y.x+rht.y.y;
     if(s1 == s2)
        return lft.x.x < rht.x.x;
     else
       return s1 > s2;
}

int main(){
    cin >> N;
    for(int i = 0;i < N;i++){
        cin >> nm >> R >> S >> D;
        arr.pb(mp(mp(nm,R),mp(S,D)));
    }
    sort(all(arr),compare);
    if( (int)arr.size() >= 2)
        cout << arr[0].x.x << endl << arr[1].x.x << endl;
    if( (int)arr.size() == 1)
        cout << arr[0].x.x;
    return 0;

{% endhighlight c++ %}




