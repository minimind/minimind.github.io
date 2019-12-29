category: Python
date: 2013/03/31 07:21:00
title: Short genetic algorithm in Python
author: Ian Macinnes

<link rel="stylesheet" href="/css/veryshortgapost.css" type="text/css" media="screen">
<p>Here is a <a href="https://github.com/minimind/verySimpleGeneticAlgorithm" target="_blank">very short genetic algorithm</a>
using rank selection written in Python. It's an exercise to help myself write concise Python.</p>
<p>We were told to write a genetic algorithm when on the 
<a href="http://www.sussex.ac.uk/easy/" target="_blank">Evolutionary and Adaptive Systems</a>
M.Sc. course at Sussex in our first term back in 2000. During this course and my following DPhil, I wrote lots of
different variations but none as short.</p>

<p>It has abstract genotypes which means the mutation operators etc. are completely changable.<p>
<div class="highlight"><pre><span class="c">#!/usr/bin/env python</span>

<span class="kn">import</span> <span class="nn">random</span>
<span class="kn">import</span> <span class="nn">bisect</span>

<span class="c"># These can be replaced to produce different populations to suit</span>

<span class="k">def</span> <span class="nf">initialise_genotypes</span><span class="p">():</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">return</span> <span class="p">(</span><span class="nb">int</span><span class="p">(</span><span class="mi">10</span> <span class="o">*</span> <span class="n">random</span><span class="o">.</span><span class="n">random</span><span class="p">())</span> <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">xrange</span><span class="p">(</span><span class="mi">10</span><span class="p">))</span>

<span class="k">def</span> <span class="nf">mutate</span><span class="p">(</span><span class="n">i</span><span class="p">):</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">return</span> <span class="n">i</span> <span class="o">+</span> <span class="n">random</span><span class="o">.</span><span class="n">randint</span><span class="p">(</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">trial</span><span class="p">(</span><span class="n">i</span><span class="p">):</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">return</span> <span class="o">-</span><span class="n">i</span>

<span class="c">##################################################</span>

<span class="k">def</span> <span class="nf">run_trials</span><span class="p">(</span><span class="n">population</span><span class="p">,</span> <span class="n">trial_fn</span><span class="p">):</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">return</span> <span class="p">[(</span><span class="n">trial_fn</span><span class="p">(</span><span class="n">x</span><span class="p">),</span> <span class="n">x</span><span class="p">)</span> <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">population</span><span class="p">]</span>

<span class="k">def</span> <span class="nf">create_next_population</span><span class="p">(</span><span class="n">population_with_fitness</span><span class="p">,</span> <span class="n">mutate_fn</span><span class="p">):</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">population_with_fitness</span><span class="o">.</span><span class="n">sort</span><span class="p">(</span><span class="n">key</span> <span class="o">=</span> <span class="k">lambda</span> <span class="n">k</span> <span class="p">:</span> <span class="p">(</span><span class="n">k</span><span class="p">[</span><span class="mi">0</span><span class="p">]))</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">sorted_population</span> <span class="o">=</span> <span class="p">[</span><span class="n">x</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">population_with_fitness</span><span class="p">]</span>

&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">rank_selection</span> <span class="o">=</span> <span class="p">[]</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">j</span> <span class="o">=</span> <span class="mi">0</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">max_relative_fitness</span> <span class="o">=</span> <span class="mi">0</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">xrange</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">sorted_population</span><span class="p">)):</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">j</span> <span class="o">+=</span> <span class="mi">1</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">max_relative_fitness</span> <span class="o">+=</span> <span class="n">j</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">rank_selection</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">max_relative_fitness</span><span class="p">)</span>

&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">new_population</span> <span class="o">=</span> <span class="p">[]</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">xrange</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">sorted_population</span><span class="p">)):</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">j</span> <span class="o">=</span> <span class="n">bisect</span><span class="o">.</span><span class="n">bisect</span><span class="p">(</span><span class="n">rank_selection</span><span class="p">,</span> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">random</span><span class="o">.</span><span class="n">random</span><span class="p">()</span> <span class="o">*</span> <span class="n">max_relative_fitness</span><span class="p">)</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">mutated_member</span> <span class="o">=</span> <span class="n">mutate_fn</span><span class="p">(</span><span class="n">sorted_population</span><span class="p">[</span><span class="n">j</span><span class="p">])</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">new_population</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">mutated_member</span><span class="p">)</span>

&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">return</span> <span class="n">new_population</span>

<span class="k">def</span> <span class="nf">start_evolving</span><span class="p">(</span><span class="n">initialise_genotypes_fn</span><span class="p">,</span> <span class="n">mutate_fn</span><span class="p">,</span> <span class="n">trial_fn</span><span class="p">):</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">population</span> <span class="o">=</span> <span class="n">initialise_genotypes_fn</span><span class="p">()</span>

&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">xrange</span><span class="p">(</span><span class="mi">1000</span><span class="p">):</span> <span class="c"># 1000 generations</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">population_with_fitness</span> <span class="o">=</span> <span class="n">run_trials</span><span class="p">(</span><span class="n">population</span><span class="p">,</span> <span class="n">trial_fn</span><span class="p">)</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">population</span> <span class="o">=</span> <span class="n">create_next_population</span><span class="p">(</span><span class="n">population_with_fitness</span><span class="p">,</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">mutate_fn</span><span class="p">)</span>

&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="n">population</span><span class="p">:</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="k">print</span> <span class="n">i</span>

<span class="k">if</span> <span class="n">__name__</span> <span class="o">==</span> <span class="s">&quot;__main__&quot;</span><span class="p">:</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="n">start_evolving</span><span class="p">(</span><span class="n">initialise_genotypes</span><span class="p">,</span> <span class="n">mutate</span><span class="p">,</span> <span class="n">trial</span><span class="p">)</span>
</pre></div>
