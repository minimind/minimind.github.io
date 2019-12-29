category: Javascript
date: 2013/03/26 21:45:00
title: jQuery plugin to slide and fade elements
author: Ian Macinnes

<link rel="stylesheet" href="/css/javascriptpost.css" type="text/css" media="screen">
<script type="text/javascript" src="/js/jquery.slideandfade.js"></script>
<script type="text/javascript" src="/js/javascriptpost.js"></script>

I've written a new effects jQuery plugin and it's available on my
<a class="non-selected"  target="_blank" href="https://github.com/minimind">GitHub</a> page.
It's called the "slide and fade" jQuery plugin and can be used as a very simple
and quick method of sliding whole groups of elements randomly out of frame and moving other
ones in. It's under active development so please contact me for fixes/suggestions etc.
The plugin can simply make divisions flying around randomly either up, down,
left, or right. Below is an example - click on the box to
see it working. Each group of elements should be of the class 'displayBox' and each element
should be of the class fragment (I'll make this configurable via the options soon).

<div class="display-screen-description" id="display-screen1">
    <div class="displayBox" id="displayBox1_0">
        <div class="fragment" id="a">The owl and the pussy cat</div>
        <div class="fragment" id="b">Went to sea</div>
        <div class="fragment" id="c">In a beautiful pea green boat</div>
    </div>
    <div class="displayBox" id="displayBox1_1">
        <div class="fragment" id="d">They took some honey,</div>
        <div class="fragment" id="e">And plenty of money</div>
        <div class="fragment" id="f">Wrapped up in a five pound note.</div>
    </div>
</div>

Here's the HTML code for displaying the above box:

<div class="highlight"><pre><span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;display-screen-description&quot;</span> <span class="na">id=</span><span class="s">&quot;display-screen&quot;</span><span class="nt">&gt;</span>
&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;displayBox&quot;</span> <span class="na">id=</span><span class="s">&quot;displayBox0&quot;</span><span class="nt">&gt;</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;fragment&quot;</span> <span class="na">id=</span><span class="s">&quot;a&quot;</span><span class="nt">&gt;</span>The owl and the pussy cat<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;fragment&quot;</span> <span class="na">id=</span><span class="s">&quot;b&quot;</span><span class="nt">&gt;</span>Went to sea<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;fragment&quot;</span> <span class="na">id=</span><span class="s">&quot;c&quot;</span><span class="nt">&gt;</span>In a beautiful pea green boat<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;displayBox&quot;</span> <span class="na">id=</span><span class="s">&quot;displayBox1&quot;</span><span class="nt">&gt;</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;fragment&quot;</span> <span class="na">id=</span><span class="s">&quot;d&quot;</span><span class="nt">&gt;</span>They took some honey,<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;fragment&quot;</span> <span class="na">id=</span><span class="s">&quot;e&quot;</span><span class="nt">&gt;</span>And plenty of money<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">&quot;fragment&quot;</span> <span class="na">id=</span><span class="s">&quot;f&quot;</span><span class="nt">&gt;</span>Wrapped up in a five pound note.<span class="nt">&lt;/div&gt;</span>
&nbsp;&nbsp;<span class="nt">&lt;/div&gt;</span>
<span class="nt">&lt;/div&gt;</span>
</pre></div>

The id's here are just to specify the positions of the elements in the stylesheet and aren't used by slideandfade.
The plugin works by passing selectors so the id names are arbitrary. The code to move a particular display box into shot is:

<div class="highlight"><pre><span class="nx">$</span><span class="p">(</span><span class="s1">&#39;#display-screen&#39;</span><span class="p">).</span><span class="nx">slideandfade</span><span class="p">(</span><span class="nx">$</span><span class="p">(</span><span class="s1">&#39;#displayBox0&#39;</span><span class="p">));</span>
</pre></div>

You can also specify an optional callback for after the whole process is finished.
You use a settings dictionary to pass along various options.
Above, I use an option called callback to toggle which text to display.
This callback is always called after the text has been moved into position.

<div class="highlight"><pre><span class="kd">var</span> <span class="nx">screenToDisplay</span> <span class="o">=</span> <span class="mi">1</span><span class="p">;</span>
<span class="nx">$</span><span class="p">(</span><span class="s1">&#39;#display-screen&#39;</span><span class="p">).</span><span class="nx">click</span><span class="p">(</span><span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;<span class="kd">var</span> <span class="nx">settings</span> <span class="o">=</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">callback</span> <span class="o">:</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">screenToDisplay</span> <span class="o">=</span> <span class="nx">screenToDisplay</span> <span class="o">===</span> <span class="mi">0</span> <span class="o">?</span> <span class="mi">1</span> <span class="o">:</span> <span class="mi">0</span><span class="p">;</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;<span class="p">};</span>

&nbsp;&nbsp;<span class="nx">$</span><span class="p">(</span><span class="s1">&#39;.display-screen&#39;</span><span class="p">).</span><span class="nx">slideandfade</span><span class="p">(</span><span class="nx">$</span><span class="p">(</span><span class="s1">&#39;#displayBox&#39;</span> <span class="o">+</span> <span class="nx">screenToDisplay</span><span class="p">),</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">settings</span><span class="p">);</span>
<span class="p">});</span>
</pre></div>

It's also easy to have two slideandfade boxes on the same page:

<div class="display-screen-description" id="display-screen2">
    <div class="displayBox" id="displayBox2_0">
        <div class="fragment" id="g">Pussy said to the Owl,</div>
        <div class="fragment" id="h">You elegant fowl!</div>
        <div class="fragment" id="i">How charmingly sweet you sing!</div>
    </div>
    <div class="displayBox" id="displayBox2_1">
        <div class="fragment" id="j">O let us be married!</div>
        <div class="fragment" id="k">too long we have tarried:</div>
        <div class="fragment" id="l">But what shall we do for a ring?</div>
    </div>
</div>
