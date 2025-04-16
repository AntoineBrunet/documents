# Data analysis procedure

COSPAR - Panel on Radiation Belt Environment Modeling (PRBEM)

S. Bourdarie (ONERA - France), B. Blake (Aerospace Corporation – USA), J.B. Cao (CSSAR – China), R. Friedel (LANL – USA), Y. Miyoshi (STELAB – Japan), M. Panasyuk (MSU – Russia), C. Underwood (U. Of Surrey – UK)

## Introduction

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

## Context

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
uncertainties in an instrument's performance [^1]. Full modeling of
space radiation instruments is a time and computer-intensive procedure, and has only been
done for a few instruments (e.g. LANL-GPS, some LANL-GEO). These instrument-modeling
studies can also tell under what conditions an instrument will perform well, and when it might
be in principle impossible to recover a "clean" spectrum from an instrument.
Given the limitations of each individual instrument's pre-flight calibrations, further, on-orbit,
inter-comparisons using actual, collected data need to be performed.

[^1]: Cayton, T. E., and M. Tuszewski (2005), Improved electron fluxes from the synchronous orbit particle analyzer, Space Weather, 3, S11B05, doi:10.1029/2005SW000150.

## Coordinates to sort the data

A first step to go through before analyzing on-orbit measurements is to define appropriate
coordinates to sort the data. The basic positional information associated with any in-situ
measurements is the time and location in geographic coordinates. Because the trapped particle
motion is governed by the Earth's magnetic field (see annex A), magnetic coordinates have to
be computed to organize the data.

### Set of coordinates

Because the Earth magnetic field is not constant in time the best set of parameters to use are
the three adiabatic invariants J1, J2 and J3 (see annex A). Unfortunately this coordinate system
has some limitations: it is only appropriate for trapped particles and not for cold plasma data,
measurements are always done at constant energy (this means different J1 and J2 for each
measurements) and from the physics point of view it is easier to manipulate variables like
energy and pitch angle.

Because in a slowly varying magnetic field only K ($J_1-J_2$) and L* ($J_3$) are conserved along a
trapped particle motion, a good consensus is to adopt the following set of coordinates: $E$, $`\alpha^*_{eq}`$,
$L^*$, and MLT, where $E$ is the particle energy, $`\alpha^*_{eq}`$ is deduced from the equation
```math
\frac{K\sqrt{L^*}}{Re\sqrt{Bo}} = \frac{Y(y)}{y}
```
($`y=\sin(\alpha^*_{eq})`$ and $Y(y) = 2.760346 + 2.357194 y – 5.117540 y^{3/4}$), $L^*$ is the Roederer L
parameter and MLT the magnetic local time (crucial for non-trapped particles).

This set of parameters has twoadvantages; they may easily be computed from measurements,
and they are convenient to use for performing data assimilation in physical radiation belt
models. 

### Magnetic field model

An ideal magnetic field model is one which allows removal of adiabatic flux variations when
data are sorted according to J1, J2 and J3. Unfortunately, though there are many time
dependant magnetic field models available, none of them are realistic enough during very
disturbed periods to sort the data properly (one magnetic field model can be better at low L*,
another one better on the day side, etc …). Moreover for sophisticated, external magnetic field
models various input are required. For the simplest, magnetic indices are sufficient but for
some others solar-wind parameters are necessary. Both can be used. Nevertheless, if in-situ
radiation-belt particle data are analyzed in order to develop a radiation-belt model,
measurements from long duration satellites must be considered and solar wind parameters
may not always be available. Moreover, computing L* with such models over decades of
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

## Obtaining sanitized data

In the following sections the terms contamination and background are defined such as:
 - "contamination" = high level dynamic background due to non primary instrument response
 that can completely dominate an instruments response
 - "background" = low level more or less constant background due to electronic and detector
 noise and cosmic rays.

### Electron data contamination

#### Contamination by proton

It is well known that during times of solar energetic proton events (SEPs) many of the
detectors are contaminated with strong background counts. Figure 1 gives an example of a
period of SEP contamination. The SEP event is clearly visible in the elevated flux levels of
energetic protons measured at GOES 08. These ions can penetrate the electron detectors at 
LANL-GPS and LANL-GEO, leading to elevated electron flux observations, across all
sampled L-values, as clearly shown in Figure 1.

