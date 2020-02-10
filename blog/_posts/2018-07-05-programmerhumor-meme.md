---
title: Ruining /r/programmerhumor memes #1
tags: meme meta prose noob
toc: true
---

![https://www.reddit.com/r/ProgrammerHumor/comments/8uqhfu/hell_yeah/](img/drake_learn_programming.jpg "Reasons to learn programming"){:height="360px" width="360px"}

Hi everyone, today I'll be ruthlessly destroying some /r/programmerhumor memes for those who are new to programming.

### Gru Tries Recursion

![https://www.reddit.com/r/ProgrammerHumor/comments/85a6n7/gru_tries_recursion/](img/gru_recursion.jpg "No exit..."){:height="284px" width="412px"}

[Recursion](https://en.wikipedia.org/wiki/Recursion) is an elementary operation in logic, maths, and computer science where an operation is partially defined in terms of itself. While the most common example is the Fibonnaci function, the best example is perhaps addition of two integers:

<h4>Recursion in various languages</h4>
<div role="navigation">
<ul class="nav nav-tabs" role="tablist">
<li class="nav-item">
<a id="basicRecur-tab" class="nav-link active" data-toggle="tab" href="#basicRecur" role="tab" aria-controls="basicRecur" aria-selected="true">Basic Recursion</a>
</li>
  <li class="nav-item">
  <a id="pythonRecur-tab" class="nav-link" data-toggle="tab" href="#pythonRecur" role="tab" aria-controls="pythonRecur" aria-selected="true">Python</a>
  </li>
    <li class="nav-item">
      <a id="rubyRecur-tab" class="nav-link" data-toggle="tab" href="#rubyRecur" role="tab" aria-controls="rubyRecur" aria-selected="false">Ruby</a>
    </li>
  </ul>
  <div class="tab-content">
  <div class="tab-pane fade show active" id="basicRecur" role="tabpanel" aria-labelledby="pythonRecur-tab">
  <h5>Basic Recursion: Addition</h5>
  {% highlight python %}
>def add(a, b):
>  if b = 0: # The 'exit condition' allows recursion to end, return value is *not* defined in terms of this function.
>    return a
>  else:
>    return add(a+1, b-1) # Here, the add function is called on incremented/decremented arguments. 
  {% endhighlight %}
</div>
    <div class="tab-pane fade" id="pythonRecur" role="tabpanel" aria-labelledby="pythonRecur-tab">
      <h5>Fibonacci: Iterative, Recursive, Tail-recursive</h5>

{% highlight python %}
# P E R F O R M A N C E
# 2.7.10, GCC 4.2.1, Apple LLVM 8.0 on 2011 OSX
>import numpy
>import timeit
# Regular, non-recursive imperative/procedural implementation.
>def fibIter(n):
>  if n < 2:
>    return n
>  fibPrev = 1
>  fib = 1
>  for num in xrange(2, n):
>    fibPrev, fib = fib, fib + fibPrev
>  return fib
# Recursive, simple, and intuitive. But not very efficient for *machines* for larger n
>def fibRecur(n):
>  if n < 2:
>    return n
>  else:
>    return fibRec(n - 1) + fibRec(n - 2)
# Tail recursive, simple outer wrapper with defaults, efficient for machines.
>def fibTailRecur(n):
>  def fib(a, b, m):
>    if m < 1:
>      return a
>    else:
>      return fib(b, a + b, m - 1)
>  return fib(0, 1, n)

{% endhighlight %}


    </div>
    <div class="tab-pane fade" id="rubyRecur" role="tabpanel" aria-labelledby="rubyRecur-tab">
      <h5>Fibonacci: Iterative, recursive, tail recursive</h5>
{% highlight ruby %}
>require 'memory_profiler'
>def fibIter(n):
>  if n < 2:
>    return n
>  else:
>    a, b = 1, 1
>    (n-2).times {|x| a,b = b, a + b}
>  return n
>end

>def fibRecur(n)
>  if n < 2:
>    return n
>  else:
>    return fibRecur(n-1) + fibRecur(n-2)
>end

>def fibTailRecur(n)
>  def fib(a, b, m)
>    if m < 1
>      return 1
>    else
>      return fib(b, a+b, m-1)
>    end
>  end
>  return fib(0, 1, n)
>end

>
{% endhighlight %}
      
    </div>
  </div>
</div>


{::nomarkdown}
<p>
While the concept seems simple as presented here, a special type of recursion should be mentioned (as it's mentioned in nearly every other ignorant hipster treatment of recursion these days): tail recursion. <a href="#" data-toggle="popover" title="" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/pooh_recursion.gif" height="100%" width="100%" />'>Tail recursion</a> is a <a href="#" data-toggle="popover" title="cat.exe has stopped working" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/cat_tail_recursion.jpg" height="100%" width="100%" />'>special</a> <a href="#" data-toggle="popover" title="HotlinePing.jpg" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/drake_memes_without_recursion.jpg" height="100%" width="100%" />'>type</a> of <a href="#" data-toggle="popover" title="Wonder when these guys will pop up again..." data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/creed_recursion_look_at_this_photograph.jpg" height="100%" width="100%" />'>recursive</a> treatment that results in a <a href="#" data-toggle="popover" title="There are two types of people in the world..." data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/xkcd_tail_recursion.png" height="100%" width="100%" />'>constant memory</a> requirement as the recursion progresses, instead of the linear memory requirements afforded by poorly formed recursive functions. The differences can be seen in the 'Python' and 'Ruby' panels above.
</p>
{:/}

### Object-oriented Zucc

![https://www.reddit.com/r/ProgrammerHumor/comments/8bh21u/not_oc_zuckerborgdrink/](img/zucc_quenched.gif "expect(zucc.isQuenched()).to(be_true)"){:height="338px" width="600px"}

{::nomarkdown}

<p>Next, no discussion is complete in 2018 without mentioning <a href="https://en.wikipedia.org/wiki/Object-oriented_programming">OOP</a>, by and far the <a href="https://en.wikipedia.org/wiki/Comparison_of_multi-paradigm_programming_languages">dominant programming paradigm</a> in industry. Too simplify, we'll assume basic knowledge of computer programs and describe <a href="#" id="listly" data-toggle="popover" data-placement="auto" data-trigger="hover" data-html="true" data-content="<div><ul><li>Scripts / Command-line utilities (e.g. <code class='highlighter-rouge'>cat</code>, <code class='highlighter-rouge'>example.py</code>, <code class='highlighter-rouge'>git</code>, <code class='highlighter-rouge'>apt-get</code>)</li><li>Libraries</li><li>Thick applications:<ul><li>Applications, utilities, games, etc.</li><li>Web servers</li><ul><li>Websites (optional db)</li><li>Databases (and related terminology)</li><li>REST/SOAP services (no GUI)</li></ul></ul></li></ul></div>">a few classes of software</a>. In the gif above is the actual source code of Zucc's internal programming. The .gif alludes to event-driven programming, where the 'state' of Mark's thirst is stored in a boolean attribute/instance-variable, accessed from <code class='highlighter-rouge'>zucc.isQuenched()</code>. Mark responds to thirst with the following instance methods, some of which receive a hand instance: <code class='highlighter-rouge'>zucc.reach(right)</code>, <code class='highlighter-rouge'>zucc.grip(right)</code>, <code class='highlighter-rouge'>zucc.raise(right)</code>, <code class='highlighter-rouge'>zucc.sip()</code>. The joke is obvious due to the high-level abstractions of the tasks, as well as appropriate method/instance names. Looking for more reading on objects, classes, instances and OOP? Here is an <a href="https://www.khanacademy.org/computing/computer-programming/programming/object-oriented/a/review-object-oriented-design">excellent review from Salman Khan</a>.</p>
{:/}

### FrontEnd vs. BackEnd

![https://www.reddit.com/r/ProgrammerHumor/comments/7zfgwg/frontend_vs_backend/](img/frontend_vs_backend.jpg "One banana, two banana, three banana, four"){:height="705px" width="500px"}

I debated whether this meme deserved to make the cut and decided to include it for those who don't have much experience making different kinds of applications. Not every developer will want to make different kinds of applications but I think this can still help everyone understand the perspective we give towards applications. There's also a dual nature to discuss here and I'll start with UI. User interfaces always need to be simplistic and intuitive to reach the most users. In my opinion, it is sad but true that utility doesn't always win when it comes to the 'value' of an application and software developers that neglect their UIs are often punished.

That said, the image above the surface is calm, tranquil, and simple. The user is given a pretty, simplistic picture of the state of the creature, without any distraction or clutter. Of course this is the way things look to the user, but that does not at all mean that the code for the UI is in any way simple. In fact the above-the-surface depiction of simplicity and aesthetic is in stark contrast to the complexity of UI design. It takes a true love for artistic principles and design concepts to understand how the user thinks and what they value.

In contrast, the image shows beneath the 'surface' of the communication layer (HTTP/TCP) is an ugly creature, with different appendages, and a gruesome mechanical and technical feel. I'm sure that the abstract content of back-end source code might seem 'ugly' to front-end developers or even the user, but the functionality is anything but ugly. The aesthetic is in the structure of the app but also in convenient frameworks used by back server-side developers (e.g. Rails, Flask, Django, Express).



That's it for this post! I'll be doing this often I hope, ruining the punchline...



#### Extras


{::nomarkdown}
<p>
Does anyone else remember <a href="#" data-toggle="popover" title="You're all wrong. This is why it happened. https://goo.gl/KyLh7Y" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/hawaii_missile1.png" height="100%" width="100%" />'>the</a> <a href="#" data-toggle="popover" title="REAL Screenshot from Hawaii Warning System (ACTUAL!) https://goo.gl/MeYRZD" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/hawaii_missile2.gif" height="100%" width="100%" />'>Hawaii</a> <a href="#" data-toggle="popover" title="The Missile Alert System used simple HTML! How bad could it be?! https://goo.gl/rE5g7n" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/hawaii_missile3.gif" height="100%" width="100%" />'>missile</a> <a href="#" data-toggle="popover" title="A Github solution to the Hawaii missile alert https://goo.gl/6MoXNW" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/hawaii_missile4.png" height="100%" width="100%" />'>warning</a> <a href="#" data-toggle="popover" title="What if /r/ProgrammerHumor was responsible for the Missile Alert UI https://goo.gl/RznXCt" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/hawaii_missile5.gif" height="100%" width="100%" />'>false</a> <a href="#" data-toggle="popover" title="Hawaii alert system, confirmed UI. Made by EA https://goo.gl/98TGe6" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/hawaii_missile6.png" height="100%" width="100%" />'>alarm</a>?

</p>
{:/}

