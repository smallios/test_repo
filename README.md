<!----- Conversion time: 1.528 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* GD2md-html version 1.0β13
* Fri Dec 07 2018 08:27:52 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1CmRJ6JLkAdi7kQasbTpEZmxgk7TaVqeLxGKtyrJptSM
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server.
----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 1.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



# Description

This is a simple alignment and error checking protocol for asynchronous 16Gb/s links. The local clock is running at 240MHz and the link clock at 250 MHz. Crossing between clock domains and aligning the links are based on the insertion of a padding word (currently 7

The 16 Gbps links firmware is a lightweight, link-layer protocol that can be used to move \
data point-to-point across one or more high-speed serial lanes. It supports simplex operation with \
continues data transfer. The links are asynchronous, meaning that the main algorithmic logic is \
clocked with a lower frequency than the link clock, allowing more flexibility when choosing the \
logic clock. This is achieved by using asynchronous FIFOs in the receiving and transmitting sides. \
	To compensate for the difference of the frequency, padding words are being injected on the transmitting side and are stripped away on the receiving side. The link initialization and error handling are also based on the insertion and checking of those padding words. For testing purposes the local clock is running at 240 MHz and the link clock at 250 MHz. The link encoding is the 64b/66b encoding that transforms 64-bit data to 66-bit line code, to provide enough state changes to allow reasonable clock recovery and alignment of the data stream at the receiver [4]. The protocol overhead of 64b/66b encoding is 2 coding bits for every 64 payload bits or 3.125%. This makes the encoding considerably more efficient than the 25% overhead of the previously-used 8b/10b encoding scheme, which added 2 coding bits to every 8 payload bits.


## Asynchronous links structure :

TXDATA_IN → ADD CRCs→ TX FIFO → INJECT PADS & IDLES → MGT 

MGT → REMOVE PADS → RX BRAM → CRC CHECK → RX_DATA_OUT


## Link initialization and error handling

The link bring-up and error detection is based on the generic 2-bit 64b/66b encoding header, combined with the periodically sending of a padding word and CRC blocks. 

An illegal header value or a bad PAD/IDLE word is considered an error. Receiving >16 errors/64 bit words initiates the bit-alignment process by setting the gearbox_slip signal. This procedure is repeating until the links are aligned.

After the bit alignment is done the link_initialization_done indicator is set and the link error indicator is monitored. The link error indicator is not sensitive to single errors. Receiving however more than 2 errors per 64 words will force the indicator to go down and a re-initialization procedure to commence.


## Data quality

To ensure the quality of the data we periodically inject a CRC-32 checksum. A high-to-low transition of the data valid bit triggers the CRC word insertion before we cross to the link clock domain. On the receiving end, after crossing to the local clock domain, the CRC word is checked for errors. A CRC error indicator and a CRC 8-bit counter is implemented. After an error is received the CRC error indicator will remain set until we issue a reset error counter command.


## Data format 

The format of the special words (PADDING/IDLE/CRC) are in agreement with the Aurora protocol.  

<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/links-readme-md0.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/links-readme-md0.png "image_tooltip")



## INTERFACE PORTS/GENERICS description


<table>
  <tr>
   <td colspan="2" ><strong>GENERICS</strong>
   </td>
  </tr>
  <tr>
   <td><strong>LINK</strong>
   </td>
   <td>Choose synchronous or asynchronous operation. 
<p>
ASYNC : link clock is independent from the logic clock. Fifos are used to cross clock domains.
<p>
SYNC : logic clock derives from the link clock and has a fixed phase difference.
<p>
Always use ASYNC.
   </td>
  </tr>
  <tr>
   <td><strong>PATTERN</strong>
   </td>
   <td>Choose data source.
<p>
USER :  send incoming data.
<p>
PRBS:  send internal generated PRBS-31 data for BER tests.
   </td>
  </tr>
  <tr>
   <td><strong>DATA_WIDTH</strong>
   </td>
   <td>Dynamically controls the data width. Should be 64.
   </td>
  </tr>
  <tr>
   <td><strong>N_CHANNELS</strong>
   </td>
   <td>Number of channels per quad (1 to 4).
   </td>
  </tr>
  <tr>
   <td><strong>STABLE_CLOCK_PERIOD</strong>
   </td>
   <td>The 'stable' clock frequency in MHz. (used from initialization state machines)
   </td>
  </tr>
</table>



<table>
  <tr>
   <td colspan="2" ><strong>Input Ports</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ttc_clk_in</strong>
   </td>
   <td>TTC clock (multiple of 40MHz)
   </td>
  </tr>
  <tr>
   <td><strong>ttc_rst_in</strong>
   </td>
   <td>TTC reset
   </td>
  </tr>
  <tr>
   <td><strong>stable_clk_in</strong>
   </td>
   <td>The stable clock. Should be the ipbus clock.
   </td>
  </tr>
  <tr>
   <td><strong>top_mgtrefclk0</strong>
   </td>
   <td>Dedicated MGTs reference clock. Should derive from a gigabit transceiver input pad buffer component ( IBUFDS_GTE3 ).
   </td>
  </tr>
  <tr>
   <td><strong>txn/p_in</strong>
   </td>
   <td>Serial data ports for transceiver ch 0-3 (MGTs work fine without ports wired up).
   </td>
  </tr>
  <tr>
   <td><strong>txdata_in</strong>
   </td>
   <td>TX DATA input stream port. 
   </td>
  </tr>
  <tr>
   <td><strong>soft_reset_in</strong>
   </td>
   <td>reset links. SW controlled 
   </td>
  </tr>
  <tr>
   <td><strong>reset_error_counter_in</strong>
   </td>
   <td>Resets the CRC error counters and sets the RX BRAM read/write pointers to a fixed value.
   </td>
  </tr>
  <tr>
   <td><strong>loopback_mode_in</strong>
   </td>
   <td>Select the GTH loopback mode. (UG576 : Table 2-35,  p. 86)
   </td>
  </tr>
</table>



<table>
  <tr>
   <td colspan="2" ><strong>Output Ports</strong>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>crc_error_out</strong>
   </td>
   <td>Indicates a CRC error and will remain high  after a CRC error and until reset.
   </td>
  </tr>
  <tr>
   <td><strong>link_error_latched_out</strong>
   </td>
   <td>1 : indicates at least 1 error. \
0 : no link errors.
<p>
Sensitive to soft errors.
   </td>
  </tr>
  <tr>
   <td><strong>rxn/p_out</strong>
   </td>
   <td>Serial data ports for transceiver ch 0-3 (MGTs work fine without ports wired up).
   </td>
  </tr>
  <tr>
   <td><strong>rxdata_out</strong>
   </td>
   <td>ldata type. Data output from receiver. 
   </td>
  </tr>
  <tr>
   <td><strong>top_link_status</strong>
   </td>
   <td>1 : link  up. \
0 : link down. \
Not sensitive to soft (single) errors.
   </td>
  </tr>
  <tr>
   <td><strong>top_initialization_done</strong>
   </td>
   <td>This active-High signals indicate if the links are initialized and bit-aligned
   </td>
  </tr>
  <tr>
   <td><strong>reset_tx_done_out \
reset_rx_done_out</strong>
   </td>
   <td>This active-High signals indicate the GTH transceiver TX/RX has \
finished reset and is ready for use.
   </td>
  </tr>
  <tr>
   <td><strong>buffbypass_tx_done_out \
buffbypass_rx_done_out</strong>
   </td>
   <td>Active high TX/RX buffer bypass procedure done user indicators
   </td>
  </tr>
</table>


          \
 \
    \



<!-- GD2md-html version 1.0β13 -->
