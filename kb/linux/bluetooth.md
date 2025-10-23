---
layout: page
title: Bluetooth
---

You can use the command `bluetoothctl` to work with Bluetooth devices in
GNU/Linux.

I bought a pair of wireless bluetooth earbuds, model `TWS88`.

```bash
# Scan for device
$ hcitool scan
Scanning ...
	2D:E8:33:77:F4:AC	TWS88
$ bluetoothctl pair 2D:E8:33:77:F4:AC
Attempting to pair with 2D:E8:33:77:F4:AC
[CHG] Device 2D:E8:33:77:F4:AC Connected: yes
[CHG] Device 2D:E8:33:77:F4:AC Bonded: yes
[CHG] Device 2D:E8:33:77:F4:AC Modalias: bluetooth:v05D6p000Ad0240
[CHG] Device 2D:E8:33:77:F4:AC UUIDs: 0000110b-0000-1000-8000-00805f9b34fb
[CHG] Device 2D:E8:33:77:F4:AC UUIDs: 0000110c-0000-1000-8000-00805f9b34fb
[CHG] Device 2D:E8:33:77:F4:AC UUIDs: 0000110e-0000-1000-8000-00805f9b34fb
[CHG] Device 2D:E8:33:77:F4:AC UUIDs: 0000111e-0000-1000-8000-00805f9b34fb
[CHG] Device 2D:E8:33:77:F4:AC UUIDs: 00001200-0000-1000-8000-00805f9b34fb
[CHG] Device 2D:E8:33:77:F4:AC ServicesResolved: yes
[CHG] Device 2D:E8:33:77:F4:AC Paired: yes
Pairing successful
$ bluetoothctl devices
Device 2D:E8:33:77:F4:AC TWS88
$ bluetoothctl info 2D:E8:33:77:F4:AC
Device 2D:E8:33:77:F4:AC (public)
	Name: TWS88
	Alias: TWS88
	Class: 0x00240404 (2360324)
	Icon: audio-headset
	Paired: yes
	Bonded: yes
	Trusted: no
	Blocked: no
	Connected: no
	LegacyPairing: no
	UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
	UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
	UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
	UUID: Handsfree                 (0000111e-0000-1000-8000-00805f9b34fb)
	UUID: PnP Information           (00001200-0000-1000-8000-00805f9b34fb)
	Modalias: bluetooth:v05D6p000Ad0240
	ManufacturerData Key: 0x7262 (29282)
	ManufacturerData Value:
  32 32 78 78 11 22 33 44 55 66 aa bb 00 00        22xx."3DUf....  
	RSSI: 0xffffffc0 (-64)
```
```bash
$ bluetoothctl trust 2D:E8:33:77:F4:AC
[CHG] Device 2D:E8:33:77:F4:AC Trusted: yes
Changing 2D:E8:33:77:F4:AC trust succeeded
```
To actually send the sound to the earbud:
```bash
$ bluetoothctl connect 2D:E8:33:77:F4:AC
Attempting to connect to 2D:E8:33:77:F4:AC
[CHG] Device 2D:E8:33:77:F4:AC Connected: yes
[NEW] Endpoint /org/bluez/hci0/dev_2D_E8_33_77_F4_AC/sep1 
[NEW] Endpoint /org/bluez/hci0/dev_2D_E8_33_77_F4_AC/sep2 
[NEW] Transport /org/bluez/hci0/dev_2D_E8_33_77_F4_AC/sep1/fd15 
Connection successful
```
If `hcitool` lists your device but `bluetoothctl` doesn't, then you could try:
```bash
$ bluetoothctl
Waiting to connect to bluetoothd...[bluetooth]# Agent registered
[bluetooth]# power on
[bluetooth]# agent on
[bluetooth]# default-agent
[bluetooth]# scan on
[bluetooth]# pair 48:73:CB:20:F9:E73:CB:20:F9:E7
```