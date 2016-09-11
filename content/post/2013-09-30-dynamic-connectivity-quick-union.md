---
comments: true
date: 2013-09-30T00:00:00Z
excerpt: 'Dynamic Connectivity: Quick-Union in Go'
share: true
tags:
- dynamic connectivity
- go
- go lang
- quick-union
title: 'Dynamic Connectivity: Quick-Union'
url: /2013/09/30/dynamic-connectivity-quick-union/
---

<figure>
  <a href="/images/gopher_tree.jpg"><img src="/images/gopher_tree.jpg"></a>
</figure>

Understood <a href="http://www.laquysoft.com/?p=92" title="Dynamic Connectivity: Quick-Find" target="_blank">Quick-Find</a> algorithm is too slow if we have many union operations. We have to design a new algorithm to do better, let&#8217;s use a *lazy approach* where we try to avoid doing work until we have to:  
we could think the array as a set of trees where each entry contains a reference to its parent in the tree. Each element of the array has associated with it a root and we can say that two elements are connected if they have the same root.

The *connected* function simply checks whether given two index (p, q) their root are equal and returns.

The *union* function here is simpler then <a href="http://www.laquysoft.com/?p=92" title="Dynamic Connectivity: Quick-Find" target="_blank">Quick-Find</a> one, given two index *(p, q)* it retrieves their roots *(i, j)* and set *i* to *j* (the root of *q*)

The *root* function given an element *i* look for the its parent until *i* is equal to *id[i]*, when we reach the root element.

{{< highlight go >}}

package dyncon

type QuickUnionUF struct {
   Elements []int
}

func initQuickUnionUF(size int) *QuickUnionUF {
   quUF := QuickUnionUF{Elements: make([]int, size)}
   for i := range quUF.Elements {
      quUF.Elements[i] = i
   }
   return &quUF
}

func (quUF QuickUnionUF) root(i int) int {
   for i != quUF.Elements[i] {
       i = quUF.Elements[i]
   }
   return i
}

func (quUF QuickUnionUF) connected(p, q int) bool {
   return quUF.root(p) == quUF.root(q)
}

func (quUF *QuickUnionUF) union(p, q int) {
   i := quUF.root(p)
   j := quUF.root(q)
   quUF.Elements[i] = j

}
{{< / highlight >}}

By evaluating this algorithm by the number of times it access the array we can assume the following measurements:

| Algorithm | initQuickUnionUF | connected | union |
|:--------|:-------:|--------:|--------:|
| Quick-Union | N | tree-height | tree-height |
|=====

>Union algorithm some times could be faster than <a href="/dynamic-connectivity-quick-find/" title="Dynamic Connectivity: Quick-Find" target="_blank">Quick-Find</a> but it&#8217;s too slow with tall trees!
