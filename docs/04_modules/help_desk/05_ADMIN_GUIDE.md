# Administrator Guide - SH All-In-One Helpdesk

Management and administration guide for team leaders and managers.

---

## Table of Contents

1. [Team Management](#team-management)
2. [User Management](#user-management)
3. [SLA Management](#sla-management)
4. [Workflow Management](#workflow-management)
5. [Reporting](#reporting)
6. [Bulk Operations](#bulk-operations)
7. [Maintenance Tasks](#maintenance-tasks)
8. [Security Considerations](#security-considerations)

---

## Team Management

### Creating Teams

**Location:** `Helpdesk` → `Configuration` → `Teams`

1. Click `Create`
2. Configure team:

| Field | Description | Notes |
|-------|-------------|-------|
| Name | Team identifier | e.g., "Tier 1 Support" |
| Team Head | Leader user | Has leader permissions |
| Members | Team users | Can be assigned tickets |
| Working Hours | Calendar | Used for SLA calculations |

3. Save

### Managing Team Structure

**Recommended Structure:**
```yaml
support_organization:
  tier_1:
    name: "Front Line Support"
    purpose: "First response, basic issues"
    skills: "General support, triage"

  tier_2:
    name: "Technical Support"
    purpose: "Complex technical issues"
    skills: "Technical expertise"

  tier_3:
    name: "Engineering"
    purpose: "Bugs, development issues"
    skills: "Development, debugging"

  specialized:
    billing:
      name: "Billing Support"
      purpose: "Payment, invoice issues"

    sales:
      name: "Sales Support"
      purpose: "Pre-sales, quotes"
```

### Team Head Responsibilities

Team heads can:
- View all tickets for their team
- Reassign tickets within team
- Monitor team SLA compliance
- Access team reports

### Transferring Tickets Between Teams

1. Open ticket
2. Change `Team` field
3. Optionally assign to specific user
4. Save
5. New team is notified

---

## User Management

### Adding Support Users

1. Navigate to: `Settings` → `Users`
2. Create or edit user
3. Under "Helpdesk" section:
   - Select `Support User`, `Team Leader`, or `Support Manager`
4. Save
5. Add user to appropriate team(s)

### Role Comparison

| Capability | User | Leader | Manager |
|------------|------|--------|---------|
| Create tickets | ✓ | ✓ | ✓ |
| View own tickets | ✓ | ✓ | ✓ |
| View team tickets | ✗ | ✓ | ✓ |
| View all tickets | ✗ | ✗ | ✓ |
| Edit team tickets | ✗ | ✓ | ✓ |
| Delete tickets | ✗ | ✗ | ✓ |
| Configure system | ✗ | ✗ | ✓ |
| View all reports | ✗ | Team | All |

### Portal User Setup

For customer self-service:

1. Go to contact record
2. `Action` → `Grant Portal Access`
3. Configure portal access level:
   - **User**: Own tickets only
   - **Leader**: Team tickets
   - **Manager**: All tickets

### User Signature Setup

For email replies:

1. Edit user profile
2. Go to "Preferences" tab
3. Set "Signature" field
4. HTML supported

---

## SLA Management

### Designing SLA Policies

#### Step 1: Define Service Levels

```yaml
service_levels:
  platinum:
    first_response: "1 hour"
    resolution: "4 hours"
    applies_to: "Enterprise customers"

  gold:
    first_response: "4 hours"
    resolution: "24 hours"
    applies_to: "Business customers"

  standard:
    first_response: "8 hours"
    resolution: "48 hours"
    applies_to: "All customers"
```

#### Step 2: Create Policies

**Location:** `Helpdesk` → `Configuration` → `SLA Policies`

For each service level, create policies:

**Example: Platinum First Response**
```yaml
policy:
  name: "Platinum - First Response"
  team: "Tier 1 Support"
  ticket_type: "Platinum Support"  # Optional
  target: "reaching_stage"
  target_stage: "In Progress"
  time:
    days: 0
    hours: 1
    minutes: 0
```

#### Step 3: Link to Teams

Policies are linked to teams. When a ticket is created:
1. System checks ticket's team
2. Finds applicable SLA policies
3. Calculates deadlines using team's working calendar
4. Creates SLA status records

### Monitoring SLA Compliance

#### Dashboard View
- Dashboard shows SLA status overview
- Filter by team, user, period

#### Ticket View
- Each ticket shows SLA status
- Color coding: Green (on track), Red (failed), Orange (at risk)

#### Reports
- Filter tickets by SLA status
- Export for analysis

### SLA Escalation

When SLA is at risk:

1. **Alarms**: Set up reminder alarms
2. **Email**: Auto-notify team head
3. **Popup**: Show notification to assigned user
4. **Reassign**: Consider escalating to senior agent

### SLA Best Practices

```yaml
sla_best_practices:
  design:
    - "Define realistic targets based on team capacity"
    - "Consider working hours in calculations"
    - "Create separate policies for priority levels"
    - "Document SLA definitions for team"

  monitoring:
    - "Review SLA reports weekly"
    - "Identify patterns in failures"
    - "Adjust policies based on data"
    - "Celebrate SLA successes"

  improvement:
    - "Analyze failed tickets for root cause"
    - "Provide training where needed"
    - "Consider resource allocation"
    - "Update knowledge base"
```

---

## Workflow Management

### Stage Configuration

**Location:** `Helpdesk` → `Configuration` → `Stages`

#### Stage Properties

| Property | Purpose |
|----------|---------|
| Sequence | Order in workflow |
| Email Templates | Auto-send on entry |
| Next Stage | Auto-advance option |
| Group Access | Restrict who can use |
| Cancel Button | Show cancel action |
| Done Button | Show resolve action |

#### Workflow Design

```yaml
recommended_workflow:
  stages:
    - name: "New"
      sequence: 1
      purpose: "Incoming tickets"
      auto_email: true

    - name: "Triaged"
      sequence: 2
      purpose: "Reviewed, priority set"
      auto_email: false

    - name: "In Progress"
      sequence: 3
      purpose: "Work started"
      show_done_button: true

    - name: "Pending Customer"
      sequence: 4
      purpose: "Awaiting response"
      auto_email: true

    - name: "Pending Internal"
      sequence: 5
      purpose: "Awaiting colleague/vendor"
      auto_email: false

    - name: "Resolved"
      sequence: 6
      purpose: "Solution provided"
      auto_email: true

    - name: "Closed"
      sequence: 7
      purpose: "Confirmed resolved"
      auto_email: true
```

### Email Template Configuration

1. Navigate to: `Settings` → `Technical` → `Email Templates`
2. Find or create helpdesk template
3. Configure:
   - Subject line
   - Body (HTML with variables)
   - Recipients
   - Attachments

**Available Variables:**
```python
${object.name}              # Ticket number
${object.partner_id.name}   # Customer name
${object.user_id.name}      # Assigned agent
${object.stage_id.name}     # Current stage
${object.description}       # Description
${object.ticket_subject}    # Subject line
${object.team_id.name}      # Team name
${object.priority.name}     # Priority
```

---

## Reporting

### Built-in Reports

#### Ticket Analysis
- Tickets by stage
- Tickets by team
- Tickets by user
- Tickets by period

#### SLA Reports
- SLA compliance rate
- Failed SLAs by team
- Average resolution time

### Creating Custom Reports

1. Navigate to: `Helpdesk` → `Tickets`
2. Apply filters as needed
3. Group by relevant field
4. Click `Favorites` → `Save current search`
5. Check "Share with all users" for team access

### Exporting Data

1. Apply filters/grouping
2. Switch to List view
3. Select all records (checkbox header)
4. `Action` → `Export`
5. Choose format (CSV/Excel)
6. Select fields
7. Export

### Key Metrics to Track

```yaml
helpdesk_kpis:
  volume:
    - "Tickets created per day/week/month"
    - "Tickets by category"
    - "Tickets by source"

  performance:
    - "Average first response time"
    - "Average resolution time"
    - "SLA compliance rate"
    - "Tickets resolved per agent"

  quality:
    - "Customer satisfaction rating"
    - "Reopened ticket rate"
    - "Escalation rate"

  efficiency:
    - "Time spent per ticket"
    - "Backlog size"
    - "Agent utilization"
```

---

## Bulk Operations

### Mass Update Wizard

For updating multiple tickets at once:

1. Select tickets in list view (checkboxes)
2. `Action` → `Mass Update`
3. Configure changes:
   - Stage
   - Assigned User
   - Priority
   - Team
4. Apply

### Mass Follower Update

1. Select tickets
2. `Action` → `Mass Update Followers`
3. Add or remove followers
4. Apply

### Bulk Assignment

**By Filter:**
1. Filter tickets (e.g., unassigned)
2. Select all
3. Mass update assigned user

**By Round Robin:**
- Not built-in, but can be automated

---

## Maintenance Tasks

### Daily Tasks

```yaml
daily_admin_tasks:
  - id: DAILY-001
    task: "Review unassigned tickets"
    action: "Assign to appropriate team/agent"
    status: pending

  - id: DAILY-002
    task: "Check SLA alerts"
    action: "Escalate at-risk tickets"
    status: pending

  - id: DAILY-003
    task: "Monitor ticket queue sizes"
    action: "Balance workload if needed"
    status: pending
```

### Weekly Tasks

```yaml
weekly_admin_tasks:
  - id: WEEKLY-001
    task: "Review SLA compliance report"
    action: "Identify patterns, take corrective action"
    status: pending

  - id: WEEKLY-002
    task: "Review customer feedback"
    action: "Address concerns, recognize successes"
    status: pending

  - id: WEEKLY-003
    task: "Check for stale tickets"
    action: "Follow up or close"
    status: pending

  - id: WEEKLY-004
    task: "Team meeting"
    action: "Discuss issues, share knowledge"
    status: pending
```

### Monthly Tasks

```yaml
monthly_admin_tasks:
  - id: MONTHLY-001
    task: "Analyze ticket trends"
    action: "Identify common issues for knowledge base"
    status: pending

  - id: MONTHLY-002
    task: "Review team performance"
    action: "Provide feedback, training"
    status: pending

  - id: MONTHLY-003
    task: "Update quick reply templates"
    action: "Add common responses"
    status: pending

  - id: MONTHLY-004
    task: "Review and update categories"
    action: "Add/remove as needed"
    status: pending
```

### System Maintenance

#### Auto-Close Configuration

Enable auto-close for inactive tickets:

1. `Helpdesk` → `Configuration` → `Settings`
2. Enable "Auto-Close Tickets"
3. Set days of inactivity
4. Tickets will be closed by cron job

#### Cron Jobs

The module includes these scheduled actions:

| Job | Frequency | Purpose |
|-----|-----------|---------|
| Auto-Close Tickets | Daily | Close inactive tickets |
| Ticket Reminders | Configurable | Send reminder notifications |

#### Database Cleanup

Periodically archive old tickets:
- Export closed tickets older than X months
- Consider database archiving strategies

---

## Security Considerations

### Access Control

```yaml
access_control_checklist:
  users:
    - "[ ] All agents assigned appropriate role"
    - "[ ] No over-privileged users"
    - "[ ] Managers are limited in number"

  data:
    - "[ ] Sensitive tickets use appropriate category"
    - "[ ] Internal notes not exposed to customers"
    - "[ ] Attachments scanned for malware"

  portal:
    - "[ ] Portal users can only see own tickets"
    - "[ ] Portal file upload size limited"
    - "[ ] Portal access regularly reviewed"
```

### Audit Trail

All ticket actions are logged:
- Message history in chatter
- Stage change tracking
- Assignment changes
- System notifications

### Data Privacy

- Customer data is company-filtered
- Multi-company installations isolate data
- Portal access respects user permissions

### Backup Recommendations

```yaml
backup_strategy:
  database:
    frequency: "Daily"
    retention: "30 days"

  attachments:
    frequency: "Daily"
    retention: "90 days"

  configuration:
    frequency: "Weekly"
    retention: "12 weeks"
```

---

## Troubleshooting Admin Issues

### Common Issues

#### Tickets Not Auto-Assigning
- Check company default team/user settings
- Verify user permissions
- Check team membership

#### SLA Not Calculating
- Verify SLA policy exists for team
- Check team has working hours calendar
- Confirm ticket has correct team/type

#### Email Not Sending
- Check outgoing mail server configuration
- Verify email templates are linked to stages
- Check user email addresses

#### Dashboard Not Showing Data
- Verify dashboard stage configuration
- Check user permissions
- Confirm tickets exist in date range

---

## Quick Reference

### Configuration Locations

| Setting | Path |
|---------|------|
| General Settings | Helpdesk → Configuration → Settings |
| Teams | Helpdesk → Configuration → Teams |
| Stages | Helpdesk → Configuration → Stages |
| Categories | Helpdesk → Configuration → Categories |
| SLA Policies | Helpdesk → Configuration → SLA Policies |
| Alarms | Helpdesk → Configuration → Ticket Alarms |
| Email Templates | Settings → Technical → Email Templates |
| Users | Settings → Users |

### Emergency Actions

| Situation | Action |
|-----------|--------|
| All tickets assigned to wrong user | Mass update to correct user |
| SLA deadlines wrong | Check/fix working hours calendar |
| Customer getting internal notes | Check message type, fix template |
| Agent can't see tickets | Verify team membership and role |

---

*Next: [Technical Reference](06_TECHNICAL_REFERENCE.md)*
