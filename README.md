Spartan 3-AN Starter Kit Amp and ADC Controller
===============================================

This RTL module interfaces with the Linear Tech LTC6912-1 programmable gain
amplifier and LTC1407A-1 dual 14-bit ADC converter available on the
[Spartan 3-AN Starter Kit](http://www.xilinx.com/products/boards-and-kits/HW-SPAR3AN-SK-UNI-G.htm).
It is essentially a minimal SPI core that both programs the amplifier and
interfaces with the ADC for continuous capture.


## Using the Module

Interfacing with this module is very simple.  Hook up all of the appropriate
signals (see the comments in the file) and control it as follows.

1. Start by providing a reset pulse of some duration.  I found that 64 clock
   cycles at 50MHz worked well.  If you don't, the pre-amp hardware doesn't
   settle correctly.

2. When you are ready to program the gain, load the pre-amp gain into the
   `gain` bus, and then pulse `amp_cfg` for one clock cycle.

3. Wait for `amp_done` to go high.  Now you can begin sending ADC conversion
   requests.

4. To retrieve a sample from the ADC, pulse `start_conv` for one clock cycle
   and wait for `adc_done` to go high.  At this point, the signals `adc_a` and
   `adc_b` will be valid until the next conversion request.
