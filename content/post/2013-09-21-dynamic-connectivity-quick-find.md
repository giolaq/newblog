---
comments: true
date: 2013-09-21T00:00:00Z
excerpt: 'Dynamic Connectivity: Quick-Find in Go'
tags:
- dynamic connectivity
- go
- go lang
- quick-find
title: 'Dynamic Connectivity: Quick-Find'
url: /2013/09/21/dynamic-connectivity-quick-find/
---

<figure>
  <a href="/images/DynamicConnectivity.jpg"><img src="/images/DynamicConnectivity.jpg"></a>
</figure>

Let&#8217;s assume our object can be identified by an integer number, we can use an integer Array as data structure for our solution (*id[]*) . In Quick-Find we consider two sites *p* and *q* are in the same component if *id[p]* is equal do *id[q]*.

The *initQuickFindUF* function allocates the zeroed array and assign the slice that refers to that array to *Elements*; then go through it and set the value corresponding to each index ( *Elements[i] = i *).

The *connected* function simply checks whether given two index *(p, q)* their entries are equal and returns.

The *union* function is a little more complicated: given two index  *(p, q)* it retrieves their entries in the array, then loop through the whole array looking for entries equal to the *p* entry and set those to q entry.

<div class="divider">
</div>

{{< highlight go >}}
package dyncon

type QuickFindUF struct {
   Elements []int
}

func initQuickFindUF(size int) *QuickFindUF {
   qfUF := QuickFindUF{Elements: make([]int, size)}
   for i := range qfUF.Elements {
      qfUF.Elements[i] = i
   }
   return &qfUF
}

func (qfUF QuickFindUF) connected(p, q int) bool {
   return qfUF.Elements[p] == qfUF.Elements[q]
}

func (qfUF *QuickFindUF) union(p, q int) {
   pid := qfUF.Elements[p]
   qid := qfUF.Elements[q]
   for i := range qfUF.Elements {
      if qfUF.Elements[i] == pid {
         qfUF.Elements[i] = qid
      }
   }
}
{{< / highlight >}}


By evaluating this algorithm by the number of times it access the array we can assume the following measurements:


| Algorithm | initQuickFindUF | connected | union |
|:--------|:-------:|--------:|--------:|
| Quick-Find | N | 1 | N |
|=====

>The *union* function costs N accesses to the array; if we have N union operations this algorithm will take quadratic time complexity.

So the Quick-find algorithm is too slow for big N.
