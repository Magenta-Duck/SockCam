SockCam: Specification Phase 1
==============================
*Minimum Viable Product (MVP)*
----------------------------

## Overview

The first phase of the project will focus on getting an end-to-end minimum viable product working.  This will have several key goals:

* Provide a working application that can be tested in the field to start gathering feedback from end users in order to refine the requirements and future phases.
* Gain a working knowledge of the technologies and frameworks being used.
* Provide a foundation on which to build upon in future phases.

In-short, the funcionality we'll initially implement will be to connect to a server and listen for messages to take photos, take the photo then send it back to the server.

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

### 4. Open Connection

4.1. Close any existing connections.

4.2. Open a TCP/IP socket connection to the specified server.

4.3. Update the *Visual Indicator* to reflect the status of the connection.

4.4. If there was a problem opening or maintaining the connection, show *Connection Error*.

4.5. Start listening for messages from the server.

### 5. Application Close

5.1. *Close Connection*.

### 6. Close Connection

6.1. Immediately close the TCP/IP socket connection to the server.

6.2. Update the *Visual Indicator* to reflect the status of the connection.

### 7. Visual Indicator

7.1. Traffic light icon to represent the status of the connection to the server:

  * Red: No connection
  * Green: Connection active.

7.2. Have a "Status" text label that displays a short messag so that we can keep the user informed as to what the application is currently doing (e.g. Taking Photo, Sending Photo, Idle).

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
  - content_type: String - The MIME Content Type of the image, e.g. image/jpeg
  - content_length: Big Int - The size of the content in bytes.
  - content: Blob - The photo binary data.

NOTE: This is a best guess without looking at any implementation specifics, e.g. socket frameworks.  

## Unknowns

These are things that need to be researched / experimented with before implementation begins in earnest:

### Device Unique Identifer

Need to look into whether there is a unique identifier we can obtain for a device, or whether we need to generate and store one ourselves.
