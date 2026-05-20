# Public/Internal Audit Preparation Notes

## 1. Documentation Focus
Audits place strong emphasis on the completeness, accuracy, and currency of documentation, including:

- Standard Operating Procedures (SOPs)
- Training records and documentation
- Staff competency assessments and records
- Evidence of staff authorization/qualification to run pipelines
- Software installation, maintenance, and update records
- Troubleshooting and incident documentation

### Potential auditor questions:
- How is staff training conducted and documented?
- How is competency assessed, validated, and recorded?
- Where are training and competency records stored?
- How is documentation maintained to ensure it remains current?

---

## 2. Pipeline Workflow and Analysis Process
Auditors will expect a clear, end-to-end explanation of the full analysis workflow, from data receipt to final reporting.

### You should be prepared to explain:
- How sequencing/data are received and ingested
- Initial data handling and preprocessing steps
- How analysis pipelines are initiated and executed
- Downstream processing and interpretation steps
- Intermediate outputs and final deliverables
- QC metrics, thresholds, and acceptance criteria
- Actions taken when QC thresholds are not met
- Final report generation and dissemination process

### Possible auditor requests:
- Live demonstration of pipeline execution
- Review of log files and run outputs
- Explanation of intermediate and final results
- Description of failure handling and troubleshooting steps

---

## 3. Bioinformatics Tools and Components
Auditors may ask detailed technical questions regarding pipeline architecture and tool usage, particularly for systems such as PathogenSeq.

### Topics may include:
- Pipeline structure and components
- QC metrics and evaluation logic
- Tools used for different data types and analyses
- Rationale for tool selection
- Software version control and dependency management
- Database structure, updates, and validation

### Key expectations:
- Version tracking and reproducibility
- Validation of tools and pipelines
- Documentation of database updates and integrity checks
- Maintenance procedures for software and dependencies

---

## 4. Data Storage and Backup
Auditors will assess how data are stored, secured, and protected over time.

### Likely questions:
- Where and how data are stored (systems and locations)
- Backup frequency and retention policies
- Data access controls and user permissions
- Disaster recovery and redundancy strategies
- Data lifecycle and archival procedures

### Be prepared to describe:
- Storage architecture
- Backup schedules and verification processes
- Access governance and security controls

---

## 5. Pipeline Updates and Change Management
Change control is a major focus, particularly for clinically relevant outputs.

### Potential questions:
- How pipeline updates are implemented and validated
- How changes are communicated to users and stakeholders
- Whether re-analysis is performed after updates (e.g., lineage/database changes)
- How versioning is tracked and documented

### Key focus areas:
- Change control procedures
- Validation after updates
- Reproducibility of historical results
- Impact assessment on clinical reports

Auditors will expect clear evidence that changes are controlled, documented, and traceable.

---

## 6. Logging, Traceability, and Databases
Auditors will examine how system activity and analytical processes are tracked and reviewed.

### Topics may include:
- EBS database or equivalent tracking systems
- Content and structure of stored metadata
- Log generation, storage, and retention policies
- Audit trails for analyses and user actions
- Investigation of errors and anomalies

### Likely questions:
- How are logs reviewed and by whom?
- How long are logs retained?
- How is traceability maintained from raw data to final report?
- How are incidents investigated and documented?

---

## 7. Overall Audit Emphasis
The audit primarily evaluates the robustness and integrity of the entire bioinformatics and reporting framework, with emphasis on:

- Documentation completeness and accuracy
- End-to-end traceability of analyses
- Reproducibility of results
- Quality control systems and thresholds
- Change and version management
- Data governance and security
- Staff training and competency assurance
- Understanding of full pipeline workflow
- Impact of analytical results on clinical reporting
