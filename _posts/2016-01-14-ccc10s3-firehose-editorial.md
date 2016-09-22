---
author: admin
comments: true
date: 2016-01-14 06:57:39+00:00
layout: post
slug: ccc10s3-firehose-editorial
title: CCC10S3 - Firehose (Editorial)
wordpress_id: 69
categories:
- Competitive Programming
tags:
- Binary Search
- Canadian Computing Competition
- CCC
---

<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>
This problems asks us to find the minimum length of hose required to connect a series of houses if were only given K fire hydrants from which to extend our hose from. We must then take into consideration the maximum hose length possible is 1,000,000, because this number is so large we know that a simple iterative solution will likely not run in time. What we need to notice is that once we have found one possible hose length that satisfies the condition above we do not need to consider hose lengths that are longer, likewise we also have to notice that if the current hose length does not satisfy the conditions then all hose lengths less than this hose length are also impossible. Once we see this property the problem becomes a trivial binary search problem, we're using binary search on the hose length.

Now that this idea has been formulated we must define a predicate. The predicate should be if the current hose length is possible then the upper bound is now the hose length, otherwise the lower bound is now equal to the hose length. In provided solution the predicate written has a time complexity of $$O(H^2)$$ where $$H$$ is the number of houses and the binary search has a time complexity of $$O(\log{}L)$$ where $$L$$ is the length of the hose.

Time complexity: $$O(H^2\log{}L)$$

{% highlight c++ %}
#include<iostream>
#include<cstdio>
#include<cstring>
#include<vector>
#include<algorithm>
using namespace std;

const int MAXH = 1000;
int H,K,lo,hi,mid;
vector<int>house;
bool v[MAXH];


/*minimum between distance taking wrapping into consideration*/
int dist(int h1,int h2){
  return min(h2-h1,(int)1e6-h2+h1);
}

/* Places a hydrant hose_len units away from the current house. For all houses 
 * in front of the current house, if their distance from the current house is 
 * less than or equal to 2*hose_len meaning they are either on the left side of
 * the hydrant (placed hose_len units ahead of the current house) or on the 
 * right side of the hydrant (hence the extra hose_len) then they are marked
 * as visisted.
 */
bool predicate(int hose_len,int k_left = K,int h_left = H){
  memset(v,false,sizeof(v));
  for(int i = 0;i < (int)house.size() && h_left > 0 && k_left > 0;++i){
      if(v[i])
        continue;
      else{
        k_left--,h_left--,v[i] = true;
        for(int k = (i+1)%H;k < (int)house.size();++k){
            if(!v[k] && dist(house[i],house[k]) <= 2*hose_len)
                h_left--,v[k] = true;
            else
                break;
        }
      }
  }
  return (h_left == 0) ? true:false;
}

int main(){
    scanf("%d",&H);
    for (int i = 0,j = 0; i < H; ++i){
      scanf("%d",&j);
      house.push_back(j);
    }
    sort(house.begin(),house.end());
    scanf("%d",&K);
    lo = 0,hi = (int)1e6,mid = (lo+hi)/2;
    while(lo < hi){
        // this hose length works so it's now the upper bound
        if(predicate(mid))
            hi = mid;
        // this hose length is too short
        else              
            lo = mid+1;
        mid = (lo+hi)/2;
    }
    printf("%d\n",mid);
    return 0;
}
{% endhighlight c++ %}
