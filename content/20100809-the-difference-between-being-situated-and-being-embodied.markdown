category: Artificial Intelligence
date: 2010/08/09 11:56:00
title: The difference between being situated and being embodied
author: Ian Macinnes

<p>After discussing robots with a fellow artificial lifer, I can see confusion between two attributes of the physical existence of a robot or
organism. I want to clarify my definitions, at least to see if my use is compatible with others. These terms are <em>situatedness</em> and
<em>embodiment</em>. A physical agent acting in the world is:</p>

<ul>
<li><strong>situated</strong> such that the relationships between the  robot and the elements in its environment have a spatial and temporal
quality that varies as the robot moves; and</li>
<li><strong>embodied</strong> such that the body of a robot has its own physical dynamics.</li>
</ul>

<p>Many of the problems tackled  by behaviour-based robotics use robots that react with objects that are spatially  located in the environment
(as opposed to symbolic objects or objects detected using magic sensors), and hence the robots are <em>situated</em>. Including a
spatial element in the  relationship between the robot and   the object provides the robot with  feedback information that may be   complicated
to derive with a  non-situated controller. For example, a   phototactic robot is provided  with immediate feedback regarding its   orientation
from the varying  strength of the signals that are produced by its   sensors as the robot rotates.</p>

<p>But the controller of a real robot interacts with the environment through its body, and this body can have complex physical dynamics i.e.
it is also <em>embodied</em>. If a robot is embodied, then it is very likely situated. Most simulated robots or animats are situated to some
degree, but they are not often embodied, especially with the very simple robots we use in artificial life experiments. This is because embodiment
refers to the physical existence of a robot or  organism, and it is more difficult to model the embodiment of a simulated robot than its
situatedness. If fact, sometimes the robot effectively has almost zero embodiment, but is only represented as little more than a position,
a radius, and an acceleration of movement.</p>

<p>Why is the distinction important?
<a title="Rodney Brooks's home page" href="http://people.csail.mit.edu/brooks/" target="_blank">Rodney Brooks</a>
believes that acknowledging embodiment is important because it encourages the robot's builder to
<a title="PDF of Intelligence Without Representation by Rodney Brooks" href="http://people.csail.mit.edu/brooks/papers/representation.pdf" target="_blank">
continually test and correct</a> the    robot's behaviour physically in the real world, a process that he compares with software debugging.
This debugging process must take place    on a real robot rather than a simulated or modelled robot because of    the difficulty of transferring
behaviours from simulation to reality.    Brooks also believes that debugging a real robot can reveal that some    problems which are difficult to
solve when considered in abstraction  do   not exist when a robot is physically constructed.</p>

<p>For evolved robots that are to "cross the reality gap" to the real  world, it is essential we develop methodologies to model
a robot's  embodiment in such a way to make its behaviour transferable. The principle methodology we use is called minimal simulations,
which was first developed by Nick Jakobi in the mid nineties. This shall be the subject of a future post.</p>
