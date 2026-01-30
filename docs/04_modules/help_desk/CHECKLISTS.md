# AI-Parseable Checklists - SH All-In-One Helpdesk

Structured checklists designed for AI assistants (Claude, ChatGPT, etc.) to help users set up and use the helpdesk module.

---

## Document Purpose

This document provides machine-readable checklists that AI assistants can use to:
1. Guide users through setup procedures
2. Track progress on multi-step tasks
3. Diagnose issues systematically
4. Ensure completeness of configurations

**Format:** YAML-structured checklists with unique IDs for tracking.

---

## Installation Checklist

```yaml
checklist:
  id: INSTALL
  name: "Helpdesk Module Installation"
  description: "Complete installation of sh_all_in_one_helpdesk module"
  estimated_time: "30 minutes"

  prerequisites:
    - id: PRE-001
      task: "Odoo 18.0 is installed and running"
      verification: "Access Odoo web interface"
      status: pending

    - id: PRE-002
      task: "Admin credentials available"
      verification: "Can log in as administrator"
      status: pending

    - id: PRE-003
      task: "Python 3.10+ installed"
      verification: "python3 --version shows 3.10 or higher"
      status: pending

    - id: PRE-004
      task: "beautifulsoup4 package installed"
      verification: "pip3 show beautifulsoup4"
      status: pending

  dependency_installation:
    - id: DEP-001
      task: "Install mail module"
      action: "Apps → search 'mail' → Install"
      status: pending

    - id: DEP-002
      task: "Install portal module"
      action: "Apps → search 'portal' → Install"
      status: pending

    - id: DEP-003
      task: "Install sale_management module"
      action: "Apps → search 'Sales' → Install"
      status: pending

    - id: DEP-004
      task: "Install purchase module"
      action: "Apps → search 'Purchase' → Install"
      status: pending

    - id: DEP-005
      task: "Install account module"
      action: "Apps → search 'Invoicing' → Install"
      status: pending

    - id: DEP-006
      task: "Install hr_timesheet module"
      action: "Apps → search 'Timesheets' → Install"
      status: pending

    - id: DEP-007
      task: "Install project module"
      action: "Apps → search 'Project' → Install"
      status: pending

    - id: DEP-008
      task: "Install crm module"
      action: "Apps → search 'CRM' → Install"
      status: pending

  module_installation:
    - id: MOD-001
      task: "Copy module to addons path"
      action: "Place sh_all_in_one_helpdesk folder in Odoo addons directory"
      status: pending

    - id: MOD-002
      task: "Update Odoo configuration"
      action: "Ensure addons path includes module location in odoo.conf"
      status: pending

    - id: MOD-003
      task: "Restart Odoo service"
      action: "systemctl restart odoo OR docker-compose restart"
      status: pending

    - id: MOD-004
      task: "Update Apps List"
      action: "Apps → Update Apps List"
      status: pending

    - id: MOD-005
      task: "Install helpdesk module"
      action: "Apps → search 'All In One Helpdesk' → Install"
      status: pending

  verification:
    - id: VER-001
      task: "Helpdesk menu appears"
      verification: "Main menu shows 'Helpdesk' option"
      status: pending

    - id: VER-002
      task: "Configuration accessible"
      verification: "Helpdesk → Configuration shows submenus"
      status: pending

    - id: VER-003
      task: "Default stages created"
      verification: "Helpdesk → Configuration → Stages shows entries"
      status: pending
```

---

## Initial Configuration Checklist

```yaml
checklist:
  id: CONFIG
  name: "Helpdesk Initial Configuration"
  description: "Configure helpdesk for first use"
  estimated_time: "20 minutes"

  company_settings:
    - id: CFG-001
      task: "Open company settings"
      action: "Helpdesk → Configuration → Settings"
      status: pending

    - id: CFG-002
      task: "Enable category field"
      action: "Check 'Enable Category' checkbox"
      recommended: true
      status: pending

    - id: CFG-003
      task: "Enable sub-category field"
      action: "Check 'Enable Sub-Category' checkbox"
      recommended: true
      status: pending

    - id: CFG-004
      task: "Enable customer rating"
      action: "Check 'Enable Customer Rating' checkbox"
      recommended: true
      status: pending

    - id: CFG-005
      task: "Enable multi-user assignment"
      action: "Check 'Display Multi-User' checkbox"
      recommended: true
      status: pending

    - id: CFG-006
      task: "Set New/Draft stage"
      action: "Select stage for new tickets"
      required: true
      status: pending

    - id: CFG-007
      task: "Set Resolved stage"
      action: "Select stage for resolved tickets"
      required: true
      status: pending

    - id: CFG-008
      task: "Set Closed stage"
      action: "Select stage for closed tickets"
      required: true
      status: pending

    - id: CFG-009
      task: "Set Cancel stage"
      action: "Select stage for cancelled tickets"
      required: true
      status: pending

    - id: CFG-010
      task: "Enable auto-add customer as follower"
      action: "Check 'Auto Add Customer as Follower'"
      recommended: true
      status: pending

    - id: CFG-011
      task: "Save settings"
      action: "Click Save button"
      required: true
      status: pending

  team_setup:
    - id: TEAM-001
      task: "Navigate to teams"
      action: "Helpdesk → Configuration → Teams"
      status: pending

    - id: TEAM-002
      task: "Create first team"
      action: "Click Create"
      status: pending

    - id: TEAM-003
      task: "Set team name"
      action: "Enter descriptive name (e.g., 'Support Team')"
      required: true
      status: pending

    - id: TEAM-004
      task: "Assign team head"
      action: "Select user as team leader"
      required: true
      status: pending

    - id: TEAM-005
      task: "Add team members"
      action: "Select users who will handle tickets"
      recommended: true
      status: pending

    - id: TEAM-006
      task: "Set working hours"
      action: "Select resource calendar"
      required: true
      status: pending

    - id: TEAM-007
      task: "Save team"
      action: "Click Save"
      status: pending

  user_permissions:
    - id: USER-001
      task: "Navigate to users"
      action: "Settings → Users & Companies → Users"
      status: pending

    - id: USER-002
      task: "Assign Support Manager role"
      action: "Edit admin user → Helpdesk → Support Manager → Save"
      required: true
      status: pending

    - id: USER-003
      task: "Assign Team Leader role"
      action: "Edit leader users → Helpdesk → Team Leader → Save"
      recommended: true
      status: pending

    - id: USER-004
      task: "Assign Support User role"
      action: "Edit agent users → Helpdesk → Support User → Save"
      recommended: true
      status: pending

  test_creation:
    - id: TEST-001
      task: "Create test ticket"
      action: "Helpdesk → Tickets → Create"
      status: pending

    - id: TEST-002
      task: "Fill required fields"
      action: "Set customer, subject, team"
      status: pending

    - id: TEST-003
      task: "Save test ticket"
      action: "Click Save"
      status: pending

    - id: TEST-004
      task: "Verify ticket number generated"
      verification: "Ticket shows HD00001 or similar"
      status: pending

    - id: TEST-005
      task: "Delete or resolve test ticket"
      action: "Clean up test data"
      status: pending
```

---

## SLA Setup Checklist

```yaml
checklist:
  id: SLA
  name: "SLA Policy Configuration"
  description: "Set up service level agreements"
  estimated_time: "15 minutes"

  prerequisites:
    - id: SLA-PRE-001
      task: "At least one team exists"
      verification: "Helpdesk → Configuration → Teams has entries"
      status: pending

    - id: SLA-PRE-002
      task: "Working hours calendar configured"
      verification: "Team has working hours assigned"
      status: pending

  policy_creation:
    - id: SLA-001
      task: "Navigate to SLA Policies"
      action: "Helpdesk → Configuration → SLA Policies"
      status: pending

    - id: SLA-002
      task: "Create First Response SLA"
      action: "Create policy for first response time"
      fields:
        name: "First Response - 4 Hours"
        team: "[Your team]"
        target_type: "reaching_stage"
        target_stage: "In Progress"
        days: 0
        hours: 4
        minutes: 0
      status: pending

    - id: SLA-003
      task: "Create Resolution SLA"
      action: "Create policy for resolution time"
      fields:
        name: "Resolution - 24 Hours"
        team: "[Your team]"
        target_type: "reaching_stage"
        target_stage: "Resolved"
        days: 1
        hours: 0
        minutes: 0
      status: pending

    - id: SLA-004
      task: "Create Urgent SLA (optional)"
      action: "Create faster SLA for urgent tickets"
      fields:
        name: "Urgent Resolution - 4 Hours"
        team: "[Your team]"
        ticket_type: "Urgent"
        target_type: "reaching_stage"
        target_stage: "Resolved"
        days: 0
        hours: 4
        minutes: 0
      status: pending

  verification:
    - id: SLA-VER-001
      task: "Create test ticket with team"
      action: "Create ticket assigned to team with SLA"
      status: pending

    - id: SLA-VER-002
      task: "Verify SLA appears on ticket"
      verification: "Ticket shows SLA deadline and status"
      status: pending
```

---

## Daily Operations Checklist

```yaml
checklist:
  id: DAILY-OPS
  name: "Daily Helpdesk Operations"
  description: "Routine tasks for support agents"
  frequency: "daily"

  start_of_day:
    - id: DAY-001
      task: "Check new tickets"
      action: "Helpdesk → Tickets → Filter: New"
      status: pending

    - id: DAY-002
      task: "Review assigned tickets"
      action: "Helpdesk → Tickets → Filter: My Tickets"
      status: pending

    - id: DAY-003
      task: "Check SLA alerts"
      action: "Look for red/orange SLA indicators"
      status: pending

    - id: DAY-004
      task: "Check pending customer responses"
      action: "Filter: state = customer_replied"
      status: pending

  during_day:
    - id: DAY-005
      task: "Update ticket stages as work progresses"
      action: "Move tickets to appropriate stage"
      status: pending

    - id: DAY-006
      task: "Respond to customer messages"
      action: "Reply to tickets with customer messages"
      status: pending

    - id: DAY-007
      task: "Track time on tickets"
      action: "Start timer before working, stop when done"
      status: pending

    - id: DAY-008
      task: "Add internal notes for complex issues"
      action: "Log notes for team reference"
      status: pending

  end_of_day:
    - id: DAY-009
      task: "Update all worked ticket statuses"
      action: "Ensure stages reflect current state"
      status: pending

    - id: DAY-010
      task: "Stop any running timers"
      action: "End time tracking sessions"
      status: pending

    - id: DAY-011
      task: "Flag tickets needing follow-up"
      action: "Star or note tickets for tomorrow"
      status: pending
```

---

## Troubleshooting Checklist

```yaml
checklist:
  id: TROUBLESHOOT
  name: "Helpdesk Troubleshooting"
  description: "Systematic issue diagnosis"

  general_checks:
    - id: TS-001
      issue: "Any helpdesk issue"
      check: "Odoo service status"
      action: "Verify Odoo is running"
      command: "systemctl status odoo"
      status: pending

    - id: TS-002
      issue: "Any helpdesk issue"
      check: "Module installed"
      action: "Apps → search 'helpdesk' → verify 'Installed'"
      status: pending

    - id: TS-003
      issue: "Any helpdesk issue"
      check: "User permissions"
      action: "Settings → Users → verify helpdesk group assigned"
      status: pending

  ticket_issues:
    - id: TS-TICKET-001
      issue: "Cannot create tickets"
      check: "Required fields"
      action: "Ensure Partner and Subject are filled"
      status: pending

    - id: TS-TICKET-002
      issue: "Cannot create tickets"
      check: "Team exists"
      action: "Verify at least one team configured"
      status: pending

    - id: TS-TICKET-003
      issue: "Tickets not visible"
      check: "User role matches visibility"
      action: "User sees own tickets; Leader sees team; Manager sees all"
      status: pending

    - id: TS-TICKET-004
      issue: "Stage change blocked"
      check: "Stage group access"
      action: "Edit stage → check Group Access field"
      status: pending

  email_issues:
    - id: TS-EMAIL-001
      issue: "Emails not sending"
      check: "Outgoing mail server"
      action: "Settings → Technical → Outgoing Mail Servers → Test"
      status: pending

    - id: TS-EMAIL-002
      issue: "Emails not sending"
      check: "Email templates linked"
      action: "Stage → Email Templates field populated"
      status: pending

    - id: TS-EMAIL-003
      issue: "Emails not sending"
      check: "Mail queue"
      action: "Settings → Technical → Emails → check for failed"
      status: pending

  sla_issues:
    - id: TS-SLA-001
      issue: "SLA not showing"
      check: "SLA policy exists for team"
      action: "Helpdesk → Configuration → SLA Policies"
      status: pending

    - id: TS-SLA-002
      issue: "SLA not showing"
      check: "Team has working calendar"
      action: "Team → Working Hours field populated"
      status: pending

    - id: TS-SLA-003
      issue: "SLA deadline wrong"
      check: "Working calendar correct"
      action: "Verify calendar days/hours match expectations"
      status: pending

  portal_issues:
    - id: TS-PORTAL-001
      issue: "Customer can't access portal"
      check: "Portal access granted"
      action: "Contacts → customer → Action → Grant Portal Access"
      status: pending

    - id: TS-PORTAL-002
      issue: "Customer sees wrong tickets"
      check: "Partner ID matches"
      action: "Ticket partner = portal user partner"
      status: pending
```

---

## Team Leader Weekly Review

```yaml
checklist:
  id: WEEKLY-LEADER
  name: "Team Leader Weekly Review"
  description: "Weekly management tasks"
  frequency: "weekly"

  metrics_review:
    - id: WEEK-001
      task: "Review ticket volume"
      action: "Check tickets created this week"
      metric: "Total tickets, tickets per day"
      status: pending

    - id: WEEK-002
      task: "Review SLA compliance"
      action: "Check SLA pass/fail rates"
      metric: "% tickets meeting SLA"
      status: pending

    - id: WEEK-003
      task: "Review resolution times"
      action: "Check average time to resolve"
      metric: "Hours from create to resolve"
      status: pending

    - id: WEEK-004
      task: "Review customer feedback"
      action: "Check ratings and comments"
      metric: "Average rating, negative feedback"
      status: pending

  team_management:
    - id: WEEK-005
      task: "Review workload distribution"
      action: "Check tickets per agent"
      status: pending

    - id: WEEK-006
      task: "Identify training needs"
      action: "Note recurring issues or escalations"
      status: pending

    - id: WEEK-007
      task: "Check for stale tickets"
      action: "Find tickets inactive > 7 days"
      status: pending

    - id: WEEK-008
      task: "Update knowledge base"
      action: "Document solutions for common issues"
      status: pending

  process_improvement:
    - id: WEEK-009
      task: "Review quick reply templates"
      action: "Add/update common responses"
      status: pending

    - id: WEEK-010
      task: "Review categories"
      action: "Add new categories if needed"
      status: pending
```

---

## New Agent Onboarding

```yaml
checklist:
  id: ONBOARD
  name: "New Support Agent Onboarding"
  description: "Steps to onboard a new helpdesk agent"
  estimated_time: "2 hours"

  account_setup:
    - id: ONB-001
      task: "Create Odoo user account"
      action: "Settings → Users → Create"
      status: pending

    - id: ONB-002
      task: "Assign Support User role"
      action: "User → Helpdesk → Support User"
      status: pending

    - id: ONB-003
      task: "Add to team"
      action: "Helpdesk → Configuration → Teams → edit → add member"
      status: pending

    - id: ONB-004
      task: "Set user email"
      action: "Ensure valid email for notifications"
      status: pending

  training:
    - id: ONB-005
      task: "Share User Guide"
      action: "Provide link to 04_USER_GUIDE.md"
      status: pending

    - id: ONB-006
      task: "Demo ticket creation"
      action: "Walk through creating a ticket"
      status: pending

    - id: ONB-007
      task: "Demo ticket workflow"
      action: "Show stage transitions, reply, resolve"
      status: pending

    - id: ONB-008
      task: "Demo time tracking"
      action: "Show start/stop timer"
      status: pending

    - id: ONB-009
      task: "Explain SLA expectations"
      action: "Review SLA policies and deadlines"
      status: pending

    - id: ONB-010
      task: "Review quick replies"
      action: "Show available quick reply templates"
      status: pending

  practice:
    - id: ONB-011
      task: "Create practice ticket"
      action: "Agent creates test ticket"
      status: pending

    - id: ONB-012
      task: "Reply to practice ticket"
      action: "Agent sends test reply"
      status: pending

    - id: ONB-013
      task: "Resolve practice ticket"
      action: "Agent resolves test ticket"
      status: pending

    - id: ONB-014
      task: "Verify agent can see real tickets"
      action: "Check visibility of team tickets"
      status: pending
```

---

## Migration Checklist

```yaml
checklist:
  id: MIGRATE
  name: "Helpdesk Data Migration"
  description: "Migrate data to or upgrade helpdesk module"
  estimated_time: "Variable"

  pre_migration:
    - id: MIG-001
      task: "Create database backup"
      action: "pg_dump -U odoo database > backup.sql"
      critical: true
      status: pending

    - id: MIG-002
      task: "Document current configuration"
      action: "Export teams, stages, SLA policies"
      status: pending

    - id: MIG-003
      task: "Export ticket data"
      action: "Export tickets to CSV for reference"
      status: pending

    - id: MIG-004
      task: "Notify users of downtime"
      action: "Communicate maintenance window"
      status: pending

  migration:
    - id: MIG-005
      task: "Update module files"
      action: "Replace module with new version"
      status: pending

    - id: MIG-006
      task: "Upgrade module"
      action: "odoo-bin -d database -u sh_all_in_one_helpdesk"
      status: pending

    - id: MIG-007
      task: "Review upgrade logs"
      action: "Check for errors in Odoo logs"
      status: pending

  post_migration:
    - id: MIG-008
      task: "Verify data integrity"
      action: "Check ticket counts match"
      status: pending

    - id: MIG-009
      task: "Verify configuration"
      action: "Check teams, stages, SLA intact"
      status: pending

    - id: MIG-010
      task: "Test ticket operations"
      action: "Create, edit, resolve test ticket"
      status: pending

    - id: MIG-011
      task: "Notify users of completion"
      action: "Communicate system available"
      status: pending
```

---

## How to Use These Checklists

### For AI Assistants

1. **Present checklist to user** when they request help with the related task
2. **Track progress** by updating status (pending → in_progress → completed)
3. **Reference by ID** (e.g., "Let's complete CFG-001")
4. **Skip completed items** when resuming interrupted tasks

### For Humans

1. Copy the relevant checklist
2. Work through items in order
3. Mark items complete as you go
4. Use IDs to communicate with AI assistants

### Status Values

```yaml
status_values:
  - pending: "Not yet started"
  - in_progress: "Currently working on"
  - completed: "Finished successfully"
  - blocked: "Cannot proceed - needs resolution"
  - skipped: "Intentionally skipped"
```

---

*Return to [INDEX.md](INDEX.md)*
