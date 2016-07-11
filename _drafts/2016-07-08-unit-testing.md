---
layout: post
title:  "Testing Done Right"
date:   2016-07-08 11:49:42 -0700
categories: testing
---
How do I know a test is good?

How do I know I have the right tests?

How do I know that the tests that are written *actually* highlight errors in the application when they fail?

I've had these questions for a while. I'm not really sure I can answer them yet.

I think that knowing how to test an application is way more complicated than we treat it.

Take code coverage for example. For some reason we've gotten it into our heads that if we visit all the lines (or branches) of a method then we've done enough testing. This is simple enough to disprove:

{% highlight java %}
public int add5(int x){
  return x + 5;
}
{% endhighlight %}

If we <code>assertEquals(add5(1), 6)</code> we've visited all the lines of code and we're done, right? What happens when you call <code>add5(2147483646)</code>?

Even if we are in the habit of considering edge cases, null cases, zero cases, and suit cases I don't think I've seen tests that are really *valuable* in real life.

Perhaps one of the things that bothers me the most about unit tests is a reaction not uncommon when they fail. If your immediate reaction is to fix the test, what is the point of your test in the first place? If tests exist just so we can fix them when they fail, we're just making more work for ourselves.

Of course I'm not saying that writing tests is bad nor that they shouldn't be fixed when they fail. My point is, if a unit test fails there must be a series of questions we ask ourselves including whether the new code we've written is right and whether the test was right in the first place and if the change we made in the code is one that is correct and if we expect this unit test to fail. We may even have to reconsider if this unit test is still valuable to us. We can't just say, "Oh, I changed <code>add5()</code> to <code>return x + 6</code> so I need to change the unit tests." Yes, that is as ridiculous as it sounds.

## Essential Testing
When I learned to program I was trained to think in a very imperative mindset. <code>step1; step2; step3;</code> As I matured in my career I was introduced to a declarative mind set. The difference I think was answering the question 'What should it do?' rather than 'How should it do it?'. I've begun to think of testing in the same way. Rather than asserting every conceivable validation we can think of, we should think of the essential properties of what an algorithm should do.

After watching an [excellent talk about bottom up programming](https://www.youtube.com/watch?v=Tb823aqgX_0) I decided to play around a little and write a simple app that shuffled a deck of cards. Each card has two keywords in a vector representing the suit and the rank like this: <code>[:hearts :two]</code>. I came up with a pretty simple solution:

{% highlight clojure %}
(def ranks [:ace :two :three :four :five :six :seven :eight :nine :ten :jack :queen :king])

(def new-deck
  (for [suit [:hearts :diamonds :spades :clubs] rank ranks] [suit rank]))

(defn shuffle-deck
  ([] (shuffle-deck new-deck))
  ([deck] (shuffle deck)))
{% endhighlight %}

Of course I tested this in the REPL and I saw what I expected, but I doubted that I had done it correctly the first time (because that never happens) so I started to write some unit tests.

My first unit tests were a mess:

{% highlight clojure %}
(defn deck-assertions [deck]
  (println "testing deck " deck)
  (is (= (count (set deck)) 52))
  (is (= (count (filter #(= (first %) :clubs) deck)) 13))
  ...
  (is (= (count (filter #(= (last %) :ace) deck)) 4))
  ...
  (is (= (count (filter #(= (last %) :king) deck)) 4)))

(deftest test-shuffle-deck
  (deck-assertions (shuffle-deck))
  (deck-assertions (shuffle-deck (shuffle-deck)))
  (is (not= (shuffle-deck) (shuffle-deck) (shuffle-deck) (shuffle-deck))))

(deftest test-new-deck
  (is (= new-deck [[:hearts :ace]
                   [:hearts :two]
                   ...
                   [:clubs :queen]
                   [:clubs :king]])))
{% endhighlight %}

There are good aspects of the code, but I had 86 lines of test code when the code I was testing only had 8. I wasn't too concerned with the tests having more lines than the solution, but I felt that my tests were missing something. That's when I added:

{% highlight clojure %}
  (is (= (sort (shuffle-deck)) (sort new-deck)))
{% endhighlight %}

Which I think is probably the first good test I wrote. But I wanted to wright a test that verified that two shuffled decks were probably different. My first naïve test:

{% highlight clojure %}
  (is (not= (shuffle-deck) (shuffle-deck) (shuffle-deck) (shuffle-deck)))
{% endhighlight %}

But I didn't think that was good enough so I wrote another naïve test:

{% highlight clojure %}
  (is (apply not= (repeatedly 100 shuffle-deck)))
{% endhighlight %}

But there's a problem with this test. It doesn't provide any real knowledge. If it fails then all 100 <code>shuffled-deck</code>s contain cards that are in the same order. If it passes I know only that one deck is different than the other 99.

I think a better test is this one:

{% highlight clojure %}
  (is (< 990 (count (set (repeatedly 1000 shuffle-deck)))))
{% endhighlight %}

Here I still generate several shuffled decks, but I get the count of only the unique decks and expect that number to be greater than a large percentage of the generated decks (99% in this case). (So far every time I've looked at it, the count of the set of shuffled decks has been 1000).

At this point I feared that my tests were tautological. I had too many tests that were testing the same thing. But I still wanted to verify that a deck of cards contained 52 cards and that each of these cards were a vector of a suit and a rank, each one being unique. Of course we know that if each card contains one of <code>[:hearts :diamonds :spades :clubs]</code> as the first value and one of <code>[:ace ... :king]</code> as its second element and that they all have only two values, then maybe the right answer is something like this:

{% highlight clojure %}
(defn test-cards [cards]
  (if-let [card (first cards)]
    (do
      (is (= (count card) 2))
      (is (contains? #{:hearts :diamonds :spades :clubs} (first card)))
      (is (contains? #{:ace :two :three :four :five :six :seven :eight :nine :ten :jack :queen :king} (last card)))
      (recur (rest cards)))))

(deftest test-shuffle-deck
  (is (= (count (set (shuffle-deck))) 52))
  (test-cards (shuffle-deck))
  (is (= (sort (shuffle-deck)) (sort new-deck)))
  (is (< 990 (count (set (repeatedly 1000 shuffle-deck))))))
{% endhighlight %}
