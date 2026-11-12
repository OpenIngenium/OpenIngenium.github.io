---
title: User Guide
---

# User Guide

Welcome to the Ingenium User Guide.

!!! info "Content needed"
    The original user guide opened with an "Ingenium Overview Video". A publicly hosted overview video (or a replacement walkthrough) is needed here.

| Ingenium Page | Description |
| ------------- | ----------- |
| [Procedure Creation and Configuration Management](procedure_creation_cm.md) | How Ingenium manages saved procedures, tags, and the typical procedure flow from authoring to execution |
| [Editing a Procedure / Execution](editing_procedure_execution.md) | How to edit a procedure and/or execution |
| [Built-in Steps](built_in_steps.md) | Detailed description of the steps that Ingenium can perform in a procedure |
| [Executing a Procedure](executing_a_procedure.md) | Overview of the Ingenium Dashboard, starting and watching an execution, Running for Score and Redlines |
| [Reporting and Search](reporting_and_search.md) | Reports, search, and review of in progress or completed executions |
| [Import/Export](import_export.md) | Transferring procedures between Ingenium instances |
| [V&V Integration](vnv_integration.md) | Mapping requirements and verification items to procedures and steps |
| [Step Development](step_development.md) | Extending Ingenium with project specific Custom Scripts |
| [Administration](administration.md) | How to configure Ingenium, manage venues and users |
| [Deployment and Configuration](deployment_and_config.md) | Project Configuration (dictionaries, verification items, scripts) and project specific settings |

!!! info "Content needed"
    The original guide also referenced "Support, Training, Frequently Asked Questions", "Venue Configuration" (tracking the configuration of each venue within Ingenium) and "Venue Details" pages that were not included in the source document.

## Ingenium FAQ

### Failure to Locate Telemetry / Correct Values

This is a complicated issue as it involves both procedure timing and vehicle configuration.

- Make sure the time range you are querying telemetry over will capture the telemetry you are interested in
    - Use Ingenium time constants (`CURRENT_TIME`, `LAST_STEP_END`). See [Time Formats and References](built_in_steps.md#time-formats-and-references).
- Make sure your wait times are aligned with your telemetry selection criteria
    - If the channel is being downlinked every 30 seconds, you will need to wait longer than that to get a successful measurement
- Make sure your downlink rate is sufficient to get telemetry
    - If your downlink rate is very low it will add latency and uncertainty in the telemetry stream

## Ingenium Idiosyncrasies

### Formatting Text Pasted from Other Applications

Ingenium uses a 3rd party application ([Froala](https://froala.com/){target="_blank"}) for text editing. It is able to handle formatting from other applications (e.g. Word, Excel, etc.) to a point. Observed issues related to this are as follows:

- Format of pasted content is not always editable (e.g. background color)
- Format of pasted content may not exactly match source content (it is effectively best effort)

As these issues are with the underlying library it is unlikely for Ingenium to directly address them. We will incorporate updates to the library and, if issues increase, will consider alternative solutions.

!!! tip "Workaround"
    Use the "Paste as plain text" option and format content within Ingenium.

### Pasting from MS Excel to Data Grids (### or missing data)

When copying and pasting from MS Excel to Ingenium data grids please ensure that the Excel cell width is sufficient to display the data contained within.

Pasting data that Excel has truncated because of cell size will result in incorrect values in your data grid. This seems to vary from `###` (Windows) to missing/truncated data (Mac).

This is a limitation with the 3rd party library that we use (Handsontable) to implement Excel like content in a webpage.

To work around this issue, ensure that all your data is visible in the Excel spreadsheet you are copying from.

### Re-Execution of Steps

Ingenium allows test conductors to re-execute steps. While Ingenium only displays the most current version it captures the results of all executions. These are displayed in the PDF and Ingenium Integrated Report output.

!!! warning "Do this in moderation"
    We have seen users re-execute entire procedures (repeatedly) in the same execution. This is not recommended as it can create extremely large executions and negatively impact performance.

### Copy/Paste from another Website

The descriptions of each step are stored as HTML. Copying content from another website and pasting it into step descriptions can inadvertently paste HTML that is rendered by the text editing application in strange ways.

!!! tip "Paste as Plain Text"
    When pasting content from another website, it is recommended to paste as plain text to avoid embedding HTML into the text description.

### Image Resolution When Copied from MS Word

Microsoft Word does not retain the full resolution of images inserted into Word documents. Pasting images from MS Word into Ingenium can result in blurry/lower resolution images. To work around this issue please use a full resolution source for the images you insert into Ingenium.

### Text Editor Issues

Ingenium uses a third party library to handle rich text editing ([Froala](https://froala.com/){target="_blank"}). This library has some limitations and we can not directly fix issues with the library. We will track them and explore alternate implementations if the number of issues increases.

- Can not edit image captions
    - Clicking on an image caption triggers a "blur" or "click away" event that Ingenium uses to indicate when to save the field in question
    - Support ticket filed with Froala
    - Workaround: right click on the image and edit the caption (this does reset your captions)
- Can not select rows / columns in tables
    - Currently not supported by Froala but on their roadmap
