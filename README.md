# Zigbee CC2531 USB-Dongle
This repository provides detailed instructions on how to use the CC2531 USB dongle as a Zigbee coordinator or for other Zigbee applications. It covers flashing the dongle with the latest firmware using a Raspberry Pi (64-bit architecture), setting up Zigbee2MQTT or Home Assistant, and integrating Zigbee devices into your home automation network.


# INTRODUCTION

## Hardware Discription

![image](https://github.com/user-attachments/assets/a16ddb4f-589d-4f67-9cfa-97674da99858)

The CC2531 USB Dongle has two buttons and two LEDs that can be used to interact with the user.
Table bellow shows which CC2531 signals are connected to what IO on the dongle

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


| IO Connector | CC2531 |  
|--------------|--------|
| Green LED    | P0.0   |
| RED LED      | P1.1   |
| SWITCH S1    | P1.2   |
| SWITCH S2    | P1.3   |
