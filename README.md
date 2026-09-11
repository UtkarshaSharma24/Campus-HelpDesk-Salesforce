# Campus HelpDesk — Salesforce

A Salesforce-based student service request management system designed to manage and track campus support requests across IT Support, Hostel, Library, Transport, and Academic Services.

The application uses Salesforce Custom Objects, Fields & Relationships, Record-Triggered Flows, Validation Rules, Reports, Dashboards, and SOQL to manage the complete service-request lifecycle.

---

## 📌 Project Overview

Campus HelpDesk allows student service requests to be recorded, categorized, assigned, prioritized, tracked, and resolved using Salesforce.

### Request Lifecycle

```text
Student submits Service Request
            │
            ▼
     Validation Rules
            │
            ▼
     Category Evaluation
        │          │
        ▼          ▼
 Department      Priority
 Assignment      Assignment
        │          │
        └────┬─────┘
             ▼
      Status → Assigned
             │
             ▼
      Request Processing
             │
             ▼
       Resolution Date
             │
             ▼
          Resolved
```

---

## ✨ Key Features

* Student and Service Request management
* Category-based department assignment
* Category-based priority assignment
* Automatic status update when staff is assigned
* Automatic resolution-date handling
* Validation rules for data consistency
* Custom list views for request tracking
* SOQL queries for request analysis
* Reports for category, department, priority, and status analysis
* Lightning Dashboard for service-request monitoring

---

## 🧩 Salesforce Components

| Component              | Details                                                                                                                                                    |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Custom Objects         | Student, Service Request                                                                                                                                   |
| Custom Fields          | Student ID, Department, Year, Subject, Description, Category, Priority, Status, Student, Assigned Department, Assigned Staff, Resolution Date, Request Age |
| Relationship           | Student → Service Request (Lookup)                                                                                                                         |
| Record-Triggered Flows | 4                                                                                                                                                          |
| Validation Rules       | 3                                                                                                                                                          |
| Reports                | 5                                                                                                                                                          |
| Dashboard              | Campus HelpDesk Dashboard                                                                                                                                  |
| Query Language         | SOQL                                                                                                                                                       |
| Source Format          | Salesforce DX                                                                                                                                              |
| Development Tools      | VS Code, Salesforce CLI                                                                                                                                    |

---

## 🧱 Custom Objects

### Student

The `Student__c` custom object stores student information used by the helpdesk system.

Main fields:

* Student ID
* Email
* Department
* Year

### Service Request

The main business object is `Request_Number__c`.

Main fields:

* Request Number
* Subject
* Description
* Category
* Priority
* Status
* Student
* Assigned Department
* Assigned Staff
* Resolution Date
* Request Age

### Service Request Fields & Relationships

<img width="1273" height="668" alt="image" src="https://github.com/user-attachments/assets/6425adfc-c009-4e59-bfba-18cbab9ece91" />


The Service Request object uses a Lookup Relationship with the Student object so that multiple service requests can be associated with a student.

---

## ⚙️ Salesforce Automation

The project contains four Record-Triggered Flows.

| Flow                         | Purpose                                                    |
| ---------------------------- | ---------------------------------------------------------- |
| Auto Assign Department       | Assigns the department based on request category           |
| Set Default Request Priority | Sets priority according to request category                |
| Update Status When Assigned  | Changes status to Assigned when staff is assigned          |
| Set Resolution Date          | Populates the resolution date when the request is resolved |

### Flow Configuration

<img width="1600" height="345" alt="image" src="https://github.com/user-attachments/assets/d3bf3ddc-dd15-4609-be07-300a284c6abb" />


### 1. Auto Assign Department

The flow evaluates the selected request category and assigns the corresponding department.

Supported categories:

* IT Support
* Hostel
* Library
* Transport
* Academic Services

### 2. Set Default Request Priority

The flow assigns a default priority according to the selected category.

Examples:

* IT Support → Medium
* Hostel → Medium
* Library → Low
* Transport → High
* Academic Services → Medium

### 3. Update Status When Assigned

When an Assigned Staff value is added to a request, the flow updates the request status to `Assigned`.

### 4. Set Resolution Date

When a request is marked as `Resolved`, the flow populates the Resolution Date using the current date and time.

<img width="1600" height="530" alt="image" src="https://github.com/user-attachments/assets/74e1aa2f-6a47-4e27-b98a-cc70d4eed0cc" />

---

## ✅ Validation Rules

Three validation rules are configured on the Service Request object.

<img width="1592" height="428" alt="image" src="https://github.com/user-attachments/assets/c8a16730-522c-4550-a768-27f66d11e7fc" />


| Validation Rule                            | Purpose                                                          |
| ------------------------------------------ | ---------------------------------------------------------------- |
| `Required_Request_Information`             | Ensures Subject, Description, Student, and Category are provided |
| `Resolution_Date_Cannot_Be_Before_Created` | Prevents Resolution Date from being earlier than Created Date    |
| `Resolved_Requires_Resolution_Date`        | Requires Resolution Date when Status is Resolved                 |

These rules help maintain consistent and accurate service-request data.

---

## 📊 Reports & Dashboard

The project includes Salesforce reports for monitoring service requests.

### Reports

* Requests by Category
* Requests by Department
* Resolved vs Unresolved
* High Priority Requests
* Open Service Requests

### Campus HelpDesk Dashboard

The dashboard provides a visual overview of:

* Requests by Category
* Requests by Department
* Open Service Requests
* High Priority Requests
* Status Distribution

<!-- IMAGE 4: Put screenshots/dashboard.png here -->

<img width="1600" height="759" alt="image" src="https://github.com/user-attachments/assets/dce31867-c948-4f38-b3d1-3f18da0d7ca8" />


---

## 🔎 SOQL Queries

SOQL was used to retrieve and analyze service-request data.

### High Priority Requests

```sql
SELECT Id, Name, Subject__c, Priority__c, Status__c
FROM Request_Number__c
WHERE Priority__c = 'High'
```

### Open Requests

```sql
SELECT Id, Name, Subject__c, Status__c
FROM Request_Number__c
WHERE Status__c NOT IN ('Resolved', 'Closed')
```

### Requests by Department

```sql
SELECT Id, Name, Subject__c, Priority__c, Status__c
FROM Request_Number__c
WHERE Assigned_Department__c = 'IT Support'
```

### Requests by Category

```sql
SELECT Category__c, COUNT(Id)
FROM Request_Number__c
GROUP BY Category__c
```

---

## 🔄 Request Lifecycle

The main request-processing workflow is:

1. Create Service Request
2. Validate required information
3. Evaluate Category
4. Assign Department
5. Set Default Priority
6. Assign Staff
7. Update Status
8. Process Request
9. Set Resolution Date
10. Mark Request as Resolved

The core business logic is implemented using Salesforce Flow and Validation Rules.

---

## 📁 Project Structure

The project follows the Salesforce DX source format.

```text
CampusHelpDesk/
│
├── force-app/
│   └── main/
│       └── default/
│           ├── flows/
│           │   ├── Auto_Assign_Department.flow-meta.xml
│           │   ├── Set_Default_Request_Priority.flow-meta.xml
│           │   ├── Set_Resolution_Date.flow-meta.xml
│           │   └── Update_Status_When_Assigned.flow-meta.xml
│           │
│           ├── objects/
│           │   ├── Student__c/
│           │   └── Request_Number__c/
│           │
│           └── tabs/
│               └── Request_Number__c.tab-meta.xml
│
├── manifest/
│   └── package.xml
│
├── screenshots/
│
├── .forceignore
├── .gitignore
├── README.md
└── sfdx-project.json
```

### Important Directories

| Directory / File          | Purpose                                   |
| ------------------------- | ----------------------------------------- |
| `force-app/main/default/` | Main Salesforce metadata source directory |
| `objects/`                | Custom object and field metadata          |
| `flows/`                  | Salesforce Flow metadata                  |
| `tabs/`                   | Custom tab metadata                       |
| `manifest/package.xml`    | Metadata package manifest                 |
| `sfdx-project.json`       | Salesforce DX project configuration       |
| `screenshots/`            | Project screenshots                       |

---

## 🛠️ Salesforce DX Setup

### Prerequisites
