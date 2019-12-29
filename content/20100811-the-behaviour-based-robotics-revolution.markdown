category: Artificial Intelligence
date: 2010/08/11 10:24:00
title: The behaviour-based robotics revolution
author: Ian Macinnes

<p>I mentioned Rodney Brooks in an earlier post so I though I'd briefly write a profile of Brooks and the behaviour-based robotics revolution he
helped start at the beginning of the 1990's. Rodney Brooks is an Austrialian who, after a series of insightful and highly influential
papers, eventually ascended to head the <a title="Artificial Intelligence Laboratory at MIT home page" href="http://www.csail.mit.edu" target="_blank">Artificial Intelligence Laboratory at MIT</a> from 2003 to 2007. He brought various issues into the mainstream of the field, but I&#8217;ll discuss functional and behavioural decomposition here, rather than his fascinating arguments for <a title="The distinction between being situated and being embodied" href="the-difference-between-being-situated-and-being-embodied.html" target="_self">embodiment and situatedness</a> (incidentally, the importance of which he had in fact <em>re</em>discovered, as they had been discussed, among others, by members of the cybernetics movement way back in the 1950&#8242;s).</p>

<p>Brooks tried to construct robots that could successfully operate in the noisy, complicated everyday world but he found he couldn't.
Most robots then, as now, are designed to operate in simplified environments so that the robot can respond to clear and unambiguous environmental
cues. These robots would expect objects to be placed in the same locations time after time, or read large clear black text on a white background.
These simple, structured environments were no match for the real world and the robots designed to operate in them couldn't operate when placed
in a house, or a street with a rough pavement and varying street lighting. This problem caused Brooks to draw a conclusion that proved to be
controversial within the field of artificial intelligence; that the mainstream symbolic approach was wrong.</p>

<p>From the 1950's onwards (really ever since the
<a title="Dartmouth Conference of 1956 at Wikipedia" href="http://en.wikipedia.org/wiki/Dartmouth_conference" target="_blank">Dartmouth Conference of 1956</a>
), the mainstream approach to artificial intelligence was to represent the world as <em>propositions</em> that could be manipulated. Propositions
are data items or symbols that  represent facts, objects in the environment, or abstract attributes. The propositions would be logically manipulated
using one of a variety of algorithms to solve a specific problem. Eventually, in order to apply any of the common methodologies and tools used by
artificial intelligence researchers such as Lisp, Prolog, or expert systems, the given problem had to be broken down as propositions, then
manipulated using a system of logical rules, then the answer would be presented as another list of propositions. This methodology is called
<em>functional decomposition</em>.</p>

<p>By the beginning of the eighties, the symbolic approach started to look shaky. Hubert Dreyfus criticized the approach on philosophical
grounds, whereas John Searle's Chinese Room argument and Joseph Weizenbaum's computer program Eliza highlighted grave drawbacks
in the symbolic approach. Their arguments were broadly pointedly ignored within the artificial intelligence field.</p>

<p>At the end of the eighties, Brooks criticised functional decomposition as a means to design the control system of a robot. If you were to
functionally decompose a desired behaviour, you would divide the behaviour into individual functional tasks, such as pattern recognition,
language processing, and abstract reasoning. You would then treat each task as an individual problem to be solved. However, it is not at all
clear that tasks or behaviours are separated in this way in organisms. Evolution tends to produce highly integrated solutions that are not
easy to to decompose in the way a software engineering task may be.</p>

<p>So instead of using functional decomposition, Brooks urged the use of <em>behavioural decomposition</em>. This approach breaks up behaviours
into a series of local actions. He suggested that modules that implement specific actions be triggered by specific cues. If a behaviour comes
from separate but cooperating actions responding to  local cues, it means that a robot doesn't need a central planning unit  to decide
what to do next. The overall behaviour of the robot emerges  from the local decisions made by various parts distributed about the  body of the
robot. This approach is bottom-up rather that top-down, the  very opposite of the functional decomposition method.</p>

<p>Brooks also suggested that every action should first be tested on the real robot placed in its operating environment to ensure that all the
actions work. Only once all the actions have been "debugged" should they be coordinated to compose more complex behaviours. The
robot should be repeatedly debugged until it performed the desired behaviours successfully. A real robot should be used rather than a simulation
since the real world (including the physical dynamics of the robot) is very hard to model, and really "the world acts as its own best
model". This means that instead of the robot responding to events that occur or are predicted to occur in a <em>model</em> of the
environment, it would respond directly to an event in the <em>real</em> environment.</p>

<p>How has his ideas influenced evolved virtual creatures or evolutionary robotics? Well, an awful lot. Although he himself doesn't think
that evolved neural networks are the best technique to use, actually a robot controlled by a neural controller matches very closely his ideas
of individual actions triggered by specific cues. Such a robot is definitely behaviour-based. But most evolved robots are evolved in
simulation, not, as Brooks advocated, tested in the real world. The problem he has identified remains the biggest problem in evolved
morphology robots - how to transfer robots across the reality gap from simulation to the real physical world.</p>
