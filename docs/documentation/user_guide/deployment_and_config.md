---
title: Deployment and Configuration
---

# Deployment and Configuration

## Project Configuration

The "Project Configuration" page in Ingenium holds a variety of information that is specific to each project. This includes Flight and SSE Dictionaries, Verification Items, and project Custom Scripts.

### Flight & SSE Dictionaries

Ingenium allows the user to load project command and telemetry dictionaries for both the flight system and the simulation and support equipment (SSE). Ingenium will then use this information to assist in procedure creation and validation with features like autocomplete and an integrated command builder. Ingenium ingests and supports Command, EHA, and EVR dictionaries.

!!! note
    Ingenium dictionaries are limited to providing an enhanced user interface. The actual dictionary/FSW of the vehicle is governed by the version of FSW used and the dictionary used with the ground data system (GDS).

Dictionaries are accessed via the top menu bar "Project Configuration" and are further subdivided into "Flight Dictionaries" and "SSE Dictionaries". Selecting the type of dictionary will display a list of existing dictionaries that have been defined in Ingenium. Clicking on the name of an individual dictionary will allow the user to browse and search the contents of that particular dictionary.

!!! warning "Screenshot needed"
    The Project Configuration menu expanded, and the Flight Dictionaries list showing several dictionaries with their states.

!!! warning "Screenshot needed"
    The contents of an individual dictionary being browsed/searched (e.g. the command list).

#### Dictionary States

Each dictionary defined in Ingenium has a user controlled state that governs its use and availability in the rest of the application.

| State | Description |
| ----- | ----------- |
| Not Published | Used to indicate the dictionary is not complete and not ready for use in execution and authoring. |
| Published | Used to indicate that the dictionary is complete. Can be used in execution and authoring. |
| Released | Used to indicate that the dictionary is formally released. Can be used in execution and authoring. |
| Retired | Used to indicate that the dictionary is no longer in active use. Preferable to deletion to maintain backwards compatibility with older tests. |

!!! note
    Only dictionaries with the state **PUBLISHED** or **RELEASED** can be used to author or execute procedures.

#### Dictionary Adding/Modification

As dictionaries are often in flux in the test environment, Ingenium allows projects and users to define any number of dictionaries. To define a dictionary in Ingenium a user with the `config_mgmt` permission clicks on the "Add Dictionary" button in either the FLIGHT or SSE dictionary view.

!!! warning "Screenshot needed"
    The "Add Dictionary" dialog showing the Version, State, Choose File and Description fields.

| Field | Description |
| ----- | ----------- |
| Version | The unique ID of the dictionary (e.g. `m20cedl-pkg-C3_0_0_20170831`). **Note: cannot be changed.** *Recommendation: use the same naming convention used to identify dictionaries in GDS/FSW.* |
| State | Initial state of the dictionary. |
| Choose File | Will accept either an XML file or a tar.gz archive of the entire dictionary. Note that this interface only accepts a single file so if individual XML files are loaded the dictionary will need to be modified after creation to load each one. |
| Description | Description of the dictionary. Used for searching and context. |

##### File Names

Ingenium uses the XML file names to determine what dictionary you are trying to upload. Please make sure your files are named accordingly (or Ingenium will not be able to parse them):

| Dictionary | File Name |
| ---------- | --------- |
| Command Dictionary | `command.xml` |
| EVR Dictionary | `evr.xml` |
| EH&A Dictionary | `channel.xml` |
| 1553 Dictionary | `mil1553.xml` |

Ingenium will ingest the provided dictionary files and parse them according to the dictionary parser defined in the [Administration](administration.md#parsers) configuration page. It is expected that the provided dictionary is valid and follows the project schema. Modifying a dictionary utilizes a very similar interface except that the version is not editable. If a user decides to re-load XML content into an existing dictionary it will **append** to the content stored in Ingenium.

After the user clicks the create (or modify) button the XML file will be uploaded and parsed by Ingenium. This could take a few seconds based on the size and number of elements in the file. Ingenium will then display a window indicating the number of elements found in the dictionary.

!!! warning "Screenshot needed"
    The window displayed after a dictionary upload, showing the number of elements found.

!!! note "No OPCODES"
    Ingenium extracts a common, multi-mission data set from each dictionary and **does NOT store Op Codes** (only command stems).

### Verification Items

Ingenium holds a cached copy of project Verification Items (VIs), mapped to verification activities (VAs) and verification activity collections (VACs). These are used in procedures to track which VIs are being addressed (and their derived status after execution). See [V&V Integration](vnv_integration.md) for more information.

!!! note "Official VI Sources"
    Ingenium is not the official source for Verification Items. Ingenium caches Verification Items for ease of use and efficiency. It is up to projects to keep the verification items updated.

Unlike dictionaries there is only a single version of verification items inside Ingenium. This is updated by uploading an XML file containing the verification items into Ingenium. Depending on the options checked it will update or replace existing content.

| Update Method | Effect |
| ------------- | ------ |
| Update | Update verification items in place. Update existing VIs, create new VIs for new content. |
| Add | Delete all verification items and replace with uploaded content. |

!!! warning "Screenshot needed"
    The Project Configuration → Verification Items page, including the upload dialog with the Update/Add options.

!!! note "Open Ingenium Note"
    The Ingenium V&V XML format (schema) and the reference scripts for translating requirements database exports into the Ingenium format have not been published to [OpenIngenium](https://github.com/OpenIngenium){target="_blank"} yet.

!!! tip "ProTip"
    As the V&V sync with other databases is a manual process there is likely latency between V&V mapping (VI → VA, etc.) and that being reflected in Ingenium. Check if the mapping exists in the source requirements database; if it does, remind your Ingenium administrator to perform the sync.

### Custom Scripts

Custom scripts are project specific (and authored) scripts that can be invoked by Ingenium in procedures. These must be managed both on the venue computers and inside Ingenium. Their capabilities, limitations, and formats are discussed in [Step Development](step_development.md).

Custom scripts can be added or modified by uploading the appropriate definition XML file.

!!! warning "Screenshot needed"
    The Project Configuration → Scripts list showing registered custom scripts and the upload/add control.

## Project Specific Settings

There are a number of settings that are specific to each project. They are controlled via deployment scripts and environment variables and require either project GDS or Ingenium team members to update.

### Command Verification Settings

!!! info "Content needed"
    This section was empty in the source document. Describe the project configurable command verification settings.

### Ingenium Custom Scripts Directory

In order to execute custom scripts, Ingenium must have a designated custom script directory where they are stored. It is recommended that projects specify a shared location, under which each test team can manage their scripts.

!!! info "Content needed"
    Add the default/recommended custom script directory locations.

### Application Users

Each instance of Ingenium on an agent machine allows for an additional Ingenium "Venue" to target that particular server. This is commonly used to allow multiple simultaneous simulator sessions per server. It can also be used to enable parallel operations. These are primarily transparent to the user but it is important to recognize that Ingenium interacts with local files/scripts as the application user (so set permissions accordingly).

!!! info "Content needed"
    Add the list/naming convention of the application users.

### Local Log File

On each agent machine, Ingenium has a detailed log file indicating all interactions between the Ingenium server (website) and the agent (client machine). This log file can be useful for users to troubleshoot more complex issues.

!!! info "Content needed"
    Add the log file locations.

## Procedure Storage

- Procedure/as-run content is relatively small (a few MB)
    - These are stored in the Ingenium ArangoDB
- The majority of storage space is driven by attached files / images
    - These are stored either in Amazon S3 storage (effectively bottomless) or on project provided storage (controlled via project/GDS)
- Projects could choose to archive/delete content (e.g. WSTS/non run for score executions) via an external script.

See [Architecture](../architecture.md) for more information.
