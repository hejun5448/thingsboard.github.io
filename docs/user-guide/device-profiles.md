---
layout: docwithnav
assignees:
- ashvayka
title: Device Profiles
description: IoT device profiles
redirect_from: "/docs/user-guide/ui/device-profiles"
ruleChainSetting:
    0:
        image: /images/user-guide/device-profile/rule-chain-setting.png

queueNameSetting:
    0:
        image: /images/user-guide/device-profile/queue-name-setting.png

transportSetting:
    0:
        image: /images/user-guide/device-profile/transport-setting.png

mqttProtobufSetting:
    0:
        image: /images/user-guide/device-profile/mqtt-protobuf-setting.png
    
alarmСonditions:
    0:
        image: /images/user-guide/device-profile/alarm-example-1-step-1.png  
        title: 'Step 1. Open the device profile and toggle edit mode.'
    1:
        image: /images/user-guide/device-profile/alarm-example-1-step-2.png
        title: 'Step 2. Click "Add alarm rule" button.'
    2:
        image: /images/user-guide/device-profile/alarm-example-1-step-3.png
        title: 'Step 3. Input Alarm Type and click on the red "+" sign.'
    3:
        image: /images/user-guide/device-profile/alarm-example-1-step-4.png
        title: 'Step 4. Click "Add Key Filter" button.'
    4:
        image: /images/user-guide/device-profile/alarm-example-1-step-5.png
        title: 'Step 5. Select "Timeseries" key type. Input "temperature" key name. Change "Value type" to "Numeric". Click "Add" button.'
    5:
        image: /images/user-guide/device-profile/alarm-example-1-step-6.png
        title: 'Step 6. Select "greater then" operation and input the threshold value. Click "Add".'
    6:
        image: /images/user-guide/device-profile/alarm-example-1-step-7.png
        title: 'Step 7. Click "Save" button.'
    7:
        image: /images/user-guide/device-profile/alarm-example-1-step-8.png
        title: 'Step 8. Finally, apply changes.'

alarmСonditionsWithDuration:
    0:
        image: /images/user-guide/device-profile/alarm-example-2-step-1.png  
        title: 'Step 1. Edit the alarm condition and change the condition type to "Duration". Specify duration value and unit. Save the condition.'
    1:
        image: /images/user-guide/device-profile/alarm-example-2-step-2.png
        title: 'Step 2. Apply changes.'
        
alarmСonditionsWithRepeating:
    0:
        image: /images/user-guide/device-profile/alarm-example-3-step-1.png  
        title: 'Step 1. Edit the alarm condition and change the condition type to "Repeating". Specify 3 as "Count of events".  Save the condition.'
    1:
        image: /images/user-guide/device-profile/alarm-example-3-step-2.png
        title: 'Step 2. Apply changes.'
        
alarmСonditionsClear:
    0:
        image: /images/user-guide/device-profile/alarm-example-4-step-1.png  
        title: 'Step 1. Open the device profile and toggle edit mode. Click "Add clear condition" button.'
    1:
        image: /images/user-guide/device-profile/alarm-example-4-step-2.png
        title: 'Step 2. Click on the red "+" sign.'
    2:
        image: /images/user-guide/device-profile/alarm-example-4-step-3.png
        title: 'Step 3. Add Key Filter.'
    3:
        image: /images/user-guide/device-profile/alarm-example-4-step-4.png
        title: 'Step 4. Finally, apply changes.'
 
alarmСonditionsSchedule:
    0:
        image: /images/user-guide/device-profile/alarm-example-5-step-1.png  
        title: 'Step 1. Edit the alarm rule schedule'
    1:
        image: /images/user-guide/device-profile/alarm-example-5-step-2.png
        title: 'Step 2. Select timezone, days, time interval and click "Save"'
    2:
        image: /images/user-guide/device-profile/alarm-example-5-step-3.png
        title: 'Step 3. Finally, apply changes'
        
alarmСonditionsAdvanced:
    0:
        image: /images/user-guide/device-profile/alarm-example-6-step-1.png  
        title: 'Step 1. Modify temperature key filter and change the value type to dynamic.'
    1:
        image: /images/user-guide/device-profile/alarm-example-6-step-2.png
        title: 'Step 2. Select dynamic source type and input the *temperatureAlarmThreshold*, then click "Update".'
    2:
        image: /images/user-guide/device-profile/alarm-example-6-step-3.png
        title: 'Step 3. Add another key filter for the *temperatureAlarmFlag*, then click "Add".'
    3:
        image: /images/user-guide/device-profile/alarm-example-6-step-4.png
        title: 'Step 4. Finally, click "Save" and apply changes.'
    4:
        image: /images/user-guide/device-profile/alarm-example-6-step-5.png
        title: 'Step 5. Provision device attributes either manually or via the script.'

ruleNode:
    0:
        image: /images/user-guide/device-profile/device-profile-rule-node.png 
    1:
        image: /images/user-guide/device-profile/device-profile-rule-node2.png
        
notifications:
    0:
        image: /images/user-guide/device-profile/device-profile-notifications.png           
---

* TOC
{:toc}

## Overview

Since ThingsBoard 3.2, Tenant administrator is able to configure common settings for multiple devices using Device Profiles. 
Each Device has one and only one profile at a single point in time. 

Experienced ThingsBoard users may notice that Device Type is deprecated in favor of Device Profile. 
The upgrade script will automatically create Device Profiles based on unique Device Types and assign them to corresponding devices.  

Let's review the settings available in the device profile, one-by-one.
 
## Device Profile settings

### Rule Chain

By default, [Root Rule Chain](/docs/user-guide/rule-engine-2-0/overview/#rule-chain) will process all incoming messages and events for any device. 
However, the more different device types you have, the more complex your Root Rule Chain may become. 
Many platform users design their root rule chain with only one purpose - dispatch the messages to specific rule chains based on device type. 

To avoid this painful and routine activity, since ThingsBoard 3.2, you may specify custom root Rule Chain for your devices.
The new Rule Chain will receive all telemetry, device activity(Active/Inactive) and device lifecycle(Created/Updated/Deleted) events.
This setting is available in the Device Profile wizard and Device Profile details.  

{% include images-gallery.html imageCollection="ruleChainSetting" %}

### Queue Name

By default, [Main](/docs/user-guide/rule-engine-2-0/overview/#rule-engine-queue) queue will be used to store all incoming messages and events from any device.
The transport layer will submit messages to that queue and Rule Engine will poll the queue for new messages.
However, with multiple use cases, you might want to use different queues for different devices. 
For example, you might want to isolate data processing for Fire Alarm/Smoke Detector sensors and other devices.
So, even if your system has a peak load produced by millions of water meters, whenever the Fire Alarm is reported, it will be processed with no delays.
Separation of the queues also allows you to configure different [submit](/docs/user-guide/rule-engine-2-0/overview/#queue-submit-strategy) and [processing](/docs/user-guide/rule-engine-2-0/overview/#queue-processing-strategy) strategies.

This setting is available in the Device Profile wizard and Device Profile details. 
Please note, that if you decided to use custom queue name, you should configure it in [thingsboard.yml](/docs/user-guide/install/config/#thingsboard-core-settings) file before you start using it.

{% include images-gallery.html imageCollection="queueNameSetting" %}

### Transport configuration

Since ThingsBoard 3.2, the platform supports two transport types: Default and MQTT. 

#### Default transport type

The Default transport type is for backward compatibility with previous releases. 
With Default transport type, you can continue to use platform default [MQTT](/docs/reference/mqtt-api/), [HTTP](/docs/reference/http-api/) and [CoAP](/docs/reference/mqtt-api/) APIs to connect your devices.
There are no specific configuration setting for the default transport type. 

#### MQTT transport type

The MQTT transport type enables advanced MQTT transport settings. 
Now you are able to specify custom MQTT topics filters for time-series data and attribute updates which correspond to 
[telemetry upload API](/docs/reference/mqtt-api/#telemetry-upload-api) and [attribute update API](/docs/reference/mqtt-api/#publish-attribute-update-to-the-server) respectively.

The MQTT transport type has the following settings.

##### MQTT device topic filters

Custom MQTT topic filters support single '+' and multi-level '#' wildcards and allow you to connect almost any MQTT based device that sends payload using JSON or Protobuf.
For example, using configuration from the image below will allow you to publish time-series data with the following command:

```bash
mosquitto_pub -h 'demo.thingsboard.io' -i 'c1' -u 't1' -P 'secret' -t '/telemetry' -m '{"humidity": 10.3}'
```
{: .copy-code}

and attribute updates with the following command:

```bash
mosquitto_pub -h 'demo.thingsboard.io' -i 'c1' -u 't1' -P 'secret' -t '/attributes' -m '{"firmwareVersion": "1.3"}'
```
{: .copy-code}

assuming you have provisioned basic mqtt credentials for your device with the client id 'c1', username 't1' and password 'secret'.

{% include images-gallery.html imageCollection="transportSetting" %}

##### MQTT device payload

By default, platform expects devices to send data via JSON. However, it is also possible to send data via [Protocol Buffers](https://developers.google.com/protocol-buffers)

Protocol Buffers or Protobuf is a language-neutral and platform-neutral way to serialize the structured data. It is convenient to minimize size of transmitted data.  

At the moment of writing (ThingsBoard 3.2) platform supports custom proto schemas for [telemetry upload](/docs/reference/mqtt-api/#telemetry-upload-api) 
and [attribute upload]/docs/reference/mqtt-api/#publish-attribute-update-to-the-server). 
We plan to add ability to define the schema for the downlink messages (RPC calls and attribute updates) in the future releases.  

{% include images-gallery.html imageCollection="mqttProtobufSetting" %}


ThingsBoard parses the protobuf structures dynamically, that is why, it does not support some protobuf features like OneOf, extensions and maps, yet.


### Alarm Rules

Platform users may use Rule Engine to configure alarms. Rule Engine is a quite powerful feature but requires some programming skills.
Since ThingsBoard 3.2, we have introduced Alarm Rules to simplify the process of configuring most popular alarm types.
Now you don't need to be the Rule Engine guru to configure your processing logic. 
Under the hood, Rule Engine evaluates Alarm Rules using the "Device Profile" rule node.


Alarm Rule consists of the following properties:

 * Alarm Type - type of the Alarm. Alarm type must be unique within the device profile alarm rules;
 * Create conditions - defines criteria when the Alarm will be created/updated. Condition consists of the following properties;
   * Severity - will be used to create / update the alarm. ThingsBoard verifies create conditions in the descending order of the severity. For example, if condition with Critical severity is true, platform will raise alarm with Critical severity and "Major", "Minor", "Warning" conditions will not be evaluated. Severity must be unique per alarm rule (e.g., two create conditions within the same alarm rule can't have the same severity);        
   * Key Filters - list of logical expressions against attributes or telemetry values. For example, *"(temperature < 0 OR temperature > 20) AND softwareVersion = '2.5.5'"*;
   * Condition Type - either simple, duration or repeating. For example, *3 times in a row* or *during 5 minutes*. Simple condition will raise alarm once the first matching event occurred;
   * Schedule - defines time interval during which the rule is active. Either "active all the time", "active at specific time" or "custom";
   * Details - alarm details template supports substitution of the telemetry and/or attribute values using ${attributeName} syntax;
 * Clear condition - defines criteria when the Alarm will be cleared;
 * Advanced settings - defines alarm propagation to related assets, customers, tenant or other entities;    

Let's learn how to use the Alarm Rules by example. Let's assume we would like to monitor a temperature inside of the fridge with valuable goods.  
We also assume that we have already created device profile called "Temperature Sensors", and provisioned our device with the temperature sensor and with access token - "ACCESS_TOKEN".
The command listed below will upload the temperature readings to demo.thingsboard.io.  

```bash
mosquitto_pub -d -h 'demo.thingsboard.io' -t "v1/devices/me/telemetry" -u "$ACCESS_TOKEN" -m '{"temperature": 5.3}'
```
{: .copy-code}

#### Example 1. Simple alarm conditions 
 
We would like to create **Critical** alarm when temperature is greater than 10 degrees.

{% include images-gallery.html imageCollection="alarmСonditions" showListImageTitles="true" %} 

#### Example 2. Alarm condition with duration

Let's assume that we would like to modify Example 1 and raise alarms only if the temperature exceeds a certain threshold for 1 minute. 

For this purpose, we need to edit the alarm condition and modify the condition type from "Simple" to "Duration". We should also specify duration value and unit.

{% include images-gallery.html imageCollection="alarmСonditionsWithDuration" showListImageTitles="true" %} 

#### Example 3. Repeating alarm condition

Let's assume we would like to modify Example 1 and raise alarms only if the sensor reports the temperature that exceeds the threshold 3 times in a row.

For this purpose, we need to edit the alarm condition and modify the condition type from "Simple" to "Repeating". We should also specify 3 as 'Count of events'.

{% include images-gallery.html imageCollection="alarmСonditionsWithRepeating" showListImageTitles="true" %} 

#### Example 4. Clear alarm rule

Let's assume we would like to automatically clear the alarm if temperature in the fridge goes back to normal.

{% include images-gallery.html imageCollection="alarmСonditionsClear" showListImageTitles="true" %}

#### Example 5. Define alarm rule schedule

Let's assume we would like alarm rule to evaluate alarms only during the work hours.

{% include images-gallery.html imageCollection="alarmСonditionsSchedule" showListImageTitles="true" %}

#### Example 6. Advanced thresholds

Let's assume we would like our users to be able to overwrite the thresholds from Dashboard UI. 
We may also want to add the flag to enable or disable certain alarms for each device. 
For this purpose, we will use dynamic values in the alarm rule condition. 
We will use two attributes, boolean *temperatureAlarmFlag* and numeric *temperatureAlarmThreshold*.
Our goal is to trigger alarm creation when "*temperatureAlarmFlag* = True AND *temperature* is greater than *temperatureAlarmThreshold*".

{% include images-gallery.html imageCollection="alarmСonditionsAdvanced" showListImageTitles="true" %}

#### Device profile rule node

Device Profile rule node creates and clears alarms based on the alarm rules defined in the device profile. 
By default, it is the first rule node in the chain of processing. 
The rule node is processing all incoming messages and reacts on the attributes and telemetry values.
There are two important settings in the rule node.

**Persist state of alarm rules** - force rule node to store the state of processing. Disabled by default. This setting is useful if you have duration or repeating conditions. 
Let's assume you have condition "Temperature is greater than 50 for 1 hour", and the first event with temperature greater than 50 was reported at 1pm. 
At 2pm you should receive the alarm (assuming temperature conditions will not change). 
However, if you will restart the server after 1pm and before 2pm, the rule node needs to lookup the state from DB.
Basically, if you enable this and the 'Fetch state of alarm rules' option, the rule node will be able to raise the alarm. 
If you leave it disabled, the rule node will not generate the alarm.
We disable this setting by default for performance reasons. If enabled, and if incoming message matches at least one of the alarm conditions, it will cause additional write operation to persist the state.

**Fetch state of alarm rules** - force rule node to restore the state of processing on initialization. Disabled by default. This setting is useful if you have duration or repeating conditions. 
It should work in a pair with 'Persist state of alarm rules' option, but there is a rare case when you may want to disable this setting while the 'Persist state of alarm rules' option is enabled.
Assuming you have a lot of devices that often and constantly send data, you may avoid loading the state from the DB on initialization. 
The Rule Node will fetch the state from DB when the first message from the particular device will arrive.     

{% include images-gallery.html imageCollection="ruleNode" %}

#### Notifications about alarms

Assuming you have configured alarm rules you may also want to receive a notification when ThingsBoard creates or updates the alarm.
The device profile rule node has three main outbound relation types that you can use: 'Alarm Created', 'Alarm Severity Updated' and 'Alarm Cleared'.
See example rule chain below. Please make sure the system administrator have configured the sms/email providers before you proceed or configure your own settings in the rule nodes. 

You may also use existing guides: 
[Send email on alarm](/docs/user-guide/rule-engine-2-0/tutorials/send-email/) (Use part which explains 'to email' and 'send email' nodes) 
or [Telegram notifications](/docs/user-guide/rule-engine-2-0/tutorials/integration-with-telegram-bot/).
There is also additional 'Alarm Updated' relation type that most of the use cases should ignore to avoid duplicate notifications.

{% include images-gallery.html imageCollection="notifications" %}

### Device provisioning

Device provisioning allows device to automatically register in ThingsBoard either during or after manufacturing. 
See separate documentation [page](/docs/user-guide/device-provisioning/) for more details.




 
    
