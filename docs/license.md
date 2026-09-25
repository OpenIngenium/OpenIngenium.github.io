---
title: License
hide:
  - navigation
---

# License

Open Ingenium is is released under the Apache 2.0 License.

## Closed Source Dependencies 

Ingenium currently has several dependencies on licensed software. Users will need to procure appropriate licenses or make modifications to use alternates approaches. 

This includes:

- [Froula (WYSIWYG Text/HTML Editor)](https://froala.com/)
-- Planned for removal in R16.0
- [HandsonTable (Web DataGrids)](https://handsontable.com/)
-- Removed in R15.1.0 (in work)
- [NASA Advanced Multi-Mission Operations System (AMMOS) Mission Data Processing and Control System (MPCS) or AMPCS](https://github.com/NASA-AMMOS)
    - Note that AMPCS has not been released as open source.
    - Ingenium users will need to either modify venueserver code or create custom scripts to use alternate Ground Data Systems.
    - This dependencies has been resolved in R15.0 (use the [venue-agent](https://github.com/OpenIngenium/venue-agent))