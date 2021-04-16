# Eurorack Power Breakout

**Warning**: I am, perhaps, the furthest thing there is from an EE or CE. 
I have a rudimentary (at best) understanding of the physics involved in the electronics.
I mostly understand the following:

1. Give black rectangles too much power and \*pop\*.
2. Give spicy cylinders too much power and \*pop\*.
3. Send power the wrong way and something very expensive goes \*pop\*.
4. \*pop\* is bad.

I am typing this now as I've _already_ sent the first revision of these boards off to be made and am just now realizing that I probably should've included some diodes on the power lines from the rack power supply, but I haven't (see above).
When I get my rev0.1 boards back from the fab I'll make some updates, I'm sure.

The `eurorack_breakout` directory contains the KiCad project, schematic, and pcb files for this project.
I have not, as of yet, received my first rev of fabricated boards.
Please do not use this project unless you've got more knowledge than me and understand better why it works or why it's horribly broken and going to catch your rack power supply on fire, and then your apartment on fire, and then you on fire.

## Oops! All SMD

You'll notice everything is a SMD component, aside from the pin headers on either end of the PCB.
None of the package sizes are _too_ too difficult, so I'm confident you'll figure it out.
The two ceramic decoupling capacitors on the linear regulators (`C5` and `C6`) are the smallest package, and may be tricky.
Just be patient use tweezers, flux, and make sure your soldering iron is tinned and you're using a relatively-small-ish conical tip.
I used the hand-soldering extended footprints KiCad provides for each package, so every component should have enough room to get a soldering iron around.

## Optional Regulated Power

As is, the unpopulated board can be used to route 10- or 16-pin Eurorack power from a 2-by connector to a 1-by connector with one less ground. 
The Gate and CV lines on the 16-pin connector are also not routed anywhere (except between columns).
If you need these lines my recommendation is to simply not need them (I don't need them for my purposes so I decided not to route them, I don't know how many modules actually use them).
There are two solder jumpers on the board -- labelled `3V3` and `5V` -- to pass +12V from the Eurorack power through the LM1117-3.3 and LM1117-5.0 circuits, respectively.
If you're using 5V tolerant digital circuitry in your module and want to support just 16-pin connectors, you don't even need to populate the board.
The 3V3 circuit is comprised of all **odd-numbered** components (`C1`, `C3`, `U1`, etc.).
The 5V circuit is comprised of all **even-numbered** components.
This may be helpful for you to visualize the circuit on the board.

### Messing up

If you mess up, both halves of the circuit are identical (sans ferrite bead).
The silkscreen will be wrong, but there's not much stopping you from using the 3V3 side as the 5V side or vice-versa.
This might be helpful if you make a mistake while soldering.
If you have soldered some components on successfully before your mistake, I'd recommend cutting the jumper trace that corresponds to the circuit on which you've made your mistake.

### 3V3 Circuit

The 3.3V circuit consists of the following:

|Part Ref|Value|Type|
|:-:|--:|--:|
|`U1`|LM1117-3.3|Linear Regulator|
|`C1`|47uF|Aluminum Polymer Cap|
|`C3`|47uF|Aluminum Polymer Cap|
|`C5`|0.1uF|Ceramic Cap|
|`FB1`\*| -- |Ferrite Bead|

_\*The ferrite bead is not needed if you don't want/need a slightly cleaner 3.3V supply._
_The circuit will work without it populated, but pin 7 on the breakout-side connector will be NC._

If you don't need 3.3V power for your testing, cut the trace connecting the two halves of the jumper with a razor blade or hobby knife (carefully).
You can also not cut it, and just leave the entire portion of the board unpopulated.
Note that the Aluminum caps are polarized - the footprints and packaging should make it clear which side is which. 
All 4 caps have the same orientation.

### 5V Circuit

The 5V circuit consists of the following:

|Part Ref|Value|Type|
|:-:|--:|--:|
|`U2`|LM1117-5.0|Linear Regulator|
|`C2`|47uF|Aluminum Polymer Cap|
|`C4`|47uF|Aluminum Polymer Cap|
|`C6`|0.1uf|Ceramic Cap|

If you don't need 5V power for your testing, cut the trace connecting the two halves of the jumper with a razor blade or hobby knife (carefully).
The same applies to the 3V3 circuit - you can choose to not cut it as well. 
If the circuit is unpopulated nothing will change.
I would personally recommend cutting it only in the case that you mess up one half of it and still want to use the other.
