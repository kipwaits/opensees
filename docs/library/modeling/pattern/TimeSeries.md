# Time Series Command

This command is used to construct a TimeSeries
object which represents the relationship between the time in the domain,
 $t $ , and the load factor applied to the loads, $t $ , a
$\lambda$, in the load pattern with which the
`TimeSeries` object is associated, i.e. $\lambda = F(t)$

```tcl
timeSeries seriesType? arg1? ...
```
<hr />
<p>The type of time series created and the additional arguments required
depends on the <strong>seriesType?</strong> provided in the command.</p>
<p>The following contain information about seriesType? and the args
required for each of the available time series types:</p>
<p>&lt;/noinclude&gt;</p>
<ul>
<li><a href="Constant_TimeSeries" title="wikilink">Constant
TimeSeries</a></li>
<li><a href="Linear_TimeSeries" title="wikilink">Linear
TimeSeries</a></li>
<li><a href="Trigonometric_TimeSeries" title="wikilink">Trigonometric
TimeSeries</a></li>
<li><a href="Triangular_TimeSeries" title="wikilink">Triangular
TimeSeries</a></li>
<li><a href="Rectangular_TimeSeries" title="wikilink">Rectangular
TimeSeries</a></li>
<li><a href="Pulse_TimeSeries" title="wikilink">Pulse
TimeSeries</a></li>
<li><a href="Path_TimeSeries" title="wikilink">Path TimeSeries</a></li>
<li><a href="PeerMotion" title="wikilink">PeerMotion</a></li>
<li><a href="PeerNGAMotion" title="wikilink">PeerNGAMotion</a></li>
</ul>
