# Zigbee CC2531 USB-Dongle
This repository provides detailed instructions on how to use the CC2531 USB dongle as a Zigbee coordinator or for other Zigbee applications. It covers flashing the dongle with the latest firmware using a Raspberry Pi (64-bit architecture), setting up Zigbee2MQTT or Home Assistant, and integrating Zigbee devices into your home automation network.


# INTRODUCTION

## Hardware Discription

The CC2531 is a low-cost, low-power 2.4-GHz IEEE 802.15.4 USB dongle from Texas Instruments, commonly used for Zigbee communication. It is equipped with an onboard antenna and a USB interface, making it an ideal choice for applications such as home automation, smart lighting, and wireless sensor networks. It acts as a Zigbee coordinator, allowing it to control and communicate with Zigbee devices like sensors, switches, and lights.

The CC2531 dongle typically comes with preprogrammed firmware that can operate in packet sniffing mode. This default firmware is often used for capturing Zigbee network traffic, making it a valuable tool for developers debugging Zigbee networks. However, this firmware can be replaced or updated with custom Zigbee coordinator firmware, which transforms the CC2531 into a central controller for Zigbee networks.

![image](https://github.com/user-attachments/assets/a16ddb4f-589d-4f67-9cfa-97674da99858)

The CC2531 USB Dongle has two buttons and two LEDs that can be used to interact with the user.
Table bellow shows which CC2531 signals are connected to what IO on the dongle

<table>
  <tr>
    <td>

| IO Connector | CC2531 |                                         
|--------------|--------|                                        
|      1       | P0.2   |                                        
|      2       | P0.3   |                                         
|      3       | P0.4   |                                         
|      4       | P0.5   |                                         
|      5       | P1.7   |
|      6       | P1.6   |
|      7       | P1.5   |
|      8       | P1.4   |

</td>
    <td>


| IO Connector | CC2531 |  
|--------------|--------|
| Green LED    | P0.0   |
| RED LED      | P1.1   |
| BUTTON S1    | P1.2   |
| BUTTON S2    | P1.3   |

 </td>
  </tr>
</table>



### What It Can Be Used For

### What It Can Be Used For

- **Zigbee Coordinator**:  
  When flashed with coordinator firmware (e.g., `CC2531ZNP-Prod.hex`), the dongle can act as the central hub for a Zigbee network. This allows it to control and manage Zigbee devices in a smart home setup.

- **Packet Sniffer**:  
  As a Zigbee packet sniffer, the CC2531 can capture and analyze Zigbee communications, useful for network troubleshooting and security analysis.

- **Home Automation**:  
  Integrating with software like **Zigbee2MQTT** or **Home Assistant**, the CC2531 becomes a key component in automating and controlling smart home devices such as lights, sensors, and switches.<br><br>



### How to Use It

- **Flashing Firmware**:  
  To use the CC2531 as a Zigbee coordinator, you'll need to flash it with custom firmware. This is typically done using a Raspberry Pi or a CC Debugger. The most common firmware is the **Z-Stack Coordinator firmware**, which allows communication with Zigbee devices.

- **Connecting to Software**:
  - **Zigbee2MQTT**:  
    After flashing, you can use the CC2531 with **Zigbee2MQTT** to integrate Zigbee devices with an MQTT broker.
  - **Home Assistant**:  
    You can also integrate it into **Home Assistant** for managing smart devices through a user-friendly interface.

- **Use in Zigbee Networks**:  
  Once configured, the CC2531 can join a Zigbee network as the coordinator and allow you to control connected Zigbee devices (like sensors, switches, and lights).


- **Home Automation**:  
  Integrating with software like **Zigbee2MQTT** or **Home Assistant**, the CC2531 becomes a key component in automating and controlling smart home devices such as lights, sensors, and switches. <br><br>



# FLASH CC2531 USING RASPBERRY PI 4 <br><br>

To do that you need to have,
- CC2531 USB Dongle
- Raspberrypi 4
- Jumper Wires

<p align="center">
  <img src="https://github.com/user-attachments/assets/7b1e18e3-ebfe-4ec5-a772-3ddf43471ed7" alt="Image 1" width="200"/>
  <img src="https://github.com/user-attachments/assets/02c8c4b2-842d-4931-8f3e-2587c753ff91" alt="Image 2" width="220"/>
  <img src="image3-url" alt="Image 3" width="200"/>
</p>



<br><br>


## Wiring to Raspberrypi


**Debug connector pin-out** <br>


<p align="center">
  <img src="https://github.com/user-attachments/assets/f4d6775d-f8ce-4879-bad9-782c3ec81035"   hight="600"  width="600"/>
</p>

<br>

<div align="center">

| Pin          |Description                 |  
|--------------|----------------------------|
| Vdd          | Target Voltage sense       |
| DC           | Debug Clock                |
| DD           | Debug Data                 |
| MOSI         | SPI data out               |
| MISO         | SPI data in                |
</div>

<br><br>

## The wiring to the raspberrypi should be as follows <br>
<div align="center">

| **Debugger Pin** |**GPIO Pin of Raspberrypi**     |  
|------------------|--------------------------------|
| GND              | PIN 39 (GND)                   |
| DC               | PIN 36                         |
| DD               | PIN 38                         |
| RESET            | PIN 35                         |

</div>

<br><br>

<p align="center">
  <img src="image3-url" alt="Image 3" width="200"/>
</p>

## Flashing the Adapter

To flash the CC2531 USB Adapter, you need **WiringPi** and **flash_cc2531**. If you're using **Raspberry Pi OS Lite (64-bit)**, you also need **git**.

#### Step 1: Install Git
Run the following command to install git:
```bash
sudo apt install -y git
```


#### Step 2: Install WiringPi
```bash
sudo dpkg -i wiringpi-2.61-1-arm64.deb
```
After installation, verify it with the following command 
```bash
gpio -v
```
You should see wiringpi 2.61 in the output. 


#### Step 3: Clone flash_cc2531 Repository
Clone the flash_cc2531 repository from GitHub
```bash
git clone https://github.com/jmichault/flash_cc2531.git
```
Move to the directory of the cloned repository
```bash
cd flash_cc2531
```


#### Step 4: Recompile flash_cc2531
Recompile the repository for the ARM aarch64 architecture
```bash
make
```


#### Step 5: Verify the Wiring
Once you've connected the CC2531 to the Raspberry Pi, check if the wiring is correct by running
```bash
./cc_chipid
```
You should see a response like,
```bash
ID = b524
```


<br><br><br>
## Flashing the CC2531 with New Firmware <br><br>


#### Step 1: Download Firmware
Download the latest version of the firmware for the CC2531 as a Zigbee Coordinator
```bash
wget https://github.com/Koenkk/Z-Stack-firmware/raw/master/coordinator/Z-Stack_Home_1.2/bin/default/CC2531_DEFAULT_20211115.zip
```

#### Step 2: Unzip the Firmware
```bash
unzip CC2531_DEFAULT_20211115.zip
```

#### Step 3: Erase the Existing Firmware
```bash
./cc_erase
```

#### Step 4: Write the New Firmware
```bash
./cc_write CC2531ZNP-Prod.hex
```
Once the flashing is complete, you can remove the GPIO cables from the Raspberry Pi.<br><br>

*Congratulations! You've successfully flashed your CC2531 USB dongle as a Zigbee coordinator. You can now use it with tools like Zigbee2MQTT or Home Assistant to integrate and manage your Zigbee devices in a smart home environment.*








