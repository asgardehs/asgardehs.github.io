---
layout: docs
title: Database Design
permalink: /docs/odin/database/
---

[Back to Odin docs](/docs/odin/)

# Database Design

Single SQLite database (`odin.db`) with 132+ tables across core infrastructure,
three MVP compliance modules, and the Schema Builder. All user data is scoped by
`establishment_id`.

## Core (Migration 000)

Foundation tables shared by all modules.

| Table             | Purpose                                    |
| ----------------- | ------------------------------------------ |
| `establishments`  | Facilities (name, address, NAICS)          |
| `employees`       | Personnel records                          |
| `settings`        | App configuration (current establishment)  |
| `audit_log`       | Change audit trail across all modules      |
| `module_config`   | Which modules are enabled per establishment|

---

## Incidents Module (Migration 001)

**Regulatory coverage:** OSHA 300, 300A, 301

| Table                  | Purpose                                       |
| ---------------------- | --------------------------------------------- |
| `incidents`            | Core incident records with case numbers       |
| `corrective_actions`   | Actions linked to incidents                   |
| `injury_illness_types` | Reference: OSHA classification codes          |
| `body_parts`           | Reference: injured body parts                 |
| `osha_300a_summaries`  | Annual summary data for Form 300A             |

Case numbers auto-generated as YYYY-NNN. Recordability determined from OSHA
classification rules in the service layer. Reports assemble data in exact OSHA
form structure.

---

## Chemicals Module (Migration 002)

**Regulatory coverage:** OSHA HazCom, EPA Tier II (EPCRA 311/312), SARA 313/TRI

| Table                    | Purpose                                    |
| ------------------------ | ------------------------------------------ |
| `storage_locations`      | Where chemicals are stored                 |
| `chemicals`              | Chemical records (CAS, name, GHS flags)    |
| `chemical_components`    | Component breakdown per chemical           |
| `sds_documents`          | SDS tracking with revision linkage         |
| `chemical_inventory`     | Snapshots of quantities on hand            |
| `ghs_hazard_statements`  | Reference: GHS H-codes                     |
| `ghs_pictograms`         | Reference: GHS pictogram codes             |
| `tier2_reports`          | Tier II annual report data                 |
| `sara313_chemicals`      | TRI-reportable chemical tracking           |
| `tri_annual_activity`    | TRI Form R annual activity data            |
| `tri_releases_transfers` | TRI release and transfer quantities        |
| `tri_reports`            | TRI report submissions                     |

Key views: `v_current_inventory`, `v_tier2_reportable`, `v_sds_review_status`,
`v_tri_reportable_chemicals`. GHS hazard flags stored as booleans for fast
Tier II threshold queries.

---

## Training Module (Migration 003)

**Regulatory coverage:** HazCom, respiratory protection, PPE, LOTO, forklift,
confined space, emergency procedures, DOT HazMat

| Table                    | Purpose                                    |
| ------------------------ | ------------------------------------------ |
| `training_courses`       | Course definitions with duration/frequency |
| `course_requirements`    | Which regulations require which courses    |
| `training_completions`   | Individual completion records              |
| `training_assignments`   | Assigned but not yet completed             |
| `employee_activities`    | Job activities that drive requirements     |
| `work_areas`             | Facility areas with associated hazards     |
| `employee_work_areas`    | Employee-to-area assignments               |
| `activity_codes`         | Reference: standardized activity codes     |

Three requirement paths: all-employees, activity-based, and hazard-exposure.
10+ views for gap analysis and compliance summary. Triggers auto-calculate
expiration dates.

---

## Post-MVP Modules (Schema Designed)

### Waste Management (Migration 002 extension)
RCRA container-centric tracking. Generator status determination from 12-month
history. Manifest tracking with LDR notifications. Tables: `waste_codes`,
`waste_streams`, `waste_containers`, `accumulation_areas`, `waste_manifests`.

### Inspections & Audits (Migration 003 extension)
EPA SWPPP/SPCC, ISO 14001/45001/50001, OSHA equipment inspections. Pre-seeded
checklists. Year-based CAR numbering. 5-Why root cause analysis. Tables:
`inspections`, `inspection_findings`, `audits`, `audit_findings`,
`corrective_actions` (CAR system).

### Permits & Licenses (Migration 004)
Clean Air Act Title V, Clean Water Act, RCRA, state permits. Conditions
(text) and limits (numeric) tracked separately. 17 permit types, 24 license
types with CE tracking. Compliance calendar views.

### Emissions (Migration 005–006)
AP-42 emission factors, MAERS, state air reporting. Material usage as anchor
with source-specific detail tables (welding, coating, combustion). Calculation:
usage x factor x (1 - control efficiency). Annual inventory rollup.

### Water (Migration 007)
NPDES DMR, pretreatment permits. Configuration layer (locations, parameters,
monitoring requirements) + operational layer (sampling events, results). Lab
submission grouping, equipment calibration tracking.

---

## Design Patterns

All modules follow consistent patterns:

- **Reference data pre-seeded** — regulatory codes, hazard classifications
- **Views for reporting** — complex queries expressed as SQL views
- **Triggers for automation** — expiration calculation, status transitions
- **Audit trail** — all changes logged with old/new values
- **Establishment scoping** — every user data table filters by facility
- **Regulatory-first fields** — columns map directly to form requirements
