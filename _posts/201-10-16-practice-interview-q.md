---
layout: post
# cover: 'assets/images/boston.jpg'
title: Interview Practice Problem
date: 2018-10-16 10:55:00
tags: practice interview
author: jpbulman
---
<iframe width="560" height="315" src="https://www.youtube.com/embed/wwIysnVmAUg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
<p>Using the problem from this video</p>
<h2 id="heading2">Initial Problem</h2>
<p>Given an <b>ordered</b> list of integers A, where A = {a<sub>0</sub>,...,a<sub>n</sub>} and for any i a<sub>i</sub> < a<sub>i+1</sub>, A ⊆ &#8484; (positive and negative integers), and a value k, find any pair of numbers in the list such that a<sub>i</sub> + a<sub>j</sub> = k. Assume that A is already in memory and the list can be treated as an array</p>
<h3 id="heading3">Brute force solution</h3>
(I'm going to do my solutions in Java)The most obvious and initial answer is just to iterate through each item, look at every other item in the list, see if there sum is k, and then return if so.

```java
private static boolean sumTest(int[] givenList,int k){

    for(int i=0;i<givenList.length;i++){
        for(int j=i+1;j<givenList.length;j++){
            if(givenList[i]+givenList[j]==k){
                return true;
            }
        }
    }

    return false;
}
```

This solution works, but is not very good because of it's O(n<sup>2</sup>) time complexity.
<h3 id="heading3">BAS</h3>
<p>The next suggestion in the video is using binary array search to find the number's complements. Essentially going through the list and for every n, looking to see if k-n, or n-complement, is in the list. </p>

```java
private static boolean BAS(int[] arr, int k){
    int lo = 0;
    int hi = arr.length-1;

    while (lo < hi){
        int mid = (lo+hi)/2;

        if(arr[mid] < k){
            lo = mid+1;
        }
        else if(arr[mid] > k){
            hi = mid-1;
        }
        else{
            return true;
        }

    }

    return false;
}

private static boolean BASsumTest(int[] givenList, int k){
    for(int i:givenList){
        if(BAS(givenList, k-i)){
            return true;
        }
    }
    return false;
}
```
The solution I would argue is fairly efficient because it will run in O(n*log(n)), but there is an even better way that the video does this this.

<h3 id="heading3">Marker solutions</h3>
If we have a left and right marker on each side of the array, and move them accordingly, we can tell if there is a sum. If the current sum of the markers is less than, we move the left one over one, and if the current sum is greater, we move the right marker one to the left. This runs in O(n), which is great!

```java
private static boolean NsumTest(int[] givenList, int k){
    int lo = 0;
    int hi = givenList.length-1;

    while (lo < hi){
        int currSum = givenList[lo]+givenList[hi];

        if(currSum < k){
            lo+=1;
        }
        else if (currSum > k){
            hi-=1;
        }
        else {
            return true;
        }
    }

    return false;
}
```

<h2 id="heading2">No more sorting</h2>
Now this gets interesting, the list is no longer sorted. The solution to this is simple, but also very subtle. The video suggests storing a HashSet of the complements of each number and then look at each number and see if it is in the HashSet. This works very well because Hash lookups are constant time operations O(1), and looking through the list is just O(n), which means the whole operation will run in O(n)time.

```java
private static boolean hashSumTest(int[] givenList, int k){

    HashSet<Integer> complements = new HashSet<Integer>();

    for(int i:givenList){
        if(complements.contains(i))
            return true;
        else
            complements.add(k-i);
    }
    return false;
}
```

Another solution is to store the previous numbers, and just see if the current number is a complement. Essentially the same solution with the algebra rearranged. This still runs in O(n) time, so it is just as efficient.

```java
private static boolean hashSumTest1(int[] givenList, int k){

    HashSet<Integer> nums = new HashSet<Integer>();

    for(int i:givenList){
        if(nums.contains(k-i))
            return true;
        else
            nums.add(k-i);
    }
    return false;
}
```

