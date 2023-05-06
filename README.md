Download Link: https://assignmentchef.com/product/solved-ee209as-computational-robotics
<br>



<strong>Objectives</strong>

The goal of this lab is to explore Markov Decision Processes (MDPs) to control a simple discretized robot. You will develop and implement a model of the robot behavior and use it to accomplish a prescribed task.

<h1>Preliminaries</h1>

0(a). What is the link to your (fully commented) github repo for this pset?

0(b). Who did you collaborate with?

0(c). What was the aggregate % contributions of each team member?

<h1>1        MDP system</h1>

Consider a simple robot in a 2D grid world of length <em>L </em>and width <em>W</em>. That is, the robot can be located at any lattice point (<em>x,y</em>) : 0 ≤ <em>x &lt; L,</em>0 ≤ <em>y &lt; W</em>;<em>x,y </em>∈N. At each point, the robot can face any of the twelve headings identified by the hours on a clock <em>h </em>∈{0<em>…</em>11}, where <em>h </em>= 0 represents 12 o’clock is pointing up (i.e. positive <em>y</em>) and <em>h </em>= 3 is pointing to the right (i.e. positive <em>x</em>).

In each time step, the robot can chose from several actions. Each singular action will consist of a movement followed by a rotation.

<ul>

 <li>The robot can choose to take no motion at all, staying still and neither moving nor rotating.</li>

 <li>Otherwise the robot can choose to either move “forwards” or “backwards”.

  <ul>

   <li>This may cause a pre-rotation error, see below.</li>

   <li>This will cause the robot move one unit in the direction it is facing, rounded to the nearest cardinal direction.</li>

  </ul></li>

</ul>

That is, if the robot is pointing towards either 2, 3, or 4 and opts to move “forwards”, the robot will move one unit in the +<em>x </em>direction. Similarly, if the robot is pointing towards either 11, 0, or 1 and opts to move “backwards”, it will move one unit in the −<em>y </em>direction.

<ul>

 <li>After the movement, the robot can choose to turn left, not turn, or turn right. A left (counter-clockwise) turn will decrease the heading by 1 (mod 12); right (clockwise) will increase the heading by 1 (mod 12). The robot can also keep the heading constant.</li>

 <li>Attempting to move off of the grid will result in no linear movement, but the rotation portion of the action will still happen.</li>

</ul>

Note that aside from the at edges of the grids, the robot can only rotate if it also moves forwards or backwards; it can move without rotating though.

The robot has an error probability <em>p<sub>e</sub></em>: if the robot choses to move, it will <em>first </em>rotate by +1 or -1 (mod 12) with probability <em>p<sub>e </sub></em>each, before it moves. It will not pre-rotate with probability 1 − 2<em>p<sub>e</sub></em>. Choosing to stay still (take no motion) will not incur an error rotation.

Create code to simulate this system. You may need to create objects to represent states <em>s </em>∈ <em>S </em>and/or actions <em>a </em>∈ <em>A</em>. Note that the state <em>s </em>will later be used as an index to a matrix/array when computing policies and values.

1(a). Create (in code) your state space <em>S </em>= {<em>s</em>}. What is the size of the state space <em>N<sub>S</sub></em>?

1(b). Create (in code) your action space <em>A </em>= {<em>a</em>}. What is the size of the action space <em>N<sub>A</sub></em>?

1(c). Write a function that returns the probability <em>p<sub>sa</sub></em>(<em>s</em><sup>0</sup>) given inputs <em>p<sub>e</sub>,s,a,s</em><sup>0</sup>.

1(d). Write a function that uses the above to return a next state <em>s</em><sup>0 </sup>given error probability <em>p<sub>e</sub></em>, initial state <em>s</em>, and action <em>a</em>. Make sure the returned value <em>s</em><sup>0 </sup>follows the probability distribution specified by <em>p<sub>sa</sub></em>.

<h1>2        Planning problem</h1>

Consider the grid world shown below, with <em>L </em>= <em>W </em>= 8:

x

The rewards for each state are independent of heading angle (or action taken). The border states {<em>x </em>= 0<em>,x </em>= <em>L,y </em>= 0<em>,y </em>= <em>W</em>} (red, marked X) have reward -100. The lane markers (yellow, marked —) have reward -10. The goal square (green, marked <em>?</em>) has reward +1. Every other state has reward 0.

2(a). Write a function that returns the reward <em>R</em>(<em>s</em>) given input <em>s</em>.

<h1>3        Policy iteration</h1>

Create an initial policy <em>π</em><sub>0 </sub>of taking an action that approximately gets you closest to the goal square. For example, if the goal is in front of you, move forward; if it is behind you, move backwards; then turn the amount that aligns your next direction of travel closer towards the goal (if necessary). If the goal is directly to your left or right, move forward then turn appropriately.

3(a). Create and populate a matrix/array that stores the action <em>a </em>= <em>π</em><sub>0</sub>(<em>s</em>) prescribed by the initial policy <em>π</em><sub>0 </sub>when indexed by state <em>s</em>.

3(b). Write a function to generate and plot a trajectory of a robot given policy matrix/array <em>π</em>, initial state <em>s</em><sub>0</sub>, and error probability <em>p<sub>e</sub></em>.

3(c). Generate and plot a trajectory of a robot using policy <em>π</em><sub>0 </sub>starting in state <em>x </em>= 1<em>,y </em>= 6<em>,h </em>= 6 (i.e. top left corner, pointing down). Assume <em>p<sub>e </sub></em>= 0.

3(d). Write a function to compute the policy evaluation of a policy <em>π</em>. That is, this function should return a matrix/array of values <em>v </em>= <em>V <sup>π</sup></em>(<em>s</em>) when indexed by state <em>s</em>. The inputs will be a matrix/array storing <em>π </em>as above, along with discount factor <em>λ</em>.

3(e). What is the value of the trajectory in 3(c)? Use <em>λ </em>= 0<em>.</em>9.

3(f). Write a function that returns a matrix/array <em>π </em>giving the optimal policy given a one-step lookahead on value <em>V </em>.

3(g). Combine your functions above in a new function that computes policy iteration on the system, returning optimal policy <em>π</em><sup>∗ </sup>with optimal value <em>V </em><sup>∗</sup>.

3(h). Run this function to recompute and plot the trajectory and value of the robot described in 3(c) under the optimal policy <em>π</em><sup>∗</sup>.

3(i). How much compute time did it take to generate your results from 3(h)? You may want to use your programming language’s built-in runtime analysis tool.

<h1>4        Value iteration</h1>

4(a). Using an initial condition <em>V </em>(<em>s</em>) = 0 ∀<em>s </em>∈ <em>S</em>, write a function (and any necessary subfunctions) to compute value iteration, again returning optimal policy <em>π</em><sup>∗ </sup>with optimal value <em>V </em><sup>∗</sup>.

4(b). Run this function to recompute and plot the trajectory and value of the robot described in 3(c) under the optimal policy <em>π</em><sup>∗</sup>. Compare these results with those you got from policy iteration in 3(h).

4(c). How much compute time did it take to generate your results from 4(b)? Use the same timing method as in

3(i).

<h1>5        Additional scenarios</h1>

5(a). Recompute the robot trajectory and value given initial conditions from 3(c) but with <em>p<sub>e </sub></em>= 25%.

5(b). Assume the reward of +1 only applies when the robot is pointing straight down, e.g. <em>h </em>= 6 in the goal square; the reward is 0 otherwise. Recompute trajectories and values given initial conditions from 3(c) with <em>p<sub>e </sub></em>∈{0<em>,</em>25%}.

5(c). Qualitatively describe some conclusions from these scenarios.

<h1>Git Resources</h1>

<ul>

 <li>Getting started with GitHub: <a href="https://guides.github.com/activities/hello-world/">https://guides.github.com/activities/hello-world/</a></li>

 <li>Detailed documentation on how to use git: <a href="https://git-scm.com/book/en/v2">https://git-scm.com/book/en/v2</a></li>

 <li>Try git in the browser: <a href="https://try.github.io/levels/1/challenges/1">https://try.github.io/levels/1/challenges/1</a></li>

</ul>