Requirements
============

## Overview

We need a system that will allow us to use groups of Android phones to simultaneously take photos of a subject and transfer the photos to a server for use by other systems and tools.

A key component of this setup is an application that will run on an Android phone, connect to a specified server with a real-time socket connection listening for instructions, then upon recieving instructions take and transfer photos.

This project is specifically for the Android application, SockCam - all other elements have their own projects.

## Use Cases

### Online Listing Photography in Charity Shops

When listing used items online to sell, if there is no stock photography available for the item - or it otherwise needs a photo of a specific reason - it's a time-consuming and labour-intensive process to take the necessary photos.  It is a significant bottleneck in the listing process.

Therefore we want to be able to setup a photography area where the process can be as quick, easy, and automated as possible.

We also need the solution to be inexpensive and small scale, as charities don't have the budget or space for the dedicated hardware the big online sellers like Amazon and World of Books use.

Being able to use inexpensive android phones as remotely operated cameras would allow a system where we just need to push one button and have all the photos of an item taken and transferred to a device for listing online.

## Functional Requirements

### Server Connection

The user needs to be able to specify the server to connect to as well as potentially specifying some authentication information (yet to be determined).

### Unique ID

Each device will have a unique ID.  Either we get this from the hardware, or generated and store a UUID.

### Device Metadata

Meta-data about the device needs to be able to be entered - simple string key/value pairs.  These will let us provide business-specific information such as which photography area it's in, which angle it's shooting from, and so on.

### Realtime Persistant Connection

The application will make a realtime persistant connection to the specified server.

The connection needs to always be maintained where possible, therefore automatic re-connection will be necessary.

### Messages

#### Take a Photo

Take a photo and store it to storage on the android device.

We will want to support both auto and manual focusing.

With the message to take a photo, we want to be able to pass some meta-data that will be stored against the photo.

#### Send Photo

Transfer the photo to the server.

A checksum should be used to verify that the photo has been correctly recieved before flagging it for removal from the android device.

Sending photos should take the lowest priority over other actions (taking more photos, configuring settings, and so on).  Therefore a queue system will be needed so more photos can be taken and those are transferred.

Any meta-data associated with the photo and the device will be transferred along with the photo.

#### Camera Settings

Various camera settings need to be set remotely.  Which specifically is yet to be determined - obviously things like which camera to use, white balance, image resolution, image format, and so on - therefore any implementation needs to be flexible.

#### Set Metadata

Set the device metadata remotely.

### Visual Indicator

We want to have a visual indicator on the device to show what the application is currently doing.

### Live Preview

We want to show a live preview of the camera's view - but as this will be a reasonable power drain, and affect lighting, this should be optional.

## Non-Functional Requirements

### Operating System

Must target Android.  They're more plentifully available to charity shops, and we tend to get more money selling iPhones, so it makes sense to use the Android phones.

There is, therefore, no need to target other operating systems like iOS.

We don't have a specific version of Android targetted - we should err on the side of compatibility, but not at the expense of a key application feature.

### Robust and Fault Tolerant

The application must robust and fault tolerant, a significant effort needs to go into ensuring this is the case: framework selection, error logging system, automated tests, network connections, device compatibility verification, resource usage monitoring, exception handling, message verification, and so on all play a part.

### Well Documented and Maintainable

Maintainability is more important than other metrics like performance unless a critical path is identified that needs optimising.  As we will be a disparate team of developers, with people joining and leaving the project, it's important that onboarding and maintainability is good.

### Network Connectivity

Wifi will be assumed to always be available.

Needs to work on just a LAN with no Internet.  Many charity shops have poor internet connectivity, that is also used for a variety of other tasks, therefore we don't want to be wasting bandwidth sending high resolution images online then downloading them again on a different device in the same location.

### Camera Configuration

As we may need to get into the nitty-gritty of camera configuration for specific setups, our implementation choices need to reflect that.

### Always Running

It's expected that on any given device, the application will always be running.

### Energy Consumption

While we will, for the most part, be operating the devices tethered to USB power supplies, thought should still be given to minimise power usage where possible.

### Easy to Use: Plug & Play

The user-interface on the device needs to be really clear and walk the user through any tasks they need to do.  Due to the spread-out nature of charity shops, technical support will rarely be available.

### Google Play Store

The application needs to be freely available on the Google Play store such that it can be easily distributed, updated, as well as being discoverable by other potential users.

### Video

There is currently no intention to support video in any way.