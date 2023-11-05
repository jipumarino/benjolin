![plugdata Benjolin](https://github.com/jipumarino/benjolin/blob/master/plugdata_benjolin.png)

# Introduction

The Benjolin is a standalone synthesizer designed by Rob Hordijk from the Netherlands in 2009 and available as an open hardware project online. It contains two oscillators (one LFO and one VCO), a voltage controlled filter and a circuit called a Rungler, which allows chaotic cross-modulation possibilities between the different parts of the circuit. Hordijk refers to the Benjolin as a circuit which has been "bent by design."

The original Pure Data implementation of the Benjolin was coded by Derek Holzer between September and November 2016 in Helsinki, and it's available at https://github.com/macumbista/benjolin.

This implementation is an adaptation of Derek's to make it work with [plugdata](https://plugdata.org) v0.8.0, only using included libraries.

# Usage

Just open `benjolin-help.pd` in plugdata 0.8.0.

## Controls

- O1 FRQ: manual frequency control of Oscillator 1 (VCO)
- O1 RUN: amount of Rungler control voltage sent to Oscillator 1 (VCO)
- O2 FRQ: manual frequency control of Oscillator 2 (LFO/Clock)
- O2 RUN: amount of Rungler control voltage sent to Oscillator 2 (LFO/Clock)
- FLT FRQ: manual frequency control of the Voltage Controlled Filter
- FLT RES: manual resonance control of the Voltage Controlled Filter
- FLT RUN: amount of Rungler control voltage sent to the Voltage Controlled Filter
- FLT SWP: amount of Oscillator 2 triangle wave control voltage sent to the Voltage Controlled Filter

## Patchbay signal outputs

The matrix patchbay of the Pure Data Benjolin has 8 outputs, three control voltage (CV) inputs and a main output. Some combinations (having an oscillator modulate its own frequency) don’t make a lot of sense and have been marked with an "X", but are still possible to try if you wish.

- T1: Triangle wave output of Oscillator 1
- P1: Pulse wave output of Oscillator 1
- T1: Triangle wave output of Oscillator 2
- P1: Pulse wave output of Oscillator 2
- PWM: Pulse-width-modulated combination of Oscillator 1 and 2
- RUN: the Rungler produces a chaotic, stepped wave output which is clocked by Oscillator 2 (used as control information, so not anti-aliased!)
- XOR: The XOR (exclusive/or logic operation) of the pulse waves of Oscillators 1 and 2 (used as control information, so not anti-aliased!)
- FLT: output of the Voltage Controlled Filter

## Patchbay and external control inputs

- O1 CV: Control Voltage Input to Oscillator 1
- O2 CV: Control Voltage Input to Oscillator 2
- FLT CV: : Control Voltage Input to the Voltage Controlled Filter

# Known issues

- In order to see the OUT and RUN waveforms (OUT and RUN) you will need to open the benjolin.pd subpatch in the background (just having it open is enough, you can still interact with the instrument through `benjolin-help.pd`. This is an issue with plugdata with updating the display of oscope~ objects belonging to subpatches.

# Details

The Benjolin is a chaotic, internally sequenced generative instrument. Understanding how it works requires considering O1 (Oscillator 1) as the "VCO" (the "voice" oscillator), and O2 as the LFO (the "clock" oscillator). The Pulse Width Modulation (a comparison of the two Triangle waveforms from each oscillator) is then fed into a resonant "VCF" (a multi-mode filter), which can then be sent to the output. The various waveforms of different internal sections of the Benjolin can also be sent to either the main output of the instrument, or the various control inputs of the oscillators and filter, using the matrix patchbay in the upper left of the screen.

All the control information is produced by the Rungler (labelled RUN in the patch), which is a digital shift register followed by an "analog-to-digital" converter which takes the last three bits exiting the shift register and makes a value from them. The shift register is fed "data" from the Pulse wave of O1, and is clocked by the Pulse wave of O2. For every cycle of O2, a new bit of information enters the shift register, and the value of each cell in the shift register is passed on to the next. Normally, the last bit leaving the shift register makes an Exclusive-OR ("XOR" in the interface) operation on the incoming bit from O1, which allows for some chaotic nonlinear feedback in the Rungler and helps to produce self-similar but still emergent patterns. The LOOP switch defeats the incoming bits and recycles the bits leaving the shift register to make looped patterns possible.

The instrument can also bypass the internal oscillators and take an external audio signal either as the input of the filter, or the clock of the Rungler, using the switches in the upper right of the interface.

# Changelog

- 05/11/2023: first version for plugdata made available

November 2023
Juan Pumarino

