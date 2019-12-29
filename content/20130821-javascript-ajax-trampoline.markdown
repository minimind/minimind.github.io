category: Javascript
date: 2013/09/07 18:05:00
title: Javascript Ajax Trampoline Example
author: Ian Macinnes

<link rel="stylesheet" href="/css/ajax-trampoline.css" type="text/css" media="screen">

<p>A trampoline is a function that arranges another function (sometimes itself) 
to be called in a non-recursive fashion. They are most known for 
<a href="http://en.wikipedia.org/wiki/Tail_call_optimization#Through_trampolining" target="_blank">tail call 
optimization</a> as a technique to avoid recursive calls and so save stack space. They can be used to make Ajax 
calls more reliable.</p>

<p>Ajax calls can fail due to temporary network errors, but these can resolve themselves if you wait and try again. One way
to deal with this would be to write an explicit loop that terminates when an Ajax call has succeeded. A better 
alternative is to use a trampoline to deal with errors or successes autonomously and asynchronously in the background. 
That way, you can let Ajax functions handle network issues or other problems themselves without having to poll the 
status of the call.</p>

<p>A example of a Ajax trampoline can be found here. Once set up, it continually tries to perform
an Ajax call, dealing with errors and successes using callbacks. It also includes a callback to check if it should
be canceled for other reasons e.g. somebody clicks on a cancel button.</p>
 
<p>This is the trampoline function itself:</p>

<div class="highlight"><pre><span class="kd">var</span> <span class="nx">ajaxTrampoline</span> <span class="o">=</span> <span class="kd">function</span><span class="p">(</span><span class="nx">interval</span><span class="p">,</span> <span class="nx">url</span><span class="p">,</span> <span class="nx">data</span><span class="p">,</span> <span class="nx">carryOn</span><span class="p">,</span> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">onSuccess</span><span class="p">,</span> <span class="nx">onSingleFailure</span><span class="p">,</span> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">onFinalFailure</span><span class="p">,</span> <span class="nx">maxTries</span><span class="p">)</span> <span class="p">{</span>
&nbsp;&nbsp;<span class="kd">var</span> <span class="nx">loopAround</span> <span class="o">=</span> <span class="kd">function</span><span class="p">(</span><span class="nx">i</span><span class="p">,</span> <span class="nx">timeToWait</span><span class="p">)</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">if</span> <span class="p">(</span><span class="nx">carryOn</span><span class="p">())</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="kd">var</span> <span class="nx">timerFunc</span> <span class="o">=</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">if</span> <span class="p">(</span><span class="nx">carryOn</span><span class="p">())</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">$</span><span class="p">.</span><span class="nx">ajax</span><span class="p">({</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">type</span><span class="o">:</span>    <span class="s1">&#39;POST&#39;</span><span class="p">,</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">url</span><span class="o">:</span>     <span class="nx">url</span><span class="p">,</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">data</span><span class="o">:</span>    <span class="nx">data</span><span class="p">,</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">success</span><span class="o">:</span> <span class="nx">onSuccess</span><span class="p">,</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">error</span><span class="o">:</span>   <span class="kd">function</span><span class="p">(</span><span class="nx">req</span><span class="p">,</span> <span class="nx">text</span><span class="p">,</span> <span class="nx">e</span><span class="p">)</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">if</span> <span class="p">(</span><span class="nx">carryOn</span><span class="p">())</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">if</span> <span class="p">(</span><span class="nx">i</span> <span class="o">&gt;=</span> <span class="nx">maxTries</span><span class="p">)</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">onFinalFailure</span><span class="p">();</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span> <span class="k">else</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">onSingleFailure</span><span class="p">(</span><span class="o">++</span><span class="nx">i</span><span class="p">,</span> <span class="nx">maxTries</span><span class="p">,</span> <span class="nx">req</span><span class="p">,</span> <span class="nx">text</span><span class="p">,</span> <span class="nx">e</span><span class="p">);</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">loopAround</span><span class="p">(</span><span class="nx">i</span><span class="p">,</span> <span class="nx">interval</span><span class="p">);</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">});</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">};</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">if</span> <span class="p">(</span><span class="nx">timeToWait</span> <span class="o">===</span> <span class="mi">0</span><span class="p">)</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">timerFunc</span><span class="p">();</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span> <span class="k">else</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">setTimeout</span><span class="p">(</span><span class="nx">timerFunc</span><span class="p">,</span> <span class="nx">interval</span><span class="p">);</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">}</span>
&nbsp;&nbsp;<span class="p">};</span>
&nbsp;&nbsp;<span class="nx">loopAround</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">);</span>
<span class="p">};</span>
</pre></div>

<p><em>interval</em> is the time in milliseconds between attempts. <em>carryOn</em> is a call back that states whether
to carry on trying. For example, if there is a cancel button which should stop the attempt, then <em>carryOn</em>
might be defined something like:</p>

<div class="highlight"><pre><span class="kd">var</span> <span class="nx">carryOnModule</span> <span class="o">=</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;<span class="kd">var</span> <span class="nx">carryOnFlag</span> <span class="o">=</span> <span class="kc">true</span><span class="p">;</span>
&nbsp;&nbsp;<span class="nx">$</span><span class="p">(</span><span class="s1">&#39;#cancel-button&#39;</span><span class="p">).</span><span class="nx">one</span><span class="p">(</span><span class="s1">&#39;click&#39;</span><span class="p">,</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="nx">carryOnFlag</span> <span class="o">=</span> <span class="kc">false</span><span class="p">;</span>
&nbsp;&nbsp;<span class="p">});</span>

&nbsp;&nbsp;<span class="k">return</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">return</span> <span class="nx">carryOnFlag</span><span class="p">;</span>
&nbsp;&nbsp;<span class="p">}</span>
<span class="p">};</span>

<span class="kd">var</span> <span class="nx">carryOn</span> <span class="o">=</span> <span class="nx">carryOnModule</span><span class="p">();</span>
</pre></div>

<p><em>onSuccess</em>, <em>onSingleFailure</em>, and <em>onFinalFailure</em> are all callbacks and might be defined as:

<div class="highlight"><pre><span class="kd">var</span> <span class="nx">onSuccess</span> <span class="o">=</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;<span class="nx">console</span><span class="p">.</span><span class="nx">log</span><span class="p">(</span><span class="s1">&#39;Succeeded!&#39;</span><span class="p">);</span>
<span class="p">};</span>

<span class="kd">var</span> <span class="nx">onSingleFailure</span> <span class="o">=</span> <span class="kd">function</span><span class="p">(</span><span class="nx">i</span><span class="p">,</span> <span class="nx">maxTries</span><span class="p">)</span> <span class="p">{</span>
&nbsp;&nbsp;<span class="nx">console</span><span class="p">.</span><span class="nx">log</span><span class="p">(</span><span class="s1">&#39;Now tried &#39;</span> <span class="o">+</span> <span class="nx">i</span> <span class="o">+</span> <span class="s1">&#39; out of &#39;</span> <span class="o">+</span> <span class="nx">maxTries</span><span class="p">);</span>
<span class="p">};</span>

<span class="kd">var</span> <span class="nx">onFinalFailure</span> <span class="o">=</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
&nbsp;&nbsp;<span class="nx">console</span><span class="p">.</span><span class="nx">log</span><span class="p">(</span><span class="s1">&#39;I have now given up!&#39;</span><span class="p">);</span>
<span class="p">};</span>
</pre></div>