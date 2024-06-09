---
title: 'NutriSense: Automated Hydroponics Dosing System'
author: '**Kushagra Tiwari and Shengmin Liu** (website template by Ryan Tsang)'
date: '*EEC172 WQ24*'

# subtitle: '<blockquote><b>EEC172 Final Project Webpage Example</b><br/>
# Note to current students: this is an <i>example</i> webpage and
# may not fulfill all stated requirements of the current quarter''s 
# assignment.<br/>The website source is hosted 
# <a href="https://github.com/ucd-eec172/project-website-example">on github</a>.
# </blockquote>'

toc-title: 'Table of Contents'
abstract-title: '<h2>Description</h2>'
abstract: 'Stargazing is the practice of observing and identifying celestial objects in the night sky, often done for enjoyment.
However, beginner stargazers often struggle to locate the stars in the night sky. Our project assists stargazers with this by providing a live view of the sky and star location at a particular location and time.
The user can input their time, location (in longitude and latitude), and date to calibrate their view, then tilt the CC3200 microcontroller to see what is upward from a particular direction of the tilt direction. The user can also take a screenshot of the OLED display by pressing MUTE on their remote, which will send the compressed image data to email via Amazon Web Services. The user can then paste this data into a Python program which we implemented and hosted on Replit to decompress the data and convert it into a black and white image.
<br/><br/>
Our source code can be found 
<!-- replace this link -->
<a href="https://github.com/PaggieZ/EEC172_Star_Detector">
  here</a>.

Our data decompression and screenshot generation program can be found 

<a href="https://replit.com/@PassingBot/Starmapdecoder">
  here</a>.


# <div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;padding-top:20px">
#   <div style="display: inline-block; vertical-align: bottom;">
#     <img src="./media/Image_001.jpg" style="width:auto;height:2in"/>
#     <!-- <span class="caption"> </span> -->
#   </div>
#   <div style="display: inline-block; vertical-align: bottom;">
#     <img src="./media/Image_002.jpg" style="width:auto;height:2in" />
#     <!-- <span class="caption"> </span> -->
#   </div>
# </div>

<h2>Video Demo</h2>
<div style="text-align:center;margin:auto;max-width:560px">
  <div style="padding-bottom:56.25%;position:relative;height:0;">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/4CK4W5hTfbQ?si=0aQBXdd3ysT_JZh3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>
'
---

<!-- EDIT METADATA ABOVE FOR CONTENTS TO APPEAR ABOVE THE TABLE OF CONTENTS -->
<!-- ALL CONTENT THAT FOLLWOWS WILL APPEAR IN AND AFTER THE TABLE OF CONTENTS -->

# Market Survey

There are various stargazing apps available on the App Store that perform the same function as our program. There are also physical star maps that are available for purchase, which require some amount of calibration and knowledge to be used effectively. Our project offers an in-between option, as it does not require users to have a phone and does not use a GPS module, but still handles the calibration steps for the users after the receiving location, date, and time from the user.
<!-- 
<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style='display: inline-block; vertical-align: top;'>
    <img src="./media/Image_003.jpg" style="width:auto;height:200"/>
    <span class="caption">
      <a href="https://aerogarden.com/gardens/harvest-family/Harvest-2.0.html">AeroGarden Harvest 2.0</a>
      <ul style="text-align:left;">
      <li>Inexpensive ($90)</li>
      <li>Not Automated</li>
      <li>Small Scale</li>
      <li>No remote monitoring</li>
    </ul>
    </span>
  </div>
  <div style='display: inline-block; vertical-align: top;'>
    <img src="./media/Image_004.jpg" style="width:auto;height:200" />
    <span class="caption">
      <a href="https://www.hydroexperts.com.au/Autogrow-MultiGrow-Controller-All-In-One-Controller-8-Growing-Zones">Autogrow Multigrow</a>
      <ul style="text-align:left;">
      <li>Expensive ($4500)</li>
      <li>Fully Automated</li>
      <li>Huge Scale</li>
      <li>Cloud monitoring</li>
    </ul>
    </span>
  </div>
</div> -->

# Design

## System Architecture

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 400px;">
  

  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 400px;">
    <div class="fig">
      <img src="./media/Image_005.jpg" style="width:90%;height:auto;" />
      <span class="caption">System Flowchart</span>
    </div>
  </div>
</div>

## Functional Specification

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 300px;">
  First, the application displays prompts asking for the location in longitude and latitude, time, and date on the OLED. After each prompt, the user can input the information with the number keys (0-9) on the AT&T remote control. 

  Next, the application loads data for the stars’ locations. The data it reads includes the name of the star for identification, as well as its location in celestial coordinates. 

  After the program reads in the user input and celestial coordinate data, it performs various calculations to calibrate the location of the stars displayed on the OLED to the user’s location and inputted time. 

  Next, the OLED begins to display stars. The display continuously updates based on readings from the accelerometer on the CC3200 LaunchPad microcontroller. If the user wants to take a screenshot of what is displayed on the OLED, they can press the “MUTE” button of the remote control to send a compressed message from the CC3200 to email via AWS. After that, they can feed the compressed data into an external Python program that we implemented to convert it back into a black-and-white image.

  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/Image_006.jpg" style="width:90%;height:auto;" />
      <span class="caption">State Diagram</span>
    </div>
  </div>
</div>

# Implementation

### CC3200-LAUNCHXL Evaluation Board


## Functional Blocks: Master

### AWS IoT Core

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 200px'>

  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <!-- <div class="fig">
      <img src="./media/Image_007.jpg" style="width:auto;height:2.5in" />
      <span class="caption">Device Shadow JSON</span>
    </div> -->
  </div>
</div>

### OLED Display

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>

  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <!-- <div class="fig">
      <img src="./media/Image_008.jpg" style="width:auto;height:2in" />
      <span class="caption">OLED Wiring Diagram</span>
    </div> -->
  </div>
</div>

### IR Receiver

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>

  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <!-- <div class="fig">
      <img src="./media/Image_009.jpg" style="width:auto;height:2in" />
      <span class="caption">IR Receiver Wiring Diagram</span>
    </div> -->
  </div>
</div>


# Challenges




## Current Limitation


# Future Work

Given more time, we had the idea of developing a web app to allow users
to control the device from their cell phone. Another idea we wanted to
implement in the future is adding a grow light and pH controller to
maintain a more suitable and stable environment for different plants to
grow.


# Finalized BOM

<!-- you can convert google sheet cells to html for free using a converter
  like https://tabletomarkdown.com/convert-spreadsheet-to-html/ -->

<table style="border-collapse:collapse;">
<thead>
  <tr>
    <th><p>No.</p></th>
    <th><p>PART NAME</p></th>
    <th><p>DESCRIPTION</p></th>
    <th><p>Qty</p></th>
    <th><p>SUPPLIER / MANUFACTURER</p></th>
    <th><p>UNIT COST</p></th>
    <th><p>TOTAL PART COST</p></th>
    <th><p>Purpose</p></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><p>1</p></td>
    <td><p>CC3200-LAUNCHXL</p></td>
    <td><p>MCU Evaluation Board</p></td>
    <td><p>2</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$66.00</p></td>
    <td><p>$132.00</p></td>
    <td><p>Control Remote and Local Devices</p></td>
  </tr>
  <tr>
    <td><p>2</p></td>
    <td><p>Adafruit 1431 OLED</p></td>
    <td><p>128x128 RGB OLED Display. SPI protocol</p></td>
    <td><p>2</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$39.95</p></td>
    <td><p>$79.90</p></td>
    <td><p>Display PPM, Temperature, Thresholds, Inputs</p></td>
  </tr>
  <tr>
    <td><p>3</p></td>
    <td><p>Adafruit 4547 3VDC Pump</p></td>
    <td><p>Submersible pump. 3V 100mA DC</p></td>
    <td><p>2</p></td>
    <td><p>Adafruit</p></td>
    <td><p>$2.95</p></td>
    <td><p>$5.90</p></td>
    <td><p>For dispensing water and nutrient solution</p></td>
  </tr>
  <tr>
    <td><p>4</p></td>
    <td><p>Adafruit 4545 6mm Tube</p></td>
    <td><p>6mm Silicone Tube: 1 meter length</p></td>
    <td><p>1</p></td>
    <td><p>Adafruit</p></td>
    <td><p>$1.50</p></td>
    <td><p>$1.50</p></td>
    <td><p>For dispensing water and nutrient solution</p></td>
  </tr>
  <tr>
    <td><p>5</p></td>
    <td><p>NTC Thermistor 10k</p></td>
    <td><p>10k ohm nominal resistance, 100cm lead</p></td>
    <td><p>1</p></td>
    <td><p>(Already had one, available on Aliexpress)</p></td>
    <td><p>$0.92</p></td>
    <td><p>$0.92</p></td>
    <td><p>For temperature compensation</p></td>
  </tr>
  <tr>
    <td><p>6</p></td>
    <td><p>Adafruit AD1015 12-bit ADC</p></td>
    <td><p>12-bit resolution, 4 channels, I2C</p></td>
    <td><p>1</p></td>
    <td><p>Adafruit</p></td>
    <td><p>$9.95</p></td>
    <td><p>$9.95</p></td>
    <td><p>To convert Thermistor and TDS sensor reading to digital</p></td>
  </tr>
  <tr>
    <td><p>7</p></td>
    <td><p>PN2222A Transistor</p></td>
    <td><p>NPN BJT (40V, 1000mA)</p></td>
    <td><p>2</p></td>
    <td><p>Digikey (onSemi)</p></td>
    <td><p>$0.40</p></td>
    <td><p>$0.80</p></td>
    <td><p>For digital motor control</p></td>
  </tr>
  <tr>
    <td><p>8</p></td>
    <td><p>1N4001 Rectifier Diode</p></td>
    <td><p>Diffused junction: 50V 1000mA</p></td>
    <td><p>2</p></td>
    <td><p>Digikey (Good-Ark Semi)</p></td>
    <td><p>$0.16</p></td>
    <td><p>$0.32</p></td>
    <td><p>Reverse Current Protection</p></td>
  </tr>
  <tr>
    <td><p>9</p></td>
    <td><p>10k ohm resistor</p></td>
    <td><p>10k ohm , 1% tolerance, 0.25W</p></td>
    <td><p>1</p></td>
    <td><p>Digikey (Stackpole Electronics)</p></td>
    <td><p>$0.10</p></td>
    <td><p>$0.10</p></td>
    <td><p>Voltage divider for Thermistor</p></td>
  </tr>
  <tr>
    <td><p>10</p></td>
    <td><p>Vishay TSOP31130 IR RCVR</p></td>
    <td><p>30kHz carrier frequency</p></td>
    <td><p>2</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$1.41</p></td>
    <td><p>$2.82</p></td>
    <td><p>Decode user inputs</p></td>
  </tr>
  <tr>
    <td><p>11</p></td>
    <td><p>330 ohm resistor</p></td>
    <td><p>330 ohm resistor, &lt;5% tolerance, 3W</p></td>
    <td><p>2</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$0.59</p></td>
    <td><p>$1.18</p></td>
    <td><p>Current Limit for IR Receiv er</p></td>
  </tr>
  <tr>
    <td><p>12</p></td>
    <td><p>ATT-RC1534801 Remote</p></td>
    <td><p>General-purpose TV remote. IR NTC protocol</p></td>
    <td><p>1</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$9.99</p></td>
    <td><p>$9.99</p></td>
    <td><p>Allow user inputs</p></td>
  </tr>
  <tr>
    <td><p>13</p></td>
    <td><p>CQRSENTDS01 TDS Sensor</p></td>
    <td><p>Analog reading 0-2.3V. 0-1000ppm range</p></td>
    <td><p>1</p></td>
    <td><p>CQRobot</p></td>
    <td><p>$7.99</p></td>
    <td><p>$7.99</p></td>
    <td><p>Measure TDS of plant solution</p></td>
  </tr>
  <tr>
    <td><p>14</p></td>
    <td><p>10uF Capacitor</p></td>
    <td><p>Electrolytic Cap 100V</p></td>
    <td><p>2</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$0.18</p></td>
    <td><p>$0.36</p></td>
    <td><p>DC Filtering for IR Receiv er</p></td>
  </tr>
  <tr>
    <td><p>15</p></td>
    <td><p><u>AA Battery (4ct</u>)</p></td>
    <td><p>1.5 Volt, Non-rechargable</p></td>
    <td><p>1</p></td>
    <td><p>Already had, but</p>
      <p>available on Amazon</p></td>
    <td><p>$3.65</p></td>
    <td><p>$3.65</p></td>
    <td><p>Provide Power to Motors</p></td>
  </tr>
  <tr>
    <td><p>16</p></td>
    <td><p>Battery Holder</p></td>
    <td><p>2xAA (3 Volts Total)</p></td>
    <td><p>2</p></td>
    <td><p>Amazon</p></td>
    <td><p>$2.50</p></td>
    <td><p>$4.99</p></td>
    <td><p>Provide Power to Motors</p></td>
  </tr>
  <tr>
    <td colspan="3">
      <p>TOTAL PARTS</p></td>
    <td><p>25</p></td>
    <td colspan="2">
      <p>TOTAL</p></td>
    <td><p>$262.37</p></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="3">
      <p>TOTAL PARTS (Excluding Provided)</p></td>
    <td><p>14</p></td>
    <td colspan="2">
      <p>TOTAL (Exluding Provided)</p></td>
    <td><p>$36.12</p></td>
    <td></td>
  </tr>
</tbody>
</table>