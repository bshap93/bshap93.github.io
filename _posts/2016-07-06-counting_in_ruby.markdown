---
layout: post
title:  "Counting in Ruby"
date:   2016-07-06 00:49:35 +0000
---

Counting the number of times something occurs is something everyone learning ruby will need to learn, but for people new to programming, the methods for this can be a bit confusing. At its simplest, arguably you use the method .each. Here's an example: say you have a receipt from a food stand:
<pre>array = [
    {"AVOCADO" =&gt; {:cost =&gt; 5.00}},
    {"BEER" =&gt; {:cost=&gt; 20.00}},
    {"AVOCADO" =&gt; {:cost =&gt; 5.00}},
    {"AVOCADO" =&gt; {:cost =&gt; 5.00}},
    {"BEER" =&gt; {:cost=&gt; 20.00}},
    {"CHEESE" =&gt; {:cost =&gt; 15.00}},
    {"CHEESE" =&gt; {:cost =&gt; 15.00}},
    {"CHEESE" =&gt; {:cost =&gt; 15.00}},
]</pre>
You can apply each, taking just the name of the item (the keys) and counting that:
<pre>count_hash = Hash.new(0)
array.each do |item|
  item_name = item.keys
  count_hash[item_name] = count_hash[item_name] + 1
end ##You will get {["AVOCADO"]=&gt;3, ["BEER"]=&gt;2, ["CHEESE"]=&gt;3}</pre>
Also, if you wanted,, or you could use "for" to do the same thing as above or you could add up the costs using "each":
<pre>count_hash = Hash.new(0)
for item in array
  item_name = item.keys
  count_hash[item_name] = count_hash[item_name] + 1
end ##You will get {["AVOCADO"]=&gt;3, ["BEER"]=&gt;2, ["CHEESE"]=&gt;3}

sum = 0
array.each do |item|
  item.each do |item_name, cost|
    sum += cost[:cost]
  end
end ## You will get 100.0</pre>
You could also use inject or reduce in order to count the items:
<pre>array.inject(Hash.new(0)) do |sum, item|
  item.each do |item_name, cost|
    sum[item_name] += 1
  end
  sum
end ## You will get {"AVOCADO"=&gt;3, "BEER"=&gt;2, "CHEESE"=&gt;3}</pre>
This is probably the best method so far, but it can better be expressed with "each_with_object". Each with object uses its block with an arbitrary object argument and returns that object without needing to specifically call the variable of sum or memo, or whatever you want to call it.

As defined on in the ruby docs, each_with_object:
<blockquote>Iterates the given block for each element with an arbitrary object given, and returns the initially given object.</blockquote>
as opposed to inject or reduce which don't, despite what the docs say:
<blockquote>If you specify a block, then for each element in enum the block is passed an accumulator value (memo) and the element. If you specify a symbol instead, then each element in the collection will be passed to the named method of memo. In either case, the result becomes the new value for memo. At the end of the iteration, the final value of memo is the return value for the method.</blockquote>
Feel free to correct or clarify this in the comments or on twitter. Here is my example applied to each_with_object:
<pre>array.each_with_object(Hash.new(0)) {|item, sum| sum[item] += 1 }</pre>
or if you prefer the return value cleaner,
<pre>x = array.each_with_object(Hash.new(0)) do |item, sum|
  item.each do |item_name, item|
    sum[item_name] += 1 
  end 
end ## You will get {"AVOCADO"=&gt;3, "BEER"=&gt;2, "CHEESE"=&gt;3}</pre>
