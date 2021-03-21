---
layout: post
title: The Skyline Problem (LeetCode 238);
date: 2021-03-21 
description:  Constructive Solution to The Skyline Problem.
img: LeetCode.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [dp,Dynamic-Programming,LeetCode,Constructive]
---
# Problem Statement

Problem Link: [The Skyline Problem -238](https://leetcode.com/problems/the-skyline-problem/)

# Approach

The very rudimentary solution would be to traverse all the ranges and update the values of the x coordinates with maximum heights from [li,ri);

But Since the values of cooridinates range from 0 to 1e9, the normal approach might result in a memory limit exceeded outcome.


## Solution 1: 
### prerequisite:
[Coordinate Compression](https://codeforces.com/blog/entry/23180)

Coordinate Compression technique is as simple as it sounds.Although the range of endpoints is large, the number of values in the range remain limited. So we map the values to a specific value in a one-one mapping.
So if Range => LARGE and Quantity => small; 

You might wanna give it a thought.

### Outline:
Now the first step to approach the problem should be Coordinate Compression (No points for guessing now :P).
After Mapping all the present coordinates.The solution is more or less similar to rudimentary solution with some optimizations.

For optimising the solution we have to maintain a vector array points which stores all the mapped endpoints in a sorted order(We'll know soon).

Now we rearrange the given vector input to be of the form (b[2],b[0],b[1]) and sort it according to first item i.e height.After doing that we traverse the point in decreasing order of the heights and also removing them simultaneously because they won't be updated further (We sorted by height remember? So save time here or you get stuck at the last testcase.).

Now at last traverse through the points and simultaneously pushing the answer only when height changes i.e. h[i]!=h[i-1];

=
# CODE :
``` cpp
class Solution {
public:

    vector<vector<int>> getSkyline(vector<vector<int>>& b1) {
        //to determine unique endpoints
        set<int> s;
        //stores all the mapped points
        vector<int> points;
        for(int i=0;i<b1.size();i++)
        {
            s.insert(b1[i][0]);
            s.insert(b1[i][1]);
        }
        // m maps x0->x1 and b maps x1=>x0;
        unordered_map<int,int> m,b;
        int x=0;
        for(auto i:s)
        {
            b[x]=i;
           points.push_back(x);
            m[i]=x++;
        }

        //coordinate compression finished

        vector<vector<int>> v;
        //rearrange to sort by heights;
        for(int i=0;i<b1.size();i++)
        {
            vector<int> x1;
            x1.push_back(b1[i][2]);
            x1.push_back(b1[i][0]);
            x1.push_back(b1[i][1]);
            v.push_back(x1);
        }
        //sort by heights.
        sort(v.begin(),v.end());
        //a mapping to assign heights
        unordered_map<int,int> h;
        //traverse in ddecreasing order of heights.
        for(int i=v.size()-1;i>=0;i--)
        {
            //traverse in the range [li,ri);
            auto x=lower_bound(points.begin(),points.end(),m[v[i][1]]);
            auto y=lower_bound(points.begin(),points.end(),m[v[i][2]]);
            vector<int> p;
            for(auto j=x;j!=y;j++)
            {
                h[*j]=max(v[i][0],h[*j]);
              
            }
            //erase the points to save some time !important or 40th TC won't run.
            points.erase(x,y);
          
        }
        vector<vector<int>> ans;
        //traversing through the mapped points to find the answer;
        for(int i=0;i<x;i++)
        {
            if(h[i]!=h[i-1])
            {
                ans.push_back({b[i],h[i]});
            }
          
        }
        return ans;
```


