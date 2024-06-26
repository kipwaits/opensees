# Manzari Dafalias Material

This command is used to construct a multi-dimensional Manzari-Dafalias (2004)
material.

```tcl
nDmaterial ManzariDafalias $matTag $G0 $nu $e_init $Mc $c
        $lambda_c $e0 $ksi $P_atm $m $h0 $ch $nb $A0 $nd $z_max $cz
        $Den
```

<table>
<tbody>
<tr class="odd">
<td><code class="parameter-table-variable">matTag</code></td>
<td><p>integer tag identifying material</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">G0</code></td>
<td><p>shear modulus constant</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">nu</code></td>
<td><p>poisson ratio</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">e_init</code></td>
<td><p>initial void ratio</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">Mc</code></td>
<td><p>critical state stress ratio</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">c</code></td>
<td><p>ratio of critical state stress ratio in extension and
compression</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">lambda_c</code></td>
<td><p>critical state line constant</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">e0</code></td>
<td><p>critical void ratio at $p = 0$</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">ksi</code></td>
<td><p>critical state line constant</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">P_atm</code></td>
<td><p>atmospheric pressure</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">m</code></td>
<td><p>yield surface constant (radius of yield surface in stress ratio
space)</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">h0</code></td>
<td><p>constant parameter</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">ch</code></td>
<td><p>constant parameter</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">nb</code></td>
<td><p>bounding surface parameter, $nb &amp;ge; 0</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">A0</code></td>
<td><p>dilatancy parameter</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">nd</code></td>
<td><p>dilatancy surface parameter $nd &amp;ge; 0</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">z_max</code></td>
<td><p>fabric-dilatancy tensor parameter</p></td>
</tr>
<tr class="even">
<td><code class="parameter-table-variable">cz</code></td>
<td><p>fabric-dilatancy tensor parameter</p></td>
</tr>
<tr class="odd">
<td><code class="parameter-table-variable">Den</code></td>
<td><p>mass density of the material</p></td>
</tr>
</tbody>
</table>

> The material formulations for the Manzari-Dafalias object are
> `ThreeDimensional` and `PlaneStrain`


<hr />

<h2 id="notes">Notes</h2>

- Valid Element Recorder queries are

  - <strong>stress</strong>, <strong>strain</strong></li>
  - <strong>alpha</strong> (or <strong>backstressratio</strong>) for $\mathbf{\alpha}$</li>
  - <strong>fabric</strong> for $\mathbf{z}$</li>
  - <strong>alpha_in</strong> (or <strong>alphain</strong>) for $\mathbf{\alpha_{in}}$</li>
  For example:
  ```tcl
  recorder Element -eleRange 1 $numElem -time -file stress.out stress
  ```

- Elastic or Elastoplastic response could be enforced by
  <dl>
  <dt>Elastic</dt><dd>

  ```tcl
  updateMaterialStage -material $matTag -stage 0
  ```
  </dd><dt>Elastoplastic</dt><dd>

  ```tcl
  updateMaterialStage -material $matTag -stage 1
  ```
  </dd>
  </dl>

<h2 id="theory">Theory</h2>

$$p = \frac{1}{3} \mathrm{tr}(\mathbf{\sigma})$$</p>

$$\mathbf{s} = \mathrm{dev} (\mathbf{\sigma}) =
  \mathbf{\sigma} - \frac{1}{3} p \mathbf{1}$$

<h3 id="elasticity">Elasticity</h3>

Elastic moduli are considered to be functions of $p$ and current void
ratio:

$$ G = G_0
p_{atm}\frac{\left(2.97-e\right)^2}{1+e}\left(\frac{p}{p_{atm}}\right)^{1/2}$$


$$K = \frac{2(1+\nu)}{3(1-2\nu)} G$$


The elastic stress-strain relationship is:

$$ d\mathbf{e}^\mathrm{e} = \frac{d\mathbf{s}}{2G}$$

$$ d\varepsilon^\mathrm{e}_v = \frac{dp}{K}$$

<h3 id="critical_state_line">Critical State Line</h3>

A power relationship is assumed for the critical state line:

$$e_c = e_0 - \lambda_c\left(\frac{p_c}{p_{atm}}\right)^\xi$$

where $e_0$ is the void ratio at $p_c = 0$ and $\lambda_c$ and $\xi$ constants.

<h3 id="yield_surface">Yield Surface</h3>
Yield surface is a stress-ratio dependent surface in this model and
is defined as

$$ \left\| \mathbf{s} - p \mathbf{\alpha} \right\| -
\sqrt\frac{2}{3}pm = 0 $$

with $\mathbf{\alpha}$ being the deviatoric back stress-ratio.

<h3 id="plastic_strain_increment">Plastic Strain Increment</h3>

The increment of the plastic strain tensor is given by

$$ d\mathbf{\varepsilon}^p = \langle L \rangle \mathbf{R}$$

where

$$ \mathbf{R} = \mathbf{R'} + \frac{1}{3} D \mathbf{1}$$

therefore

$$ d\mathbf{e}^p = \langle L \rangle \mathbf{R'}$$

and $d\varepsilon^p_v = \langle L \rangle D$. The hardening modulus in 
this model is defined as

$$ K_p = \frac{2}{3} p h (\mathbf{\alpha}^b_{\theta} -
\mathbf{\alpha}): \mathbf{n}$$

where $\mathbf{n}$ is the deviatoric part of the gradient to yield surface.

$$ \mathbf{\alpha}^b_{\theta} = \sqrt{\frac{2}{3}}
\left[g(\theta,c) M_c exp(-n^b\Psi) - m\right] \mathbf{n}$$

, $\Psi$ being the state parameter. The hardening parameter, $h$, is defined as

$$ h =
\frac{b_0}{(\mathbf{\alpha}-\mathbf{\alpha_{in}}):\mathbf{n}}
$$

, $\mathbf{\alpha_{in}}\!$ is the value of $\mathbf{\alpha}\!$ at initiation of loading cycle.


$$b_0 = G_0 h_0 (1-c_h e)
\left(\frac{p}{p_{atm}}\right)^{-1/2}$$


<p>Also the dilation parameters are defined as</p>

$$ D = A_d (\mathbf{\alpha}^d_{\theta}-\mathbf{\alpha}) :
\mathbf{n}
$$

$$ \mathbf{\alpha}^d_{\theta} = \sqrt{\frac{2}{3}}
\left[g(\theta,c) M_c exp(n^d\Psi) - m\right] \mathbf{n}
$$

$$ A_d = A_0 (1+\langle \mathbf{z : n}\rangle)$$

where $\mathbf{z}$ is the fabric tensor.

The evolution of fabric and the back stress-ratio tensors are defined as

$$d\mathbf{z} = - c_z \langle -d\varepsilon^p_v \rangle
(z_{max}\mathbf{n}+\mathbf{z}) $$


$$ d\mathbf{\alpha} = \langle L \rangle (2/3) h
(\mathbf{\alpha}^b_{\theta} - \mathbf{\alpha})
$$

## Examples
This example, provides an undrained confined triaxial compression
test using one 8-node SSPBrickUP element and ManzariDafalias material
model.

<details><summary>Tcl script</summary>

```tcl
# ===================================================================== #
# 3D Undrained Conventional Triaxial Compression Test Using One Element #
# University of Washington, Department of Civil and Environmental Eng   #
# Geotechnical Eng Group, A. Ghofrani, P. Arduino - Dec 2013            #
# Basic units are m, Ton(metric), s										                  #
# ===================================================================== #

wipe

# ------------------------ #
# Test Specific parameters #
# ------------------------ #
# Confinement Stress
set pConf -300.0
# Deviatoric strain
set devDisp -0.3
# Permeablity
set perm 1.0e-10
# Initial void ratio
set vR 0.8

# Rayleigh damping parameter
set damp   0.1
set omega1 0.0157
set omega2 64.123
set a1 [expr 2.0*$damp/($omega1+$omega2)]
set a0 [expr $a1*$omega1*$omega2]

# =====================================================================
# ---------------------------Create Model------------------------------
# =====================================================================

# Create a 3D model with 4 Degrees of Freedom
model BasicBuilder -ndm 3 -ndf 4

# Create nodes
node 1	1.0	0.0	0.0
node 2	1.0	1.0	0.0
node 3 	0.0	1.0	0.0	
node 4	0.0	0.0	0.0
node 5	1.0	0.0	1.0
node 6 	1.0	1.0	1.0
node 7 	0.0	1.0	1.0
node 8 	0.0	0.0	1.0

# Create Fixities
fix 1 	0 1 1 1
fix 2 	0 0 1 1
fix 3	1 0 1 1
fix 4 	1 1 1 1
fix 5	0 1 0 1
fix 6 	0 0 0 1
fix 7	1 0 0 1
fix 8 	1 1 0 1


# Create material
#          ManzariDafalias  tag    G0   nu   e_init   Mc    c    lambda_c    e0    ksi   P_atm   m    h0   ch    nb  A0      nd   z_max   cz    Den  
nDMaterial ManzariDafalias   1    125  0.05   $vR    1.25  0.712   0.019    0.934  0.7    100   0.01 7.05 0.968 1.1 0.704    3.5    4     600  1.42  

# Create element
#       SSPbrickUP  tag    i j k l m n p q  matTag  fBulk  fDen    k1    k2   k3   void   alpha    <b1 b2 b3>
element SSPbrickUP   1     1 2 3 4 5 6 7 8    1     2.2e6   1.0  $perm $perm $perm  $vR   1.5e-9 

# Create recorders
recorder Node    -file disp.out   -time -nodeRange 1 8 -dof 1 2 3 disp
recorder Node    -file press.out  -time -nodeRange 1 8 -dof 4     vel
recorder Element -file stress.out -time stress
recorder Element -file strain.out -time strain
recorder Element -file alpha.out  -time alpha
recorder Element -file fabric.out -time fabric


# Create analysis
constraints Penalty 1.0e18 1.0e18
test        NormDispIncr 1.0e-5 20 1
algorithm   Newton
numberer    RCM
system      BandGeneral
integrator  Newmark 0.5 0.25
rayleigh    $a0 0. $a1 0.0
analysis    Transient

# Apply confinement pressure
set pNode [expr $pConf / 4.0]
pattern Plain 1 {Series -time {0 10000 1e10} -values {0 1 1} -factor 1} {
    load 1  $pNode  0.0    0.0    0.0
    load 2  $pNode  $pNode 0.0    0.0
    load 3  0.0     $pNode 0.0    0.0
    load 4  0.0     0.0    0.0    0.0
    load 5  $pNode  0.0    $pNode 0.0
    load 6  $pNode  $pNode $pNode 0.0
    load 7  0.0     $pNode $pNode 0.0
    load 8  0.0     0.0    $pNode 0.0
}
analyze 100 100

# Let the model rest and waves damp out
analyze 50  100

# Close drainage valves
for {set x 1} {$x<9} {incr x} {
   remove sp $x 4
}
analyze 50 100

# Read vertical displacement of top plane
set vertDisp [nodeDisp 5 3]
# Apply deviatoric strain
set lValues [list 1 [expr 1+$devDisp/$vertDisp] [expr 1+$devDisp/$vertDisp]]
set ts "{Series -time {20000 1020000 10020000} -values {$lValues} -factor 1}"

# loading object deviator stress
eval "pattern Plain 2 $ts { 
	sp 5  3	$vertDisp
	sp 6  3	$vertDisp
	sp 7  3 $vertDisp
	sp 8  3 $vertDisp
}"

# Set number and length of (pseudo)time steps
set dT      100
set numStep 10000

# Analyze and use substepping if needed
set remStep $numStep
set success 0
proc subStepAnalyze {dT subStep} {
	if {$subStep > 10} {
		return -10
	}
	for {set i 1} {$i < 3} {incr i} {
		puts "Try dT = $dT"
		set success [analyze 1 $dT]
		if {$success != 0} {
			set success [subStepAnalyze [expr $dT/2.0] [expr $subStep+1]]
			if {$success == -10} {
				puts "Did not converge."
				return success
			}
		} else {
			if {$i==1} {
				puts "Substep $subStep : Left side converged with dT = $dT"
			} else {
				puts "Substep $subStep : Right side converged with dT = $dT"
			}
		}
	}
	return success
}

puts "Start analysis"
set startT [clock seconds]

while {$success != -10} {
	set subStep 0
	set success [analyze $remStep  $dT]
	if {$success == 0} {
		puts "Analysis Finished"
		break
	} else {
		set curTime  [getTime]
		puts "Analysis failed at $curTime . Try substepping."
		set success  [subStepAnalyze [expr $dT/2.0] [incr subStep]]
        set curStep  [expr int(($curTime-20000)/$dT + 1)]
        set remStep  [expr int($numStep-$curStep)]
		puts "Current step: $curStep , Remaining steps: $remStep"
	}
}
set endT [clock seconds]
puts "loading analysis execution time: [expr $endT-$startT] seconds."

wipe
```
</details>

<h2 id="references">References</h2>
<p>Dafalias YF, Manzari MT. "Simple plasticity sand model accounting for
fabric change effects". Journal of Engineering Mechanics 2004</p>

<hr />
<p>Code Developed by: <span style="color:blue">Alborz Ghofrani, <a
href="http://www.ce.washington.edu/people/faculty/bios/arduino_p.html">Pedro
Arduino, U Washington</a></span></p>

