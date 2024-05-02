SockCam: Specification Phase 1
==============================
*Minimum Viable Product (MVP)*
----------------------------

## Overview

The first phase of the project will focus on getting an end-to-end minimum viable product working.  This will have several key goals:

* Provide a working application that can be tested in the field to start gathering feedback from end users in order to refine the requirements and future phases.
* Gain a working knowledge of the technologies and frameworks being used.
* Provide a foundation on which to build upon in future phases.

In-short, the functionality we'll initially implement will be to connect to a server and listen for messages to take photos, take the photo then send it back to the server.

## Functionality

### 1. Application Start

1.1. Open *Live Camera Preview*.

1.2. Check to see if we have any stored server connection details, if so *Open Connection*
Otherwise open *Server Connection Input*.

### 2. Server Connection Input

2.1. Show a screen asking the user for the IP address and port of the server to connect to.

2.2. Store the server details on the device.

2.3. Connect button to *Open Connection*.

2.4. Close button to hide this screen.

### 3. Live Camera Preview

3.1. Open access to the camera and show a live preview of the camera in as large a portion of the display as possible.

3.2. Settings button to open *Server Connection Input*.

3.3. Ensure application has permissions for using the camera.

### 4. Open Connection

4.1. Close any existing connections.

4.2. Open a TCP/IP socket connection to the specified server.

4.3. Update the *Visual Indicator* to reflect the status of the connection.

4.4. If there was a problem opening or maintaining the connection, show *Connection Error*.

4.5. Start listening for messages from the server.

4.6. Ensure application has permissions for using the network.

### 5. Application Close

5.1. *Close Connection*.

### 6. Close Connection

6.1. Immediately close the TCP/IP socket connection to the server.

6.2. Update the *Visual Indicator* to reflect the status of the connection.

### 7. Visual Indicator

7.1. Traffic light icon to represent the status of the connection to the server:

  * Red: No connection
  * Green: Connection active.

7.2. Have a "Status" text label that displays a short message so that we can keep the user informed as to what the application is currently doing (e.g. Taking Photo, Sending Photo, Idle).

### 8. Connection Error

8.1. Show a screen that has the connection details and any associated error messages.

8.2. "Server Connection" button to show *Server Connection Input*.

### 9. Message: TAKE_PHOTO

TAKE_PHOTO:
  - message_id: UUID - The unique ID of the message.

9.2. Upon reciving a "TAKE_PHOTO" message from the server, use the camera to take a photo.

9.3. Store the photo on the device in the phone's gallery.

9.4. Send the photo back to the server with *Message: PHOTO*.

### 10. Message: PHOTO

PHOTO:
  - message_id: UUID - The unique ID of the message this is in response to.
  - device_id: ? - The unique identifier of the device (to be determined).
  - filename: String - The filename of the photo.
  - content_type: String - The MIME Content Type of the image, e.g. image/jpeg
  - content_length: Big Int - The size of the content in bytes.
  - content: Blob - The photo binary data.

NOTE: This is a best guess without looking at any implementation specifics, e.g. socket frameworks.  

## Technologies

### Development Environment

#### [Kotlin](https://kotlinlang.org/) using [Android Studio](https://developer.android.com/studio)

Now [Kotlin has become the standard for developing Android applications](https://developer.android.com/kotlin), this seems like the obvious choice:

* The recommended approach for new Android projects - learning resources, tooling, etc. all support it.
* We don't need to support any other platforms like iOS.
* Leverages the full power of Java.
* Kotlin's clarity, conciseness, default of non-mutability, and it's handling of concurrency make it particularly well-suited for multi-developer environments.

### Application Framework

#### [Android Jetpack](https://developer.android.com/jetpack)

* Jetpack is now the recommended way to build new Android applications - learning resources, tooling, etc. all support it.
* Modern, reactive, reusable, UI framework with [Jetpack Compose](https://developer.android.com/develop/ui/compose).
* Provides compatibility with older devices, which is a big help in our particular use-case.

### Realtime Networking

#### [Socket.IO](https://socket.io/)

* With auto-reconnection, heartbeat, ack, and transport failover - provides a robust connection, something that's very important for our use-case.
* Support for a [wide variety of platforms](https://socket.io/docs/v4/).  While the mobile app will only be in Kotlin/Android, we want to support interaction with a wide variety of implementations for servers, desktop, and other mobile apps - depending on the user's needs.
* [Scalable using adapters](https://socket.io/docs/v4/adapter/) - we want to support large scale operations in the future.

## Testing

### Automated Tests

We will not provide any automated testing in this phase as making sure the overall architecture is right is a goal of the current phase and therefore significant changes are possible, which would require tests to be re-written.

### Server

We will still need a server to connect to, and something to issue messages to SockCam to trigger taking photos.  As we're using Socket.IO, by the easiest way to do so is to create a simple NodeJS project.  That will be in a separate repo, and may ultimately be replaced by a Kotlin/Java version in the future.

## Unknowns

These are things that need to be researched / experimented with before implementation begins in earnest:

### Device Unique Identifer

Need to look into whether there is a unique identifier we can obtain for a device, or whether we need to generate and store one ourselves.

## Known Issues

### Message Routing and Client Roles

Currently there is no means of appropriately routing messages to clients.  Having clients identify the roles they play, e.g. "CAMERA", "TRIGGER", "IMAGE PROCESSOR", etc. along with grouping/namespace's would be the obvious solution - but is outside the scope of this phase.  For now every message will be sent to all clients, and it's up to the client if they want to deal with it or not.
