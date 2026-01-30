# Integration Guide - SH All-In-One Helpdesk

Complete guide to using helpdesk integrations with Sales, Purchase, Invoicing, CRM, Projects, and Timesheets.

---

## Table of Contents

1. [Sales Order Integration](#sales-order-integration)
2. [Purchase Order Integration](#purchase-order-integration)
3. [Invoice Integration](#invoice-integration)
4. [CRM Integration](#crm-integration)
5. [Project Task Integration](#project-task-integration)
6. [Timesheet Integration](#timesheet-integration)
7. [Integration Matrix](#integration-matrix)

---

## Sales Order Integration

**Sub-module:** `sh_helpdesk_so`

### Overview

Link helpdesk tickets to sales orders bidirectionally:
- Create tickets from sales orders
- Create sales orders from tickets
- View related records from either side

### From Sales Order → Ticket

#### Creating a Ticket from Sales Order

1. Open a sales order
2. Click `Create Ticket` button (or smart button if exists)
3. Ticket is created with:
   - Customer pre-filled
   - Products from SO linked
   - Reference to SO

#### Viewing Tickets from Sales Order

1. Open sales order
2. Click `Tickets` smart button
3. View list of linked tickets

### From Ticket → Sales Order

#### Creating Sales Order from Ticket

1. Open ticket
2. Click `Create Sales Order` button
3. New SO is created with:
   - Customer from ticket
   - Products from ticket (if any)
   - Reference to ticket

#### Viewing Sales Orders from Ticket

1. Open ticket
2. Click `Sales Orders` smart button
3. View list of linked SOs

### Technical Details

**New Fields on Sale Order:**
```python
sh_sale_ticket_ids = fields.Many2many('helpdesk.ticket')
sale_ticket_count = fields.Integer(compute='_compute_sale_ticket_count')
```

**New Fields on Ticket:**
```python
sh_sale_order_ids = fields.Many2many('sale.order')
sale_order_count = fields.Integer(compute='_compute_sale_order_count')
```

**Key Methods:**
```python
# On sale.order
def action_create_sale_ticket(self):
    """Create ticket from this SO."""

def action_view_sale_tickets(self):
    """Open linked tickets."""

# On helpdesk.ticket
def action_sale_create_order(self):
    """Create SO from this ticket."""

def action_view_sale_orders(self):
    """Open linked SOs."""
```

### Use Cases

| Scenario | Action |
|----------|--------|
| Customer has issue with order | Create ticket from SO |
| Support request needs quote | Create SO from ticket |
| Track all support for a sale | View tickets from SO |
| Verify purchase before support | View SO from ticket |

---

## Purchase Order Integration

**Sub-module:** `sh_helpdesk_po`

### Overview

Link helpdesk tickets to purchase orders:
- Report vendor issues via tickets
- Track procurement-related support

### From Purchase Order → Ticket

#### Creating a Ticket from PO

1. Open a purchase order
2. Click `Create Ticket` button
3. Ticket created with:
   - Vendor as partner (or configure customer)
   - Products linked
   - PO reference

#### Viewing Tickets from PO

1. Open purchase order
2. Click `Tickets` smart button
3. View linked tickets

### From Ticket → Purchase Order

#### Creating PO from Ticket

1. Open ticket
2. Click `Create Purchase Order` button
3. PO created with:
   - Products from ticket
   - Partner reference

### Technical Details

**New Fields on Purchase Order:**
```python
sh_purchase_ticket_ids = fields.Many2many('helpdesk.ticket')
purchase_ticket_count = fields.Integer(compute='_compute_purchase_ticket_count')
```

**New Fields on Ticket:**
```python
sh_purchase_order_ids = fields.Many2many('purchase.order')
purchase_order_count = fields.Integer(compute='_compute_purchase_order_count')
```

### Use Cases

| Scenario | Action |
|----------|--------|
| Vendor delivery issue | Create ticket from PO |
| Support needs parts ordered | Create PO from ticket |
| Track procurement issues | View tickets from PO |

---

## Invoice Integration

**Sub-module:** `sh_helpdesk_invoice`

### Overview

Link tickets to invoices:
- Handle billing disputes
- Track invoice-related support

### From Invoice → Ticket

#### Creating Ticket from Invoice

1. Open invoice (customer or vendor)
2. Click `Create Ticket` button
3. Ticket created with:
   - Partner from invoice
   - Invoice reference
   - Products linked

### From Ticket → Invoice

#### Creating Invoice from Ticket

1. Open ticket
2. Click `Create Invoice` button
3. Invoice created with:
   - Customer from ticket
   - Line items from ticket products

### Technical Details

**New Fields on Account Move:**
```python
sh_ticket_ids = fields.Many2many('helpdesk.ticket')
ticket_count = fields.Integer(compute='_compute_ticket_count')
```

**New Fields on Ticket:**
```python
sh_invoice_ids = fields.Many2many('account.move')
invoice_count = fields.Integer(compute='_compute_invoice_count')
```

### Use Cases

| Scenario | Action |
|----------|--------|
| Customer disputes charge | Create ticket from invoice |
| Support billable | Create invoice from ticket |
| View support for billing issue | View tickets from invoice |

---

## CRM Integration

**Sub-module:** `sh_helpdesk_crm`

### Overview

Link tickets to CRM leads and opportunities:
- Convert support to sales opportunities
- Track pre-sales support

### From Lead/Opportunity → Ticket

#### Creating Ticket from Lead

1. Open lead or opportunity
2. Click `Create Ticket` button
3. Ticket created with:
   - Contact from lead
   - Description from lead
   - Lead reference

### From Ticket → Lead/Opportunity

#### Creating Lead from Ticket

1. Open ticket
2. Click `Create Lead` button
3. Lead created with:
   - Partner from ticket
   - Description from ticket

#### Creating Opportunity from Ticket

1. Open ticket
2. Click `Create Opportunity` button
3. Opportunity created with:
   - Partner from ticket
   - Expected revenue (if applicable)

### Technical Details

**New Fields on CRM Lead:**
```python
sh_ticket_ids = fields.Many2many('helpdesk.ticket')
ticket_count = fields.Integer(compute='_compute_ticket_count')
```

**New Fields on Ticket:**
```python
sh_lead_ids = fields.Many2many('crm.lead')
lead_count = fields.Integer(compute='_compute_lead_count')
opportunity_count = fields.Integer(compute='_compute_opportunity_count')
```

### Use Cases

| Scenario | Action |
|----------|--------|
| Lead has technical questions | Create ticket from lead |
| Support identifies upsell | Create opportunity from ticket |
| Pre-sales support tracking | Link ticket to lead |

---

## Project Task Integration

**Sub-module:** `sh_helpdesk_task`

### Overview

Link tickets to project tasks:
- Convert tickets to development tasks
- Track implementation work
- Link fixes to tickets

### From Ticket → Task

#### Creating Task from Ticket

1. Open ticket
2. Click `Create Task` button
3. Task created with:
   - Name from ticket
   - Description from ticket
   - Assigned user from ticket
   - Attachments copied

### From Task → Ticket

#### Viewing Tickets from Task

1. Open project task
2. Click `Tickets` smart button
3. View linked tickets

### Technical Details

**New Fields on Project Task:**
```python
sh_ticket_ids = fields.Many2many('helpdesk.ticket')
ticket_count = fields.Integer(compute='_compute_ticket_count')
```

**New Fields on Ticket:**
```python
task_ids = fields.Many2many('project.task')
task_count = fields.Integer(compute='_compute_task_count')
```

### Workflow Example

```
Ticket Created (Bug Report)
        ↓
Triage and Prioritize
        ↓
Create Task (Development Work)
        ↓
Task Completed
        ↓
Update Ticket
        ↓
Ticket Resolved
```

### Use Cases

| Scenario | Action |
|----------|--------|
| Bug requires development | Create task from ticket |
| Feature request becomes project | Create task from ticket |
| Track development progress | View tasks from ticket |
| Find original request | View tickets from task |

---

## Timesheet Integration

**Sub-module:** `sh_helpdesk_timesheet`

### Overview

Track time spent on tickets:
- Start/stop timer on tickets
- Auto-create timesheet entries
- Link to analytics/projects

### Using Time Tracking

#### Starting Timer

1. Open ticket
2. Click `Start` button (play icon)
3. Timer begins

**What happens:**
- Start time recorded
- Timesheet entry created
- Ticket marked as "running"

#### Stopping Timer

1. Click `Stop` button
2. Enter work description (if configured)
3. Click `End Ticket`

**What happens:**
- End time recorded
- Duration calculated
- Timesheet entry completed

### Configuration

**Company Settings:**
| Setting | Purpose |
|---------|---------|
| Default Project | Project for timesheet entries |
| Allow Multiple Tickets | Track multiple tickets simultaneously |
| Show Description on End | Prompt for description when stopping |

### Technical Details

**New Fields on Ticket:**
```python
start_time = fields.Datetime('Start Time')
end_time = fields.Datetime('End Time')
total_time = fields.Char('Total Time')
duration = fields.Float(compute='_compute_duration')
ticket_running = fields.Boolean('Is Running')
timesheet_ids = fields.One2many('account.analytic.line', 'ticket_id')
```

**New Fields on Analytic Line:**
```python
ticket_id = fields.Many2one('helpdesk.ticket')
start_date = fields.Datetime('Start')
end_date = fields.Datetime('End')
```

**Key Methods:**
```python
def action_ticket_start(self):
    """Start time tracking."""

def action_ticket_end(self):
    """Stop time tracking and record time."""

def get_duration(self, ticket):
    """Get elapsed time in milliseconds."""
```

### Frontend Timer Widget

The timesheet integration includes a JavaScript timer widget:
- Real-time duration display
- Start/Stop controls
- Persists across page navigation

**Files:**
- `static/src/helpdesk_timesheet/js/time_track.js`
- `static/src/helpdesk_timesheet/xml/time_track.xml`
- `static/src/helpdesk_timesheet/scss/time_track.scss`

### Use Cases

| Scenario | Action |
|----------|--------|
| Track support time for billing | Start timer when working |
| Analyze time per issue type | Report on timesheet by category |
| Resource planning | Review time allocation |

---

## Integration Matrix

### Quick Reference: What Creates What

| From | Create Ticket | Create SO | Create PO | Create Invoice | Create Lead | Create Task |
|------|---------------|-----------|-----------|----------------|-------------|-------------|
| **Ticket** | - | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Sales Order** | ✓ | - | - | - | - | - |
| **Purchase Order** | ✓ | - | - | - | - | - |
| **Invoice** | ✓ | - | - | - | - | - |
| **CRM Lead** | ✓ | - | - | - | - | - |
| **Project Task** | - | - | - | - | - | - |

### Data Flow Summary

```
                    ┌──────────────┐
                    │   TICKET     │
                    └──────┬───────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│  Sales Order  │  │ Purchase Ord. │  │    Invoice    │
└───────────────┘  └───────────────┘  └───────────────┘
        │
        ▼
┌───────────────┐
│   CRM Lead    │
└───────────────┘
        │
        ▼
┌───────────────┐
│  Project Task │
└───────────────┘
        │
        ▼
┌───────────────┐
│   Timesheet   │
└───────────────┘
```

### Integration Fields Summary

| Model | Field | Related Model | Type |
|-------|-------|---------------|------|
| helpdesk.ticket | sh_sale_order_ids | sale.order | M2M |
| helpdesk.ticket | sh_purchase_order_ids | purchase.order | M2M |
| helpdesk.ticket | sh_invoice_ids | account.move | M2M |
| helpdesk.ticket | sh_lead_ids | crm.lead | M2M |
| helpdesk.ticket | task_ids | project.task | M2M |
| helpdesk.ticket | timesheet_ids | account.analytic.line | O2M |
| sale.order | sh_sale_ticket_ids | helpdesk.ticket | M2M |
| purchase.order | sh_purchase_ticket_ids | helpdesk.ticket | M2M |
| account.move | sh_ticket_ids | helpdesk.ticket | M2M |
| crm.lead | sh_ticket_ids | helpdesk.ticket | M2M |
| project.task | sh_ticket_ids | helpdesk.ticket | M2M |
| account.analytic.line | ticket_id | helpdesk.ticket | M2O |

---

## Best Practices

### When to Use Integrations

```yaml
integration_guidelines:
  sales_orders:
    use_when:
      - "Customer reports issue with purchased item"
      - "Need to quote support services"
      - "Tracking warranty support"
    avoid_when:
      - "Generic questions unrelated to purchases"

  purchase_orders:
    use_when:
      - "Reporting vendor/supplier issues"
      - "Tracking procurement problems"
      - "Parts needed for support"
    avoid_when:
      - "Customer-facing issues"

  invoices:
    use_when:
      - "Billing disputes"
      - "Payment issues"
      - "Charging for support"
    avoid_when:
      - "Technical issues unrelated to billing"

  crm:
    use_when:
      - "Pre-sales technical questions"
      - "Support reveals upsell opportunity"
      - "Prospect evaluation support"
    avoid_when:
      - "Existing customer routine support"

  tasks:
    use_when:
      - "Bug requires development fix"
      - "Feature request accepted"
      - "Custom work needed"
    avoid_when:
      - "Standard support resolvable by team"

  timesheet:
    use_when:
      - "Billable support"
      - "Time tracking for analysis"
      - "Project-based support"
    avoid_when:
      - "Quick interactions < 5 minutes"
```

### Reporting Across Integrations

To analyze cross-functional data:

1. **Tickets by Sales Revenue**
   - Join tickets to sales orders
   - Group by SO amount

2. **Support Cost vs Sales**
   - Timesheet hours × rate
   - Compare to linked SO value

3. **Lead Conversion from Support**
   - Track leads created from tickets
   - Monitor opportunity win rate

---

*Next: [Troubleshooting](08_TROUBLESHOOTING.md)*
