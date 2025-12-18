---
layout: base.njk
title: Architecture
---

# Architecture

![Ingenium Architecture](../../assets/img/Ingenium%20Architecture.png)

<center>Ingenium Architecture</center>


# Ingenium Primary Server

The Ingenium Primary Server component is a suite of microservices running in docker containers on a virtual or physical  server. All microservices communicate via REST requests, or in certain cases (Execution Monitor → UI) via websocokets. REST API is documented for each service in their associated Github repository using a mixture of OPEN API 2.0/3.0 (Swagger).

## UI

The User interface is a combination of Django and Vue.JS.  The current architecture of the UI is that Django hosts a number of Vue.JS applications for each major UI page/function. This is because the UI was originally developed in Django and has been updated to Vue.JS over time.

## Project Configuration/Dictionary

The Project Configuration / Dictionary service is a mechanism for Ingenium to cache project specific information and normalize if for us inside the application. This simplifies internal interfaces via standard formats and provides high performance query/autocomplete of the data. This includes dictionary information (command/telemetry), V&V information (Requirements and Dictionary VIs), and project provided scripts.

## Auth

Ingenium uses LDAP/RSA for authentication and then creates JWT (JSON web tokens) for authorization with the application. The auth service generates these tokens and manages access via defined roles, each with their own set of users/groups and permissions.

## Core

As the name implies this is the core service of Ingenium. All procedure/execution management, creation, execution is managed through the core service. Core communicate with the archive service to store and retrieve procedure and execution data and with the Execution service to execute steps in procedures.

## Archive

The archive service provides storage for procedures and executions via an Arango Database. It is accessed via Core.

## Execution

The execution service executes steps via the embedded code and pushes the execution results to Core/Archive. The state of each execution such as the last value of channel values is stored in Redis. 

## Embedded Code

Each built in step has a corresponding set of logic to implement the step, send FSW/SSE commands, make queries to the venue service, and evaluate the results. 

## Minio

Ingenium is design to store attached information (images, etc) to either AWS S3 or locally via Minio (which locally mimics a S3 interface).

# Ingenium Analytics Server

The Ingenium Analytics Server is another physical or virtual server that hosts a number of micro services in docker containers. These services all relate to post execution analysis and reporting and do not directly impact the core procedure development/execution functionality of the primary server.

## Reporting

This service creates reports from Ingenium data. Due to the time required to build certain reports, this system has a queuing mechanism and informs users via email when their report is ready.

Reports include: PDF of procedure/as run, Difference, Execution Summary (Excel)

## Search

This service replicates the main Arango database and mirrors the execution and procedure data in Elastic Search for performant queries across procedures and executions

# Ingenium Agent (Venue Server)

The Ingenium agent exists to translate RESTful queries from the Ingenium server to AMPCS and other local actions (custom scripts).  Note that this uses a dedicated JWT token that is not available to users. This ensures that only the Ingenium server can command via the Ingenium agent.

## Supported Operations:

- Commanding via AMPCS MTAK (Start MTAK, Send FSW CMD, Send File (VC2), Send SCMF, Send HW Cmd, Send SSE Command)
- Telemetry queries via AMPCS GLAD (query EVRs, query EHA) and AMPCS Chill (chill_get_chanvals, chill_get_evrs, chill_get_produts)
- 1553 bus log parsing
- Ingenium Custom Scripts
  - This is an extension point of Ingenium that allows projects to write their own custom steps whose inputs and outputs will be rendered in Ingenium UI.

# Ingenium MTAK

Ingenium requires the ability to route command and telemetry requests to specific AMPCS sessions. This is to support the following use cases:

Multiple users each running WSTS on the same server (different sessions)
Commanding/Querying multiple sides of the spacecraft (e.g. TZ-A, TZ-B)

This capability was implemented by AMPCS for M2020 but was not included in the AMPCS Multi-Mission Baseline. 

Ingenium maintains the capability in an Ingenium only version of MTAK which is delivered with Ingenium and is supported for use with Ingenium only.

Note that AMPCS included the multi-session MTAK feature in AMPCS 9.1. Once that version is widely deployed across Ingenium customers the Ingenium MTAK will be retired.

Open Ingenium Note: MTAK is part of AMPCS which is not open source.

# Other

## System Tests

Ingenium has a suite of system tests which test the entire backend (server → client) without the user interface and using the venue simulator. 

## Ingenium Reference Scripts

Ingenium provides a suite of tools and python packages to aid interfacing and querying Ingenium.

These tools include:

Ingenium Integrated Report
Ingenium V&V Report
Ingenium Procedure Validator 
Ingenium python libraries for Custom Script development (similar to mtak library but using Ingenium functions)

These libraries can be used by themselves or extended by projects for project specific applications.

Open Ingenium Note: Ingenium References Scripts are being refactors into [Ingenium-lib](https://github.com/OpenIngenium/ingenium-lib)

## Deployment

This is a suite of tools that will deploy Ingenium to either test or production venues.

## Venue Simulator

This is a mock of the venue server that is used for automated tests. It does not communicate with AMPCS or custom scripts. Instead it allows test codes to change the expected behavior of its API to implement tests.