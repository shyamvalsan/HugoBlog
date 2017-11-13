---
title: Let there be light! - RaspberryPi controller for Yeelight smart bulb
author: Shyam Valsan
date: '2017-11-01'
slug: YeelightWifiRaspberryPiController
categories:
  - IoT
tags:
  - Yeelight
  - WiFi
  - IoT
  - RaspberryPi
  - HomeAutomation
  - Technology
  - Smart Light
---

**Yeelight** (now owned by **Xiaomi**) is a company which makes smart bulbs which can be controlled over WiFi or BLE (Bluetooth Low Energy). I got my hands on one of their WiFi smart bulbs as part of some hackathon goodies from work. They have a snappy android app which let's you change colors, brightness and allow the light to flash/flow according to the music you're playing. All very nice and dandy - but in the spirit of tinkerers and DIYers everywhere, that wasn't what I was here for. My goal was to be able to get to the nitty gritty, learn the messaging protocol the bulb uses and control the bulb from wherever I please. 

My initial thought was to setup some traffic captures to decode the control messages from the app and then replicate them, but to my surprise I found that Yeelight is very DIY-friendly and they have a developer mode you can enable plus a very thorough [operation specification document](http://www.yeelight.com/download/Yeelight_Inter-Operation_Spec.pdf) - this made things very (*too?*) easy. 

The bulb uses SSDP to advertise itself and its capabilities periodically, commands can be sent to the bulb in the format specified in the spec. I found the simplest way to do this is just running **netcat** (nc) with the command contents.

> Why would you want to control the bulb yourself when there's a perfectly fine app?
>
> The primary reason for me, I have to be honest, is the geek-thrill of figuring out how something works. Once you have full control of the bulb you can enable a LOT more use-cases than what the original creators would have thought of. 
> 
>	You could flash a particular color when your phone gets an alert in silent mode. 
>	Or flash a particular color to remind you to do something, water the plants or feed the pet, or use it as a silent alarm.
>	Or turn on the light automatically when the sun goes down OR when you get home from office. Turn it off when everyone's gone to sleep. 
>	Or code up disco mode and jazz up your party.
		

I wrote a simple [shell script](https://github.com/shyamvalsan/YeelightController/blob/master/light.sh) (see link to git repo below) that let's you control the smart bulb from Linux (I used a RaspberryPi) along with step by step instructions to use it - my goal with this script is to keep things as simple and short as possible, as a result I have avoided the SSDP discovery procedure as I couldn't find a *"shell"* way to do it. Instead I worked around by using arp and netcat to *"discover"* the IP address of the bulb. 

See the [git repo](https://github.com/shyamvalsan/YeelightController) for details but in a nutshell I filter for the manufacturer in the MAC and also check if a particular port is open to verify the IP, hacky but works. Python let's you do the whole thing including the SSDP part, here's a reference from the [yeelight developer page](https://www.yeelight.com/download/developer/yeelight_demo_lan_ctrl_python.zip)

## Code
<https://github.com/shyamvalsan/YeelightController>

## Step by step instructions
	1. Plug in the yeelight smart bulb
	2. Turn on the light switch
	3. Run ./configure-light.sh (if you already know the IP address of your smart bulb then write it to ip.list and skip to step 5)
	4. Wait till configure-light exits, confirm that it ended successfully. If not try repeating steps 2 and 3
	5. Run ./light.sh -h for options on how to control the bulb
	
	Note: Try running nmap (sudo nmap -sP 192.168.1.*) to ping all IPs in your subnet before running configure-light.sh 

```shell
light.sh [command] <color> -- utility to control Yeelight smart bulb over wifi

where command can have one of the following values:
    on - turn on the light
    off - turn off the light
    color <color> - set the color to <color> where <color> is a color (red/blue/green/white and so on)
    disco - turns on disco mode
    sunrise - turns on sunrise mode
    notify-blue - notification in blue color
    notify-green - notification in green color
    notify-red - notification in red color
```

## Demo
[![Yeelight smart light controller on Raspberry Pi](https://img.youtube.com/vi/EqDKSsEf1HE/0.jpg)](https://www.youtube.com/watch?v=EqDKSsEf1HE)

## Requirements
- [Yeelight wifi](http://www.ebay.in/itm/Xiaomi-Yeelight-White-Color-LED-Smart-Bulb-8W-WiFi-Control-Adjustable-Brightness-/152609584877?hash=item23883d76ed)
- [E27 adapter for Indian bulb sockets](http://www.ebay.in/itm/B22-to-E27-LED-Halogen-CFL-Light-Base-Bulb-Lamp-Adapter-Converter-Holder-Socket-/132243823330?hash=item1eca589ae2)
- RaspberryPi



**Note:** Since I had only one of these Yeelights with me, the code works for 1 bulb, if you have multiple Yeelights then it's probably a small change to awk out all the matching IPs, assign indexes or identities to them and operate them individually or together. If I ever get my hands on more of these bulbs I'll update the git repo. 
