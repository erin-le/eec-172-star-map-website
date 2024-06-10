---
title: 'Real Time Star Chart'
author: '**Erin Le and Peggy Zhu** (website template by Ryan Tsang)'
date: '*EEC172 SQ24*'

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

<div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/project_overview_image.JPG" style="width:90%;height:auto;" />
      <span class="caption">Figure1: Project Overview</span>
    </div>
</div>

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
  Looking at the overall architecture, the system comprises 5 parts in total. The center of the system is the CC3200-LAUNCHXL microcontroller from Texas Instrument which connects all components together. The CC3200 is flashed with a C program that is in charge of coordinating the display on the OLED, reading and decoding input from the IR receiver and accelerometer, establishing a network connection with Amazon’s AWS for wireless communication, and performing calculations that are necessary for calibrating and displaying the star map.

  The 128x128 OLED displays the overhead star map in black-and-white. The accelerometer on the CC3200 keeps track of the user's movement and provides data for the calibration algorithm to rotate the star map on the OLED. The IR receiver takes user input from the AT&T remote control for calibration data and signal for taking screenshots. Amazon’s AWS includes a shadow for the CC3200 to receive the compressed screenshots from CC3200 and send them to the user’s email.


  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 400px;">
    <div class="fig">
      <img src="./media/system_architecture.JPG" style="width:90%;height:auto;" />
      <span class="caption"> Figure2: System Architecture</span>
    </div>
  </div>
</div>

### CC3200 LaunchPad Microcontroller
  <div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 400px;">
  The software tools used were Code Composer Studio (CCS) and the CCS UniFlash utility. Code Composer Studio was used to open example projects, modify example projects, write new code, and load projects onto the RAM of the CC3200. The CCS UniFlash utility was used to load our programs into the flash memory of the CC3200. 

  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 400px;">
    <div class="fig">
      <img src="./media/CC3200_board.JPG" style="width:90%;height:auto;" />
      <span class="caption">Figure3: CC3200 LaunchPad</span>
    </div>
  </div>
</div>

### AWS IoT Core

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 200px'>

  Amazon AWS provides various web services. To establish privacy and data security for communications between the CC3200 and Amazon AWS over the Internet, we used the Transport Layer Security (TLS) protocol to encrypt our data. In our lab, the protocol involves 2 keys and 2 ceritificates. We have a private key and a public key to decode the encrypted messages. We have a certificate and a root certificate to validate the keys.

  IoT is a network of physical objects, “things”  that is usually embedded with sensors, software, and other applications to connect and exchange data among devices over the Internet. In this lab, We create a virtual representation of a "thing", a “shadow”, which organizes the state of the CC3200 on Amazon's AWS. A shadow is helpful as it can handle requests and save the state of the device even when the CC3200 is offline. When the CC3200 is back online, it can pull the last known state from the shadow to update. When the shadow is updated, a series of actions, “Rules”, can be triggered. For example, we use a rule to send a screenshot of the OLED when the shadow successfully executes a POST request. The SNS is what we use to send the message to our email. 
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/amazon_aws_shadow_state.JPG" style="width:auto;height:2.5in" />
      <span class="caption">Figure4: Device Shadow JSON</span>
    </div>
  </div>
</div>

### SSD1351 OLED Display

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
  The SSD1351 comes with 128x128 RGB pixels, each pixel can be set with 16 bits of resolution for a large range of colors. the CC3200 communicates with the OLED display using the SPI protocol.
  
  SPI stands for Serial Peripheral Interface. It generally requires 4 wires for communication: COPI, CIPO, SCLK, and CS. The COPI (MOSI) line sends data from the controller to the peripheral. The CIPO (MISO) line received data from the peripheral to the controller. The SCLK line keeps track of the clock signal, and the CS line enables a peripheral for communication. 
  </div>
</div>

### On-board Accelerometer
<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div>
  <!-- style='display: inline-block; vertical-align: top;flex:1 0 400px' -->
  The BMA222 can report three 8-bit data values representing the acceleration measured in the x, y, and z directions. The microcontroller communicates with the BMA222 using the I2C protocol.

  I2C only requires 2 wires. I2C stands for Inter-Integrated Circuit. The SDA line transmits data bi-directionally. The SCK line keeps track of the clock signal. The SDA line goes down first before sending a message that claims the bus. After the master writes out a command, the slave sends out an ACK bit to confirm acknowledgment.
</div>

### Vishay TSOP311xx IR Receiver
<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div>
  The IR receiver comes with 3 pins: power, ground, and output signal. To reduce the noise on the receiver, we put a low-pass filter on the power line, consisting of a resistor and a capacitor.

  The encoding method used between the remote control and the sensor is the NEC code. This method uses 32 bits in total, with bits 0-7 for the address, 8-15 as the inverse of the address, 16-23 for data, and 24-31 as the inverse of the data. 0’s are encoded as shorter high pulses, and 1’s are encoded as longer high pulses. We can perform checksum using the original data and the inverse of the data to make sure the received data is correct.
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
      <img src="./media/functional_specification.JPG" style="width:90%;height:auto;" />
      <span class="caption">Figure5: State Diagram</span>
    </div>
  </div>
</div>

# Implementation
<p></p>

## Reading in User Input
<div>
For the initial step of receiving user input, we used code we had written from previous labs. This includes code such as the GPIO and SysTick interrupts for reading in IR sensor output, the code for registering these interrupts, and code for outputting messages to the OLED screen. The user input we received was the user’s location in longitude and latitude, the current date in days, months, and years, and the current time in hours and minutes. We did not implement a multitap system, so the user is only allowed to input numbers. 
Since longitude and latitude values can be negative, with latitude ranging from -90 to 90 and longitude from -180 to 180, we separate the inputs for longitude and latitude into two parts: one for the absolute value, the other for the sign. After receiving user input, the values are stored in global variables as floats.
</div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/prompt_time.jpg" style="width:90%;height:auto;" />
      <span class="caption">Figure6: Prompts For Date And Time</span>
    </div>
  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/prompt_latitude_longitude.jpg" style="width:90%;height:auto;" />
      <span class="caption">Figure7: Prompts For Latitude And Longitude</span>
    </div>
  </div>

## Loading in Star Coordinate Data
<div>
Next, the program loads the data for the star’s locations in the Equatorial Coordinate System. When running the program using CCS’s debug mode, we used a CSV file to store the data. However, for the final flashed program, we hardcoded the values as strings within the main file. 

Celestial coordinates in the Equatorial Coordinate System are expressed as right ascension (RA), which represents longitude, and declination (Dec), which represents latitude. Right ascension is often expressed using hours, minutes, and seconds, ranging from 0 to 24 hours. Declination is often expressed as degrees, arcminutes, and arcseconds, with an arcminute being 1/60th of a degree and an arcsecond being 1/60th of an arcminute. The range is from +90 to -90 degrees. These coordinates represent the stars’ position relative to a fixed location, so they do not change with a viewer’s location and current time.
In the program, the stars’ location is stored as six values, with hours, minutes, and seconds for right ascension, and degrees, arcminutes, and arcseconds for declination. 
</div>

## Calibration

### Calibration: Converting to decimal degrees
<div>
After reading and storing the values in a struct, the program converts the RA and Dec values from three separate values to one single value, expressed as a float. For RA, we convert the hour and minute values to seconds, divide this value by 24 hours in seconds, and multiply by 360 to convert to degrees. For Dec, we divide the arcminute value by 60 and the arcsecond value by 3600, then add the values to the degrees value.
</div>

### Calibration: Converting from RA and Dec to AZ and altitude
Next, we convert the RA and Dec values to azimuth (AZ) and altitude (Alt) values. These values represent what a viewer would see from a location at a particular time. Azimuth is analogous to RA and longitude, and altitude is analogous to latitude and Dec. In order to do this, the program calculates several intermediate values. The first of these intermediate values is the number of days, including hours as a fraction of a day, since J2000, which is noon on January 1st, 2000 in the GMT time zone. This was done using the time library in C.
Next, the program uses the following equation to approximate the local sidereal time:

```
LST = 100.46 + 0.985647 * days since J2000 + longitude + 15*time in GMT
```

where the number of days since J2000 is expressed as decimal days, longitude is in decimal degrees, and the current time in the GMT timezone is in decimal hours. If the outputted value is less than 0, the program adds 360 to obtain the local sidereal time. Next, we calculate the hour angle with the following formula:
```
HA =local sidereal time-RA
```
where local sidereal time is the value previously calculated, and RA is right ascension value in degrees. Following this, we can calculate the azimuth and altitude with the following equations:

```
ALT=asin(sin(Dec)*sin(Lat)+cos(Dec )*cos(Lat) * cos(HA))

A = acos ((sin(Dec)-sin(Alt)*sin(Lat)) / (cos(Alt)*cos(Lat)))

```
If sin(HA) is positive, then AZ =A. If it is negative, then AZ=360-A.

### Calibration: Converting from AZ and Alt to x-y coordinates

After we obtain altitude and azimuth values, we perform a stereographic map projection to convert from a 3D coordinate system to a 2D coordinate system in polar coordinates. The formulas used for this are:
```
radius=2 * radius of sphere * scale * tan((pi/4)-(altitude/2))

theta = azimuth
```
We used arbitrary values for the radius of the sphere and scale variables.
We then convert from polar into rectangular coordinates, which can be directly used by the OLED. The scale factor used in the radius equation was set so that the rectangular coordinates have a range greater than the OLED’s dimensions. 

<div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/Boots_OLED.jpg" style="width:auto;height:2in" />
      <span class="caption">Figure8: OLED Wiring Diagram</span>
    </div>
  </div>

<!-- <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/Boots_Phone.png" style="width:auto;height:2in" />
      <span class="caption">OLED Wiring Diagram</span>
    </div>
  </div> -->

## Rendering the star chart

We calculate which stars should be displayed on the OLED by keeping track of the position of the top left corner of the OLED, looping through each of the Star structs and calculating their positions relative to the OLED, then drawing it if it is within the bounds of the OLED’s screen. 

To update the position of the OLED screen, we read in x and y accelerometer readings. We update the position solely based on the last accelerometer reading, so the OLED displays exactly what is in the direction of the accelerometer tilt at any given time. 


## Taking a screenshot
To take a screenshot of the OLED, we first obtain the x-y coordinates of all the stars currently displayed on the screen. Then, we use a for loop with 128 iterations. In each iteration, we create a 128-byte character array to represent a row of the OLED display. We first fill in the 128-byte array with 0s to represent the background, then we check if the current row contains any stars based on the x-y coordinates. If there is, we replace the corresponding character in the array with a 1. Then, we feed this 128-byte character array into a function to convert it from binary form to hexadecimal form. The output of the conversion function is a 32-byte character array because each 4-byte binary is converted into a 1-byte hexadecimal character. After running through 128 iterations of the for loop, we have a 128x32 buffer in the form of a 4096-byte character array representing the compressed version of the OLED screenshot.
This compression step is necessary because the uncompressed message, 128x128 = 16384 bytes exceeds the limit to the size of a POST request when using AWS which is only 8192 bytes.

These values are then formatted as a POST request and sent to the shadow on Amazon’s AWS. AWS then forwards the message body as an email to the user’s email. The user can then copy and paste this data into a Python script, written and hosted on Replit, to decompress the data into its original form and display the data as a black-and-white image. The Python script would first decode the hexadecimal message back to its binary form, multiply the array in binary by 255, and then display the array as an image using the imshow() function from the matplotlib library as follows:
plt.imshow(final_output, cmap='gray', vmin=0, vmax=255).
This function would display a 128x128 pixel block, 0 in the final_output array represents a black pixel, and 255 in the final_output array represents a white pixel. The decoder can be accessed using the following link:
https://replit.com/@PassingBot/Starmapdecoder
The user would have to paste the contents of the email into the data.txt file, then run main.py.

<div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/email_message.JPG" style="width:90%;height:auto;" />
      <span class="caption">Figure9: Email Message</span>
    </div>
  </div>

<div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/python_output.JPG" style="width:90%;height:auto;" />
      <span class="caption">Figure10: Output of Python Script</span>
    </div>
  </div>

<!-- ### CC3200-LAUNCHXL Evaluation Board

The software tools used were Code Composer Studio (CCS) and the CCS UniFlash utility. Code Composer Studio was used to open example projects, modify example projects, write new code, and load projects onto the RAM of the CC3200. The CCS UniFlash utility was used to load our programs into the flash memory of the CC3200.  -->


<!-- 

## Functional Blocks: Master

### AWS IoT Core

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 200px'>

  Amazon AWS provides various web services. To establish privacy and data security for communications between the CC3200 and Amazon AWS over the Internet, we used the Transport Layer Security (TLS) protocol to encrypt our data. In our lab, the protocol involves 2 keys and 2 ceritificates. We have a private key and a public key to decode the encrypted messages. We have a certificate and a root certificate to validate the keys.

  IoT is a network of physical objects, “things”  that is usually embedded with sensors, software, and other applications to connect and exchange data among devices over the Internet. In this lab, We create a virtual representation of a "thing", a “shadow”, which organizes the state of the CC3200 on Amazon's AWS. A shadow is helpful as it can handle requests and save the state of the device even when the CC3200 is offline. When the CC3200 is back online, it can pull the last known state from the shadow to update. When the shadow is updated, a series of actions, “Rules”, can be triggered. For example, we use a rule to send a screenshot of the OLED when the shadow successfully executes a POST request. The SNS is what we use to send the message to our email. 
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/amazon_aws_shadow_state.JPG" style="width:auto;height:2.5in" />
      <span class="caption">Device Shadow JSON</span>
    </div>
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




# Challenges

One of the main challenges was figuring out how to perform the conversions between RA and Dec to AZ and Alt, then choosing a method and learning how to project from 3D celestial coordinates onto a 2D plane. Neither team member had done work with astronomy and celestial coordinates prior to this project, so we needed to spend a significant amount of time doing learning basic information and doing research on transformation methods. 

Another struggle was getting the program to function when flashed, as the star location data was stored in an external CSV, which we struggled to flash onto the board. We solved this by hard-coding the location data into the main C program on CC3200.

We also learned that space and processing power need to be carefully managed when working on a microcontroller project. For example, our program would not load when we tried to convert a 128x128 character array in binary form into a 128x64 character array in hex form using a for loop. The terminal would show a message indicating the processor is unable to allocate enough memory for this operation. To solve this problem, we reimplemented our conversion function to take in x-y coordinates of the stars instead of a 128x128 character array and convert the coordinates into a 128x64 character array directly. 

# Future Work

When the program is run in debug mode through CCS, we read data in from a CSV file that requires manual data entry. As this is an inefficient method of adding data, a future task would finding an existing database with the locations of stars and adapting our program to use that as an input method. 

Another change could also be reading in apparent magnitudes of stars and using them to calculate the radius of each star on the OLED. We could also implement another data structure for constellations and display constellation names on the OLED.



# Finalized Bill of Materials

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
    <td><p>TI CC3200-LAUNCHXL</p></td>
    <td><p> Microcontroller</p></td>
    <td><p>1</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$55.00</p></td>
    <td><p>$55.00</p></td>
    <td><p>Control Remote and Local Devices</p></td>
  </tr>
  <tr>
    <td><p>2</p></td>
    <td><p>Adafruit Breakout OLED Display</p></td>
    <td><p>128x128 RGB OLED Display. SPI protocol</p></td>
    <td><p>1</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$39.95</p></td>
    <td><p>$39.95</p></td>
    <td><p>Display prompts for user input, user inputted values, and star chart</p></td>
  </tr>
  <tr>
    <td><p>3</p></td>
    <td><p>Vishay TSOP311xx IR receiver</p></td>
    <td><p>IR receiver</p></td>
    <td><p>1</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$1.40</p></td>
    <td><p>$1.40</p></td>
    <td><p>For receiving input from AT&T remote control</p></td>
  </tr>
  <tr>
    <td><p>4</p></td>
    <td><p>AT&T IR U-verse remote control</p></td>
    <td><p>Remote control</p></td>
    <td><p>1</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$20.00</p></td>
    <td><p>$20.00</p></td>
    <td><p>For allowing the user to input longitude, latitude, time, date, and location values</p></td>
  </tr>
  <tr>
    <td><p>5</p></td>
    <td><p>Jumper cables</p></td>
    <td><p>Male to Female</p></td>
    <td><p>12</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$0.04</p></td>
    <td><p>$0.50</p></td>
    <td><p>For connecting the Adafruit OLED, MCU, and IR receiver</p></td>
  </tr>
  <tr>
    <td><p>6</p></td>
    <td><p>100 uF capacitor</p></td>
    <td><p></p></td>
    <td><p>1</p></td>
    <td><p>Provided by EEC172 Course</p></td>
    <td><p>$1.00</p></td>
    <td><p>$1.00</p></td>
    <td><p>To filter power supply noise for the IR receiver</p></td>
  </tr>
  <tr>
    <td><p>7</p></td>
    <td><p>1k resistor</p></td>
    <td><p></p></td>
    <td><p>1</p></td>
    <td><p>Provide by EEC172 Course</p></td>
    <td><p>$0.60</p></td>
    <td><p>$0.60</p></td>
    <td><p>To filter power supply noise for the IR receiver</p></td>
  </tr>

  <tr>
    <td colspan="3">
      <p>TOTAL PARTS</p></td>
    <td><p>18</p></td>
    <td colspan="2">
      <p>TOTAL</p></td>
    <td><p>$118.45</p></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="3">
      <p>TOTAL PARTS (Excluding Provided)</p></td>
    <td><p>0</p></td>
    <td colspan="2">
      <p>TOTAL (Exluding Provided)</p></td>
    <td><p>$0.00</p></td>
    <td></td>
  </tr>
</tbody>
</table>