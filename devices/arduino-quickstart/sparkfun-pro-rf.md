# Sparkfun Pro RF

![](../../.gitbook/assets/sparkfun_pro_rf%20%281%29.jpg)

## Introduction

This guide will show you step by step how to transmit LoRaWAN packets using a longfi-arduino sketch on a [Sparkfun Pro RF - LoRa](https://www.sparkfun.com/products/14916).

{% hint style="warning" %}
Before we begin, please make sure you've followed the steps from this [guide](https://developer.helium.com/console/quickstart), which goes over some initial steps to add your device to Console.
{% endhint %}

## Objective and Requirements

In this guide, you will learn:

* How to setup your environment
* How to program a basic application that will send packets over the Helium Network
* Verify real-time packets sent to the Helium Console via Hotspot that's in range

For this example, you will need the following:

### Hardware

* [Sparkfun Pro RF - LoRa](https://www.sparkfun.com/products/14916)
* Micro USB Type B Cable - [Example](https://www.amazon.com/AmazonBasics-Male-Micro-Cable-Black/dp/B0719H12WD/ref=sr_1_2_sspa?)
* Soldering Iron
* Small gauge wire

### Software

* [Arduino software \(IDE\)](https://www.arduino.cc/en/Main/Software) 
* [Helium Console](https://console.helium.com/) 

## Hardware Setup

This step will cover adding an antenna and closing the LoRaWAN jumpers on this board. 

### Adding the Antenna

You have two options for adding an antenna on this board.

**U.FL Antenna:**  Connect any 915Mhz compatible antenna, for instance, [this ](https://www.mouser.com/ProductDetail/Anaren/66089-0906?qs=sGAEpiMZZMuBTKBKvsBmlGlvlFGR4gdSgkIgGKwINqg%3D)small, low cost antenna, to the U.FL port shown in the image below.

![](../../.gitbook/assets/sparkfun_pro_rf_antenna_ufl.jpg)

**Wire Antenna:** Solder a short wire\(78mm for Americas\) to the pin-out as shown below.

![](../../.gitbook/assets/sparkfun_pro_rf_antenna_wire.jpg)

### Closing the LoRaWAN Jumpers

In order to fully communicate with the LoRa radio transceiver on the board, you'll need to close two jumpers on the underside shown below.

![](../../.gitbook/assets/sparkfun_pro_rf_lorawan_jumpers.jpg)

## Software Setup

### Getting the Arduino IDE

Download and install the latest version of [Arduino IDE](https://www.arduino.cc/en/Main/Software) for your preferred OS.

* [Windows](https://www.arduino.cc/en/Guide/Windows)
* [Linux](https://www.arduino.cc/en/Guide/linux)
* [Mac OSX](https://www.arduino.cc/en/Guide/MacOSX)

### Arduino Board Support

The Sparkfun Pro RF requires two Arduino board support packages. Follow the instructions below to install both.

#### SAMD Boards \(32-bits ARM Cortex-M0+\)

To install, open your Arduino IDE:

1. Navigate to \(**Tools &gt; Boards &gt; Boards Manager...\)**
2. Search for **SAMD Boards \(32-bits ARM Cortex-M0+\)**
3. Select the newest version and click Install

 **SparkFun SAMD Boards**

To install, open your Arduino IDE:

1. Navigate to **\(File &gt; Preferences\)**
2. Find the section at the bottom called **Additional Boards Manager URLs:**

![](../../.gitbook/assets/arduino-board-add-sparkfun.png)

3.  Add this URL in the text box:

```text
https://raw.githubusercontent.com/sparkfun/Arduino_Boards/master/IDE_Board_Manager/package_sparkfun_index.json
```

4. Close the Preferences windows

Next, to install this board support package:

1. Navigate to \(**Tools &gt; Boards &gt; Boards Manager...\)**
2. Search for  **SparkFun SAMD Boards**
3. Select the newest version and click Install

### Arduino Library

To communicate with Helium's LoRaWAN network, we'll need to install an Arduino library.

To install, open your Arduino IDE:

1. Navigate to Library Manager \(**Sketch &gt; Include Library &gt; Manage Libraries**\).
2.  In the search box, type **IBM LMIC framework** into the search, select the version shown below, and click Install.

![](../../.gitbook/assets/sparkfun_pro_rf_library.png)

### Programming **Example Sketch**

Now that we have the required Arduino board support and library installed, lets program the board with the provided example sketch.

To create a new Arduino sketch, open your Arduino IDE, \(**File &gt; New\).** Next, replace the template sketch with the sketch found [here](https://raw.githubusercontent.com/helium/longfi-arduino/master/sparkfun-pro-rf/longfi-us915.ino), copy and paste the entirety of it. 

Next we'll need to fill in the AppEUI, DevEUI, and AppKey, in the sketch, which you can find on the device details page on Console. Be sure to use the formatting buttons to match the endianess  and formatting required for the sketch, shown below.

![](../../.gitbook/assets/sparkfun_pro_rf_console.png)

At the top of the sketch, replace the three **FILL\_ME\_IN** fields, with the matching field from Console, example shown below.

![](../../.gitbook/assets/sparkfun_pro_rf_sketch_keys.png)

### Selecting Board

Next, we need to select the correct board to build for in the Arduion IDE.  Navigate to  \(**Select Tools &gt; Board: &gt; SparkFun SAMD21 Pro RF\).**

### Enter Bootloader Mode

Next, we need to place the board into bootloader mode, which allows us to update it with new firmware. To do this, first plug the device into your computer, make sure the power switch is turned on \(you should see a red LED on\).  Next, quickly double click the reset push button on the side of the board, you should see a blue LED come on now too. If you see this then you have successfully entered bootloader mode.

### Selecting Port

We're almost ready to upload our sketch, the very last step is to select the correct Serial port in the Arduino IDE. Navigate to \(**Tools &gt; Port: Sparkfun SAMD21 Pro RF**\). You will also see either **COM\# or /dev/ttyACM\#** depending on whether you are on Windows, Mac, or Linux. If you do not see a port, please visit the Drivers section in [Sparkfun's guide](https://learn.sparkfun.com/tutorials/sparkfun-samd21-pro-rf-hookup-guide?_ga=2.148378999.1172134851.1586114454-289367592.1582349414&_gac=1.242421430.1585837307.EAIaIQobChMI86GEgfjJ6AIVBQF9Ch0mpwyeEAEYASAAEgLFn_D_BwE#hardware-overview) to make sure you have what's needed for your operating system. 

### Upload Sketch

We're finally ready to upload our sketch to the board. In the Arduino IDE, click the right arrow button, or navigate to  \(**Sketch &gt; Upload\),** to build and upload your new firmware to the board.  You should see something similar to the image below at the bottom of your Arduino IDE, when the upload is successful.

![](../../.gitbook/assets/sparkfun_pro_rf_upload.png)

### Viewing Serial Output

When your firmware update completes, the board will reset, and begin by joining the network. Let's use the Serial Monitor in the Arduino IDE to view the output from the board. We first need to select the serial port again, but this time it will be a **different port** than the one we selected to communicate with the bootloader. Once again navigate to \(**Tools &gt; Port: Sparkfun SAMD21 Pro RF**\), but make sure the serial device, either COM\# or ttyACM\# is different! Next navigate to \(**Tools &gt; Serial Monitor**\), you should begin to see output similar to below.

![](../../.gitbook/assets/sparkfun_pro_rf_console_terminal.png)

Now let's head back to [Helium Console](https://console.helium.com) and look at our device page, you should see something similar to the screenshot below.

![](../../.gitbook/assets/sparkfun_pro_rf_console_data.png)

Congratulations! You have just transmitted data on the Helium network! The next step is to learn how to use your device data to build applications, visit our Integrations docs [here](../../console/integrations/).
