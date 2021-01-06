---


---

<blockquote>
<p>Max Contiguous Subarray Sum - Cubic Time To Kadane’s Algorithm (“Maximum Subarray” on LeetCode)<br>
<a href="https://www.youtube.com/watch?v=2MmGzdiKR9Y&amp;list=PLiQ766zSC5jN42O1DBalnkom5y2LXtnnK&amp;index=4">https://www.youtube.com/watch?v=2MmGzdiKR9Y&amp;list=PLiQ766zSC5jN42O1DBalnkom5y2LXtnnK&amp;index=4</a></p>
</blockquote>
<p>bud_bottlenecks+unnecessary work+duplicate work<br>
<a href="https://youtu.be/2MmGzdiKR9Y?list=PLiQ766zSC5jN42O1DBalnkom5y2LXtnnK&amp;t=337">5:37</a></p>
<p>method 1:<br>
n<sup>3</sup>   -&gt;     n<sup>2</sup>  (time : cubic to quadratic)<br>
from 3 loop to 2 loop<br>
<a href="https://youtu.be/2MmGzdiKR9Y?list=PLiQ766zSC5jN42O1DBalnkom5y2LXtnnK&amp;t=412">use the sum we added before</a>, avoid dupicated work</p>
<p>method 2:<br>
<a href="https://youtu.be/2MmGzdiKR9Y?list=PLiQ766zSC5jN42O1DBalnkom5y2LXtnnK&amp;t=538">dynamic programming_knowing the sub problems</a></p>
<p>from the last one to think(if my subarray were to end at the index 3, what is the best i can do in terms of subarray sum )<br>
max(chioce1, choice2)<br>
chioce1: self(start new this item)<br>
choice2: continue max subarray before us</p>

