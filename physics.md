---
layout: page
title: Physics
permalink: /physics
---

<script
 src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"
  async="" type="text/javascript">// <![CDATA[
// ]]></script>

Keep in mind that this is work in progress and might contain unintended mistakes.

<br/><a href="#1._a_unit_of_measure">1. A unit of measure</a>
<br/><a href="#2._international_system_of_units">2. International system of
units</a>
<br/><a href="#3._elementary_particles">3. Elementary particles</a>
<br/><a href="#4._other">4. Other</a>

<hr/><a id="1._a_unit_of_measure" />
<h1>1. A unit of measure</h1><hr/>

Imagine that human civilization goes extinct. Billions of years later, an alien
civilization arrives to earth, looking for intelligent life signs. Maybe they
will find some text, containing a measurement of 100 meters. How will they know
how much space 100 meters is?

Let me tell you that the height of my body is 3.54 horse heads. Am I tall? You
know, the <a href="https://en.wikipedia.org/wiki/Horse_length" target="_blank">
horse</a> -
<a href="https://www.sportingindex.com/training-centre/winning-distances"
target="_blank">head</a>. It might make no sense if you are not familiar with
that measuring unit.

Anyone can define a measuring unit for any physical quantity. So for example
I could tell you that the height of my body is 27.31 PPD. Oh wait, you don't
know how much 1 PPD is - well, "Paul's Pupillary Distance" (or PPD for short)
is equal to 0.06224 meters.

So, you see, the numbers change a lot, if you change the definition of the unit.
So, let's define the units that we're going to use.






<hr/><a id="2._international_system_of_units" />
<h1>2. International System of Units</h1><hr/>
There are 7 base quantities and measurement units which form a basic set of
units from which all other units can be derived. (but no quantity in the set
should be expressed in terms of the others?)

<table border="1px">
  <thead>
    <td><b>Base quantity</b></td>
    <td><b>Symbol for quantity</b></td>
    <td><b>Symbol for dimension</b></td>
    <td><b>SI base unit</b></td>
    <td><b>SI unit symbol</b></td>
    <td><b>Read more</b></td>
  </thead>
  <tr>
    <td><a href="https://en.wikipedia.org/wiki/Length" target="_blank">
      length</a></td>
    <td>$$ l $$</td>
    <td>$$ L $$</td>
    <td><a href="https://en.wikipedia.org/wiki/Metre" target="_blank">
      metre</a></td>
    <td>$$ m $$</td>
    <td><a href="#2.1._metre">Read more</a></td>
  </tr>
  <tr>
    <td><a href="https://en.wikipedia.org/wiki/Mass" target="_blank">
      mass</a></td>
    <td>$$ m $$</td>
    <td>$$ M $$</td>
    <td><a href="https://en.wikipedia.org/wiki/Kilogram" target="_blank">
      kilogram</a></td>
    <td>$$ kg $$</td>
    <td><a href="#2.2._kilogram">Read more</a></td>
  </tr>
  <tr>
    <td><a href="https://en.wikipedia.org/wiki/Time" target="_blank">
      time</a></td>
    <td>$$ t $$</td>
    <td>$$ T $$</td>
    <td><a href="https://en.wikipedia.org/wiki/Second" target="_blank">
      second</a></td>
    <td>$$ s $$</td>
    <td><a href="#2.3._second">Read more</a></td>
  </tr>
  <tr>
    <td>electric current</td>
    <td>$$ I $$</td>
    <td>$$ I $$</td>
    <td>ampere</td>
    <td>$$ A $$</td>
    <td><a href="#2.4._ampere">Read more</a></td>
  </tr>
  <tr>
    <td>thermodynamic temperature</td>
    <td>$$ T $$</td>
    <td>$$ \Theta $$</td>
    <td>kelvin</td>
    <td>$$ K $$</td>
    <td><a href="#2.5._length">Read more</a></td>
  </tr>
  <tr>
    <td>amount of substance</td>
    <td>$$ n $$</td>
    <td>$$ N $$</td>
    <td>mole</td>
    <td>$$ mol $$</td>
    <td><a href="#2.6._length">Read more</a></td>
  </tr>
  <tr>
    <td>luminous intensity</td>
    <td>$$ I_\mathrm{v} $$</td>
    <td>$$ J $$</td>
    <td>candela</td>
    <td>$$ cd $$</td>
    <td><a href="#2.7._length">Read more</a></td>
  </tr>
</table>

Mathematically, the dimension of the quantity `Q` is given by:
$$ dim \ Q = L^a \cdot M^b \cdot T^c \cdot I^d \cdot {\Theta}^e \cdot N^f
  \cdot J^g $$
where `a`, `b`, `c`, `d`, `e`, `f`, `g` are the dimensional exponents. In other
words, this is uniquely identified by a vector $$ (a, b, c, d, e, f, g) $$

<br/>Derived units:
<table border="1px">
  <tr>
    <td colspan="2"><center><b>Quantity</b></center></td>
    <td colspan="2"><center><b>Unit</b></center></td>
    <td rowspan="2"><center><b>Dimension<br/>Symbol</b></center></td>
    <td rowspan="2"><center><b>Details</b></center></td>
    <td rowspan="2"><center><b>Vector</b></center></td>
  </tr>
  <tr>
    <td><b>Name</b></td>
    <td><b>Symbol</b></td>
    <td><b>Name</b></td>
    <td><b>Symbol</b></td>
  </tr>
  <tr>
    <td><a href="https://en.wikipedia.org/wiki/Velocity" target="_blank">
      Velocity</a></td>
    <td>$$ v $$</td>
    <td><a href="https://en.wikipedia.org/wiki/Metre_per_second"
      target="_blank">metre / second</a></td>
    <td>$$ \frac{m}{s} $$</td>
    <td>$$ L \cdot T^{-1} $$</td>
    <td><a href="#2.8._velocity">Read more</a></td>
    <td>$$ (1, 0, -1, 0, 0, 0, 0) $$</td>
  </tr>
</table>
See <a href="https://en.wikipedia.org/wiki/SI_derived_unit" target="_blank">this
</a>.

<hr/><a id="2.1._metre" />
<h2>2.1. Metre</h2><hr/>
Length is a measure of
<a href="https://en.wikipedia.org/wiki/Distance" target="_blank">distance</a>.
The `1 meter` length is used as an unit, to measure distance.

<br/>The historical origin (justification) for the value quantity of a meter is:

"<i>1 / 10 000 000 of the distance from the Earth's equator to the North Pole
measured on the median arc through Paris.</i>"

<br/>The post-2019 formal definition of a meter is:

"<i>The metre is currently defined as the length of the path travelled by light
in a vacuum in $$ \frac{1}{299\ 792\ 458} $$
 of a second.</i>"


<hr/><a id="2.2._kilogram" />
<h2>2.2. Kilogram</h2><hr/>
The `1 kilogram` unit is used to measure mass.

<br/>The historical origin (justification) for the value quantity of a kilogram
is:

"<i>The mass of one litre(one thousandth of a cubic metre = $$ \frac{1}{1000}
\cdot m^3 $$) of water at the temperature of melting ice.</i>"

<br/>The post-2019 formal definition of a kilogram is:

"<i>It is defined by taking the fixed numerical value of the <a
 href="https://en.wikipedia.org/wiki/Planck_constant" target="_blank">Planck
constant</a> $$ h $$ to be $$ 6.62607015 \cdot 10^{−34} $$ when expressed in
the unit $$ J \cdot s $$, which is equal to $$ kg \cdot m^2 \cdot s^{−1} $$,
where the metre and the second are defined in terms of
<a href="https://en.wikipedia.org/wiki/Speed_of_light" target="_blank">c</a> and
$$ \Delta ν_{Cs} $$</i>"

<br/>The average mass of a new-born human baby is approximately
$$ 3.5 \cdot kg $$ .

<br/>**Note** that the **mass** of an object is a measure of the object's
inertial property, or the amount of matter it contains. The **weight** of an
object is a measure of the force exerted on the object by gravity, or the force
needed to support it. For example, your mass is the same no matter where you go
in the universe; your weight, on the other hand, changes from place to place.
Mass is measured in kilograms. Even though we usually talk about weight in
kilograms, strictly speaking it should be measured in <a href="#2.10._newton">
newtons</a>, the units of force.



<hr/><a id="2.3._second" />
<h2>2.3. Second</h2><hr/>
The `1 second` unit is used to measure how much time has elapsed.

<br/>The historical origin (justification) for the value quantity of a second
is:

"<i>The day was divided into 24 (parts, which we call hours), each hour was
divided in 60 (parts, which we call minutes) and each minute was divided in 60
(parts, which we call seconds).
A second is $$ \frac{1}{24 \cdot 60 \cdot 60} = \frac{1}{86400} $$ of the
day.</i>"

<br/>The post-2019 formal definition of a second is:

"<i>It is defined by taking the fixed numerical value of the caesium frequency
$$ \Delta ν_{Cs} $$, the unperturbed ground-state hyperfine transition frequency
of the caesium 133 atom, to be 9 192 631 770 when expressed in the unit
$$ s ^ {-1} $$ (which is equal to Hz).</i>"


<hr/><a id="2.4._ampere" />
<h2>2.4. Ampere</h2><hr/>
1 Ampere ( $$ 1 \cdot A $$ ) is defined as 1 <a href="#2.12._coulomb">coulomb
</a> of charge, passing through a "wire" in 1 second of time:

$$ 1 A = \frac{1 C}{1 s} $$ .


<hr/><a id="2.8._velocity" />
<h2>2.8. Velocity</h2><hr/>
Dimension: $$ L \cdot T^{-1} $$ . With a length dimension component and a time
dimension component.

<br/>Unit of measure: $$ m \cdot s^{-1} \Leftrightarrow \frac{m}{s}
\Leftrightarrow m/s $$ 

<br/>Symbol for quantity: $$ a  $$.

<br/>The velocity measures the rate of change of an object's position with
respect to a frame of reference, and is a function of time.

The velocity is a vector. It has a scalar(magnitude) which is called speed and
a direction of movement(change in position).

<br/>For example a human is walking with a speed of approximately $$ 1.56 \cdot
\frac{m}{s} $$.

<br/>Galileo defined speed as the distance covered per unit of time. In equation
form that is:

$$ v = \frac{d}{t} $$
where $$ v $$ is the speed, $$ d $$ is distance, and $$ t $$ is time.

<a href="https://en.wikipedia.org/wiki/Velocity" target="_blank">Read more</a>.


<hr/><a id="2.9._acceleration" />
<h2>2.9. Acceleration</h2><hr/>
Dimension: $$ L \cdot T^{-2} $$ . With a length dimension component and a time
dimension component.

Unit of measure: $$ m \cdot s^{-2} \Leftrightarrow \frac{m}{s^2} $$

<br/>Acceleration is the rate of change of the velocity of an object with
respect to time.

<br/>The units "metre per second squared" can be understood as change in
velocity per time. For example an increase of velocity by 1 metre per second
every second is represented as $$ 1 \cdot \frac{m}{s^2} $$.

<br/>Near Earth's surface, gravitational acceleration is approximately $$ g =
9.81 \cdot \frac{m}{s^2} $$, which means that, ignoring the effects of air
resistance, the speed of an object falling freely will increase by about
$$ 9.81 $$ metres per second, every second.


<hr/><a id="2.10._newton" />
<h2>2.10. Newton</h2><hr/>
Dimension: $$ L \cdot M \cdot T^{-2} $$ . With a length dimension component, a
mass dimension component and a time dimension component.

<br/>Unit of measure: $$ 1 \cdot N = 1 \cdot \frac { kg \cdot m }{s^2} $$.

<br/>Symbol for quantity: $$ F $$.

<br/>One newton is the force needed to accelerate one kilogram of mass at the
rate of one metre per second squared in the direction of the applied force.

<br/><a href="https://en.wikipedia.org/wiki/Isaac_Newton">Isaac Newton</a>'s
"2nd law of motion" said that (and we agree with it, we accept it): the Force is
defined as a mass times(multiplied by) an acceleration: $$ F = m \cdot a $$.
Where $$ F $$ is the Force, $$ m $$ is the mass(not meters) and $$ a $$ is the
acceleration.

<br/>Now, $$ F = m \cdot a $$ is expressed symbols of quantities, if we express
it in symbols of units, we measure the force in Newtons, the mass in Kilograms
and the acceleration in $$ \frac{m}{s^2} $$, so the expression becomes: $$
N = kg \cdot \frac{m}{s^2} $$.

<br/>At average gravity on Earth (conventionally, $$ g = 9.80665 \frac{m}{s^2}
$$), a 1 kilogram mass exerts a force of about $$ 9.8 $$ newtons. An
average-sized apple exerts about one newton of force, which we measure as the
apple's weight.

<br/>$$ 1 N = 0.10197 kg \cdot 9.80665 m \cdot s^{-2} $$ where ( $$ 0.10197 kg =
101.97 $$ grams, the average weight of an apple).

<br/>So, if you ever get the chance to hold an apple in your hand, on the
surface of planet Earth, stop and think. You are feeling 1 Newton of force in
your hand.

<br/>The weight of an average adult exerts a force of about $$ 608 N $$.

<br/>$$ 608 N = 62 kg \cdot 9.80665 m \cdot s^{-2} $$ (where $$ 62 kg $$ is the
world average adult mass).


<a href="https://en.wikipedia.org/wiki/Newton_(unit)" target="_blank">Read
more</a>.


<hr/><a id="2.11._joule" />
<h2>2.11. Joule</h2><hr/>
Dimension: $$ L^{2} \cdot M \cdot T^{-2} $$ . With a length dimension component,
a mass dimension component and a time dimension component.

Unit of measure: $$ 1 \cdot J = 1 \cdot N \cdot 1 \cdot m
=1 \cdot \frac {kg \cdot m^2}{s^2} $$ 

<br/>A Joule is equal to the energy transferred to (or work done on) an object
when a force of one newton acts on that object in the direction of the force's
motion through a distance of one metre (1 newton metre or $$ N \cdot m $$).

<br/>It is also the energy dissipated as heat when an electric current of one
ampere ( $$ 1 A $$ ) passes through a resistance of one ohm ( $$ 1 \Omega $$ )
for one second ( $$ 1 s $$ ).

<br/>So, Joules are a unit of measure for **energy** as a quantity. Sometimes we
also say that "work" is done, if a Force is applied to move an object in space.

<br/>**Practical examples** - One joule represents (approximately):

2.11.1. The energy required to lift an apple up 1 metre (assume the apple has a
mass of approximately 102 grams).

2.11.2. The kinetic energy of a $$ 2 kg $$ mass traveling at $$ 1 m/s $$

2.11.3. The energy required to accelerate a $$ 1 kg $$ mass at $$ 1
\frac{m}{s^2} $$ through a distance of $$ 1 m $$.

2.11.4. The heat required to raise the temperature of $$ 1 g $$ of water by
$$ 0.24 °C $$.

2.11.5. The typical energy released as heat by a person at rest every
$$ \frac{1}{60} \cdot s $$ (approximately $$ 17 ms $$).

2.11.6. The kinetic energy of a $$ 50 kg $$ human moving very slowly (at
$$ 0.2 m/s $$ or $$ 0.72 km/h $$).

2.11.7. The kinetic energy of a $$ 56 g $$ tennis ball moving at $$ 6 m/s $$
( $$ 22 km/h $$ ).

2.11.8. The amount of electricity required to light a $$ 1 W $$ LED for
$$ 1 s $$.

2.11.9. A kilowatt-hour is $$ 3.6 $$ megajoules.


<hr/><a id="2.12._coulomb" />
<h2>2.12. Coulomb</h2><hr/>
See the <a href="#3.1._atom" target="_blank">atom</a> and get familiar with the
electrons and the electric charge(and the force) of electrons and protons.

<br/>1 coulomb is defined as the charge of
6 241 509 074 460 762 607.776 elementary charges. Note the `0.776` termination.
That is less than 1 proton/electron which can not be created, thus it is
impossible to realize exactly `1 C` of charge. An elementary_charge is a proton
(positive charge) or electron(negative charge). Thus
`6 241 509 074 460 762 607.776 protons` = `+ 1 C` and
`6 241 509 074 460 762 607.776 electrons` = `- 1 C`.


<hr/><a id="2.13._volt" />
<h2>2.13. Volt</h2><hr/>
Dimension: $$ L^{2} \cdot M \cdot T^{-3} \cdot I^{-1} $$ . With a length
dimension component, a mass dimension component, a time dimension component and
an electric current dimension component.

<br/>Unit of measure: $$ 1 \cdot V = 1 \cdot \frac{J}{C} $$.

If we consider $$ 1 A = \frac{C}{s} \Leftrightarrow 1 C = A \cdot s $$ then, $$
1 \cdot V = 1 \cdot \frac {kg \cdot m^2}{A \cdot s^3} $$.

<br/>Volt is the unit that measures the voltage, and the voltage is the quantity
of electric potential, or the electric potential difference. The symbol for the
voltage quantity is also $$ V $$.

<br/>We define that a circuit component has a potential difference of one
**Volt** ( $$ V $$ ), if one <a href="#2.11._joule">Joule</a> of energy is
required to move one <a href="#2.12._coulomb">Coulomb</a> of charge through that
component.


<hr/><a id="2.14._watt" />
<h2>2.14. Watt</h2><hr/>
The watt measures the quantity of power which is the rate of energy transfer.
The symbol of the power quantity is $$ P $$.
So, the watt is defined as:

$$ 1 W = 1 \cdot \frac{J}{s} $$

Dimension: $$ L^{2} \cdot M \cdot T^{-3} $$ . With a length dimension component,
a mass dimension component and a time dimension component.

<br/>Unit of measure: $$ 1 \cdot W = 1 \cdot \frac{kg \cdot m^{2}}{s^{3}}$$.

or, from an electrical point of view:
$$ \require{cancel} 1 W = 1 V \cdot 1 A =
\frac{J}{\bcancel{C}} \cdot \frac{\bcancel{C}}{s} = \frac{J}{s} $$

<br/>1 watt is the energy required to lift an apple up 1 metre in 1 second of
time (assume the apple has a mass of approximately 102 grams).

<br/>The watt is sometimes said that it measures "how **fast** work is done".
Considering that 1 Watt is equal to 1 Joule of work being done on an object in
1 second of time.


<hr/><a id="2.15._ohm_and_siemens" />
<h2>2.15. Ohm and Siemens</h2><hr/>
The Ohms measure the quantity of electrical resistance of an object. Meaning,
how hard it is for electrical current (charge/electrons) to flow through that
object.

Electrical resistance is measured in the unit of **ohm**, using the unit symbol
$$ \Omega $$. In the electronics industry it is common to use the character
$$ R $$ instead of the $$ \Omega $$ symbol, thus, a $$ 10 \Omega $$ resistor may
be represented as $$ 10 R $$. And $$ R $$ was preferred instead of $$ \Omega $$
because it's easier to write it since it's available in most fonts. See
<a href="https://en.wikipedia.org/wiki/Ohm#Symbol" target="_blank">this</a>.

<br/>$$ R $$ is also used as a symbol for the quantity of electrical resistance
of an object.

$$ 1 \Omega = 1 \cdot \frac{V}{A} $$

The dimension for electrical resistance is: $$ L^{2} \cdot M \cdot T^{-3} \cdot
I^{-2} $$ . With a length dimension component, a mass dimension component, a
time dimension component and an electric current dimension component.

<br/><br/>

Now, the siemens measures the quantity of electrical conductance, or how easy it
is for electrical current (charge/electrons) to flow through that object.

<br/>$$ G $$ is used as a symbol for the quantity of electrical conductance of
an object.

<br/>Of course, this is the inverse of the electrical resistance. The electrical
conductance is measured in units of **siemens**. The unit siemens uses the
symbol $$ S $$. Sometimes, to avoid confusion with lower-case symbol $$ s $$,
which is used for second, Siemens also uses the symbol $$ \Omega^{-1} $$ or
$$ ℧ $$. 

$$ 1 S = 1 \cdot \frac{A}{V} = 1 \cdot \frac{1}{\Omega} $$

The dimension for electrical conductance is: $$ L^{-2} \cdot M^{-1} \cdot T^{3}
\cdot I^{2} $$ . With a length dimension component, a mass dimension component,
a time dimension component and an electric current dimension component.

<br/>For example:

2.15.1. The conductance of a resistor with a resistance of five ohms, is
$$ (5 \Omega)^{−1} $$ , which is equal to $$ 0.2 S $$.

2.15.2. A resistance of $$ 1 \Omega $$ is when a voltage of 1 volt is placed
across a circuit and the resulting current is 1 ampere.

2.15.3. For a device with a conductance of one siemens, the electric current
through the device will increase by one ampere for every increase of one volt of
electric potential difference across the device.



<hr/><a id="3._elementary_particles" />
<h1>3. Elementary particles</h1><hr/>
<hr/>
Humanity has noticed, along the time, that all everyday objects that can be
touched are made up of smaller objects, they are divisible.

<br/>With time, we've come up with a fancy word, and "all everyday objects that
can be touched" are called **matter**.

<br/>In a more formal way, matter can be defined as anything that has mass and
volume.

<br/><a href="https://en.wikipedia.org/wiki/Democritus" target="_blank">
Democritus</a>, a Greek philosopher (born ~460 B.C.), wondered what would happen
if you cut a piece of matter, such as an apple, into smaller and smaller pieces.
He thought that a point would be reached where matter could not be cut into
still smaller pieces. He called these "uncuttable" pieces *a-tomos*
(in-divisible). This is where the modern term **atom** comes from.


<hr/><a id="3.1._atom" />
<h2>3.1. Atom</h2><hr/>
Here is an obsolete, but easier to understand representation of an atom:

<!--
Description: Stylised atom. Blue dots are electrons, red dots are protons and black dots are neutrons.
Date: 14 February 2007
Source: Own work based on: of Image:Stylised Lithium Atom.png by Halfdan. https://commons.wikimedia.org/wiki/File:Stylised_atom_with_three_Bohr_model_orbits_and_stylised_nucleus.svg
Author: SVG by Indolences. Recoloring and ironing out some glitches done by Rainer Klute.
Permission (Reusing this file) :
GNU head Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.2 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts. A copy of the license is included in the section entitled GNU Free Documentation License.
w:en:Creative Commons
attribution share alike	This file is licensed under the Creative Commons Attribution-Share Alike 3.0 Unported license.	
You are free:
to share – to copy, distribute and transmit the work
to remix – to adapt the work
Under the following conditions:
attribution – You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
share alike – If you remix, transform, or build upon the material, you must distribute your contributions under the same or compatible license as the original.
This licensing tag was added to this file as part of the GFDL licensing update.
-->
<?xml version="1.0" encoding="UTF-8"?>
<svg width="530" height="600" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
 <defs>
  <linearGradient id="a">
   <stop stop-color="#fff" offset="0"/>
   <stop stop-color="#fff" stop-opacity="0" offset="1"/>
  </linearGradient>
  <radialGradient id="l" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,274.58,-6.77)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
  <radialGradient id="k" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,235.07,-36.48)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
  <radialGradient id="j" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,235.07,-76.08)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
  <radialGradient id="h" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,195.46,-46.38)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
  <radialGradient id="g" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,195.46,-6.77)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
  <radialGradient id="f" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,234.97,13.03)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
  <radialGradient id="i" cx="16.036" cy="140.06" r="13.006" gradientTransform="matrix(2.2879,0,0,2.2879,274.68,-46.38)" gradientUnits="userSpaceOnUse" xlink:href="#a"/>
 </defs>
 <g>
  <use transform="rotate(120.89 265.83 300.59)" xlink:href="#e"/>
  <rect x="720.21" y="-42.084" width="250.78" height="250.78" fill="#da86da" fill-rule="evenodd" stroke="#000" stroke-linecap="round" stroke-width="1.0016" style="paint-order:normal"/>
  <g id="e">
   <g transform="translate(-174.79 -163.35)">
    <path d="m595.64 310.31a19.804 19.804 0 0 1-39.607 0 19.804 19.804 0 1 1 39.607 0z" fill="#223b84" stroke-width="5.2716"/>
    <use transform="matrix(.66552 0 0 .66552 372.89 121.81)" xlink:href="#c"/>
   </g>
   <path d="m363.77 160.95c-43.025 15.872-91.339 38.63-140.64 67.196-141.48 81.968-237.4 180.58-214.27 220.25 23.12 39.67 156.54 5.3986 298.02-76.569 141.48-81.968 237.4-180.58 214.27-220.25-9.6119-16.492-38.292-20.197-78.688-13.013" color="#000000" fill="none" stroke="#000" stroke-miterlimit="10" stroke-width="7.9213"/>
  </g>
  <use transform="rotate(240 263.04 298.33)" xlink:href="#e"/>
  <g id="b" style="">
   <path d="m334.66 283.23a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" stroke-width="7.9213"/>
   <path id="c" d="m341.14 274.07a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#i)" opacity=".9" stroke-width="7.9213"/>
  </g>
  <use transform="translate(-39.5,-69.311)" style="" xlink:href="#d"/>
  <use transform="translate(-79.21)" style="" xlink:href="#b"/>
  <use transform="translate(-79.11)" style="" xlink:href="#d"/>
  <use transform="translate(-39.7,59.41)" style="" xlink:href="#b"/>
  <g id="d" transform="translate(-324.49 -46.741)" style="">
   <path d="m659.05 369.58a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="#a90000" stroke-width="7.9213"/>
   <use transform="translate(324.37 86.347)" xlink:href="#c"/>
  </g>
  <use transform="translate(-39.6 9.9015)" style="" xlink:href="#b"/>
  <g stroke-width="7.9213">
   <path d="m295.06 253.53a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="#a90000"/>
   <path d="m301.53 244.36a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#j)" opacity=".9"/>
   <path d="m255.45 283.23a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z"/>
   <path d="m261.92 274.07a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#h)" opacity=".9"/>
   <path d="m255.45 322.84a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="#a90000"/>
   <path d="m261.92 313.67a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#g)" opacity=".9"/>
   <path d="m294.96 342.64a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z"/>
   <path d="m301.41 333.48a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#f)" opacity=".9"/>
   <path d="m334.56 322.84a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="#a90000"/>
   <path d="m341.02 313.67a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#l)" opacity=".9"/>
   <path d="m295.06 293.13a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z"/>
   <path d="m301.53 283.97a29.756 29.756 0 0 1-59.513 0 29.756 29.756 0 1 1 59.513 0z" fill="url(#k)" opacity=".9"/>
  </g>
 </g>
</svg>

As you can see, now we know that the atom **is** divisible and we are aware of
at least 3 distinct elements (the colored balls in the image above - the lines
are just trajectories):


- **Neutrons** - These are the **black** balls in the image. They have a big
mass but no electric charge(they are neutral).
- **Protons** - These are the **red** balls in the image. They have a big mass
and a positive electric charge.
- **Electrons** - These are the **blue** balls in the image. They have a small
mass and a negative electric charge.

<br/>It is only a convention that electric charge of the electron is *negative*
and the electric charge of the proton is *positive*.

<br/> The atomic **nucleus** is the small, dense region consisting of neutrons
and protons at the center of an atom. The nucleus is made of one or more protons
and a number of neutrons. Only the most common variety of hydrogen has no
neutrons.

<br/>More than 99.94% of an atom's mass is in the nucleus. The electron has a
mass that is approximately $$ 1/1836 $$ that of the proton. Protons and neutrons
have approximately the same mass of $$ 1.66053906660(50) \cdot 10^{−27} kg
= 1 Da $$, which is called 1 *Da*lton. Electrons have a mass of
$$ 9.1093837015(28) \cdot 10^{−31} kg $$.


<br/>Particles with the *same* electric charge(both positive or both negative),
when placed next to each other, will experience a force, they will repel each
other(they will try to move away from each other, to move in the opposite
direction, to maximize the distance between them).

<br/>Particles with *different* electric charge(a positive and a negative one),
when placed next to each other, will experience a force, they will attract each
other(they will both try to move closer together, towards the same point in
space, to minimize the distance between them).

<br/>If the number of protons and electrons are equal, then the atom is
electrically neutral. Ions are atoms that have an different number of electrons
than the number of protons, thus it has a positive or negative electric charge.
Note that only electrons can come inside an atom, and can leave the atom
structure!

<br/>Atoms are extremely small, typically around 100 picometers ($$ = 1 Å =
1^{-10} m $$) in diameter. The diameter of an electron is considered to be
$$ 2.8179403227(19) \cdot 10^{-15} m $$. The diameter of a proton is considered
to be $$ 1.682838 \cdot 10^{-15} m $$. The diameter of a neutron is considered
to be approximately $$ 1.6 \cdot 10^{-15} m $$. For some atoms, the diameter of
the nucleus is about 100 000 times smaller than the diameter of the atom.

<br/>However, the atoms are so small that accurately predicting their behavior
and studying some of their properties, using classical physics — as if they were
tennis balls, for example — is not possible due to quantum effects.

<br/>The **atomic number** is the number of protons inside an atom, and it helps
us identify the type of atom, in
<a href="https://en.wikipedia.org/wiki/Periodic_table" target="_blank">the
list/table of atom types</a>.

<br/>Read more:
- <a href="https://en.wikipedia.org/wiki/Atom" target="_blank">Atom</a>
- <a href="https://en.wikipedia.org/wiki/Electric_charge" target="_blank">
Electric charge</a>
- <a href="https://en.wikipedia.org/wiki/Periodic_table" target="_blank">
Periodic table</a>



<hr/><a id="3.2._photon" />
<h2>3.2. Photon</h2><hr/>
Photons have no mass, so they are not matter particles.


<hr/><a id="4._other" />
<h1>4. Other</h1><hr/>

1. Volts - measure the pressure. Pressure that pushes electrons through the circuit.
2. Amperes - measures how many electrons pass a certain point(section of wire) in a certain interval of time.

3. Is it true that increasing the pressure(volts), the amperes will also increase? If you put more pressure on a water pipe, more water drops(millilitres) will pass per time.

4. `Wats = Volts x Amperes` `W = V x A`
5. V(Voltage/Volts/Pressure), I(Intensity/Amperage/Flow), R(Resistance/Restriction_to_flow/Ohms/Ω) - `V = I * R` - Ohm's law. The voltage V in volts (V) is equal to the current I in amps (A) times the resistance R in ohms (Ω) . also known as $$ R = \frac{V}{I}  = ohms = \frac{volts}{amperes} $$
6. `P = V * I` = `watts = volts * amperes`
7. `1 Ampere = 1 Coulomb/second` where 1 [Coulomb](https://en.wikipedia.org/wiki/Coulomb) is = `6 241 509 074 460 762 607.776 elementary_charges` 
8. 1 Ampere_Hour = is the **amount of energy charge** in a battery that will allow 1 Ampere of current to flow for 1 Hour.

9. A typical alkaline or NiMH battery in the standard “AA” size has about 2000 to 3000 mAh (or 2 to 3 Ah)

---
From these 2 formulas: $$ R = \frac{V}{I} $$ and $$ P = V \cdot I $$ we can expand
to 12 formulas, expressing each of the 4 value as a combination of 2 of the
other 3 values.

So, we have:

2.1. $$ R = \frac{V}{I} $$ - that's how it is defined

2.2. $$ R = \frac{V^2}{P} $$ - Because ...

2.3. $$ R = \frac{P}{I^2} $$ - Because ...

2.4. $$ P = V \cdot I $$ - That's how it is defined

2.5. $$ P = I^2 \cdot R $$ - Because ...

2.6. $$ P = \frac{V^2}{R} $$ - Because ...

2.7. $$ V = I \cdot R $$ - Because ...

2.8. $$ V = \frac{P}{I} $$ - Because ...

2.9. $$ V = \sqrt{P \cdot R} $$ - Because ...

2.10. $$ I = \frac{V}{R} $$ - Because ...

2.11. $$ I = \frac{P}{V} $$ - Because ...

2.12. $$ I = \sqrt{\frac{P}{R}} $$ - Because ...




$$ (2^{n-1})-1 $$

3. I was thinking. In 2D cartoons, things can float, you can place an object in
mid air, and it can stay there, if you don't simulate gravity. Same thing
happens to a window/icon on a computer desktop, it does not fall. Or a pencil on
a physical desk, you can place it anywhere in the 2D "plane" of the desk. But
why can't we place a hammer in mid air to free your hand, and then pick it up
later when you need it again? I was thinking that the reason why the hammer does
not fall off the table is because gravity is a 1D force, cancelled by the table.
Of course, the gravity can also be "cancelled" if you are "in space", or
"in free fall". But I was thinking what if we think in 4 dimensions, and the
gravity would be applied on the 4th dimension only, having no effect on the 3
dimensions that we live in? would that be possible? :thinking:

<br/><a href="#1._a_unit_of_measure">1. A unit of measure</a>
<br/><a href="#2._international_system_of_units">2. International system of
units</a>
<br/><a href="#3._elementary_particles">3. Elementary particles</a>
<br/><a href="#4._other">4. Other</a>

<hr/><hr/><hr/>

Bibliography:

1. <a
  href="https://www.bipm.org/utils/common/pdf/si-brochure/SI-Brochure-9-EN.pdf"
  target="_blank">
  https://www.bipm.org/utils/common/pdf/si-brochure/SI-Brochure-9-EN.pdf</a>
