---
title: V&V Integration
---

# V&V Integration

## Integration Functionality

Ingenium was designed to support a tighter integration between V&V activities and procedure authoring/execution/data review. This is implemented by the following capabilities.

### Access to Project Requirements and V&V Information

Ingenium allows projects to load requirements with V&V categorical information (VA/VAC) into a local database for easy reference.

This is described in [Project Configuration](deployment_and_config.md#verification-items) and is supported by an Ingenium reference script (VI XML Builder).

!!! note "Open Ingenium Note"
    The VI XML Builder reference script has not been published to [OpenIngenium](https://github.com/OpenIngenium){target="_blank"} yet.

### Ability to Map Requirements to Procedures

Procedure authors can map requirements (that have been imported) to their procedures via the [Verification Item and Status Steps](built_in_steps.md#verification-item-and-status-steps).

When an author adds/edits verification items, Ingenium stores a copy of the verification item with the procedure to ensure traceability if the requirement changes.

### Ability to Map Steps to Requirements

Once requirements have been mapped to a procedure they can be related to steps within the procedure via the [Verification Item and Status Steps](built_in_steps.md#verification-item-and-status-steps). This creates a derived status that can be used to speed up/sharpen data review of as-runs (as engineers can spend their time reviewing telemetry vs. aggregating VI Pass/Fail information).

### Ability to Report on Requirement Status

As this information is stored with each Ingenium procedure and execution it can be accessed and used to create various reports to support both I&T and V&V efforts.

Ingenium provides reference versions of reporting scripts that can be used and/or modified by projects (see [Ingenium Reference Scripts](../architecture.md#ingenium-reference-scripts)).

#### Ingenium V&V Report

This queries an Ingenium execution and creates a Microsoft Excel report on the derived status of any VI addressed by that execution.

!!! warning "Screenshot needed"
    An example Ingenium V&V Report (Excel) showing VIs and their derived status.

#### Ingenium Procedure Validator

This queries an Ingenium procedure and checks the following things:

- If VIs referenced by the procedure have changed
- If procedures referenced by the procedure have been updated
- (optionally) If Flight dictionary content is still valid for the specified dictionary
- (optionally) If SSE dictionary content is still valid for the specified dictionary

See also the in-application [Procedure Validation](editing_procedure_execution.md#procedure-validation) tab.

## How to Use these Features

This will be somewhat project specific but a general outline is as follows:

- System engineers (VA/VAC owners) map requirements to VAs in their source database
- The project extracts this information from the requirements source and imports it into Ingenium
- I&T engineers "declare" requirements in the procedures (mapping requirements to procedures)
    - Note that sometimes it is helpful to map all incoming requirements at the beginning of a procedure, and then again in each test case. There is no issue with doing this.
- As I&T engineers build test cases they map associated requirements to key steps in those test cases.
    - Note that if individual sections/test cases of a procedure are intended to be imported into other procedures (e.g. for regression) it is helpful to capture both the declaration and mapping of the requirements within that section/test case.
- When the procedure is executed the associated requirements get a derived status (e.g. PASS/FAIL)
- System engineers and I&T engineers utilize Ingenium reporting tools to support their data review (V&V Report)
- Projects may elect to include features from [Procedure Review and As-Run Review and Closeout](reporting_and_search.md#procedure-review-and-as-run-review-and-closeout) to support their data review
    - Data Review / Activity Report comments and summaries
    - Ingenium Integrated Report
