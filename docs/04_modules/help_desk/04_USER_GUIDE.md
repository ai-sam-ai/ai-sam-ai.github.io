# User Guide - SH All-In-One Helpdesk

Day-to-day operations guide for support agents and team members.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Working with Tickets](#working-with-tickets)
3. [Ticket Lifecycle](#ticket-lifecycle)
4. [Communication](#communication)
5. [Time Tracking](#time-tracking)
6. [Using the Dashboard](#using-the-dashboard)
7. [Keyboard Shortcuts](#keyboard-shortcuts)
8. [Best Practices](#best-practices)

---

## Getting Started

### Accessing Helpdesk

1. Log in to Odoo
2. Click `Helpdesk` in the main menu
3. You'll see the ticket kanban view by default

### Understanding the Interface

```
┌─────────────────────────────────────────────────────────────┐
│ Helpdesk  │  Tickets  │  Dashboard  │  Configuration       │
├─────────────────────────────────────────────────────────────┤
│ [Create] [Import]           │ Search... │ Filters ▼ │ Group │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │   NEW    │  │IN PROGRESS│  │   DONE   │  │  CLOSED  │   │
│  ├──────────┤  ├──────────┤  ├──────────┤  ├──────────┤   │
│  │ HD00001  │  │ HD00003  │  │ HD00002  │  │          │   │
│  │ HD00004  │  │          │  │          │  │          │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### View Options

| View | Icon | Best For |
|------|------|----------|
| Kanban | Grid icon | Visual workflow overview |
| List | Lines icon | Bulk operations, sorting |
| Form | (click ticket) | Full ticket details |
| Calendar | Calendar icon | Date-based planning |

---

## Working with Tickets

### Creating a New Ticket

1. Click `Create` button (or Alt+C)
2. Fill in required fields:

| Field | Required | Description |
|-------|----------|-------------|
| Customer | Yes | Select or create contact |
| Subject | Yes | Brief issue description |
| Description | No | Detailed information |
| Team | Yes | Assign to team |
| Assigned To | No | Primary agent |
| Priority | No | Urgency level |
| Category | No | Issue classification |
| Type | No | Ticket type |

3. Click `Save` (Ctrl+S)

### Finding Tickets

#### Quick Search
Type in the search bar to find tickets by:
- Ticket number (HD00001)
- Subject text
- Customer name
- Description content

#### Using Filters

Click `Filters` dropdown:

| Filter | Shows |
|--------|-------|
| My Tickets | Assigned to you |
| Unassigned | No assignee |
| Open | Not closed/cancelled |
| Starred | Favorited tickets |
| Created Today | Today's tickets |
| This Week | This week's tickets |

#### Custom Filters

1. Click `Filters` → `Add Custom Filter`
2. Select field, operator, value
3. Click `Apply`

Example: Find all high priority tickets:
```
Priority → = → High
```

#### Grouping

Click `Group By` to organize tickets:
- By Stage
- By Team
- By Assigned User
- By Priority
- By Category

### Ticket Form Overview

```
┌─────────────────────────────────────────────────────────────┐
│ HD00001 - Customer Login Issue                    [★] [⋮]  │
├─────────────────────────────────────────────────────────────┤
│ [Advance] [Resolve] [Close] [Cancel] [Reply] [WhatsApp]     │
├──────────────────────────────────┬──────────────────────────┤
│ Customer: John Smith             │ Stage: In Progress       │
│ Email: john@example.com          │ Priority: High           │
│ Mobile: +1-555-0123              │ Team: Tier 1 Support     │
│                                  │ Assigned: Jane Agent     │
├──────────────────────────────────┴──────────────────────────┤
│ Subject: Customer Login Issue                               │
├─────────────────────────────────────────────────────────────┤
│ Description:                                                │
│ Customer unable to log in to portal. Getting "Invalid       │
│ credentials" error even with correct password.              │
├─────────────────────────────────────────────────────────────┤
│ Category: Technical Support > Software Issue                │
│ Type: Support Request                                       │
│ Products: [Portal Access]                                   │
│ Tags: [login] [password] [urgent]                           │
├─────────────────────────────────────────────────────────────┤
│ SLA Status: ● On Track (Due: 2h 30m)                        │
├─────────────────────────────────────────────────────────────┤
│ [Chatter - Messages and Notes]                              │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Jane Agent (2 min ago)                                  │ │
│ │ Investigating the issue...                              │ │
│ ├─────────────────────────────────────────────────────────┤ │
│ │ John Smith (10 min ago)                                 │ │
│ │ Please help, I cannot access my account!                │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Editing Tickets

1. Open ticket (click in kanban or list)
2. Click `Edit` or directly modify fields
3. Save changes (Ctrl+S)

**Editable Fields:**
- Subject
- Description
- Priority
- Category/Sub-category
- Team
- Assigned User(s)
- Tags
- Products
- Due Date

### Reassigning Tickets

**To Single User:**
1. Open ticket
2. Click `Assigned To` field
3. Select new user
4. Save

**To Multiple Users:**
1. Open ticket
2. Click `Multi Assign` field
3. Select users
4. Save

**To Different Team:**
1. Open ticket
2. Change `Team` field
3. Update `Assigned To` if needed
4. Save

---

## Ticket Lifecycle

### Standard Workflow

```
    ┌─────────┐
    │   NEW   │  Ticket created
    └────┬────┘
         │
         ▼
    ┌─────────┐
    │ASSIGNED │  Agent takes ownership
    └────┬────┘
         │
         ▼
    ┌─────────┐
    │IN PROG. │  Work in progress
    └────┬────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────┐
│PENDING│ │RESOLVED│  Waiting or complete
└───┬───┘ └───┬───┘
    │         │
    └────┬────┘
         ▼
    ┌─────────┐
    │ CLOSED  │  Finalized
    └─────────┘
```

### Stage Actions

| Action | Button | Effect |
|--------|--------|--------|
| Advance | `Advance` | Move to next stage |
| Resolve | `Resolve` | Mark work as done |
| Close | `Close` | Finalize ticket |
| Cancel | `Cancel` | Cancel ticket |
| Re-open | `Re-open` | Reopen closed ticket |

### Advancing Stages

**Method 1: Button**
- Click `Advance` button to move to next configured stage

**Method 2: Drag & Drop**
- In kanban view, drag ticket card to target column

**Method 3: Field Edit**
- In form view, change `Stage` field directly

### Resolving Tickets

1. Ensure all work is complete
2. Add final notes in chatter
3. Click `Resolve` button
4. Ticket moves to "Done" stage

### Closing Tickets

1. Verify customer satisfaction
2. Click `Close` button
3. Ticket moves to "Closed" stage
4. Close date and user recorded

### Cancelling Tickets

1. Click `Cancel` button
2. Enter cancellation reason (optional)
3. Ticket moves to "Cancelled" stage

---

## Communication

### Using Chatter

The chatter (message area) at the bottom of tickets allows:
- Customer communication
- Internal notes
- Activity logging

#### Sending a Message (Visible to Customer)

1. Click `Send message` in chatter
2. Type your message
3. Add attachments if needed
4. Click `Send`

#### Logging an Internal Note

1. Click `Log note` in chatter
2. Type internal information
3. This is NOT visible to customer
4. Click `Log`

### Reply via Email

1. Click `Reply` button
2. Compose message in wizard
3. Select/create attachments
4. Click `Send`

### Quick Reply Templates

1. In reply wizard, click `Quick Reply Template` dropdown
2. Select a template
3. Template content is inserted
4. Modify as needed
5. Send

### WhatsApp Integration

1. Ensure customer has mobile number
2. Click `WhatsApp` button
3. Message is converted to text format
4. WhatsApp Web opens with pre-filled message
5. Send via WhatsApp

### Email Notifications

Automatic emails are sent when:
- New ticket created (to customer)
- Ticket assigned (to agent)
- Stage changes (if template configured)
- Reply sent (to customer)

### Following Tickets

To receive updates on tickets you're not assigned to:
1. Open ticket
2. Click `Follow` button (star icon)
3. You'll receive notifications

---

## Time Tracking

### Starting Time Tracking

1. Open ticket
2. Click `Start` button (timer icon)
3. Timer begins counting

### Stopping Time Tracking

1. Click `Stop` button
2. Enter description of work done
3. Click `End Ticket`
4. Time is logged to timesheet

### Viewing Time Entries

1. Open ticket
2. Go to `Timesheet` tab
3. View all time entries

### Time Tracking Rules

- Only one ticket can be tracked at a time (unless configured otherwise)
- Time is calculated from start to stop
- Entries link to project for reporting

---

## Using the Dashboard

**Location:** `Helpdesk` → `Dashboard`

### Dashboard Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    HELPDESK DASHBOARD                        │
├─────────────────────────────────────────────────────────────┤
│ Filter: [Team ▼] [Leader ▼] [User ▼] [Period ▼]             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐        │
│  │   NEW   │  │IN PROG. │  │  DONE   │  │ CLOSED  │        │
│  │   12    │  │    8    │  │   45    │  │   123   │        │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘        │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  Today: 5  │  This Week: 23  │  This Month: 89             │
├─────────────────────────────────────────────────────────────┤
│  [Detailed Table View]                                      │
└─────────────────────────────────────────────────────────────┘
```

### Filtering Dashboard

| Filter | Options |
|--------|---------|
| Team | Select specific team |
| Team Leader | Filter by leader |
| User | Filter by assigned user |
| Period | Today, Yesterday, Weekly, Monthly, Yearly, Custom |

### Counter Cards

Click any counter card to open filtered ticket list.

### Using Date Filters

1. Select `Period` dropdown
2. Choose time range
3. Or select `Custom` for date picker
4. Dashboard updates automatically

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Alt + C | Create new record |
| Ctrl + S | Save |
| Ctrl + D | Discard changes |
| Ctrl + K | Open command palette |
| Ctrl + Enter | Send message |
| Esc | Close dialog |
| ← → | Navigate records |

---

## Best Practices

### Ticket Creation

```
DO:
✓ Use clear, descriptive subjects
✓ Include relevant details in description
✓ Set appropriate priority
✓ Categorize correctly
✓ Attach relevant files

DON'T:
✗ Create duplicate tickets
✗ Leave subject vague ("Help needed")
✗ Forget to assign team
✗ Skip categorization
```

### Communication

```
DO:
✓ Respond promptly
✓ Keep customer informed
✓ Use professional language
✓ Log internal notes separately
✓ Use quick replies for common responses

DON'T:
✗ Share internal notes with customers
✗ Leave tickets without updates
✗ Use jargon customers won't understand
✗ Forget to follow up
```

### Workflow

```
DO:
✓ Update stage as work progresses
✓ Track time for all work
✓ Close tickets when complete
✓ Add resolution notes
✓ Request customer feedback

DON'T:
✗ Leave tickets in wrong stage
✗ Forget to resolve before closing
✗ Skip time tracking
✗ Close without resolution
```

### Daily Routine Checklist

```yaml
daily_agent_checklist:
  start_of_day:
    - "[ ] Check new tickets assigned to me"
    - "[ ] Review tickets awaiting customer response"
    - "[ ] Check SLA alerts for approaching deadlines"

  during_day:
    - "[ ] Update ticket stages as work progresses"
    - "[ ] Respond to customer messages within SLA"
    - "[ ] Track time on tickets"
    - "[ ] Add internal notes for complex issues"

  end_of_day:
    - "[ ] Ensure all worked tickets have updated status"
    - "[ ] Stop any running time trackers"
    - "[ ] Check for tickets needing follow-up tomorrow"
    - "[ ] Review personal statistics"
```

---

## Common Operations Quick Reference

### Create Ticket
```
Helpdesk → Tickets → Create → Fill form → Save
```

### Reply to Customer
```
Open ticket → Reply button → Compose → Send
```

### Resolve Ticket
```
Open ticket → Resolve button
```

### Assign to Colleague
```
Open ticket → Edit → Change Assigned To → Save
```

### View My Tickets
```
Helpdesk → Tickets → Filters → My Tickets
```

### Track Time
```
Open ticket → Start → (work) → Stop → Enter description → End
```

### Check SLA Status
```
Open ticket → View SLA Status section
```

---

*Next: [Admin Guide](05_ADMIN_GUIDE.md)*
