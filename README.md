# Eurorack Power Breakout

**Warning**: I am, perhaps, the furthest thing there is from an EE or CE. 
I have a rudimentary (at best) understanding of the physics involved in the electronics.
I mostly understand the following:

1. Give black rectangles too much power and \*pop\*.
2. Give spicy cylinders too much power and \*pop\*.
3. Send power the wrong way and something very expensive goes \*pop\*.
4. \*pop\* is bad.

The `eurorack_breakout` directory contains the KiCad project, schematic, and pcb files for this project.

The first prototypes of the `norev` boards have performed well at what they're meant to do, and a new batch of the boards has been ordered with a few improvements (reverse-polarity protection diodes for the rack supply +12/-12/+5V rails, primarily).
I have some extras of both the `norev` and `rev0.3` boards, and would be happy to send you one if you reach out and cover shipping.

## Oops! All SMD

You'll notice everything is a SMD component, aside from the pin headers on either end of the PCB.
None of the package sizes are _too_ too difficult, so I'm confident you'll figure it out.
The two ceramic decoupling capacitors on the linear regulators (`C3` and `C6`), as well as the reverse polarity diodes (`D1-D3`) are the smallest packages, and may be tricky.
Just be patient use tweezers, flux, and make sure your soldering iron is tinned and you're using a relatively-small-ish conical tip.
I used the hand-soldering extended footprints KiCad provides for each package when possible, so every component should have enough room to get a soldering iron around.

## Optional Regulated Power

As is, the board (when populated with only the reverse-polarity protection diodes) can be used to route 10- or 16-pin Eurorack power from a 2-by connector to a 1-by connector with one less ground. 
The Gate and CV lines on the 16-pin connector are also not routed anywhere (except between columns).
If you need these lines my recommendation is to simply not need them (I don't need them for my purposes so I decided not to route them, I don't know how many modules actually use them).
There are two solder jumpers on the board -- `JP1` and `JP2` -- to pass +12V from the Eurorack power through the LM1117-3.3 and LM1117-5.0 circuits, respectively.
If you're using 5V tolerant digital circuitry in your module and want to support just 16-pin connectors, you don't even need to populate the board (sans diodes - those need to be populated for anything but ground be connected).

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
|`C2`|47uF|Aluminum Polymer Cap|
|`C3`|0.1uF|Ceramic Cap|
|`FB1`\*| -- |Ferrite Bead|

_\*The ferrite bead is not needed if you don't want/need a slightly cleaner 3.3V supply._
_The circuit will work without it populated, but pin 7 on the breakout-side connector will be NC. You can bridge the FB footprint, or pin 7 and 8 on the breakout-side connector directly if you would like 2 3V3 pins._

If you don't need 3.3V power for your testing, cut the trace connecting the two halves of the jumper with a razor blade or hobby knife (carefully).
You can also not cut it, and just leave the entire portion of the board unpopulated.
Note that the Aluminum caps are polarized - the footprints and packaging should make it clear which side is which. 
All 4 caps have the same orientation.

### 5V Circuit

The 5V circuit consists of the following:

|Part Ref|Value|Type|
|:-:|--:|--:|
|`U2`|LM1117-5.0|Linear Regulator|
|`C4`|47uF|Aluminum Polymer Cap|
|`C5`|47uF|Aluminum Polymer Cap|
|`C6`|0.1uf|Ceramic Cap|

If you don't need 5V power for your testing, cut the trace connecting the two halves of the jumper with a razor blade or hobby knife (carefully).
The same applies to the 3V3 circuit - you can choose to not cut it as well. 
If the circuit is unpopulated nothing will change.
I would personally recommend cutting it only in the case that you mess up one half of it and still want to use the other.

## BOM

|Part Ref|Value|Type|Link|
|:-:|--:|--:|:-:|
|`C1`, `C2`, `C4`, `C5`|47uF|Aluminum Polymer Cap|([digikey](https://www.digikey.com/en/products/detail/kemet/A768EB476M1VLAE042/13420538))|
|`C3`, `C6`|0.1uF|Ceramic Cap|([digikey](https://www.digikey.com/en/products/detail/w-rth-elektronik/885012208087/5454754))|
|`U1`|LM1117-3.3\*|Linear Regulator|([digikey](https://www.digikey.com/en/products/detail/on-semiconductor/LM1117MPX-33NOPB/13559953))|
|`U2`|LM1117-5.0\*|Linear Regulator|([digikey](https://www.digikey.com/en/products/detail/on-semiconductor/LM1117MPX-50NOPB/13559966))|
|`FB1`| -- |Ferrite Bead|([digikey](https://www.digikey.com/en/products/detail/w-rth-elektronik/742792312/1639577))|
|`D1-D3`|1N4819\*\*|Schottky Diode|([digikey](https://www.digikey.com/en/products/detail/diodes-incorporated/1N5819HW-7-F/814970))

_\*I bought these very cheaply._
_Notice that they're from ON Semiconductor rather than the more expensive LM1117-series from TI._
_As best I could tell, the principal difference is a slightly lower rejection ratio of 65dB, whereas the TI models report a PSRR of 75dB._
_My presumption here is that, while you can power your dev board/prototypes from your rack while it's fully on, you might want to have a test Eurorack supply for doing this dev work._
_Better yet, a benchtop supply (a Eurorack case w/ supply and a reasonble hobbyist benchtop supply run you about the same $$$, so I bought a case first)._
_This board might be slightly noisier with the ON Semiconductor regulators, but they're footprint compatible with the TI models if you are really worried about noise from the switching supply on your rack._

_\*\*I bought a few hundred of these a long time ago on `<insert Chinese Amazon here>` for another project, and used them here because I had them lying around._
_As of writing this, they're unavailable in the appropriate footprint on Digikey, but I imagine that they can be had on `<Chinese Amazon>` still - you may also find them on other electronic component supply shops._
_I won't link to a specific listing, since I prefer to source parts from places like Mouser or Digikey these days, but these can be had for about $1/lot of 100, if you're willing to wait a month or so for them to arrive in the mail `:^ )`_

## Board revision history

A quick rundown of the revision history of the board, mostly for my own sanity. 
Note that I'm tracking the schematic revision and board revision numbers separately. 
The schematic may be unchanged between board revisions.

### `norev`

My first boards were sent off to the fab without a revision marked on them because it was like 2AM when I uploaded them.
These boards have the wrong license shorthand on the silkscreen, and no test points.

### `rev01`

The `rev01` boards were the boards first uploaded to github with the correct CC license on the silkscreen, along with a revision number.
Front-side test points were added for the 3V3 and 5V traces.
I have not sent this board revision to be fabbed.

### `rev02`

The `rev02` boards differ from the previous revisions as follows:

- +12V and -12V lines are 0.4mm think, rather than 0.25mm as on previous revisions
- The info silkscreen in the top right hand corner was change to remove the line about the optional regulators - too much text
- The length of the board was reduced by 25 mils (should reduce the cost of the board a tiny amount)
- More vias were added to the copper pours used for heat dissipation for the linear regulators
- The positions of `C2` and `C3` were swapped, so that all **odd-numbered** components are on the upper half of the front of the board, and all **even-numbered** components are on the lower half (this should hopefully make populating only the 3V3 or 5V regulator circuits a bit more clear, as you'll only be soldering one half of the board).

I have not sent this board revision to be fabbed.

### `rev0.3`

The `rev0.3` boards differ from the previous revisions as follows:

- Reverse-polarity protection diodes for the rack +12V, -12V, and +5V lines (I recommend 1N5819 diodes, any reasonably tolerant schottky diode should work). These have an SOD-123 footprint, and are somewhat tricky to hand solder.
- OSHW Silkscreen and new `zzzzzzzz` graphic on back silkscreen/mask.
- Various silkscreen adjustments to move some component references out from underneath others
- Pad jumpers for the 3v3 and 5v enable lines are incorporated into the schematic and labelled as `JP1` and `JP2` respectively.
- Capacitor reference values have changed. `C1-C3` are the two aluminum and ceramic decoupling capacitor for the 3v3 circuit. `C4-C6` are the two aluminum and ceramic decoupling capacitor for the 5v circuit.
- The project licence has been switched to the CERN-OHL-P v2 license, and the silkscreen updated accordingly.

A panel of these boards has been sent off to the fab.
