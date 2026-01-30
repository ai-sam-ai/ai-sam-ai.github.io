# SH All-In-One Helpdesk - Complete Documentation

**Module:** `sh_all_in_one_helpdesk`
**Version:** 18.0.0.0
**Odoo Version:** 18.0
**Original Author:** Softhealer Technologies
**Maintained By:** SAM AI / SME Business Support

---

## Documentation Index

| Document | Description | Audience |
|----------|-------------|----------|
| [01_QUICK_START.md](01_QUICK_START.md) | Get up and running in 15 minutes | All Users |
| [02_INSTALLATION.md](02_INSTALLATION.md) | Installation and deployment guide | System Admins |
| [03_CONFIGURATION.md](03_CONFIGURATION.md) | System configuration reference | Administrators |
| [04_USER_GUIDE.md](04_USER_GUIDE.md) | Day-to-day usage guide | Support Agents |
| [05_ADMIN_GUIDE.md](05_ADMIN_GUIDE.md) | Administration and management | Team Leaders/Managers |
| [06_TECHNICAL_REFERENCE.md](06_TECHNICAL_REFERENCE.md) | Models, fields, methods | Developers |
| [07_INTEGRATIONS.md](07_INTEGRATIONS.md) | Sales, Purchase, CRM integrations | All Users |
| [08_TROUBLESHOOTING.md](08_TROUBLESHOOTING.md) | Common issues and solutions | All Users |
| [CHECKLISTS.md](CHECKLISTS.md) | AI-parseable task checklists | AI Assistants |

---

## Module Overview

### What is SH All-In-One Helpdesk?

A comprehensive, enterprise-grade helpdesk and ticket management system for Odoo 18. It provides:

- **Multi-Team Support** - Organize support staff into teams with leaders
- **SLA Management** - Define and track service level agreements
- **Customer Portal** - Self-service ticket submission and tracking
- **Email Integration** - Automatic ticket creation from emails
- **Business Integrations** - Link tickets to Sales, Purchases, Invoices, CRM, Projects
- **Time Tracking** - Built-in timesheet integration
- **WhatsApp Integration** - Send messages via WhatsApp
- **Advanced Dashboard** - Real-time metrics and reporting

### Key Features

```
TICKET MANAGEMENT
├── Create, assign, track tickets
├── Multi-stage workflow
├── Priority and categorization
├── File attachments
├── Email notifications
└── Customer portal access

TEAM MANAGEMENT
├── Multiple support teams
├── Team leaders and members
├── Working hours calendars
├── Auto-assignment rules
└── Team-based filtering

SLA MANAGEMENT
├── Define SLA policies
├── Deadline calculation
├── Working hours aware
├── Status tracking (pass/fail)
├── Reminder alarms
└── Escalation support

INTEGRATIONS
├── Sales Orders
├── Purchase Orders
├── Invoices
├── Project Tasks
├── CRM Leads/Opportunities
└── Timesheets
```

---

## User Roles

| Role | Group | Permissions |
|------|-------|-------------|
| **Support User** | `helpdesk_group_user` | View/edit own tickets, create tickets |
| **Team Leader** | `helpdesk_group_team_leader` | Manage team tickets, view team reports |
| **Support Manager** | `helpdesk_group_manager` | Full access, configuration, all tickets |

---

## Quick Navigation

### For New Users
1. Start with [Quick Start Guide](01_QUICK_START.md)
2. Read [User Guide](04_USER_GUIDE.md) for daily operations
3. Check [Troubleshooting](08_TROUBLESHOOTING.md) for common issues

### For Administrators
1. Follow [Installation Guide](02_INSTALLATION.md)
2. Complete [Configuration Guide](03_CONFIGURATION.md)
3. Review [Admin Guide](05_ADMIN_GUIDE.md) for management

### For Developers
1. Read [Technical Reference](06_TECHNICAL_REFERENCE.md)
2. Review [Integrations](07_INTEGRATIONS.md) for API details

### For AI Assistants
1. Use [CHECKLISTS.md](CHECKLISTS.md) for structured task lists
2. Reference [Technical Reference](06_TECHNICAL_REFERENCE.md) for field/method details

---

## Module Dependencies

```yaml
Required Odoo Modules:
  - mail          # Messaging and chatter
  - portal        # Customer portal access
  - product       # Product references
  - resource      # Working hours calendars
  - sale_management  # Sales order integration
  - purchase      # Purchase order integration
  - account       # Invoice integration
  - hr_timesheet  # Time tracking
  - project       # Task integration
  - crm           # Lead/opportunity integration

Python Dependencies:
  - beautifulsoup4  # HTML to text conversion (bundled in lib/)
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 18.0.0.0 | 2024 | Initial Odoo 18 release |
| 18.0.0.1 | 2025 | Added local html2text lib, removed external dependency |

---

## Support

- **Documentation Issues:** Update this documentation
- **Module Issues:** Contact SAM AI support
- **Original Module:** Softhealer Technologies

---

## License

Original module: OPL-1 (Odoo Proprietary License)

---

*Last Updated: 2026-01-30*
