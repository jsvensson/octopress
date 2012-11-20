---
layout: post
title: "Sorting a Hash in Ruby"
date: 2012-11-19 22:11
categories:
 - programming
 - ruby
comments: true
published: true
---

It started as a silly little contest.

<!-- more -->

We've just opened a new section at work, and we needed more signs for the new rooms. These signs are wooden letters, bought individually. We wrote down the signs we needed, and then someone started counting the letters by hand.

"I can do that faster!" I boldly exclaimed. And the contest was on.

This was the first quick and dirty version.

{% codeblock Counting letters, first version lang:ruby %}
#encoding: utf-8

words = [
  "Skapande",
  "Möte",
  "Musik",
  "Kontor 1",
  "Kontor 2",
  "Städ",
  "Förråd",
  "Jobb",
  "WC",
  "WC",
  "Information"
]

wordlist = words.join.upcase
letters = {}

wordlist.length.times do |i|
  count = letters[wordlist[i]] ? letters[wordlist[i]] : 0
  letters[wordlist[i]] = count + 1
end

letters.each do |k, v|
  puts "#{k}: #{v}"
end

{% endcodeblock %}

It worked, but it was dirty. But hey, I had a contest to win. Which, by the way, I did win.

Take an array of the signs, mash it together into a string of letters, loop through it and make a hash with the letters as keys, counting occurences.

## Cleanup ##

Afterwards, I spent a little time making it better, because it's a shame to leave dirty code behind when it's easy to fix.

The first step was remembering that passing an object as argument to [`Hash::new`][Hash::new] will change the default `nil` a hash returns if you access a non-existing key. That lets me get rid of the clumsy ternary operator and just count letters like this:

{% codeblock Improved version lang:ruby %}
letters = Hash.new(0)

wordlist.length.times do |i|
  letters[wordlist[i]] += 1
end
{% endcodeblock %}

That's a lot more readable -- it boils down to just doing `letters["A"] += 1` to count letters, without having to bother about what may or may not already be stored in the hash.

## Let's sort stuff ##

The next step was to sort the output. In Ruby, a Hash returns the keys in the same order they were inserted -- by default there's no real concept of a "sorted hash", though Ruby >= 1.9.2 adds a `Hash::Ordered`. But I wanted to play around a bit.

We'll use this hash for testing.

``` ruby
h = {:a=>1, :e=>5, :c=>3, :d=>4, :b=>2}
```

## Take one ##

The first try was the classic way. `Hash` mixes in [`Enumerable#sort`][Enumerable#sort] which will return an array containing arrays with key-value pairs in order sorted by the keys of the original hash:

``` ruby
>> h.sort
=> [[:a, 1], [:b, 2], [:c, 3], [:d, 4], [:e, 5]]
```

[`Hash::[]`][Hash::bracket] can take this array and turn it back into a hash, which is now sorted by the original keys. Then go through each hash entry and print them.

{% codeblock Hash sorting, take 1 lang:ruby %}
Hash[h.sort].each { |key, value| puts "#{key}: #{value}" }
{% endcodeblock %}

It works, but I felt it was a bit obtuse and not very Ruby-like, recasting it to an array and then throwing that into a new hash constructor. Let's try again.

## Take two ##

[`Hash#keys`][Hash#keys] returns an array containing just the keys. We sort that with [`Array#sort`][Array#sort].

``` ruby
>> h.keys
=> [:a, :e, :c, :d, :b]
>> h.keys.sort
=> [:a, :b, :c, :d, :e]
```

Now just loop through it and print the keys and values. However, note that we end up working with an array of the keys and thus lose access to the key-value pairs in the original hash -- we'll have to access the values via keys in the hash rather than having the block pass the values to us.

{% codeblock Hash sorting, take 2 lang:ruby %}
h.keys.sort.each { |key| puts "#{key}: #{h[key]}"}
{% endcodeblock %}

The syntax outside of the block feels a bit better than the first try, but I didn't like not getting the key-value pairs as variables in the block. It doesn't feel properly Ruby-like to work on another object than the one you actually want the information from.

Let's see if we can improve.

## Take three ##

I looked at [`Enumerable#sort`][Enumerable#sort] again:

``` ruby
>> h.sort
=> [[:a, 1], [:b, 2], [:c, 3], [:d, 4], [:e, 5]]
```

It returns the array-form of a hash. So I just tried looping through it with `each`. And it *worked*:

{% codeblock Hash sorting, take 3 lang:ruby %}
h.sort.each { |key, value| puts "#{key}: #{value}"}
{% endcodeblock %}

This felt like magic when I ran it. It's definitely the most Ruby-like syntax of them all -- take the hash `h`, sort it, loop through it with key and value as variables.

I just don't understand how it works internally. `h.sort` returns an `Array`; `h.sort.each` returns an `Enumerator`. [`Enumerator#each`][Enumerator#each] says this:

{% blockquote %}
Iterates over the block according to how this Enumerable was constructed. If no block is given, returns self.
{% endblockquote %}

Enumerables are still a bit of a mystery to me. Definitely something I need to look closer at.

## Wrapping up ##

This started as a very simple little exercise in counting words, and I ended up looking at various Ruby internals under the hood. That's what I like about these little excursions. It may start as something very simple, but as I look at better (or just different) ways to do the same thing, I end up learning a whole lot more.

As a side note, my letter-counting code tells me we need to buy three spaces for the signs.



[Hash::new]: http://www.ruby-doc.org/core-1.9.3/Hash.html#method-c-new
[Hash::bracket]: http://www.ruby-doc.org/core-1.9.3/Hash.html#method-c-5B-5D
[Enumerable#sort]: http://ruby-doc.org/core-1.9.3/Enumerable.html#method-i-sort
[Array#sort]: http://www.ruby-doc.org/core-1.9.3/Array.html#method-i-sort
[Hash#keys]: http://www.ruby-doc.org/core-1.9.3/Hash.html#method-i-keys
[Enumerator#each]: http://www.ruby-doc.org/core-1.9.3/Enumerator.html#method-i-each
