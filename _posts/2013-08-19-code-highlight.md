---
layout: post
title: 代码高亮测试
categories: [tool]
tags: [highlight]
---

下面是用来测试不同的代码的高亮情况来着

# C

{% highlight c %}
#include <stdio.h>

int main(int argc, char* argv[]) {
  fprintf(stdout, "Say hello to the world\n");
  return 0;
}
{% endhighlight %}

# C++

{% highlight c++ %}
#include <iostream>

int main(int argc, char* argv[]) {
  std::cout << "I am the C++" << std::endl;
  
  return 0;
}

{% endhighlight %}

# Python

{% highlight python %}
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
{% endhighlight %}

# Scala

{% highlight scala %}
scala> def f1(f: Int => Int) {
     |   println(">> f1")
     |   println("  got " + f(23))
     |   println("  got " + f(42))
     |   println("<< f1")
     | }
f1: (f: (Int) => Int)Unit
{% endhighlight %}