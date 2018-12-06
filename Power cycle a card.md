Power cycle a card

**From cern network :**

<table>
  <tr>
    <td>ssh -t -L 2233:localhost:2233 smallios@cmsusr ssh -t -Y -L 2233:localhost:22 smallios@ctrl-s2c16-21-01.cms</td>
  </tr>
</table>


**From outside : **

<table>
  <tr>
    <td>ssh smallios@lxpus.cern.chssh cmsusrssh  ctrl-s2c16-21-01.cms</td>
  </tr>
</table>


**Connect to mch : **

Top :

<table>
  <tr>
    <td>telnet mch-s1d03-17-01</td>
  </tr>
</table>


Bottom :

<table>
  <tr>
    <td>telnet mch-s1d03-17-01</td>
  </tr>
</table>


MCH commands :

> show_fru

> shutdown <fru_id>

> fru_start <fru_id>

**Top crate - wedges 1-6** used FED: 1376

<table>
  <tr>
    <td>MCH</td>
    <td>mch-s1d03-17-01</td>
  </tr>
</table>


<table>
  <tr>
    <td>Wedge</td>
    <td>01</td>
    <td>02</td>
    <td>03</td>
    <td>04</td>
    <td>05</td>
    <td>06</td>
  </tr>
  <tr>
    <td>AMC</td>
    <td>01</td>
    <td>03</td>
    <td>05</td>
    <td>07</td>
    <td>09</td>
    <td>11</td>
  </tr>
  <tr>
    <td>FRU</td>
    <td>5</td>
    <td>7</td>
    <td>9</td>
    <td>11</td>
    <td>13</td>
    <td>15</td>
  </tr>
</table>


**Bottom crate - wedges 7-12** used FED: 1377

<table>
  <tr>
    <td>MCH</td>
    <td>mch-s1d03-09-01</td>
  </tr>
</table>


<table>
  <tr>
    <td>Wedge</td>
    <td>07</td>
    <td>08</td>
    <td>09</td>
    <td>10</td>
    <td>11</td>
    <td>12</td>
  </tr>
  <tr>
    <td>Slot</td>
    <td>02</td>
    <td>04</td>
    <td>06</td>
    <td>08</td>
    <td>10</td>
    <td>12</td>
  </tr>
  <tr>
    <td>FRU</td>
    <td>6</td>
    <td>8</td>
    <td>10</td>
    <td>12</td>
    <td>14</td>
    <td>16</td>
  </tr>
</table>


Configure cards with mp7butler.py

**From cern network :**

   ssh -t -L 2233:localhost:2233 smallios@cmsusr ssh -t -Y -L 2233:localhost:22 smallios@srv-s2g16-34-01.cms

**From outside : **

<table>
  <tr>
    <td>ssh smallios@lxpus.cern.chssh cmsusrssh  srv-s2g16-34-01.cms</td>
  </tr>
</table>


**Set the enviroment :**

<table>
  <tr>
    <td>cd mp7_sw/mp7/tests
source env.sh</td>
  </tr>
</table>


**butler basic commands :**

<table>
  <tr>
    <td>mp7butler.py showmp7butler.py connect bmtf_w1 (bmtf_w1 → bmtf_w12)mp7butler.py scansd bmtf_w1mp7butler.py rebootfpga bmtf_w1 <firmware_name.bin>mp7butler.py reset bmtf_w1</td>
  </tr>
</table>


SWATCH Cell Manipulation

Once logged in the machine "l1ts-bmtf" under the “cmsusr” network.BMTF Cell is being handled by a system service, its name is “**trigger.bmtf-cell@srv-s2g16-34-01**”

  

<table>
  <tr>
    <td> sudo service trigger.bmtf-cell@srv-s2g16-34-01 status</td>
  </tr>
</table>




To check the service status, if sudo used one can check also the log file.In there is being dumped the log output which exists also in the cell ControlPanels->Logging tab.

If the cell is stuck, the service is not responding. To restart it one needs to restart the service.

<table>
  <tr>
    <td>sudo service trigger.bmtf-cell@srv-s2g16-34-01 stopsudo service trigger.bmtf-cell@srv-s2g16-34-01 start###---- OR ----sudo service trigger.bmtf-cell@srv-s2g16-34-01 restart  # in once</td>
  </tr>
</table>


In case of a stuck cell, the logging from the SWATCH page is not accessible.One can check the log file "/var/log/trigger.bmtf-cell.log". An example of checking the wedge06:

<table>
  <tr>
    <td>   grep wedge06 /var/log/trigger.bmtf-cell.log   ## --- OUTPUT EXAMPLE of succeed transition to start   INFO  bmtf.wedge06 <> - Finished action 'align'   INFO  bmtf.wedge06 <> - Starting action 'start' (adopted)   INFO  bmtf.wedge06 <> - Starting transition 'start' within action 'start'   INFO  bmtf.wedge06 <> - Finished transition 'start'. Entering state 'Running'   INFO  bmtf.wedge06 <> - Finished action 'start'</td>
  </tr>
</table>


The long command which follows report the last 3 lines in the log file for each board:

<table>
  <tr>
    <td>for board in {01..12}; do echo "----> LOG for board-$board "; grep wedge$board /var/log/trigger.bmtf-cell.log | tail -3; done</td>
  </tr>
</table>


##--- ITS OUTPUT is like what follows

----> LOG for board-01

22 Nov 2018 18:05:20.423 [140094484756224] INFO  bmtf.wedge01 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.426 [140094518327040] INFO  bmtf.wedge01 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge01 <> - Finished action 'stop'

----> LOG for board-02

22 Nov 2018 18:05:20.432 [140094484756224] INFO  bmtf.wedge02 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.435 [140094954518272] INFO  bmtf.wedge02 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge02 <> - Finished action 'stop'

----> LOG for board-03

22 Nov 2018 18:05:20.442 [140094484756224] INFO  bmtf.wedge03 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.445 [140094979696384] INFO  bmtf.wedge03 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge03 <> - Finished action 'stop'

----> LOG for board-04

22 Nov 2018 18:05:20.451 [140094484756224] INFO  bmtf.wedge04 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.454 [140092505061120] INFO  bmtf.wedge04 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge04 <> - Finished action 'stop'

----> LOG for board-05

22 Nov 2018 18:05:20.461 [140094484756224] INFO  bmtf.wedge05 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.464 [140092991575808] INFO  bmtf.wedge05 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge05 <> - Finished action 'stop'

----> LOG for board-06

22 Nov 2018 18:05:20.471 [140094484756224] INFO  bmtf.wedge06 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.473 [140094509934336] INFO  bmtf.wedge06 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge06 <> - Finished action 'stop'

----> LOG for board-07

22 Nov 2018 18:05:20.363 [140094484756224] INFO  bmtf.wedge07 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.369 [140094971303680] INFO  bmtf.wedge07 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge07 <> - Finished action 'stop'

----> LOG for board-08

22 Nov 2018 18:05:20.375 [140094484756224] INFO  bmtf.wedge08 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.378 [140094962910976] INFO  bmtf.wedge08 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge08 <> - Finished action 'stop'

----> LOG for board-09

22 Nov 2018 18:05:20.385 [140094484756224] INFO  bmtf.wedge09 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.388 [140094501541632] INFO  bmtf.wedge09 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge09 <> - Finished action 'stop'

----> LOG for board-10

22 Nov 2018 18:05:20.394 [140094484756224] INFO  bmtf.wedge10 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.397 [140094946125568] INFO  bmtf.wedge10 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge10 <> - Finished action 'stop'

----> LOG for board-11

22 Nov 2018 18:05:20.404 [140094484756224] INFO  bmtf.wedge11 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.407 [140094937732864] INFO  bmtf.wedge11 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge11 <> - Finished action 'stop'

----> LOG for board-12

22 Nov 2018 18:05:20.413 [140094484756224] INFO  bmtf.wedge12 <> - Starting transition 'stop' within action 'stop'

22 Nov 2018 18:05:20.416 [140094493148928] INFO  bmtf.wedge12 <> - Finished transition 'stop'. Entering state 'Configured'

22 Nov 2018 18:05:20.474 [140094484756224] INFO  bmtf.wedge12 <> - Finished action 'stop'

Special case, a "--full-restart" argument exists for the services. Using it the scripts will run twice simulating the initialization that is being done during the boot up. For example, the following command will cause the scripts to run once with stop and once with start, causing boot up simulation.

<table>
  <tr>
    <td>sudo service trigger.bmtf-cell@srv-s2g16-34-01 --full-restart # I didn't need this one yet</td>
  </tr>
</table>


R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=47 SET_FRU_LED_STATE_REQ

AMC1(5): WARNING: entering Communication lost state !

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=10 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=35 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=6 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=39 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=10 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=43 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=14 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=47 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=18 GET_DEVICE_ID_REQ

R(5,1,1)R(5,1,2)ipmi_SendFru(5): timeout - no response for REQ: 0x20->0x72, Seq=53 GET_DEVICE_ID_REQ

AMC1(5): Communication regained !

AMC1(5): Handle=0x02 - opened

ekey_DisableAMCPorts(5): disabling all ports

