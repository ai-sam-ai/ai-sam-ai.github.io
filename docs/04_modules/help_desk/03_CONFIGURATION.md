# Configuration Guide - SH All-In-One Helpdesk

Complete configuration reference for administrators.

---

## Table of Contents

1. [Company Settings](#company-settings)
2. [Team Configuration](#team-configuration)
3. [Stage Configuration](#stage-configuration)
4. [Category Setup](#category-setup)
5. [Priority Levels](#priority-levels)
6. [SLA Policies](#sla-policies)
7. [Email Templates](#email-templates)
8. [Reminder Alarms](#reminder-alarms)
9. [User Permissions](#user-permissions)
10. [Portal Configuration](#portal-configuration)
11. [Dashboard Settings](#dashboard-settings)

---

## Company Settings

**Location:** `Helpdesk` → `Configuration` → `Settings`

### General Settings

| Field | Type | Description | Recommended |
|-------|------|-------------|-------------|
| Enable Category | Boolean | Show category field on tickets | ✓ Enabled |
| Enable Sub-Category | Boolean | Show sub-category field | ✓ Enabled |
| Enable Customer Rating | Boolean | Allow customer feedback | ✓ Enabled |
| Display Multi-User | Boolean | Allow multiple assignees | ✓ Enabled |
| Enable Product Management | Boolean | Link products to tickets | Optional |
| Display Ticket Reminder | Boolean | Enable reminder alarms | ✓ Enabled |

### Auto-Close Settings

| Field | Type | Description |
|-------|------|-------------|
| Auto-Close Tickets | Boolean | Automatically close inactive tickets |
| Close Days | Integer | Days of inactivity before auto-close |

**Example Configuration:**
```yaml
auto_close:
  enabled: true
  days: 30
  runs: daily via cron
```

### Default Assignments

| Field | Purpose |
|-------|---------|
| Default Team | Auto-assign new tickets to this team |
| Default User | Auto-assign new tickets to this user |

### Stage Mappings

Configure which stages represent each lifecycle state:

| Setting | Purpose | Example |
|---------|---------|---------|
| New/Draft Stage | Initial ticket stage | "New" |
| Resolved Stage | Work completed | "Done" |
| Closed Stage | Ticket finalized | "Closed" |
| Cancel Stage | Cancelled tickets | "Cancelled" |
| Re-open Stage | Reopened tickets | "Re-opened" |

### Email Settings

| Field | Purpose |
|-------|---------|
| Allocation Email Template | Sent when ticket assigned |
| Reply Email Template | Sent when agent replies |
| Include Signature | Add user signature to emails |
| Display in Chatter | Log messages in chatter |
| Send PDF in Message | Include report URL |
| Send Ticket URL | Include portal link |
| Email on Customer View | Notify when customer views ticket |

### Portal Settings

| Field | Purpose |
|-------|---------|
| Auto Add Customer as Follower | Subscribe customer to updates |
| Portal File Size Limit | Max attachment size (KB) |

### Configuration Checklist

```yaml
company_settings_checklist:
  general:
    - id: CFG-001
      setting: "Enable Category"
      recommended: true
      status: pending

    - id: CFG-002
      setting: "Enable Sub-Category"
      recommended: true
      status: pending

    - id: CFG-003
      setting: "Enable Customer Rating"
      recommended: true
      status: pending

    - id: CFG-004
      setting: "Display Multi-User"
      recommended: true
      status: pending

  stages:
    - id: CFG-010
      setting: "New Stage configured"
      status: pending

    - id: CFG-011
      setting: "Resolved Stage configured"
      status: pending

    - id: CFG-012
      setting: "Closed Stage configured"
      status: pending

    - id: CFG-013
      setting: "Cancel Stage configured"
      status: pending

  email:
    - id: CFG-020
      setting: "Allocation template set"
      status: pending

    - id: CFG-021
      setting: "Auto-add follower enabled"
      recommended: true
      status: pending
```

---

## Team Configuration

**Location:** `Helpdesk` → `Configuration` → `Teams`

### Creating a Team

| Field | Required | Description |
|-------|----------|-------------|
| Team Name | Yes | Display name for the team |
| Team Head | Yes | User who leads the team |
| Team Members | No | Users who belong to team |
| Working Hours | Yes | Resource calendar for SLA calculations |

### Team Structure Example

```yaml
support_organization:
  teams:
    - name: "Tier 1 Support"
      head: "support_lead@company.com"
      members:
        - "agent1@company.com"
        - "agent2@company.com"
      calendar: "Standard 40 Hours"

    - name: "Tier 2 Technical"
      head: "tech_lead@company.com"
      members:
        - "tech1@company.com"
        - "tech2@company.com"
      calendar: "Standard 40 Hours"

    - name: "Billing Support"
      head: "billing_lead@company.com"
      members:
        - "billing1@company.com"
      calendar: "Standard 40 Hours"
```

### Working Hours Calendar

The working hours calendar is critical for accurate SLA calculations.

1. Navigate to: `Settings` → `Technical` → `Resource` → `Working Times`
2. Create or select a calendar
3. Define working days and hours

**Example:**
```yaml
working_calendar:
  name: "Standard Business Hours"
  timezone: "America/New_York"
  schedule:
    monday: "09:00 - 17:00"
    tuesday: "09:00 - 17:00"
    wednesday: "09:00 - 17:00"
    thursday: "09:00 - 17:00"
    friday: "09:00 - 17:00"
    saturday: "closed"
    sunday: "closed"
```

---

## Stage Configuration

**Location:** `Helpdesk` → `Configuration` → `Stages`

### Default Stages

The module creates these default stages:

| Stage | Sequence | Purpose |
|-------|----------|---------|
| New | 1 | Newly created tickets |
| In Progress | 2 | Being worked on |
| Done | 3 | Work completed |
| Closed | 4 | Fully resolved |
| Cancelled | 5 | Cancelled tickets |

### Stage Fields

| Field | Description |
|-------|-------------|
| Name | Stage display name |
| Sequence | Sort order (lower = earlier) |
| Email Templates | Templates sent on stage transition |
| Next Stage | Auto-advance to this stage |
| Group Access | Restrict to specific user groups |
| Show Cancel Button | Display cancel action |
| Show Done Button | Display resolve action |

### Stage Workflow Example

```yaml
ticket_workflow:
  stages:
    - name: "New"
      sequence: 1
      email_templates: ["New Ticket Notification"]
      next_stage: null
      show_cancel: true
      show_done: false

    - name: "Assigned"
      sequence: 2
      email_templates: ["Ticket Assigned"]
      next_stage: null
      show_cancel: true
      show_done: false

    - name: "In Progress"
      sequence: 3
      email_templates: []
      next_stage: null
      show_cancel: true
      show_done: true

    - name: "Pending Customer"
      sequence: 4
      email_templates: ["Awaiting Response"]
      next_stage: null
      show_cancel: true
      show_done: true

    - name: "Resolved"
      sequence: 5
      email_templates: ["Ticket Resolved"]
      next_stage: "Closed"
      show_cancel: false
      show_done: false

    - name: "Closed"
      sequence: 6
      email_templates: ["Ticket Closed"]
      next_stage: null
      show_cancel: false
      show_done: false
```

### Restricting Stages by Group

To limit who can move tickets to certain stages:

1. Edit the stage
2. In "Group Access" field, select allowed groups
3. Only users in those groups can transition to this stage

---

## Category Setup

**Location:** `Helpdesk` → `Configuration` → `Categories`

### Creating Categories

| Field | Required | Description |
|-------|----------|-------------|
| Name | Yes | Category name |
| Sequence | Auto | Sort order |

### Example Category Structure

```yaml
categories:
  - name: "Technical Support"
    subcategories:
      - "Software Issue"
      - "Hardware Issue"
      - "Network Problem"

  - name: "Billing"
    subcategories:
      - "Invoice Question"
      - "Payment Issue"
      - "Refund Request"

  - name: "Sales"
    subcategories:
      - "Product Inquiry"
      - "Quote Request"
      - "Order Status"

  - name: "General"
    subcategories:
      - "Information Request"
      - "Feedback"
      - "Other"
```

### Sub-Categories

**Location:** `Helpdesk` → `Configuration` → `Sub Categories`

| Field | Required | Description |
|-------|----------|-------------|
| Name | Yes | Sub-category name |
| Parent Category | Yes | Parent category reference |
| Sequence | Auto | Sort order |

---

## Priority Levels

**Location:** `Helpdesk` → `Configuration` → `Priority`

### Default Priorities

| Priority | Color | Use Case |
|----------|-------|----------|
| Low | Green | Non-urgent issues |
| Medium | Yellow | Standard priority |
| High | Orange | Important issues |
| Urgent | Red | Critical issues |

### Creating Custom Priorities

| Field | Required | Description |
|-------|----------|-------------|
| Name | Yes | Priority name |
| Sequence | Auto | Sort order |
| Color | No | Display color |

---

## SLA Policies

**Location:** `Helpdesk` → `Configuration` → `SLA Policies`

### Understanding SLA Policies

SLA (Service Level Agreement) policies define response and resolution time targets.

### Creating an SLA Policy

| Field | Required | Description |
|-------|----------|-------------|
| Policy Name | Yes | Descriptive name |
| Team | Yes | Which team this applies to |
| Ticket Type | No | Optional: specific ticket type |
| Target Type | Yes | "Reaching Stage" or "Assign To" |
| Target Stage | If reaching_stage | Stage that must be reached |
| Days | Yes | SLA days component |
| Hours | Yes | SLA hours component |
| Minutes | Yes | SLA minutes component |

### SLA Calculation

The SLA deadline is calculated using the team's working hours calendar:
```
SLA Deadline = Ticket Created + (Days + Hours + Minutes) in business hours
```

### Example SLA Policies

```yaml
sla_policies:
  - name: "First Response - 4 Hours"
    team: "Tier 1 Support"
    target_type: "reaching_stage"
    target_stage: "In Progress"
    time:
      days: 0
      hours: 4
      minutes: 0

  - name: "Resolution - Standard (24 Hours)"
    team: "Tier 1 Support"
    target_type: "reaching_stage"
    target_stage: "Resolved"
    time:
      days: 1
      hours: 0
      minutes: 0

  - name: "Resolution - Urgent (4 Hours)"
    team: "Tier 1 Support"
    ticket_type: "Urgent"
    target_type: "reaching_stage"
    target_stage: "Resolved"
    time:
      days: 0
      hours: 4
      minutes: 0

  - name: "Assignment - 30 Minutes"
    team: "Tier 1 Support"
    target_type: "assign_to"
    time:
      days: 0
      hours: 0
      minutes: 30
```

### SLA Status Colors

| Status | Color | Meaning |
|--------|-------|---------|
| Passed | Green | SLA met on time |
| Failed | Red | SLA deadline missed |
| Partial | Orange | Partially met (some SLAs passed, some failed) |

---

## Email Templates

**Location:** `Settings` → `Technical` → `Email Templates`

### Default Templates

The module creates these email templates:

| Template | Trigger | Purpose |
|----------|---------|---------|
| Helpdesk: Ticket Notification | New ticket | Notify customer |
| Helpdesk: Allocation | Assignment | Notify assigned agent |
| Helpdesk: Ticket Reply | Agent reply | Send response to customer |
| Helpdesk: Reminder | Scheduled | Reminder before due date |

### Customizing Templates

1. Navigate to email template
2. Edit HTML body with Qweb syntax
3. Available variables:
   ```
   ${object.name}          - Ticket number
   ${object.partner_id.name} - Customer name
   ${object.user_id.name}  - Assigned agent
   ${object.description}   - Ticket description
   ${object.stage_id.name} - Current stage
   ```

---

## Reminder Alarms

**Location:** `Helpdesk` → `Configuration` → `Ticket Alarms`

### Creating Reminder Alarms

| Field | Required | Description |
|-------|----------|-------------|
| Type | Yes | Email or Popup |
| Remind Before | Yes | Time before due date |
| Reminder Unit | Yes | Hours, Minutes, or Seconds |

### Example Alarms

```yaml
reminder_alarms:
  - name: "1 Hour Before (Email)"
    type: email
    remind_before: 1
    unit: hours

  - name: "30 Minutes Before (Popup)"
    type: popup
    remind_before: 30
    unit: minutes

  - name: "1 Day Before (Email)"
    type: email
    remind_before: 24
    unit: hours
```

---

## User Permissions

### Security Groups

| Group | Technical Name | Capabilities |
|-------|----------------|--------------|
| Support User | `helpdesk_group_user` | View/edit own tickets |
| Team Leader | `helpdesk_group_team_leader` | Manage team tickets |
| Support Manager | `helpdesk_group_manager` | Full access |

### Assigning Groups

1. Navigate to: `Settings` → `Users & Companies` → `Users`
2. Edit user
3. Under "Helpdesk" section, check appropriate group

### Permission Matrix

| Action | User | Leader | Manager |
|--------|------|--------|---------|
| View own tickets | ✓ | ✓ | ✓ |
| View team tickets | ✗ | ✓ | ✓ |
| View all tickets | ✗ | ✗ | ✓ |
| Create tickets | ✓ | ✓ | ✓ |
| Edit own tickets | ✓ | ✓ | ✓ |
| Edit team tickets | ✗ | ✓ | ✓ |
| Edit all tickets | ✗ | ✗ | ✓ |
| Delete tickets | ✗ | ✗ | ✓ |
| Configure stages | ✗ | ✗ | ✓ |
| Configure teams | ✗ | ✗ | ✓ |
| Configure SLA | ✗ | ✗ | ✓ |
| View dashboard | ✓ | ✓ | ✓ |

---

## Portal Configuration

### Enabling Customer Portal

1. Ensure `portal` module is installed
2. Create portal user for customer:
   - `Contacts` → Select contact → `Action` → `Grant Portal Access`

### Portal User Access Levels

Configure in user profile under Helpdesk section:

| Level | Access |
|-------|--------|
| User | Own tickets + assigned tickets |
| Leader | Above + team head tickets |
| Manager | All tickets |

### Portal Features

Customers can:
- View ticket list
- View ticket details
- Add messages
- Upload attachments
- Rate satisfaction
- Submit feedback

---

## Dashboard Settings

### Configuring Dashboard Filters

In Company Settings, configure:

| Setting | Purpose |
|---------|---------|
| Dashboard Filter Stages | Stages shown in filter dropdown |
| Dashboard Table Stages | Stages shown in data tables |

### Dashboard Metrics

The dashboard displays:
- Ticket counts by stage
- Tickets by time period (today, week, month)
- Team performance
- SLA compliance

---

## Configuration Checklist - Complete

```yaml
complete_configuration_checklist:
  company_settings:
    - "[ ] Enable Category: Enabled"
    - "[ ] Enable Sub-Category: Enabled"
    - "[ ] Enable Customer Rating: Enabled"
    - "[ ] Display Multi-User: Enabled"
    - "[ ] Default Team: Set"
    - "[ ] Stage mappings: All 5 stages mapped"
    - "[ ] Email templates: Configured"

  teams:
    - "[ ] At least one team created"
    - "[ ] Team head assigned"
    - "[ ] Team members added"
    - "[ ] Working calendar set"

  stages:
    - "[ ] Default stages exist"
    - "[ ] Email templates linked to stages"
    - "[ ] Sequence order correct"

  categories:
    - "[ ] Categories created"
    - "[ ] Sub-categories created"

  priorities:
    - "[ ] Priority levels defined"
    - "[ ] Colors assigned"

  sla_policies:
    - "[ ] First response SLA created"
    - "[ ] Resolution SLA created"
    - "[ ] Team assigned to policies"

  users:
    - "[ ] Admin has Support Manager role"
    - "[ ] Leaders have Team Leader role"
    - "[ ] Agents have Support User role"

  portal:
    - "[ ] Portal access granted to customers"
    - "[ ] Portal user levels configured"
```

---

*Next: [User Guide](04_USER_GUIDE.md)*
