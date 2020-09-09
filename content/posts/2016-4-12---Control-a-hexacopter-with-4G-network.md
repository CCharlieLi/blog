---
title: Control a hexacopter with 4G network
date: "2016-04-12T23:46:37.121Z"
template: "post"
draft: false
slug: "drone-4G"
category: "Tech"
tags:
  - "Tech"
description: "he UAV (Unmanned Aerial Vehicle) also called as drone copter is now a popular technology in China as much as in America. Not just because the DJI is becoming the biggest UAV company, also the open source community of UAV is becoming bigger."
socialImage: "/media/image-book.jpg"
---

![architecture](/media/UAV/1.png)

The UAV (Unmanned Aerial Vehicle) also called as drone copter is now a popular technology in China as much as in America. Not just because the DJI is becoming the biggest UAV company, also the open source community of UAV is becoming bigger. Thanks to the DJI, everyone can have a UAV to take aerial photo or as a toy. Thanks to 3D Robotics (3DR), everyone can make a UAV with awesome open source flight control system APM and PX4. Both make UAV known by more people. With everything needed, people may have more choices on how a UAV should be like. As a developer, I like UAV open source technology as much as I like my girl friend. Although I'm not a good pilot of UAV, but I know how it works, the pros and cons of most UAVs, and here is what I made, a hexacopter that can be controlled by 4G network.

First of all, we'd better have some basic knowledge about UAV. According to the type of UAVs, they consist of different parts. Fuselage and wings for planes (fixed-wing aircraft), tail rotor for helicopters, canopy, frame and arms for multicopter (or multirotor). I made a multicopter for this experiment, as we can see the advantage of multicopter on [wiki](http://www.wikiwand.com/en/Multirotor):

An advantage of multirotor aircraft is the simpler rotor mechanics required for flight control. Unlike single- and double-rotor helicopters which use complex variable pitch rotors whose pitch varies as the blade rotates for flight stability and control, multirotors often use fixed-pitch blades; control of vehicle motion is achieved by varying the relative speed of each rotor to change the thrust and torque produced by each.

More rotors make it more easy to control a multicopter than other kinds of UAVs. According to the number of rotors, multicopter can be divided into tricopter, quadcopter, hexacopter and octocopter which refer to 3-, 4-, 6- and 8-rotor helicopters. I chose to make a hexacopter cause it's more safer than tricopter and quadcopter, as once a rotor was not working, the other two rotors on the same side could replace it by generating more lift force, also it's more energy-efficient than octocopter. 

Almost all the UAVs are radio-controlled aircraft also called RC aircraft. That is, UAV is controlled remotely by an operator on the ground using a hand-held radio transmitter. The transmitter communicates with a receiver within the craft that sends signals to servomechanisms (servos) which move the control surfaces based on the position of joysticks on the transmitter. The control surfaces, in turn, affect the orientation of the UAV. The available distance of almost UAVs for civil use is less than 5 miles unless you have a bigger antenna on your transmitter, which is a main problem of UAV RC control cause you may lost your aircraft if you lost control of it.  Thanks to that almost all UAV flight control systems nowadays have failsafe protection, once UAV lost connection with transmitter it would return to home point. But, this doesn't solve the problem of distance.

As a alternative, 4G network seems has no distance-limited problem if I use it to control my UAV. Now, We all know that a UAV can be controlled well with radio communication, but also with its distance limited. That means, you can fly your drone at most several miles away, at the same time without any barrier between you and your drone. That's a huge disadvantage for UAV to be used in most situation like patrol and search. Here I use companion computer (a single board computer) to control Pixhawk (a open source flight control system device of 3D Robotics ) directly, and companion computer gets feedback and data from it. As the companion computer is a whole functional computer, the data it received can be stored (as a blackbox) or sent to server through 4G LTE dongle, and that's how I break the limit of distance of RC. I used a PM2.5 sensor to collect real-time PM2.5 data, companion computer got the data and sent it back to server every second through 4G network, meanwhile companion computer got the latest command from server and sent it to Pixhawk. As you can see below, the companion computer plays a role of flight management system which responsible for collecting, storing and sending data, as to the Pixhawk board, it's the flight control system. These two are the most important parts in UAVs as much as in real planes. Now, 


## How to make a 4g-connected  hexacopter?

#### Equipment

- Aircraft body: [DJI S900](http://www.dji.com/cn/product/spreading-wings-s900)

![m](/media/UAV/2.png)

- Companion computer: [ODROID XU4](http://www.hardkernel.com/main/products/prdt_info.php?g_code=G143452239825)/[BPI-M2](http://www.bananapi.com/index.php/component/content/article?layout=edit&id=73) with Ubuntu operation system

![m](/media/UAV/3.png)

- Flight control board: [Pixhawk](http://pixhawk.com/)(with APM/PX4)

![m](/media/UAV/4.png)

- Fittings of Pixhawk: GPS module, buzzer, switch ...

- Futaba T8FG transmitter and receiver

![m](/media/UAV/5.png)

- HUAWEI E3272 4G LTE dongle

![m](/media/UAV/6.png)

- Nova SDS011 PM2.5 sensor

![m](/media/UAV/7.png)

- 5200 mah battery (for companion computer) and 16000 mah battery (for Pixhawk and rotors)

- TTL2USB module

- Dupont lines

- Matek mini power hub (DIstribution board)

![m](/media/UAV/8.png)

#### Architecture

![m](/media/UAV/9.png)

#### Set up hexacopter

- **Set up DJI S900 framework**: Follow the [instruction book](http://dl.djicdn.com/downloads/s900/cn/S900_User_Manual_cn_v1.4.pdf).

- **Prepare Pixhawk**: Install firmware ([PX4](http://www.pixhawk.com/firmware/start) or [APM](http://www.pixhawk.com/firmware/apm)), calibration on-board sensors with [Mission Planner](http://ardupilot.org/planner/index.html).

- **Set up Pixhawk into S900**: Pixhawk should be at the centre of S900 centre plate, make sure that you put Pixhawk head to right direction, also make sure that you map the rotors of PX4/APM firmware to that of DJI S900 in right order.

- **Test UAV**: Connect Pixhawk with radio reveiver and other fittings, open hand-held radio transmitter,  connect the battery and power up, arm the UAV, check if everything is working and try to take off. 

- **Set up ODROID XU4**: I installed Ubuntu 15.04 OS on my board with emmc module, actually you can install whatever OS you want.

- **Connect PM2.5 sensor with XU4**: PM2.5 sensor output data with serial port, we need to make a line with Dupont lines and TTL2USB module to establish serial communication between sensor and XU4, make sure that you connect 5v, GND, RX, TX to the right port of TTL2USB module. Then connect sensor and XU4. As the sensor exports PM data with specific protocal from serial port every second, I wrote a [python tool](https://github.com/CCharlieLi/pySensor) to read PM2.5 from it. When you see something like below, it's working. This is supposed to run on XU4.


```shell
14:59:52 PM2.5:  35
14:59:53 PM2.5:  36
14:59:54 PM2.5:  35
14:59:55 PM2.5:  35
```


- **Power XU4 with power hub**: The battery of 5200 mah has a 12v output which is too high to power the XU4 and PM2.5 sensor, that's why we need Matek distribution board which has a 5v output. By the way, the sensor can be powered directly through USB port.

- **Connect XU4 with Pixhawk**: Follow the official [manual](http://ardupilot.org/dev/docs/odroid-via-mavlink.html).

- **Communication between Pixhawk and XU4**: Thanks to 3DR development team, we can use [Mavproxy](http://dronecode.github.io/MAVProxy/html/getting_started/index.html) or [Dronekit](http://python.dronekit.io/) to establish communication between Pixhawk and companion computer easily through Mavlink (a protocal to communicate with Pixhawk). Mavproxy and Dronekit are both open source software. I made [APM connector](https://github.com/CCharlieLi/APMC) (APMC) based on Dronkit, and used it to give direct order to Pixhawk like get parameters, take off and move. This is a project under development, I'm still trying to finish the mission setting part.

#### Server
The idea is, all UAVs send data to server and get feedback constantly, server collects data from UAVs and then give missions back to them. I built a server demo with [totalVue](https://github.com/CCharlieLi/totalVue) which is a combination of [total.js](https://www.totaljs.com/) framework and [vue.js](http://vuejs.org/) framework, it's enough for me to do all backend and frontend work for this experiment. For now, we have a full functional hexacopter, the only thing we need to do is to let the companion computer ODROID XU4 send HTTP request (POST) to server every second with it's data about PM2.5, location, timestamp, state and so on, then XU4 gets returned message from server as the current order and sends it to Pixhawk.

The most important thing of this server is to make use of data and interact with users. Users should be able to see their UAVs' state on the server once their UVAs are online. Also user should be able to set current order for UAV. Each order, from server to UAV, should have a unique id. Once UAV gets the order, it will check the id to tell if it was the latest one, if not, ignore it.

![m](/media/UAV/10.png)

## Problems and discussion

**Network connection** through 4G network could be very unstable, and IP address of every UAV will be changed when it's going into a new cellular network. We can't locate the IP address of UAV, but the IP address of server is unchangeable. So, companion computer on each UAV send it's data(including UAV state, location, ......) to server through 4G LTE dongle continuously.

**Network delay** is always a problem and it's more important for UAV control. Real-time control via network requires good network condition, as to 4G LTE, it's hard to promise that network could always be that good. Instead, mission control is much better in this way. Every time UAV send request to server, it gets a mission(a list of commands) from server and then goes its way. Users only need to care about what mission to give next or terminating current mission, most of time users just need to wait and see the data received from UAV.

There are still a lot of situations and problems that I should mention and take care, like what if UAV lost connection with server for any reason, what if there's no 4G network at all. As I did all these in my spare time, I'll try to make it better, if you have any advice please feel free to [tell me](charlie@wiredcraft.com). This experiment is inspired by [4Gmetry III](http://4gmetry.voltarobots.com/), and I learned a lot from Silvio's [blog](4Gmetry concept & DIY essentials ). 4G network control of UAV would be very useful in many situations, this give me a chance to think about that we can do more with UAV and networks, as every single UAV is a full functional computer in the sky.