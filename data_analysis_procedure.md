---
title: Data Analysis Procedure
authors:
  - name: S. Bourdarie
    affiliations:
      - ONERA, France
  - name: B. Blake
    affiliations:
      - Aerospace Corporation, USA
  - name: J.B. Cao
    affiliations:
      - CSSAR, China
  - name: R. Friedel
    affiliations:
      - LANL, USA
  - name: Y. Miyoshi
    affiliations:
      - STELAB, Japan
  - name: M. Panasyuk
    affiliations:
      - MSU, Russia
  - name: C. Underwood
    affiliations:
      - U. Of Surrey, UK
exports:
  - format: pdf
    template: ./.template
    output: exports/PRBEM_Data_Analysis_Procedure.pdf
    id: dap-pdf-export 
downloads:
  - id: dap-pdf-export
    title: PDF Version
---

# Introduction

The purpose of this document is to delineate analysis methodologies for creating improved
space radiation models from a wide variety of space radiation measurements collected
worldwide.

The energetic particle environment near the Earth is composed of three different components:
1. The radiation belts where energetic electrons and ions can be found. This ionizing
environment can be highly dynamic during magnetic storms,
2. The solar energetic particles (mainly protons and heavy ions), which are present only
during Solar Particle Events, with a duration of hours to days,
3. The Galactic cosmic rays (mainly protons and heavy ions)
 
Of course all these particles cohabit in space and their measurement is not at all
straightforward. The consequence is that all measurements have to be analyzed in detail based
upon our current knowledge of radiation belt global structure and dynamics as well as
instrument physics. This analysis has to focus on instrument contamination, saturation,
background, glitches … and cross-calibration.

Guidance is needed from the worldwide community of developers of space radiation models
to permit standardization of data analysis efforts to ensure that everybody is working on the
same basis and to increase our confidence in the quality of the data actually used in models.
Note that in the following document we suppose that the instrument response (based on
Monte-Carlo simulation and/or ground tests) is known well enough to make good use of the
data, in particular this means that the counts to flux conversions are known.

In this documents examples of problems encountered with several instruments are shown.
Various spacecraft/instruments, from different countries, built with different technologies
have been chosen to show that the perfect instrument does not exist, and that most instruments
have advantages and deficiencies for space modeling. In any case there is no desire to one
point fingers at “bad” or “good” instruments but to show that with the proper analysis useful
data can be derived from almost any of these instruments.

# Context

Nowadays there are large data bases of radiation-belt and interplanetary-medium energetic
particles available throughout the world. Of course in future years more such data bases will
become available. What currently exists already is a very useful and important resource that
can be effectively used to develop new space environment models. These data have been
recorded by a wide variety of instruments, which have varied characteristics. It is obvious that
these characteristics have to be understood for future use of these in-situ data for modeling.
Data from different sources have to be merged together and well understood to ensure a
global coherence.

A fundamental requirement for our modeling purposes is the availability of multi-instrument,
multi-spacecraft data that are inter-calibrated. Here the term “inter-calibrated” means that the
response functions of all instruments must be well known so that these data can be seamlessly
merged. Ideally, given “perfect” instruments, no further efforts along these lines would be
required. However energetic particles represent a challenging measurement task in space for
many reasons. One critical reason is it is never possible to fly the amount of shielding
required for a “clean” measurement, and a second is that it is impossible to recreate the full
energetic particle environment the instrument will encounter in space in the lab for
calibrations. Existing instrument calibrations range from comprehensive to virtually non-existent.
Some laboratory calibrations are mainly limited to "through the aperture"
calibrations which do not address the instruments background response to an omnidirectional,
high-energy environment. For some instruments flown on a series of satellites (e.g. LANLGPS, 
LANL-GEO), detailed calibrations were only performed for one representative instrument of many
"identical" instruments. In the case of other instruments, only the "nominal" design specifications 
are available to the radiation-belt modeler. Designs of instruments vary widely, with some 
intrinsically having better  immunity to background than others.

While there has been much progress in the modeling of energetic particle instruments from
first principles, those methods have their own challenges and still often give factors of 2-5
uncertainties in an instrument's performance [](doi:10.1029/2005SW000150). Full modeling of
space radiation instruments is a time and computer-intensive procedure, and has only been
done for a few instruments (e.g. LANL-GPS, some LANL-GEO). These instrument-modeling
studies can also tell under what conditions an instrument will perform well, and when it might
be in principle impossible to recover a "clean" spectrum from an instrument.
Given the limitations of each individual instrument's pre-flight calibrations, further, on-orbit,
inter-comparisons using actual, collected data need to be performed.

# Coordinates to sort the data

A first step to go through before analyzing on-orbit measurements is to define appropriate
coordinates to sort the data. The basic positional information associated with any in-situ
measurements is the time and location in geographic coordinates. Because the trapped particle
motion is governed by the Earth's magnetic field (see annex A), magnetic coordinates have to
be computed to organize the data.

## Set of coordinates

Because the Earth magnetic field is not constant in time the best set of parameters to use are
the three adiabatic invariants $J_1$, $J_2$ and $J_3$ (see annex A). Unfortunately this coordinate system
has some limitations: it is only appropriate for trapped particles and not for cold plasma data,
measurements are always done at constant energy (this means different J1 and J2 for each
measurements) and from the physics point of view it is easier to manipulate variables like
energy and pitch angle.

Because in a slowly varying magnetic field only $K$ ($J_1-J_2$) and {math}`L^*` ($J_3$) are conserved along a
trapped particle motion, a good consensus is to adopt the following set of coordinates: $E$, {math}`\alpha^*_{eq}`,
{math}`L^*`, and MLT, where $E$ is the particle energy, {math}`\alpha^*_{eq}` is deduced from the equation
```math
\frac{K\sqrt{L^*}}{Re\sqrt{Bo}} = \frac{Y(y)}{y}
```
({math}`y=\sin(\alpha^*_{eq})` and $Y(y) = 2.760346 + 2.357194 y - 5.117540 y^{3/4}$), {math}`L^*` is the Roederer L
parameter and MLT the magnetic local time (crucial for non-trapped particles).

This set of parameters has twoadvantages; they may easily be computed from measurements,
and they are convenient to use for performing data assimilation in physical radiation belt
models. 

## Magnetic field model

An ideal magnetic field model is one which allows removal of adiabatic flux variations when
data are sorted according to $J_1$, $J_2$ and $J_3$. Unfortunately, though there are many time
dependant magnetic field models available, none of them are realistic enough during very
disturbed periods to sort the data properly (one magnetic field model can be better at low {math}`L^*`:,
another one better on the day side, etc …). Moreover for sophisticated, external magnetic field
models various input are required. For the simplest, magnetic indices are sufficient but for
some others solar-wind parameters are necessary. Both can be used. Nevertheless, if in-situ
radiation-belt particle data are analyzed in order to develop a radiation-belt model,
measurements from long duration satellites must be considered and solar wind parameters
may not always be available. Moreover, computing {math}`L^*` with such models over decades of
measurements would be very computer intensive. Thus for this purpose it is preferable to
select a model which is not dependent upon solar wind parameters. Only a few choices are
left: Mead-Fairfield 1975, Tsyganenko 1987 and 1989 and Olson Pfitzer quiet 1977. From
statistical studies the Olson-Pfitzer quiet model has been shown to be a good average external
magnetic field model when compared to measurements.
For the internal magnetic field, the use of the IGRF reference field is well established.
Because this field is slowly drifting in time (secular drift) updating IGRF once per year is
usually sufficient and it can be done at each mid-year.

**Therefore the proposed standard for the magnetic field model is IGRF (decimal year +
0.5) plus Olson-Pfitzer quiet 1977.**

# Obtaining sanitized data

In the following sections the terms contamination and background are defined such as:
 - "contamination" = high level dynamic background due to non primary instrument response
 that can completely dominate an instruments response
 - "background" = low level more or less constant background due to electronic and detector
 noise and cosmic rays.

## Electron data contamination

### Contamination by proton

It is well known that during times of solar energetic proton events (SEPs) many of the
detectors are contaminated with strong background counts.
[](#electron_sep_contamination) gives an example of a period of SEP
contamination. The SEP event is clearly visible in the elevated flux levels
of energetic protons measured at GOES 08. These ions can penetrate the
electron detectors at LANL-GPS and LANL-GEO, leading to elevated electron
flux observations, across all sampled L-values, as clearly shown in
[](#electron_sep_contamination).

```{figure} images/electron_sep_contamination.png
:label: electron_sep_contamination
:alt: Example of SEP-induced contamination in electron data.

Example of SEP data from GOES 8 [top panel] leading to contamination in data
from the LANL-GPS [middle panel] and LANL-GEO [bottom panel] instruments.
The SEP period is between the two vertical green lines.
```

:::{note} Ed. Note:
There are degrees of contamination. Even if electrons dominate, the data can be
compromised.
:::

Next, depending on the instrument there may be regions in the orbit where we
know that the data is contaminated by background. This can be easily seen
from survey plots where fluxes are color coded in a  {math}`L^*` versus time
map. Such an example is provided in [](#electron_proton_contamination). In
the bottom panel (the 1.24-1.6 MeV channel) contamination induced by SEPs in
the outer belt (brief enhancements across {math}`L^* > 4`) and contamination
induced by trapped protons in the south Atlantic anomaly ({math}`L^* < 2.5`)
is clearly seen. It must be noted here that in this case, if data are
contaminated by SEPs (which is more obvious to detect) there is high
probability that the same channel is contaminated by high-energy trapped
protons in the inner belt. In the lower electron channel (top panel),
because electron fluxes probably dominate high-energy proton fluxes which
could induce background, contamination is masked and electrons dominate in
that channel.

```{figure} images/electron_proton_contamination.png
:label: electron_proton_contamination
:alt: Example of trapped protons contamination in electron data.

Electron flux on-board SAC-C spacecraft (715 km – 98°) in a {math}`L^*` versus time map for three
energies (top=0.19-0.25 MeV, middle=0.53-0.69 MeV and bottom=1.24-1.6 MeV)
```

To clean up the data two possibilities emerge. For the first one let’s consider the case there are
no proton measurements (or at least not in the energy range responsible for any background)
on the same spacecraft. In this case, the analysis must be done in two steps:

1. To evaluate any possible contamination due to SEPs GOES proton data can
   be used as a proxy. It is then necessary to plot electron data (at the
   same resolution as GOES) when the spacecraft in question is outside of
   geomagnetic shielding, {math}`L^*`>7 is a safe value, but it must be
   lower for GEO orbit) versus GOES data for each time step. In this case it
   is necessary to find out the best correlation obtained by using various
   GOES proton channels. Such an example is given in
   [](#electron_contamination_correlation) where LANL-GEO-ESP electron
   (4.5-6.  MeV) is plotted versus GOES-08 15-44 MeV proton. When GOES
   measurements (on the x axis) is greater than the value indicated by the
   vertical green line, then SEP protons are recorded on-board GOES. On the
   right hand side of the plot a clear correlation is seen between LANL and
   GOES data. Finally all LANL data (for this energy channel) below the red
   line must be removed, as they correspond to contamination. The equation
   of the red line indicates the boundary where LANL data must be removed
   using GOES-08 measurements as a proxy.
2. To evaluate possible contamination in the inner belt, only a flux map in
   a {math}`L^*` versus time plot can help. See
   [](#electron_proton_contamination), bottom panel. Then a filter function
   of {math}`L^*` can be defined.

```{figure} images/electron_correlation_plot.png
:label: electron_contamination_correlation
:alt: Correlation plot between electron and proton observations.

Correlation plot between LANL-1990-095 Ele. 4.5-6. MeV and GOES-08 Pro.
15-44. MeV.  Vertical green line indicates the threshold value above which
GOES-08 detects SPEs. The red line indicates where the LANL data are
contaminated significantly (above red line electrons dominate, below the red
line protons dominate in the channel)
```

Now let us consider the case where there are proton measurements (in the
energy range responsible for any background) on the same spacecraft. In this
case, the analysis becomes easier: correlation plots can be made between the
electron channels under consideration and proton channels. The best
correlation is sought. If none is found, then there is no significant
contamination in the electron channels.

In the case a correlation is found, data can be cleaned up as described
above, where GOES data is replaced by the proton data measured on the same
spacecraft.

### Contamination by relativistic electrons

In the case of electron channel, measurements can be contaminated by
Bremsstrahlung photons. There is no easy way to get clear evidence of this
kind of contamination except from Monte-Carlo simulation of the detector
sensitivity to energetic electrons and/or by looking carefully at flux maps
in a {math}`L^*` versus time plot. Such contamination is more likely to occur one or
two days after a storm onset when electron spectra are harder in the outer
belt.

### Summary

```{figure} images/electron_contamination_summary.png
:label: electron_contamination_summary
:alt: Electron contamination filtering recommandations

Logic to define data filter to remove any contamination in trapped electron measurements.
```

## Proton data contamination

### Contamination by relativistic electron

Contamination in the proton channels frequently results from high energy
electrons. This problem occurs preferentially in the outer electron belt.
Clear evidence of such contamination can be seen on flux map in a
{math}`L^*` versus time plot if no electron measurements are available on
the same spacecraft, otherwise from correlation plots with various electron
channels. An example is provided in [](#proton_electron_contamination) where
the proton channel 28.7-33.1 MeV on-board XMM spacecraft is contaminated by
relativistic electrons during intense magnetic storms (inside the red oval).
Since the events shown are above {math}`L^*=2.8` which is outside the proton
trapping boundary for this energy, the measurements must be due to
background. The same thing can be seen on [](#proton_correlation_plot) where
the same proton channel is plotted versus electrons in the > 2.54 MeV
channel, measured on-board the same spacecraft for regions above
{math}`L^*=2`. These correlations are used to define a filter to
decontaminate the proton channels from the relativistic electron
contribution.

```{figure} images/proton_electron_contamination.png
:label: proton_electron_contamination
:alt: Example of electron contamination on proton observations.

Example on proton contamination by electrons (inside red oval) on-board XMM spacecraft
```

```{figure} images/proton_correlation_plot.png
:label: proton_correlation_plot
:alt: Correlation plot between electron and proton observations.

Correlation plot between a proton channel (28.7-33.1 MeV) and an electron
channel (>2.54 MeV) on-board XMM. The red line shows when both are well
correlated: electrons dominate protons.  
```

### Summary

```{figure} images/proton_contamination_summary.png
:label: proton_contamination_summary
:alt: Electron contamination filtering recommandations

Logic to define data filter to remove any contamination in trapped proton measurements.
```

## Saturation

Count rate saturation occurs in some instruments leading to an artificial
plateau in the observed count rate. These plateau levels are statistically
observable, and it must be ensured that only data below saturation levels
are used. [](#fig-saturation) shows the fraction of observations of a given flux
level (of total observations) for one of the channels on the POLAR HISTe
instrument across a range of L-values. The high plateau in flux observed in
the :math:`L^*=4-5` region, in the center of the radiation belts, is a
saturation effect for this instrument. Such saturation limits are normally
only due to electronic processes and are normally stable in time. However,
some instruments such as the POLAR HIST instrument are thought to have a
variable dead time and thus a variable saturation limit, probably arising
from a dependence of upon the nature of the background causing saturation.

```{figure} images/saturation.png
:label: fig-saturation
:alt: Example of saturation in data

Fraction of observations of a given flux level (of total observations) for
the 678 KeV electron channel of POLAR HISTe showing the saturation
```

Thus the saturation value for a given channel can be determined from careful
scrutiny of the observations binned in flux and {math}`L^*`. After several
refinements the threshold flux value above which the instrument saturates
can be determined.

## Background

Background levels due to thermal noise or other contamination such as cosmic
rays are present in all particle instruments. These levels can be detected
by examining data during intervals when the spacecraft are outside the
trapping region for energetic electrons. For example, this occurs over the
polar cap on open field lines for LANL-GPS and during extreme magnetospheric
compression events for the geosynchronous region. These background counts or
fluxes must be detected and tracked over time and these counts or fluxes
must be subtracted before any use of the data. [](#fig-background) shows the
background levels detected for LANL-GPS NS18 in the first electron channel.
The LANL-GPS orbit routinely leaves the trapping region for energetic
electrons due to its inclined orbit, sampling field lines that are no longer
closed at large L-values. During those times only the background and noise
counts are observed. The background can be seen as the low count plateau at
the low cumulative probability values (GPS spends approximately 30% of its
time in open field line regions).

```{figure} images/background.png
:label: fig-background
:alt: Example of background levels in data

The cumulative probability of observed counts for GPS ns18, for a number of
passes through the outer radiation belt. Each colored curve shows for one
month the fraction of observations made at count levels below the maximum
count observed during that month.
```

This observed background level may also depend on time. As silicon detectors
or micro-channel plates age on-orbit, the background counting rates may
increase. The galactic cosmic ray background has a solar cycle variation.
[](#background_evolution) shows the variation of the background levels for
GPS NS18 from 1990 to the middle of 1994.

```{figure} images/background_variation.png
:label: background_evolution
:alt: Example of background levels varying

Variation of GPS ns18 background count levels over time (one point per month).
```

The background levels due to the GCR must be subtracted from the data. In
most cases this is a small correction, because the count rates due to
magnetospheric particles are generally much higher than the background
levels due to the GCR. This correction does become important where the count
rates are low. Because when count rates are very low the count rate itself
is subject to instrument discretisation, so the background value can
oscillate from one time to the next. So another safety factor must be used
to exclude any data that is within a factor of three of the average
background levels.

Thus, a monthly background value must be defined as the low count (or flux)
plateau at the low cumulative probability values. Then for safety, three
times the background level must be subtracted from the data and any
resulting negative values need to be considered as “bad data”.

## Signal to noise ratio

Depending on the geometric factor of the instrument, for some energy channel
the signal to noise ratio can be very low especially during quiet time or
close to the edge of the radiation belts. In this case the discretisation of
the instrument can be clearly seen. Such an example is shown in [](#snr)
where on the left hand side, the fraction of observations binned in flux and
{math}`L^*` is plotted. At low flux values each individual horizontal line
shows the discrete value available on the instrument. Of course this adds
errors on the flux value itself as shown on the right hand side of [](#snr)
where MDS-1 data (for {math}`L^*>5`) are plotted versus GOES data
(uncertainties in MDS-1 measurements at low flux values induce a scattered
plot when compared to GOES). Times when such uncertainties are recorded must
be removed.

```{figure} images/snr.png
:label: snr
:alt: Effects of signal to noise ratio

(left) fraction of observations from MDS-1 (22-44 MeV proton) binned in flux
and {math}`L^*` and (right) correlation between MDS-1 ({math}`L^* > 5`) and GOES data
```

## Spacecraft charging bias

For low energy measurements (i.e. cold plasma measurements) it is well known that the
proton and electron energy seen by the plasma instrument can be shifted because of the
absolute potential of the spacecraft. In the typical case, protons are accelerated whereas
electrons are decelerated because the spacecraft acquires a negative potential with respect to
the ambient plasma. To take this effect into account the spacecraft potential must be known.
This can be done by looking at the proton spectrum where the energy of the peak flux
(protons are accelerated to the same energy) gives the value of the spacecraft absolute voltage.
Then measurements done at electron and proton energy lower than the spacecraft absolute
voltage are not correct and must be removed, and the remaining spectrum needs to be shifted
by the spacecraft potential.

## Other problems

By plotting the data in many different ways, e.g. flux versus latitude-longitude or time line
plots, ..., some additional problem in the data may be found. It could be bad spacecraft
location, glitches, spikes ... An example is given in [](#spikes) where some spikes on GOES
data can be found. In this case an appropriate filter/editing must be defined.

```{figure} images/spikes.png
:label: spikes
:alt: Spikes in fluxes observations

GOES proton data showing spikes in the instrument response
```

# Obtaining coherent data

## Inter-calibration based on trapped particle dynamics

Given the limitations of each individual instrument’s calibration, further
on-orbit calibrations have to be resorted using the actual data collected.
For this the best available instrument calibrations, some simple
assumptions, a great deal of knowledge of the magnetospheric environment and
dynamics, and the basic physics of the transport processes for energetic
electrons can be used. Criteria that tell us when we can compare data
between instruments on two spacecraft, and how to propagate these
comparisons forward and backward in time have to be established. A “gold
standard” as a basic reference for on-orbit comparisons has to be defined.
Inter-calibration will then consist of a simple scaling of all other
instrument’s spectra to a “gold standard”. Inter-calibration is done using
only omnidirectional fluxes, since not all instruments yield pitch-angle
sorted fluxes and because use of such detailed data for this purpose would
be a prohibitively huge task. In case of unidirectional measurements, they
are integrated in pitch-angle before applying the following rules.

The basic assumption here is that each instrument measures a representation
of a spectrum independent of count rate or spectral hardness, and that the
simple scaling addresses uncertainties in each instrument’s effective
geometric factor and efficiency. It is known a- priori that some of these
assumptions are violated at times, and that they ultimately need to be
addressed in each instruments’ fundamental response function. However, in
the absence of such calibrations we have to start somewhere, and hope to
achieve a set of inter-calibration factors that are at least valid most of
the time. This is an on-going process - both to incorporate continuing new
data sets and to incorporate updated higher fidelity instrument calibrations
as they become available.


As mentioned before, the on-orbit calibration procedure relies on having a
“gold-standard” - a reference instrument that is trusted to perform the best
and cleanest measurements possible. In the case of electrons, the MEA
instrument on CRRES is chosen for energies between 300keV and 1.6 MeV, since
it was the last scientific instrument to fly in the equatorial region
covering the inner magnetosphere from L=1.2 to 7.5. MEA was a magnetic
spectrometer, measuring electrons in the range of 120 keV to 1.2 MeV. A
magnetic spectrometer used magnetic deflection to bend incoming electrons
onto a detector, which is an energy selective process.  Pulse height
analysis for the detectors provided a second energy discriminator, while a
background detector that electrons could not normally reach provided a good
determination of the penetrating background.

In case of protons, the SEM instrument on GOES-08 is chosen for energies
between 10 and 100 MeV. GOES proton data are now well known throughout the
world and this database is more or less considered as a standard. Corrected
differential channels are then used. As those measurements concern non
trapped protons (except P1) see next paragraph for how to start from there.

Next adjustment factors based on spacecraft conjunctions need to be found. A
strict definition of a spacecraft conjunction would be based solely on the
actual location of two spacecraft – defining a minimum distance between
them. Such a definition would however yield a very small or even zero number
of conjunctions for most spacecraft pairs. A more relaxed definition based
on the geomagnetic coordinates discussed in the previous section, and on
current knowledge of particle motion and magnetospheric activity can be
used. The aim is to obtain a statistically meaningful set of conjunctions
that enable the derivation of good adjustment factors between the two
instruments under investigation.

Referring to [](#conjunction) the following list of conditions defining a
"conjunction" are used:
1. {math}`L^* < 6` and {math}`\Delta L^* < 0.1`
2. {math}`\Delta (B/B_{eq}) < 0.1` and {math}`B/B_{eq}` as close as possible to one
3. Magnetic Local Time ($MLT$) within 2 hours of 06:00 and 18:00
4. Magnetospheric activity quiet (Kp < 2) for two days before conjunction
5. {math}`\Delta t < 3` hours
6. Particle energy > 100 keV (particle must be trapped)

```{figure} images/conjunction.png
:label: conjunction

Schematic showing the green region of “allowed” conjunctions. Corresponding
“X” indicate two possible conjunctions.
```

The first constraint is the most strict, requiring the measurements to be
made very close to the same drift shell. The limit on ∆(B/B eq ) restricts
conjunction such that both instruments can sample the same particle
distribution and :math:`B/B_{eq}~1` restricts conjunction to the geomagnetic equator,
ensuring that both instruments can sample the same full particle
distribution (all particles bouncing along a field line go through the
geomagnetic equator). The restriction in local time is due to the use of
model magnetic fields in obtaining the required model coordinate
{math}`L^*`, which performs best in these regions as this choice of location
excludes compression events around noon and substorm-related dynamics around
midnight. The low activity requirement allows one to relax the time
constraint on conjunctions, and to exploit the noon-midnight symmetry of
drift shells (it is possible to inter-calibrate instruments which are
positioned symmetric relative to the noon-midnight plane). During low
activity it is less likely to see loss or source events, and trapped
particle fluxes are generally uniform in MLT around a drift shell.
Furthermore, magnetic field models generally perform better during these
times as well, yielding better estimates of {math}`L^*`. The use of the
Olson-Pfitzer quiet model is more than enough.

Once a set of conjunctions has been found in this manner we find an
adjustment factor for each energy channel that matches the target
instrument’s data to our gold-standard. We collect all these adjustment
factors and find a statistical average factor across all conjunctions,
ignoring factors outside of one standard deviation.  The procedure described
here has been applied to LANL-GPS-CRRES, LANL-GEO-CRRES, LANL-GPS-POLAR,
LANL-GEO-POLAR and LANL-GPS-LANL-GPS conjunctions, but not to GEO-GEO which
never have conjunctions of this nature. For GEO-GEO a long-time average
comparisons has been used, since many missions are available in
statistically the same orbits ([](#mission_timeline)).

```{figure} images/missions_timeline.png
:label: mission_timeline

Example of mission time line for application of inter-calibration procedure in the case of
electron measurements.
```

## Inter-calibration based on SEP

To inter-calibrate high energy proton measurements (E>5 MeV) another
alternative is available. It is possible to correlate proton measurements
done by two different spacecraft when they are located outside of the
geomagnetic shielding ({math}`L^* > 7` is fine except for GEO where
{math}`L^* > 5.5` can be considered carefully – in this case the procedure
is safe for E> 10 MeV).  Under these conditions, during solar flares a good
correlation should be obtained.

Starting from the existing calibrated channels of the “gold standard” (flux)
the flux values at the “target” instrument channels can be evaluated by
interpolation assuming a power law spectrum. This can accommodate both
integral and differential “target” channels. These will then form the set of
equivalent channels for comparison from which a set of adjustment factors
will be found. The last step is to plot the data at the same time resolution
when they exceed the background level (i.e. only during flares). Note that
when GOES is used having 5 minutes time resolution works well.

Such an example is provided in [](#sep_correlation) where GOES data are interpolated
such as to reconstruct the same channel as the one of SAC-C. SAC-C (715 km
98°) data are filtered such as only measurements done when {math}`L^*` is
greater than 7 are kept. In a log-log plot the slope of the fit (in red on
the [](#sep_correlation)) which crosses the origin gives the inter-calibration
coefficient.

```{figure} images/sep_correlation_plot.png
:label: sep_correlation

Correlation between SAC-C proton 12.5-18.5 MeV with GOES-08.
```

A summary of the procedure is given in [](#calibration_summary).


```{figure} images/calibration_summary.png
:label: calibration_summary

Logic to cross-calibrate proton measurements based on solar flares.
```

## Inter-calibration of cold plasma data
To date there are no clear techniques developed to inter-calibrate cold
plasma data. Because of the low energy particle (E<50 keV) motion, the
definition of a conjunction must consider magnetic and electric fields. Most
difficulties come from the introduction of electric field for which there
are no good models in existence. In this context data must be considered as
it.

# Annex A : Trapped particle motion and their associated parameters

TODO

