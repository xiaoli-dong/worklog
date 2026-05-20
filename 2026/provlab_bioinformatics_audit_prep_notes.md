# Bioinformatics Audit Prep Notes – ProvLab

# Public/Internal Audit Preparation Notes

## 1. Documentation Focus
The audit heavily focuses on documentation, including:
- SOPs
- Troubleshooting documentation
- Software maintenance records
- Training documentation
- Competency records
- Documentation showing how staff are qualified to run pipelines

Auditors may ask:
- How training is performed
- How competency is assessed and recorded
- Where records are stored
- Whether documentation is up to date

---

## 2. Pipeline Workflow and Analysis Process
Be prepared to walk the auditor through a complete typical analysis workflow step-by-step.

Possible walkthrough topics:
1. How data are received
2. What happens after receiving the data
3. How the analysis pipeline is launched
4. Downstream processing steps
5. Intermediate and final outputs
6. QC metrics, QC thresholds and acceptance criteria, What actions are taken if QC thresholds are not met,
7. Final reporting process

The auditor wants to understand the entire process from raw data to final reported result.

They may ask to:
- Run a program live
- Show program log files
- Explain outputs
- Explain downstream analysis steps
- Explain how failures/issues are identified

---

## 3. Bioinformatics Tools and Components
Possible detailed technical questions about:
- PathogenSeq
- Pipeline components
- QC matrices
- Different tools used for different data types
- Why certain tools are chosen
- How tools are maintained
- How databases are maintained
- How you know when tools or databases need updating

Possible topics include:
- Version tracking
- Database updates
- Tool validation
- Pipeline dependencies

---

## 4. Data Storage and Backup
Auditors may ask:
- Where data are stored
- Physical storage locations
- Backup schedules
- Data retention procedures
- Data security and access control

Be prepared to explain:
- Storage infrastructure
- Backup frequency
- Disaster recovery considerations
- Who has access to the data

---

## 5. Pipeline Updates and Change Management
Questions may include:
- What happens after a pipeline update
- How updates are communicated
- Validation procedures after updates
- Whether re-analysis is required after lineage or database updates
- How version changes are documented

Focus is mainly on:
- Clinical analyses
- Reported results
- Impact of updates on outgoing clinical reports

Auditors want to know:
- How changes are controlled
- How reproducibility is maintained
- How users are informed of updates

---

## 6. Logging, Traceability, and Databases
Possible questions about:
- EBS database
- What information gets stored
- Logging systems
- Audit trails
- Large log file maintenance
- Traceability of analyses

They may ask:
- How logs are reviewed
- How long logs are retained
- How activities can be traced back
- How issues are investigated

Ashwind reportedly maintains extensive log files, so auditors may expect awareness of logging and traceability procedures.

---

## 7. Overall Audit Emphasis
The audit appears to focus primarily on:
- Documentation
- Traceability
- Reproducibility
- Quality control management
- Change management
- Data management
- Staff competency and training
- Understanding of the complete bioinformatics workflow
- Clinical reporting impact
