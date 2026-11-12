---
title: Procedure Creation and Configuration Management
---

# Procedure Authoring and Configuration Management

## Introduction

Ingenium provides an integrated authoring and configuration management system to develop procedures prior to their execution. This system allows procedures to be defined, authored, versioned and referenced by other procedures and Ingenium executions.

!!! note
    Ingenium only provides configuration management (versioning) and not configuration control (approval, formal release).

!!! note "Open Ingenium Note"
    The original guide linked to an internal video tutorial for procedure authoring. A public video tutorial has not been published yet.

## Overview

Ingenium procedure authoring and configuration management can be accessed via the top level menu "Procedures". Users are presented a filterable list of procedures and when a procedure is selected (by clicking the procedure id) the detailed version history of that procedure is shown in the lower table.

!!! warning "Screenshot needed"
    The Procedures page showing the filterable procedure table (upper) and the version history of the selected procedure (lower).

## Creation and Modification of Procedures

To create a procedure simply click the "Add" button above the procedure table. It will prompt you for a procedure title and (optionally) an institutional id (e.g. the ID in your project's procedure approval system) and a description. The procedure id is automatically generated and the author is set to the user who created the procedure.

!!! warning "Screenshot needed"
    The "Add" procedure dialog with Title, Institutional ID and Description fields.

The procedure metadata can be modified later by selecting the "Update" option from the "Action" menu for that particular procedure.

!!! warning "Screenshot needed"
    The procedure "Action" menu with the "Update" option.

## Versions

### The Working Version

A key concept of the Ingenium configuration management system is that there is a "working version" and **it is the only version that can be edited**. The working version is periodically saved as a numbered version which can later be flagged as released (if approved in your project's procedure approval system).

!!! warning "Screenshot needed"
    The Working Version row in the version history table with its "Revert to", "Edit" and "Save Version" actions.

#### Revert to

This is a powerful feature that allows a user to **overwrite their working version with any version of any procedure**. This can be used to revert the working version to a prior version of the same procedure or it can be used to load the content of a similar procedure or template procedure into the working version.

#### Edit

Opens the procedure for editing. See [Editing a Procedure / Execution](editing_procedure_execution.md).

!!! note
    Only one user can edit a procedure at once. If a procedure is being edited it will be "locked" to that user and unavailable to other users. This lock is released when the user stops editing or logs off.

#### Save Version

This saves the working version as a new numbered version. It will prompt the user for a brief description of the new version.

### Numbered Versions

Numbered versions are discrete snapshots of the working version. They can be viewed and their metadata can be modified but their content is read-only.

!!! warning "Screenshot needed"
    A numbered version row with its "View", "Release/Unrelease", "Export" and "Delete" actions.

#### View

View the version of the procedure in a read-only view.

#### Release/Unrelease

Update the version metadata to indicate that a given Ingenium version has been released (or not).

!!! info "Content needed"
    The source sentence was truncated ("...has been released (or not) in e."). Confirm which external system the release status refers to.

*Note: Currently Ingenium does not track Red/Blue lines against the release status, but in the future the release status and version content will be used to determine redlines.*

#### Delete

Deletes a procedure version. Administrator only option.

!!! tip "Recommendation"
    Avoid deleting procedure versions, especially if they have been executed.

### Common Action: Export

Allows the user to export the procedure. The "Save as PDF" action provides a PDF copy of the procedure.

!!! info "Content needed"
    The source document states that export is limited to PDF and that a rich (JSON) import/export format does not exist yet. This is out of date: see [Import/Export](import_export.md) for the current tar.gzip procedure import/export capability. Please confirm and update this section.

### Expected Procedure Development Flow in Ingenium

1. User defines a procedure in Ingenium
    1. Inputs title and description
    2. Inputs institutional ID (if designed to be a released procedure)
2. User uses "Revert to" to load a standard project template
3. User begins editing the working version
4. User saves in-progress versions of the procedure (and dry runs them in WSTS)
5. User saves the "finished" version and exports a PDF to the project's procedure approval system
6. User updates version metadata with release information (once approved)
7. User runs the released version of the procedure on the testbed for official V&V

## Tags in Procedures

### Overview

Tags are a way to categorize and select content within procedures. This has a number of use cases including:

- Differentiating content between WSTS/Simulators and Testbed/Hardware
- Identifying hazardous content (marking each step)
- Identifying content for regression

This is addressed in two places: defining the tags within a procedure and selecting the tags to include when running a procedure.

### Tags in Procedure Authoring

Tags are defined on a per procedure basis. This allows flexibility when defining tags and projects can achieve standardization by defining tags within procedure templates.

#### Manage Tags

Tags are defined via the "Manage Tag" menu at the top of procedures.

!!! warning "Screenshot needed"
    The "Manage Tag" menu at the top of a procedure, with the add/edit/remove tag controls.

Tags can be added/removed/edited in this menu. Ingenium currently automatically determines a color scheme for the tag based on the tag name (we are exploring allowing users to define tag colors).

!!! warning "Hazardous Steps"
    Ingenium will specifically mark tags with the name "Hazard" or "Hazardous" with a specific color scheme (white text, red background) and will only use this color scheme for hazardous steps.

Changes to existing tags are propagated throughout the procedure.

- If a tag is modified via the Manage Tag menu it is modified on all steps/sections that have that tag.
- If a tag is deleted via the Manage Tag menu it is deleted on all steps/sections that have that tag.

All the tags defined in a procedure are visible at the top of the screen.

!!! warning "Screenshot needed"
    The tag bar at the top of a procedure showing several defined tags (including a red "Hazardous" tag).

#### Tagging Steps/Sections

Once tags have been defined in a procedure, they can then be added/removed to steps and sections of the procedure via the control in the lower left of the step or section.

!!! warning "Screenshot needed"
    The tag control in the lower left of a step/section with tags applied.

Rules for tagging steps/sections:

- If a section is tagged by a user, all children (steps and sections) are recursively tagged
- If a section is untagged, all children (steps and sections) are recursively untagged
- When a child step or section is untagged, its parents are untagged.
- Ultimately a parent can only have tags that ALL of its children share.
    - This is because children steps/sections can not exist in a procedure without their parents.

### Tags in Run Procedure

Tags are used as a filtering mechanism to select sections and steps in the [Run Procedure step](built_in_steps.md#run-procedure-step). This allows for rapid and complex filtering of which steps/sections of a procedure should be imported and executed.

Note that the step now includes the tags of steps/sections that have been selected.

!!! warning "Screenshot needed"
    A Run Procedure step showing the "Tags Selected" field populated.

Once a procedure and version have been selected, the steps/sections to be included can be chosen via the Tag/Step Selection Modal. Tags are selected on the left and steps/sections are selected on the right.

!!! warning "Screenshot needed"
    The Tag/Step Selection modal with tags on the left and the step/section tree on the right.

Rules for selecting steps/sections with tags:

- If a step/section has more than one tag, ALL tags must be selected for inclusion
- If a user unselects a tag, all steps/sections that have this tag will be unselected
- If a user selects a section, all children steps/sections that can be selected (per tags) are selected (recursive)
- If a user unselects a section, all children steps/sections are unselected (recursive)
- If a user selects a step/section, its parent sections (if any) will be selected (regardless of tags on sections)
- If a user clicks "Select All" all steps that can be selected (per tags) are selected.
- If a user clicks "Unselect All" all steps are unselected.

### Tags In Execution

Tags on steps/sections are read only in execution and can not be modified.

### Filtering on Tags

Procedures can be filtered via tags and these views can be shared via copy/paste of the URL.

!!! warning "Screenshot needed"
    A procedure view filtered by tags (filter control and resulting steps).

!!! note "Filter vs. Procedure Selection Logic"
    Note that the filters operate on a logical OR of all filters. This differs from how the Run Procedure step uses tags (logical AND). It is recommended to use the procedure preview feature of the Run Procedure step to understand exactly what will be imported.

### Previewing Procedures (with tag and step selection)

The same capability that allows for the filtered view of procedures according to tags also supports a "preview" of any Run Procedure step. This allows users to view the specific procedure content that is selected without importing the procedure.

!!! warning "Screenshot needed"
    The preview of a Run Procedure step showing only the selected steps/sections.

## Typical Ingenium Procedure Flow

### Procedure Development

#### Initial Condition

Like any procedure development it is assumed that a user has a test plan and allocated verification activities/items to test before they start writing their procedure.

#### User creates a procedure

To start the process the user would log in to Ingenium, navigate to the "Procedures" menu and create a new procedure. If they have an institutional (procedure approval system) ID they can enter it (or update it later with the ID).

User Guide references: [Creation and Modification of Procedures](#creation-and-modification-of-procedures)

#### User develops their procedure

The user would open the working copy of the procedure and begin populating it with sections and steps in order to meet the test cases defined in the test plan.

The user can save versions of their procedure for review by their colleagues or for dry runs in WSTS/Testbed.

User Guide references: [Editing a Procedure / Execution](editing_procedure_execution.md), [Built-in Steps](built_in_steps.md)

#### User dry runs their procedure in WSTS

The user starts an execution on a WSTS venue and utilizes the "Run Procedure" step to load their saved procedure into the execution.

User Guide references: [Executing a Procedure](executing_a_procedure.md), [Built-in Steps](built_in_steps.md)

#### User reviews results of the dry run

This happens both in Ingenium and with the telemetry / test artifacts available through the ground data system. Users can comment on the as-run of the dry run to help facilitate the review.

User Guide references: [Procedure Review and As-Run Review and Closeout](reporting_and_search.md#procedure-review-and-as-run-review-and-closeout)

#### User updates their procedure and submits it for approval

The user will then update their procedure based on the dry run and save a version to be reviewed and approved via the project's procedure approval system.

User Guide references: [Editing a Procedure / Execution](editing_procedure_execution.md), [Built-in Steps](built_in_steps.md)

#### Procedure is approved

Once the procedure is approved the user updates the metadata of the version that was approved with the release information.

User Guide references: [Versions](#versions)

### Procedure Execution

#### User Runs for Score

The user then starts an execution on a testbed venue.

User Guide references: [Executing a Procedure](executing_a_procedure.md), [Running for Score and Redlines](executing_a_procedure.md#running-for-score-and-redlines)

#### Test Review is performed

This happens both in Ingenium and with the telemetry / test artifacts available through the ground data system. Users can comment on the as-run to help facilitate the review.

User Guide references: [Procedure Review and As-Run Review and Closeout](reporting_and_search.md#procedure-review-and-as-run-review-and-closeout)
