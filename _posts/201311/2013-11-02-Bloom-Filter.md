---
layout: post  
title: Bloom Filter and example   
categories:  
- algorithm  
tags:    
- algorithm
---   
 
**Bloom Filter**


#### 1 Introduction 
 When we write crawler,  if we have to crawler millions websites,  we need to check whether a website is crawled or not，
 So we need both space and time efficient algorithoms to achieve this goal, Bloom filter is our choice.

A Bloom filter is a data structure designed to tell you, rapidly and memory-efficiently, whether an element is present in a set.

The price paid for this efficiency is that a Bloom filter is a probabilistic data structure: it tells us that the element either definitely is not in the set or may be in the set.[[1]](http://billmill.org/bloomfilter-tutorial/)

From **[Wikipedia](http://en.wikipedia.org/wiki/Bloom_filter)**:

> Bloom filters have a strong space advantage over other data structures for representing sets, such as self-balancing binary search trees, tries, hash tables, or simple arrays or linked lists of the entries. Most of these require storing at least the data items themselves, which can require anywhere from a small number of bits, for small integers, to an arbitrary number of bits, such as for strings (tries are an exception, since they can share storage between elements with equal prefixes). Linked structures incur an additional linear space overhead for pointers. A Bloom filter with 1% error and an optimal value of k, on the other hand, requires only about 9.6 bits per element — regardless of the size of the elements. This advantage comes partly from its compactness, inherited from arrays, and partly from its probabilistic nature. If a 1% false positive rate seems too high, each time we add about 4.8 bits per element we decrease it by ten times.
> 
 
#### 2 Use case
  a) search engine, when crawls website, check whether a website is crawled or not.

  b) browses use this to check whether a url is malicious or not.


#### 3 How Google use Bloom Filter

The original article is an excellent one to explain How Google chrome use bloom filtes, so I just quote the article.

####  [Nice Bloom filter application](http://blog.alexyakunin.com/2010/03/nice-bloom-filter-application.html)

Today I accidentally found a couple of interesting files in one of Google Chrome folders:    

*    Safe Browsing Bloom   
 
* Safe Browsing Bloom Filter 2


Conclusion: **Chrome uses Bloom filters to make a preliminary decision whether a particular web site is malicious or safe**. Cool idea!


######Let me explain how it works:

*  Web site URL is converted to some canonical form (or a set of such forms e.g. by sequentially stripping all the sub-domains; in this case check is performed for each of such URLs).

*  N of its "long" hashes are computed (likely, there are 64-bit hashes).
The value of each hash is multiplied by scale factor. That's how N addresses of bits in bit array called Bloom filter are computed.

* The filter is designed so that if all these bits are set, the site is malicious with very high probability (in this case Chrome performs its precise verification likely, by issuing a request to the corresponding Google service); otherwise it is guaranteed to be safe.

###### The benefits of this method :

*   The size of such a filter is considerably smaller than size of any other structure providing precise "yes/no" answers to similar questions. For example, if it is a set (data structure), its data length should be nearly equal to the total length of all the URLs of malicious sites it "remembers". But a Bloom filter providing false positive responses with a probability of 1% (note that it can't provide false negative response) would require just 9.6 bits per each URL of malicious web site it "remembers", i.e. nearly 1 byte! Taking into account that size of Chrome Bloom filters is about 18Mb and assuming they are really built for such a false positive response probability, this means they contain information about ~ **1 million of malicious web sites**!

*  Bloom filter allows Chrome to use precise verification service practically only when the user actually goes to a malicious web site. Isn't it wonderful? ;)



