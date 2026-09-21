# Portfolio Project - Technical Documentation (Stage 3)

## Project Name
**Network Assessment MVP**

## Author
**Eugenio Martinez**

## Version
1.0

## Date
September 2026

---

# 0. User Stories and API Interactions

## Purpose

The purpose of this section is to define the MVP features from the user’s perspective and establish clear priorities for development.

Since this MVP is focused on network assessment automation and reporting, mockups are not required at this stage. The project will initially operate through command-line interfaces, APIs, and generated reports rather than a graphical user interface.

## Prioritized User Stories (MoSCoW)

### Must Have

#### US-001  **As an IT technician, I want to discover all devices on a network so that I can create an accurate inventory.**

#### US-002  **As an IT technician, I want to scan open ports and services so that I can identify potential security risks.**

#### US-003  **As an administrator, I want to generate a network assessment report so that I can document findings and recommendations.**

#### US-004  **As a network analyst, I want to evaluate basic performance metrics so that I can identify bottlenecks and capacity issues.**

### Should Have

#### US-005  **As an administrator, I want to review device configurations so that I can identify misconfigurations and compliance issues.**

#### US-006  **As an analyst, I want to detect operational risks so that I can proactively address infrastructure weaknesses.**

### Could Have

#### US-007  **As an administrator, I want automated remediation recommendations so that I can accelerate corrective actions.**

#### US-008  **As a technician, I want historical assessment comparisons so that I can track improvements over time.**

### Won’t Have (MVP)

#### US-009  Real-time monitoring dashboard

#### US-010  AI-assisted threat detection

#### US-011  Cloud environment assessment

---

# 1. System Architecture

## Purpose

The purpose of the architecture design is to define how the major components of the system interact while ensuring maintainability, scalability, and modular development.

## High-Level Architecture Diagram

```text
+-----------------------+
|    Network Devices    |
| PCs, Routers, Switches|
| Servers, Printers     |
+-----------+-----------+
            |
            v
+-----------------------+
|  Discovery Module     |
| Device Inventory      |
+-----------+-----------+
            |
            v
+-----------------------+
|  Assessment Engine    |
|-----------------------|
| Security Analysis     |
| Performance Analysis  |
| Config Review         |
| Risk Assessment       |
+-----------+-----------+
            |
            v
+-----------------------+
|   Data Storage Layer  |
| SQLite / JSON Files   |
+-----------+-----------+
            |
            v
+-----------------------+
| Reporting Module      |
| Markdown / PDF Output |
+-----------------------+
```

## Data Flow

1. Network devices are discovered.
2. Inventory data is collected.
3. Security analysis is performed.
4. Performance metrics are collected.
5. Configuration reviews and risk assessments are executed.
6. Results are stored in the data layer.
7. Reports are generated and exported.

---

# 2. Components, Classes, and Database Design

## Purpose

This section defines the internal structure of the MVP, including its components, classes, and database schema.

## Core Components

### Inventory Module

**Purpose:**
Discover devices and maintain an accurate inventory.

#### Class: DeviceDiscovery

**Attributes**
- subnet
- active_hosts
- scan_results

**Methods**
- discover_devices()
- identify_os()
- collect_device_info()

---

### Security Module

#### Class: SecurityScanner

**Attributes**
- target_host
- open_ports
- vulnerabilities

**Methods**
- scan_ports()
- check_services()
- evaluate_security()

---

### Performance Module

#### Class: PerformanceAnalyzer

**Attributes**
- latency
- jitter
- bandwidth

**Methods**
- run_tests()
- analyze_metrics()

---

### Configuration Module

#### Class: ConfigurationAnalyzer

**Attributes**
- config_file
- findings

**Methods**
- parse_configuration()
- validate_settings()

---

### Reporting Module

#### Class: ReportGenerator

**Attributes**
- findings
- recommendations

**Methods**
- build_report()
- export_markdown()
- export_pdf()

---

## Database Design

### Selected Technology

SQLite

### Assessments Table

| Field | Type |
|---------|---------|
| id | INTEGER |
| assessment_date | DATETIME |
| network_range | TEXT |
| overall_score | INTEGER |

### Devices Table

| Field | Type |
|---------|---------|
| id | INTEGER |
| assessment_id | INTEGER |
| ip_address | TEXT |
| hostname | TEXT |
| device_type | TEXT |
| operating_system | TEXT |

### Vulnerabilities Table

| Field | Type |
|---------|---------|
| id | INTEGER |
| device_id | INTEGER |
| severity | TEXT |
| finding | TEXT |
| recommendation | TEXT |

### Performance Metrics Table

| Field | Type |
|---------|---------|
| id | INTEGER |
| device_id | INTEGER |
| latency | REAL |
| jitter | REAL |
| packet_loss | REAL |

### Entity Relationship Diagram

```text
ASSESSMENTS (1)
      |
      |
      v
DEVICES (Many)
      |
      |
      +----------------------+
      |                      |
      v                      v
VULNERABILITIES      PERFORMANCE_METRICS
```

---

# 3. High-Level Sequence Diagrams

## Purpose

This section illustrates how the main system components interact during critical processes.

## Use Case 1: Device Discovery

```text
User
 |
 | Start Scan
 v
Discovery Module
 |
 | Scan Subnet
 v
Network Devices
 |
 | Return Responses
 v
Discovery Module
 |
 | Store Results
 v
Database
```

---

## Use Case 2: Security Assessment

```text
User
 |
 v
Security Module
 |
 | Scan Ports
 v
Target Device
 |
 | Service Information
 v
Security Module
 |
 | Save Findings
 v
Database
```

---

## Use Case 3: Report Generation

```text
User
 |
 v
Reporting Module
 |
 | Retrieve Findings
 v
Database
 |
 | Return Data
 v
Reporting Module
 |
 | Generate Report
 v
Markdown/PDF Output
```

---

# 4. API Specifications

## Purpose

This section documents both the external technologies used by the project and the internal API endpoints that will support the MVP.

## External APIs and Services

### Nmap

**Purpose**
- Network discovery
- Port scanning
- Service enumeration

### SNMP

**Purpose**
- Inventory collection
- Performance monitoring
- Device information gathering

### Syslog

**Purpose**
- Log collection
- Event analysis
- Troubleshooting support

---

## Internal API Endpoints

### Endpoint: Discovery Scan

| Item | Value |
|--------|--------|
| Path | `/api/v1/discovery` |
| Method | POST |
| Input | JSON |
| Output | JSON |

**Request**

```json
{
  "subnet": "192.168.1.0/24"
}
```

**Response**

```json
{
  "status": "success",
  "devices_found": 18
}
```

---

### Endpoint: Security Scan

| Item | Value |
|--------|--------|
| Path | `/api/v1/security` |
| Method | POST |
| Input | JSON |
| Output | JSON |

**Request**

```json
{
  "target": "192.168.1.100"
}
```

**Response**

```json
{
  "open_ports": [22, 80, 443],
  "risk_level": "medium"
}
```

---

### Endpoint: Report Generation

| Item | Value |
|--------|--------|
| Path | `/api/v1/report` |
| Method | POST |
| Input | JSON |
| Output | JSON |

**Request**

```json
{
  "assessment_id": 1
}
```

**Response**

```json
{
  "report_file": "assessment-report.md"
}
```

---

### Endpoint: Retrieve Assessment

| Item | Value |
|--------|--------|
| Path | `/api/v1/assessments/{id}` |
| Method | GET |
| Input | URL Parameter |
| Output | JSON |

**Response**

```json
{
  "assessment_id": 1,
  "overall_score": 87
}
```

---

# 5. SCM and QA Strategies

## Purpose

The purpose of this section is to establish standards for source control, collaboration, testing, and quality assurance.

## Source Control Management (SCM)

### Repository

Git + GitHub

### Branching Strategy

```text
main
│
├── development
│
├── feature/inventory
├── feature/security
├── feature/performance
├── feature/configuration
├── feature/risks
└── feature/reporting
```

### Commit Strategy

- Small and focused commits
- Daily code submissions
- Meaningful commit messages
- Frequent synchronization with development branch

### Code Review Process

Although the project is maintained by a single developer, pull requests will be used before merging into the main branch to encourage review and change tracking.

---

## Quality Assurance (QA)

### Unit Testing

Tools:
- unittest
- pytest

Coverage Areas:
- Device discovery
- Security scanning
- Performance analysis
- Configuration review
- Reporting engine

### Integration Testing

Validate interactions between:

- Discovery Module → Database
- Security Module → Database
- Performance Module → Database
- Database → Reporting Module

### Manual Testing

Testing environments:

- Home lab
- Virtual machines
- Simulated network environments

### Deployment Workflow

```text
Development
      |
      v
Testing
      |
      v
Release Candidate
      |
      v
Production Release
```

---

# 6. Technical Justifications

## Purpose

This section explains the rationale behind technology and design decisions made for the MVP.

### Python

Chosen because:

- Excellent networking ecosystem
- Strong automation capabilities
- Large community support
- Fast development cycle

### SQLite

Chosen because:

- Lightweight database
- Minimal configuration requirements
- Perfect fit for MVP scale
- Easy backup and portability

### Nmap

Chosen because:

- Industry-standard network scanner
- Reliable and mature technology
- Easily integrated with Python workflows

### Modular Architecture

Chosen because:

- Easier maintenance
- Better scalability
- Clear separation of responsibilities
- Independent testing of modules

### Report-Centered Design

Chosen because:

- Aligns with the primary project objective
- Produces actionable documentation
- Delivers professional assessment results
- Supports future expansion into dashboards and monitoring

---

# Final Technical Documentation Summary

This Technical Documentation includes:

- User Stories and Prioritization
- System Architecture
- Components and Classes
- Database Design
- Sequence Diagrams
- API Specifications
- SCM Strategy
- QA Strategy
- Technical Justifications

### Author : Eugenio Martinez