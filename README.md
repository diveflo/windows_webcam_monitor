# windows_webcam_monitor

## Abstract

This is a little windows service written in python to determine if any process uses a webcam in windows 10 and publish the results via mqtt. I implemented this because I wanted to automate the lighting in my homeoffice during video calls.

The algorithm is based on registry keys in `Computer\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\webcam\NonPackaged`, where each process that has a handle on a webcam is registered. If a process currently has a handle, its `LastUsedTimeStop` key equals `0`.

I do not guarantee for this to work in the future but in my environment it works pretty well. As I did not find anything to do this I thought I'd share.

## Features

> No process using a webcam

![image](https://user-images.githubusercontent.com/10167243/106604637-311a3900-6560-11eb-830a-997270f39eff.png)

> Webcam used by a process

![image](https://user-images.githubusercontent.com/10167243/106613749-d508e200-656a-11eb-8b22-020f54a00df3.png)

## Dependencies

- windows pc
- python >= 3.4
- [paho-mqtt](https://pypi.org/project/paho-mqtt/)

# How to install

1. Install [python](https://www.python.org/downloads/windows/) on your windows machine
2. `pip install paho-mqtt`
3. edit `config.ini`
4. `python service.py`

## As a service

To run the script as windows service and start it automatically with windows, you could use [nssm](http://nssm.cc/download).

1. Install [nssm](http://nssm.cc/download)
2. Run `nssm install WindowsWebcamMonitor` as Administrator

![image](https://user-images.githubusercontent.com/10167243/106614925-31b8cc80-656c-11eb-9bf5-fd55f859683b.png)

![image](https://user-images.githubusercontent.com/10167243/106615271-92480980-656c-11eb-9b44-badbcefcbf14.png)

## Example: Integration in [Home Assistant](https://www.home-assistant.io/)

Put this in your `configuration.yaml`

```yml
sensor:
  - platform: mqtt
      name: Webcam
      state_topic: "windows_webcam_monitor/used_by" # this must match with "path" in config.ini
      icon: mdi:webcam
```