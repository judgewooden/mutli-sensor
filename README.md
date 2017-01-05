# Raspberry Pi - Sensor Monitor

I used this project to learn more about python, flask and messaging. I am new to python and would not mind any suggestions/advice from experts that come to this repo.

## Synopsis

The purpose of the project is to automate the control of Raspberry Pi sensors on the network. All the components communicate using a Multicast Messaging layer.

![Multicast Network](https://github.com/judgewooden/multi-sensor/raw/master/static/network.png)

## The programs

Program | Purpose
------- | -------
jph.py  | Library of routines to manage the Multicast messaging
jphRedis.py   | Store last state of all messages in the network
jphFlask.py   | Web Servers Gateway Interface
static/(www)       | Files for the Web interface
templates/(www)    | Files for the web interface  
sensor.py  | Routines to read sensors
router.py  | Route message between multiple networks (TBC)
control.py | Command line interface for viewing/managing network
sql.py     | Store network messages in SQL (TBC)
static/config.json | Config file used by all components
generateSQL.py     | Generate SQL to create database from config (TBC)

## The Sensor

Sensors are easy to using python. The jph library will callback the sensors reading routine which can send data to the network using sendData(). 

### Current supported Sensors

Interface | Example
--------- | -------
Pipe | Return a value from a UNIX pipe
JSON | Do a http request to read JSON
ADC pi + | Analogue-to-Digital board output [ADC Pi Plus](https://www.abelectronics.co.uk/p/56/ADC-Pi-Plus-Raspberry-Pi-Analogue-to-Digital-converter)
ADAfruit | The Temperature & Humidity sensors [AM2302](https://www.adafruit.com/products/393)
Python | Make customized calculations (using ninja2 style variable injection)

## The Multicast bus

Each component on te nework is identifed by a two characters, called **Codifiers**. There are two networks:

1. Control bus - All components talk to each other on this bus, sending keep-alive messages and commands
2. Data bus - Only collector components listen to this bus and sensors send data on this bus.

For both busses each message carries a millisecond timevalue, sequence number and a to/from Codifier. The following message types are supported:

Flag | Purpose
---- | -------
I | **I** am alive
C | Request to (re)read the **C**onfig and restart
P | **Ping** a component, requesting a {T} response message 
T | **T**ime value response to a {P} message, responding with payload from {P}
S | **S**tart the Data channel callbacks
H | **H**alt the Data channel callbacks
N/n | The sequence numbering from this point is **N**ew 

Message with Codifier destination '@@'' is a broadcast to all

## Installation

TBC
