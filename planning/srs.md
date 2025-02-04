## The Cool Company 
## **Fridgify** <br/>Software Requirements Specification <br/> For Fridge Content Tracking <br/> Version 1.3

| **Date** | **Version** | **Description** | **Author** |
| -------- | ----------- | --------------- | ---------- |
| 19.10.19 | 1.0 | Filling all information | Duc Vo Ngoc |
| 20.10.19 | 1.1 | Finalizing and filling in missing information | Duc Vo Ngoc |
| 28.11.19 | 1.2 | Add Use Cases to SRS | Duc Vo Ngoc |
| 23.06.20 | 1.3 | Update links, Update requirements | Joschua Götz |
| 26.06.20 | 1.3.1 | Fix TOC | Duc Vo Ngoc

## Table of Contents

- [Software Requirements Specification](#software-requirements-specification)
  - [Introduction](#introduction)
    - [Purpose](#purpose)
    - [Scope](#scope)
    - [Definitions, Acronyms, and Abbreviations](#definitions-acronyms-and-abbreviations)
    - [References](#references)
    - [Overview](#overview)
  - [Overall Description](#overall-description)
    - [Product Perspective](#product-perspective)
    - [Product Functions](#product-functions)
    - [User Characteristics](#user-characteristics)
    - [Constraints](#constraints)
    - [Assumptions and dependencies](#assumptions-and-dependencies)
    - [Technology Stack](#technology-stack)
  - [Specific Requirements](#specific-requirements)
  - [### Functionality](#h3-idfunctionality-309functionalityh3)
      - [User Interface](#user-interface)
      - [Scanning](#scanning)
      - [REST API](#rest-api)
      - [Notification Service](#notification-service)
  - [### Usability](#h3-idusability-309usabilityh3)
      - [Ease of use](#ease-of-use)
      - [Fridge Management](#fridge-management)
  - [### Reliability](#h3-idreliability-309reliabilityh3)
      - [Code Coverage](#code-coverage)
      - [Server Reliability](#server-reliability)
  - [### Performance](#h3-idperformance-309performanceh3)
      - [Registering Items](#registering-items)
      - [Unregistering Items](#unregistering-items)
  - [### Supportability](#h3-idsupportability-309supportabilityh3)
      - [Coding Standards](#coding-standards)
  - [### Design Constraints](#h3-iddesign-constraints-309design-constraintsh3)
      - [MVC](#mvc)
      - [Programming Languages](#programming-languages)
  - [### On-line User Documentation and Help System Requirements](#h3-idon-line-user-documentation-and-help-system-requirements-309on-line-user-documentation-and-help-system-requirementsh3)
  - [### Purchased Components](#h3-idpurchased-components-309purchased-componentsh3)
  - [### Interfaces](#h3-idinterfaces-309interfacesh3)
      - [User Interfaces](#user-interfaces)
      - [Hardware Interfaces](#hardware-interfaces)
      - [Software Interfaces](#software-interfaces)
      - [Communications Interfaces](#communications-interfaces)
  - [### Licensing Requirements](#h3-idlicensing-requirements-309licensing-requirementsh3)
  - [### Legal, Copyright, and Other Notices](#h3-idlegal-copyright-and-other-notices-309legal-copyright-and-other-noticesh3)
  - [### Applicable Standards](#h3-idapplicable-standards-309applicable-standardsh3)
  - [Supporting Information](#supporting-information)

# Software Requirements Specification

## Introduction
### Purpose

The **Fridgify SRS** provides a general overview over the project as
well as a detailed description. This document is going to delve into
the general vision, **Fridgify's** purpose and its features. System
specifications, interfaces and constraints of the product will be
illustrated in this **SRS**.

<a href="#scope"></a>
### Scope

**Fridgify** is a mobile application, designed to help people keep track
of the contents of their fridges. This is achieved by scanning the
barcode of a product and its due date, which will be kept track in a
database.\
The application should be free to download in **Apple's App Store** as
well as **Google's Play Store**.\
Furthermore, **Fridgify** requires an internet connection to fetch and
display results stored in our database. All information related to the
system, user and contents are maintained in a database, which is located
on a root server. The mobile app is going to interact with a Python
backend, which provides an API interface to retrieve, insert and process
data from the database. By using a centralized backend, the user can
synchronize his fridge on multiple devices alone and with different
users.

### Definitions, Acronyms, and Abbreviations

  | **Term** | **Definition** |
  | -------- | -------------- |
  User |               Someone who interacts with the mobile phone application
  Device |             Device, which allows users to keep track of their fridge contents (e.g. mobile phone, Raspberry Pi)
  REST    |            **RE**presentational **S**tate **T**ransfer is an architectural style for distributed hypermedia systems \[1\]
  API       |          Application Programming Interface connects *client* and *server*. \[2\]
  Application Store |  A mobile application store, where users can get the application (e.g. App Store, Play Store)
  OS              |    Operating System
  Android         |    Google's OS for mobile phones \[3\]
  iOS             |    Apple's OS for mobile phones \[4\]

### References

\[1\] "What is REST", <https://restfulapi.net/>\
\[2\] Braunstein, Mark L., „Health Informatics on FHIR: How HL7's New
API is Transforming Healthcare". Springer, 2018\
\[3\] "About the platform", <https://developer.android.com/about>\
\[4\] "iOS 13 -- Apple Developer", <https://developer.apple.com/ios/>

Here are documents and links which could be helpful to understand
**Fridgify**:

\[A\] Fridgify Blog: <https://blog.fridgify.com/>\
\[B\] Fridgify GitHub: <https://github.com/Fridgify>\
\[C\] UML-Diagram: [Google Drive](https://drive.google.com/file/d/1PtSFhhHVe-l59GPKEeMMshzpPv89wig4/view) OR [GitHub Blob](https://github.com/Fridgify/Fridgify_Documentation/blob/master/use_cases/overall_ucd_v2.png)\
\[D\] Authentication Explanation: [How Fridgify's Authentication works](https://github.com/DonkeyCo/Fridgify/blob/documentation/documentation/uc/authentication/authentication_system.md)

### Overview

The remainder of the document includes three chapters. The second one
provides an overview of the system functionality and system interaction
with other systems.

The third chapter provides the requirement specification in detailed
terms and a description of the different system interfaces.

In chapter four, extra information is provided, such as appendixes, user
stories, etc.

## Overall Description
### Product Perspective

This system is going to consist of two parts: one mobile application and
one Web-API. The mobile application is used to keep track of contents
inside the fridge as well as registering and unregistering items. The
Web-API is used to store, retrieve and process data provided by the
backend.

The mobile application needs to communicate with a Web-API. The Web-API,
designed as a REST API, provides necessary data for the client.

Since this is a data centric product, a database is required to store
data. For a client to access data, he has to communicate with the REST
API, which in conclusion works with the database. No direct access to
the database is required. To communicate with the REST API, the client
needs to authenticate himself, otherwise operations for data retrieval
as well as data storing is prohibited.

### Product Functions

With the mobile app of Fridgify, the user is able to look into the items
his fridge contains. By registering, via scanning a barcode or manual
input, a user can add items to the tracking system. Via the mobile app a
user is able to unregister an item by removing the specific item from
the list by manual input (in later versions maybe by scanning?)

Scanning a barcode produces an article identifier provided by the code.
This code is used to store the product inside of a database or retrieve
information for the product. When scanning the barcode, the user can
scan the due date as well to keep track of it.

Messages are managed by the backend. By this, a user can be notified if
an item is expired, an item is out of stock or a reminder to buy new
items.

The Web API enables the client application to add and retrieve data via
GET and POST Requests. An OAuth 2.0 Authentication is needed to
communicate with the backend.

### User Characteristics

There is one only one user group interacting with the application. Each
user has the ability to manage his or her own fridge, they also have the
opportunity to join groups. Joining groups allows them to keep track and
manage their fridges with multiple users, which is helpful for families
or people sharing an apartment. Every user is able to register,
unregister and list items of a fridge.

### Constraints

The mobile application is constrained by the capacity of the database.
Since multiple users can request items, requests could be possibly
queued.

Requests to the backend by the mobile app are constrained by the server
capacities, high traffic could possibly lead to slower times.

An internet connection is recommended, because otherwise synchronization
with the database is not possible. Data is being stored in a local
database, if possible.

### Assumptions and dependencies

One assumption about the product is that it will be used on mobile
devices, which have the necessary computing power. If the phone does not
have enough hardware resources available for the application, there may
be scenarios where the application is not working properly.

### Technology Stack

| Field of Development | Technologies |
| ------------ | ------------ |
| Backend | Python 3.7.4, unittest, Django 2.2, Docker, docker-compose, Firebase |
| Frontend | Flutter 1.9 (Test Framework integrated), Firebase |
| Database | PostgreSQL |
| Project Management | [YouTrack](https://fridgify-tracking.donkz.dev) |
| Continiuous Integration | [TeamCity](https://fridgify-tc.donkz.dev) |
| Version Control System | Git, [GitHub](https://github.com/DonkeyCo/Fridgify) |


## Specific Requirements

### Functionality
--------------
This section contains all the functional and quality requirements of the
system. It gives a detailed description of the system and all its
features.

#### User Interface

The user interface should be easily accessible. Users can log in and
register on a dedicated view.\
Users can access a list, showing every item registered in the fridge.
Inside that view, users can start a scanning process to register items,
register them manually or unregister them.

#### Scanning

Via the mobile application users should be able to scan barcodes of
items, which in turn registers them in the application. Scanning the
item automatically adds all information to a local or central database
for the user. Item information are stored in the database with the
barcode.

#### REST API

The REST API provides information for each user. Via an OAuth 2.0
Authentication model, a mobile application can communicate with the
backend and retrieve data. Calling the API endpoints allows
applications to retrieve raw and processed data as well as adding
data.

#### Notification Service

The backend should send notifications to individual users. The
notification service notifies users when an item is expiring.

### Usability
-----------

#### Ease of use

Users should be able to easily keep track of their fridge.\
Scanning an item should only require the user a maximum of 3 to 4
clicks.\
Unregistering an item should only require the user a maximum of 3 to 4
clicks.\
Looking at the list of items should only require a maximum of 2 to 3
clicks.

#### Fridge Management

Users should be able to change a fridge easily. They should be able to
change registered fridges with a maximum of 3 clicks.

### Reliability
--------------

#### Code Coverage

Via Unit Tests a code coverage of a minimum of 70% should be reached.
Reaching such a code coverage allows a very good reliability of both
the frontend and backend.

#### Server Reliability

The percentage of the backends time availability should be around 94%.
Restart of the backend should be automatically. Maintenance should not
interfere with the backends availability. The backend's maximum downtime
should have a maximum of 5 hours.

### Performance
--------------

#### Registering Items

Response time for registering items should have an average processing
time of 3 to 5 seconds and a maximum processing time of 10 seconds.

#### Unregistering Items

Unregistering items should have an average processing time of 3 to 5
seconds and a maximum processing time of 8 to 10 seconds.

### Supportability
--------------

#### Coding Standards

The python source code of the application must follow the [PEP 8 Style
Guide](https://www.python.org/dev/peps/pep-0008/) to guarantee readability and one style of code written by
multiple authors.\
The Flutter and Dart source code follows the official [Style Guide for Flutter](https://github.com/flutter/flutter/wiki/Style-guide-for-Flutter-repo) as well as the official [Style Guide for Dart](https://dart.dev/guides/language/effective-dart/style) to guarentee readability and one style of code written by multiple authors.

### Design Constraints
--------------

#### MVC

Backend as well as frontend are being developed with MVC architecture.

#### Programming Languages

This application uses two different programming languages, Python 3.7
and Dart. A PostgreSQL Database is used to store data.

### On-line User Documentation and Help System Requirements
--------------

Documentation for the API is available for the [development](https://api-dev.fridgify.com/) and the [production](https://api.fridgify.com/) environment. To be up to date with the newest development status, you can
follow the [Fridgify Blog](https://blog.fridgify.com/).
Installation tutorials can be found in a [dedicated blog post](https://blog.fridgify.com/phase-2-week-9-installation/)

### Purchased Components
--------------

The following components were purchased:\
- Root Server (60GB SSD, 16GB RAM, Intel Xeon Gold 6140 / 6320)
- Domain

### Interfaces
--------------

#### User Interfaces

The following user interfaces are available:
* iOS Application
* Android Application
* Website

#### Hardware Interfaces

These requirements will not be implemented but could be thought of. Implementation supports these features for future implementation. \
Fridgify can be combined with Smart Home Systems, such as Alexa, Google Assistant, Siri.\
The backend can be used by a Raspberry Pi to register items. 

#### Software Interfaces

The application should be accessible from:\
· Android Devices\
· iOS Devices [due to lack of MacBook, not fully supported currently]\
. Website\
· *Possibly*:  (Raspberry Pi \[Microcontrollers in general\])

#### Communications Interfaces

The backend is accessible through HTTPS on port 443. Any unencrypted
connection over HTTP on port 80 are not supported and will be redirected
to HTTPS.

### Licensing Requirements
--------------

**Fridgify** is being distributed under the *Creative Commons Attribution-NonCommercial-NoDerivs (CC-BY-NC-ND) License*.\
You find more information [here](http://creativecommons.org/licenses/by-nc-nd/4.0/).

### Legal, Copyright, and Other Notices
--------------

This document makes use of the generic he for reasons of readability.
Any terms containing the words \"he\", \"himself\", \"his\", etc. are
meant to include both women and men, unless explicitly stated otherwise.

The Fridgify Team will not take any responsibility for forgotten items,
over purchases, etc.

### Applicable Standards
--------------

Additionally, the application must be developed to provide the best
possible security. Released code needs to be checked regarding the
following types of attack:

SQL injections

Cross-site scripting (XSS)

Cross-site request forgery (CSRF)

Manual security tests needs to be performed regulary.

## Supporting Information

[UML Diagram - GitHub](https://github.com/Fridgify/Fridgify_Documentation/blob/master/use_cases/overall_ucd_v2.png) - UML Diagram for Fridgify (static version - GitHub)

[UML Diagram - Google Drive](https://github.com/Fridgify/Fridgify_Documentation/blob/master/use_cases/overallUseCaseDiagram.md) - UML Diagram for Fridgify (dynamic version - Google Drive)

[Fridgify - Blog](https://blog.fridgify.com/) - The Fridgify Blog with
all news regarding Fridgify.

[Fridgify - GitHub](https://github.com/Fridgify) - The
Fridgify GitHub contains the source code.

[Fridgify - Project Management](https://fridgify-tracking.donkz.dev) -
Fridgify uses YouTrack for project management.
