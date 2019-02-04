## Description
This is a preamp board for capacitive transducers.  It
implements a simple AGC with a BJT.  I use this as a hydrophone
preamp, and I'm generally pleased with how it works.  You could
use it in a bat detector if you want.

_NOTE:_ This circuit is not suitable for audio use, as the self
noise is higher than expected for reasons I have not yet
discovered.  Also, the filter component values would need to be changed.

## Parking sensors
This preamp was originally developed for car
reverse parking sensors pressed into service as hydrophones.
Such sensors can be had for a few dollars each, such as here:
https://www.aliexpress.com/item/Parking-Assistance-4-Pcs-22mm-Parking-Sensor-Probe-Black-Silver-Blue-Red-White-Car-Parking-Reverse/32405916393.html
I've used them underwater to a depth of a couple of meters (I'm
sure they can go much deeper) with no issues.  Often the sensors come
in a package with a parking controller, but you will only need the
sensors.

These sensors have a JST-XH2 connector, the mating header to which
can be soldered to this board.  Note the polarity: ground (braid)
is on JST-XH pin 1 (with connector pointing downwards, key towards
you, pin 1 is the rightmost pin).

In air, the parking sensors are resonant at 40kHz, but this
drops to around 36kHz in water.

## Design files
piezo-agc-preamp.sch was made by gschem, and piezo-agc-preamp.pcb
was made by pcb, both part of the gEDA package (does not run
under Windows).  If all you want are the Gerbers (in gerbers/),
you won't need to bother with gEDA.  The schematics are copied
as a PDF.

## Using the preamp
The preamp board pins are laid out on a 100-mil spacing, so you
can test it on a bread board.  All the components are on one
side so you could solder the entire module on to another board.

There is no reverse protection, so check polarity before
applying power!

In order to enable AGC action (recommended), T8 (signal output)
and T10 must be tied together.  You can interpose your own feedback
circuit between T8 and T10 to control the gain.  If you want to
reduce the AGC attack time constant, be careful not to reduce R16
too much, as this causes instability.  The AGC maintains the output
at around 1Vp-p.

## Operation
AGC voltage biases Q1.  Without a signal, AGC voltage is close
to Vcc, and Q1 amplifies (modestly).  As AGC voltage decreases,
Q1 gets pushed into cutoff.  This kind of reverse AGC used to be
quite common.

The signal then passes to the first op amp which is configured as
a multi feedback band pass filter (2nd order), then to the second
op amp (the gain stage).  This also implements band pass filtering
but it isn't particularly good at it.  This output (at T8) will
normally be connected to T10.

If the output from the second op amp is too high, it turns on Q2
which lowers the AGC voltage.  In this way, the AGC feedback
loop is closed.

## Modifying the preamp
The component values shown in the schematic are for a preamp
with band pass filter centre frequency at 36kHz and Vcc of 5V.

You can change the filter component values to tune a different band.
R1, R2, R3, C2, and C3 constitute an MFB BPF; you can use this
calculator for different component values here:
http://sim.okawa-denshi.jp/en/OPttool.php

If you do this, also change HPF R8/C5 and LPF R9/C11, or
eliminate C11 and make C5 a large value.

If you change Vcc, make sure to change R4 such that the Q1 base
quiescent voltage remains close to 0.7V.

For audio use you can also change the filter from BP to LP.  In
that case R2 and R3 will become caps, and C2 and C3 become
resistors.  The parking sensors mentioned above are useable in
the audio range but you can expect some instability.

Instead of BC846, you can use any other small signal NPN BJT.
If you have access to e.g. a 12V rail, you can use an NE5532
instead of the TL972.  This may reduce noise.  You'll have to
rebias Q1 (see above).

## Thanks
Questions and comments to htarold@gmail.com
Mon 31 Jul 12:13:59 SGT 2017
