---
# draft: true 
date: 2026-11-10
categories:
  - Release
---

# R15.1 Released

Ingenium R15.1.0 has been released. This includes the following changes:

- Update of majority of services to use LTS versions of languages and frameworks
- Migration of all testing from integration tests (relying on Jenkins and JPL infrastructure) to unit/mocked tests executed via GitHub Actions
- Removal of Django from the UI service
- Removal of Handsontable from the UI service (replaced with Tabulator)
- Updated dictionary format to allow for command argument names and default values
- Updated command builder that will allow user to populate command arguments with default values
- Adding project configuration to support custom command string formats

Last but not least, this release completes our transition from internal development to open source development. All future development will be open source first and we look forward to your feedback and contributions.