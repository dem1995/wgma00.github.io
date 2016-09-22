---
author: admin
comments: true
date: 2016-01-14 06:48:44+00:00
layout: post
slug: ccc10s2-huffman-encoding-editorial
title: CCC10S2 - Huffman Encoding (Editorial)
wordpress_id: 65
categories:
- Competitive Programming
tags:
- Canadian Computing Competition
- CCC
- No Brainers For Speed
---

<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>

This problem asks us to decrypt a string comprised of 1’s and 0’s into a message. The key observation needed is that there is only one possible decryption since all of the values are unique. With this in mind now all we have to do is keep removing the first substring in our string that can be decrypted.

Time complexity: $$O(NK)$$

{% highlight c++ %}
#include<bits/stdc++.h>
#define pb push_back
#define mp make_pair
#define db 0
#define all(x)(x).begin(),(x).end()
#define x first
#define y second
using namespace std;

int K;
string a,b,in;
vector<pair<string,string>> arr;

int main(){
    cin >> K;
    for(int i = 0;i < K;i++){
        cin >> a >> b;
        arr.pb(mp(a,b));
    }
    cin >> in;
    while(!in.empty()){
        for(int i = 0;i < K;i++){
            if(arr[i].y.size() <= in.size()){
                if(arr[i].y == in.substr(0,arr[i].y.size())){
                   cout << arr[i].x;
                   in.erase(0,arr[i].y.size());
                   break;
                }
            }
        }
    }
    cout << endl;
    return 0;
}
{% endhighlight c++ %}
