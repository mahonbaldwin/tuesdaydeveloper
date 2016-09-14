---
layout: post
title:  "Flattening Vectors"
date:   2016-09-14 15:34:56 -0700
categories: clojure
---
Recently I was working on a problem where I needed to get a vector of vectors containing an entry
for every possible path when given a vector which might contains scalar values or vectors of scaler
values.

Some examples:
input | output
--- | ---
`[:a]` | `[[:a]]`
`[1]` | `[[1]]`
`[:a :b :c]` | `[[:a :b :c]]`
`[:a [:b]]` | `[[:a :b]]`
`[:a [:b :c]]` | `[[:a :b] [:a :c]]`
`[[:a :b] [:c :d]]` | `[[:a :c] [:a :d] [:b :c] [:b :d]]`

Here is my solution to the problem:

{% highlight clojure %}
(ns pathcrawler)

(defn add-key
  [computed key]
  (case [(empty? computed) (coll? key)]
    [true true] (for [k key]
                  (conj (vec computed) k))
    [true false] [[key]]
    [false true] (for [c computed
                       k key]
                   (conj (vec c) k))
    [false false] (for [c computed]
                    (conj (vec c) key))))

(defn compute-paths
  "Returns all single dementional path-sets from a given path."
  ([path]
   (compute-paths path path []))
  ([path path-suffix computed]
   (if (not-empty path-suffix)
     (recur path (rest path-suffix) (add-key computed (first path-suffix)))
     computed)))
{% endhighlight %}

Tests:

{% highlight clojure %}
(deftest test-compute-paths
  (are [p r] (= (pathcrawler/compute-paths p) r)
             ["a"] [["a"]]

             [:a :b :c] [[:a :b :c]]

             ["a" [1 2]] [["a" 1]
                          ["a" 2]]

             [[1 2]] [[1] [2]]

             [["a" "b"] 1] [["a" 1]
                            ["b" 1]]

             [[:a :b] 1] [[:a 1]
                          [:b 1]]

             [[1 2] "a"] [[1 "a"]
                          [2 "a"]]

             [[1 2] :a] [[1 :a]
                         [2 :a]]

             [[1 2] [:a :b]] [[1 :a]
                              [1 :b]
                              [2 :a]
                              [2 :b]]

             [1 [:a :b] 2 [:c :d]] [[1 :a 2 :c]
                                    [1 :a 2 :d]
                                    [1 :b 2 :c]
                                    [1 :b 2 :d]]

             [[:a :b] [:m :n] [:x :y :z]] [[:a :m :x]
                                           [:a :m :y]
                                           [:a :m :z]
                                           [:a :n :x]
                                           [:a :n :y]
                                           [:a :n :z]
                                           [:b :m :x]
                                           [:b :m :y]
                                           [:b :m :z]
                                           [:b :n :x]
                                           [:b :n :y]
                                           [:b :n :z]]))
{% endhighlight %}

As it turns out, I can't use it. I might as well post it.