# amazon-echo-dot-aux-speaker-switch
A circuit to detect the status of external speaker connected to the stereo jack of Amazon's Echo Dot and automatically switch on or off Echo Dot's built-in speaker.

## Problem to solve: 
Amazon Echo Dot has a 3.5mm stereo jack which allows it to hook up with some stereo system or amplifier with nice speakers.  However, once hooked up, the internal speaker is always disabled.  This can be a problem when the amplifier is powered off or switched to a different input.

## Desiging a simple detection circuit:
There are some considerations in designing such circuit:

First thing first.  The 3 mm stereo socket on Echo Dot has a mechanical switch which when the stereo jack is inserted, circuit become open.  This allows Echo Dot to sense whether a jack was inserted - when the circuit is open, Echo Dot will send audio signal to the external speaker; when the circuit is close, Echo Dot will use the internal speaker.  Since we want to leave the jack in the socket, we need something to close the circuit base on whether or not the stereo is on and the correct input is selected.

We need to then decide how to power our circuit.  There are three options to power the circuit - (a) having its own power source, or (b) draw power from the stereo/amplifier or (c) draw power from Echo Dot.  

The option of using a separate power might be the safest.  As we won't accenditally fried our nice stereo or Echo Dot.  However, it also means it requires one additional wall power outlet and more components to supply/regulate the voltage for the circuit.  We probably don't want to use the power from stereo/amplifier.  If we do that, then we cannot turn off the stereo/amplifier.  Otherwise, the circuit will stop working.  The conclusion: it is best to use Echo Dot to provide the power.

With the porwer source decided, we then need to consider what to use as an indicator for switching on/off the internal speaker.  Most of the stereo or amplifier has a display panel with input indicator.  We can use that to determine whether or not we need to close the circuit of Echo Dot's mechanical sensor switch.

The last thing is that we need to isolate the circuit of stereo's input indicator and the auto switch circuit.  Since the auto switch circuit uses Echo Dot's power, it will need to have the common ground with Echo Dot.  It will be more complicated if we don't separate those circuit or worse, it might cause damage to either the stereo/amplifier or Echo Dot or both.  

Here is the schematic of the design:

![External Speaker Auto Switch Schematic](images/AutoIntSpkSwitch-schematic.png)

I used 2 photocouplers (PC817) here - one to take input from the input indicator LED and the other to close the detector circuit.  The photocoupler is basically a tiny LED and a photo resistor in one package.  So we need to put a current limiting resistor to avoid burinig out the LED.  Normally the 220 Ohm registor will do.   However, notice the resistor for the STEREO_LED_IN is much higher (4.7K Ohm).  This is so that only enough current is diverted from the indicator to trigger the speaker switch but not too much that making the indicator too dim.  It might be necessary to try a few different resistor values for best result.

Another hurdle is that we need to open the circuit when the correct input indicator is on.  However, the photocoupler closes the circuit when the input is high - which is the opposite of what we need.  So I have to use a hex inverter (SN74HC04N) to reverse the signal.  There is a 10K pull down resister to ensure the logic 0/1 can be properly read by the hex inverter.

## Photos:

1. Wiring up with breadboard to make sure the circuit works
![](images/01_breadboard_wire_up.JPG)

2. Solder the wires for ground (black), +5V (red), mechanical sensor V-hi (green), V-lo (dark blue).  Note, the +5V is a bit hard to solder on.  It requires a lot of patience.  There are a lot of places provide ground.  Use whichever is easier to solder on.
![](images/02_echo_dot_wiring.JPG)

3. Remember to put the heat-shrink tube first before soldering the connectors.
![](images/03_echo_dot_reassembled.JPG)

4. Control panel of the stereo.
![](images/04_stereo_control_panel.JPG)

5. Close up view of the soldering places to the LED's on the control panel.
![](images/05_stereo_control_panel_close_up.JPG)

6. Tests the votage from the input indicator LED.
![](images/06_stereo_input_indicator_voltage.JPG)

7. Transfer the breadboard to a prototype board and soldering up everything.  Then wire it up to the stereo.
![](images/07_final_wire_up.JPG)

8. Close up view of the circuit on the prototype board.
![](images/08_close_up.JPG)

