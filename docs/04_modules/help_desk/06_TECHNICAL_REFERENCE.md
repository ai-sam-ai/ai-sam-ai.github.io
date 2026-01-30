# Technical Reference - SH All-In-One Helpdesk

Complete technical documentation for developers and integrators.

---

## Table of Contents

1. [Module Structure](#module-structure)
2. [Models Reference](#models-reference)
3. [API Methods](#api-methods)
4. [Security Model](#security-model)
5. [Hooks and Events](#hooks-and-events)
6. [Customization Points](#customization-points)
7. [Database Schema](#database-schema)

---

## Module Structure

```
sh_all_in_one_helpdesk/
├── __init__.py
├── __manifest__.py
├── controllers/
│   ├── __init__.py
│   ├── main.py              # HTTP endpoints
│   └── portal.py            # Portal controllers
├── data/
│   ├── helpdesk_data.xml
│   ├── helpdesk_stage_data.xml
│   ├── helpdesk_email_data.xml
│   ├── helpdesk_cron_data.xml
│   └── helpdesk_reminder_*.xml
├── lib/
│   ├── __init__.py
│   └── html2text.py         # Bundled HTML converter
├── models/
│   ├── __init__.py
│   ├── helpdesk_ticket.py
│   ├── helpdesk_team.py
│   ├── helpdesk_stages.py
│   ├── helpdesk_config_settings.py
│   ├── helpdesk_policies.py
│   ├── helpdesk_alarm.py
│   └── ... (other models)
├── security/
│   ├── sh_helpdesk_security.xml
│   ├── send_mail_security.xml
│   └── ir.model.access.csv
├── sh_helpdesk_so/          # Sales integration
├── sh_helpdesk_po/          # Purchase integration
├── sh_helpdesk_invoice/     # Invoice integration
├── sh_helpdesk_timesheet/   # Timesheet integration
├── sh_helpdesk_task/        # Task integration
├── sh_helpdesk_crm/         # CRM integration
├── static/
│   └── src/
│       ├── css/
│       ├── js/
│       └── xml/
├── views/
│   └── *.xml
└── wizard/
    ├── __init__.py
    └── mail_compose.py
```

---

## Models Reference

### helpdesk.ticket

**Primary ticket model with full field reference.**

```python
class HelpdeskTicket(models.Model):
    _name = 'helpdesk.ticket'
    _inherit = ['portal.mixin', 'mail.thread', 'mail.activity.mixin']
    _description = 'Helpdesk Ticket'
```

#### Core Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | Char | Auto | Ticket number (sequence) |
| `partner_id` | Many2one | Yes | Customer contact |
| `ticket_subject` | Char | Yes | Brief description |
| `description` | Html | No | Detailed description |
| `stage_id` | Many2one | No | Current stage |
| `team_id` | Many2one | No | Assigned team |
| `user_id` | Many2one | No | Primary assignee |
| `sh_user_ids` | Many2many | No | Additional assignees |
| `priority` | Many2one | No | Priority level |
| `category_id` | Many2one | No | Category |
| `sub_category_id` | Many2one | No | Sub-category |
| `ticket_type` | Many2one | No | Ticket type |
| `tag_ids` | Many2many | No | Tags |
| `product_ids` | Many2many | No | Related products |

#### Customer Fields

| Field | Type | Description |
|-------|------|-------------|
| `email` | Char | Customer email |
| `mobile_no` | Char | Customer mobile |
| `person_name` | Char | Contact person name |

#### Status Fields

| Field | Type | Values |
|-------|------|--------|
| `state` | Selection | customer_replied, staff_replied |
| `active` | Boolean | Archive flag |

#### Date Fields

| Field | Type | Description |
|-------|------|-------------|
| `create_date` | Datetime | Creation timestamp |
| `close_date` | Datetime | When closed |
| `cancel_date` | Datetime | When cancelled |
| `replied_date` | Datetime | Last reply |
| `sh_due_date` | Datetime | Due date/reminder |

#### Computed Fields

| Field | Type | Compute Method |
|-------|------|----------------|
| `sh_sla_deadline` | Datetime | From SLA status |
| `sh_status` | Selection | SLA pass/fail |
| `done_stage_boolean` | Boolean | Is done stage |
| `open_boolean` | Boolean | Is open |

#### Key Methods

```python
def action_approve(self):
    """Advance to next stage, send emails."""

def action_done(self):
    """Mark as resolved, move to done stage."""

def action_closed(self):
    """Close ticket, record close info."""

def action_cancel(self):
    """Cancel ticket, record reason."""

def action_open(self):
    """Reopen closed ticket."""

def helpdesk_sla_create(self, team_id, ticket_type):
    """Create SLA status records for ticket."""

def change_sh_status(self):
    """Update SLA status based on current state."""

@api.model
def _run_auto_close_ticket(self):
    """Cron: Auto-close inactive tickets."""
```

---

### helpdesk.team

```python
class HelpdeskTeam(models.Model):
    _name = 'helpdesk.team'
    _description = 'Helpdesk Team'
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | Char | Team name |
| `team_head` | Many2one(res.users) | Team leader |
| `team_members` | Many2many(res.users) | Team members |
| `sh_resource_calendar_id` | Many2one | Working hours |
| `sla_count` | Integer | Computed SLA count |

---

### helpdesk.stages

```python
class HelpdeskStages(models.Model):
    _name = 'helpdesk.stages'
    _description = 'Helpdesk Stages'
    _order = 'sequence'
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | Char | Stage name |
| `sequence` | Integer | Sort order |
| `mail_template_ids` | Many2many | Email templates |
| `sh_next_stage` | Many2one | Auto-advance stage |
| `sh_group_ids` | Many2many | Required groups |
| `is_cancel_button_visible` | Boolean | Show cancel |
| `is_done_button_visible` | Boolean | Show done |

---

### sh.helpdesk.sla

```python
class SHHelpdeskSLA(models.Model):
    _name = 'sh.helpdesk.sla'
    _description = 'SLA Policy'
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | Char | Policy name |
| `sh_team_id` | Many2one | Target team |
| `sh_ticket_type_id` | Many2one | Target type (optional) |
| `sh_sla_target_type` | Selection | reaching_stage / assign_to |
| `sh_stage_id` | Many2one | Target stage |
| `sh_days` | Integer | Days |
| `sh_hours` | Integer | Hours |
| `sh_minutes` | Integer | Minutes |

---

### sh.helpdesk.sla.status

```python
class SHHelpdeskSLAStatus(models.Model):
    _name = 'sh.helpdesk.sla.status'
    _description = 'SLA Status'
```

| Field | Type | Description |
|-------|------|-------------|
| `sh_ticket_id` | Many2one | Parent ticket |
| `sh_sla_id` | Many2one | SLA policy |
| `sh_deadline` | Datetime | Computed deadline |
| `sh_status` | Selection | passed/failed/partial |
| `sh_done_sla_date` | Datetime | When target reached |
| `color` | Integer | UI color indicator |

#### Deadline Computation

```python
@api.depends('sh_sla_id', 'sh_ticket_id')
def _compute_sh_deadline(self):
    """
    Calculate deadline using team's working calendar.
    Uses resource.calendar.plan_days() and plan_hours().
    """
```

---

### res.company (Extensions)

Additional fields added to company model for configuration:

```python
# Stage configuration
new_stage_id = fields.Many2one('helpdesk.stages')
done_stage_id = fields.Many2one('helpdesk.stages')
cancel_stage_id = fields.Many2one('helpdesk.stages')
close_stage_id = fields.Many2one('helpdesk.stages')
reopen_stage_id = fields.Many2one('helpdesk.stages')

# Feature toggles
category = fields.Boolean('Enable Category')
sub_category = fields.Boolean('Enable Sub-Category')
customer_rating = fields.Boolean('Enable Rating')
sh_display_multi_user = fields.Boolean('Multi-User')

# Defaults
sh_default_team_id = fields.Many2one('helpdesk.team')
sh_default_user_id = fields.Many2one('res.users')

# Email configuration
allocation_mail_template_id = fields.Many2one('mail.template')
reply_mail_template_id = fields.Many2one('mail.template')
```

---

## API Methods

### HTTP Endpoints

**Base URL:** `/web/helpdesk/` or `/helpdesk/`

#### Dashboard Data

```python
@http.route('/get-ticket-counter-data', type='http', auth='user')
def get_ticket_counter_data(self, **kw):
    """
    Returns ticket counts by stage for dashboard.

    Parameters:
        filter: today|yesterday|weekly|monthly|yearly|custom
        start_date: ISO date (for custom)
        end_date: ISO date (for custom)
        team: team ID
        team_leader: user ID
        user: user ID

    Returns:
        JSON with stage counts
    """
```

#### Ticket Operations

```python
@http.route('/open-ticket', type='http', auth='user', methods=['POST'])
def open_ticket(self, **kw):
    """
    Opens ticket(s) in form view.

    Parameters:
        ticket_ids: comma-separated IDs

    Returns:
        Action to open tickets
    """
```

#### Feedback

```python
@http.route('/helpdesk/ticket/feedback/<int:ticket_id>',
            type='http', auth='public', methods=['POST'])
def submit_feedback(self, ticket_id, **kw):
    """
    Submit customer feedback.

    Parameters:
        rating: 1-6 scale
        comment: text feedback
    """
```

#### Report Download

```python
@http.route('/download/ht/<int:ticket_id>', type='http', auth='public')
def download_report(self, ticket_id, access_token=None):
    """
    Download ticket PDF report.

    Requires valid access_token for security.
    """
```

### Portal Routes

```python
@http.route('/my/tickets', type='http', auth='user', website=True)
def portal_tickets(self, **kw):
    """
    Customer portal ticket list.

    Access controlled by sh_portal_user_access field:
    - user: own tickets only
    - leader: team tickets
    - manager: all tickets
    """
```

---

## Security Model

### Groups Hierarchy

```xml
<record id="helpdesk_group_user" model="res.groups">
    <field name="name">Support User</field>
    <field name="category_id" ref="base.module_category_helpdesk"/>
</record>

<record id="helpdesk_group_team_leader" model="res.groups">
    <field name="name">Team Leader</field>
    <field name="implied_ids" eval="[(4, ref('helpdesk_group_user'))]"/>
</record>

<record id="helpdesk_group_manager" model="res.groups">
    <field name="name">Support Manager</field>
    <field name="implied_ids" eval="[(4, ref('helpdesk_group_team_leader'))]"/>
</record>
```

### Record Rules

```xml
<!-- User: own tickets only -->
<record id="helpdesk_ticket_user_rule" model="ir.rule">
    <field name="domain_force">
        ['|',
         ('user_id', '=', user.id),
         ('sh_user_ids', 'in', user.id)]
    </field>
    <field name="groups" eval="[(4, ref('helpdesk_group_user'))]"/>
</record>

<!-- Leader: team tickets -->
<record id="helpdesk_ticket_leader_rule" model="ir.rule">
    <field name="domain_force">
        ['|', '|', '|',
         ('user_id', '=', user.id),
         ('sh_user_ids', 'in', user.id),
         ('team_head', '=', user.id),
         ('team_id.team_head', '=', user.id)]
    </field>
    <field name="groups" eval="[(4, ref('helpdesk_group_team_leader'))]"/>
</record>

<!-- Manager: all tickets -->
<record id="helpdesk_ticket_manager_rule" model="ir.rule">
    <field name="domain_force">[(1, '=', 1)]</field>
    <field name="groups" eval="[(4, ref('helpdesk_group_manager'))]"/>
</record>
```

### Access Rights Matrix

```csv
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_ticket_user,ticket.user,model_helpdesk_ticket,helpdesk_group_user,1,1,1,0
access_ticket_manager,ticket.manager,model_helpdesk_ticket,helpdesk_group_manager,1,1,1,1
access_team_user,team.user,model_helpdesk_team,helpdesk_group_user,1,0,0,0
access_team_manager,team.manager,model_helpdesk_team,helpdesk_group_manager,1,1,1,1
```

---

## Hooks and Events

### Ticket Creation Hook

```python
@api.model
def create(self, vals):
    # Auto-generate sequence
    vals['name'] = self.env['ir.sequence'].next_by_code('helpdesk.ticket.sequence')

    # Auto-create partner if email provided
    if vals.get('email') and not vals.get('partner_id'):
        partner = self.env['res.partner'].create({
            'name': vals.get('person_name', vals['email']),
            'email': vals['email']
        })
        vals['partner_id'] = partner.id

    ticket = super().create(vals)

    # Create SLA status records
    ticket.helpdesk_sla_create(ticket.team_id, ticket.ticket_type)

    # Send creation email
    if ticket.stage_id.mail_template_ids:
        for template in ticket.stage_id.mail_template_ids:
            template.send_mail(ticket.id)

    return ticket
```

### Stage Change Hook

```python
def write(self, vals):
    if 'stage_id' in vals:
        # Send stage transition emails
        new_stage = self.env['helpdesk.stages'].browse(vals['stage_id'])
        for template in new_stage.mail_template_ids:
            for ticket in self:
                template.send_mail(ticket.id)

        # Update SLA status
        self.change_sh_status()

    return super().write(vals)
```

### Message Posted Hook

```python
class MailMessage(models.Model):
    _inherit = 'mail.message'

    @api.model
    def create(self, vals):
        if vals.get('model') == 'helpdesk.ticket':
            ticket = self.env['helpdesk.ticket'].browse(vals['res_id'])

            # Update state based on author
            if vals.get('author_id') == ticket.partner_id.id:
                ticket.state = 'customer_replied'
            elif vals.get('author_id') != 2:  # Not OdooBot
                ticket.state = 'staff_replied'

        return super().create(vals)
```

---

## Customization Points

### Adding Custom Fields

```python
# In custom module's models.py
class HelpdeskTicketCustom(models.Model):
    _inherit = 'helpdesk.ticket'

    custom_field = fields.Char('Custom Field')
    custom_related = fields.Many2one('custom.model')
```

### Custom Stage Actions

```python
class HelpdeskTicketCustom(models.Model):
    _inherit = 'helpdesk.ticket'

    def action_custom_stage(self):
        """Custom action for specific stage transition."""
        custom_stage = self.env.ref('your_module.custom_stage')
        self.write({'stage_id': custom_stage.id})
        # Custom logic here
```

### Custom Email Templates

```xml
<record id="custom_ticket_template" model="mail.template">
    <field name="name">Custom Ticket Notification</field>
    <field name="model_id" ref="sh_all_in_one_helpdesk.model_helpdesk_ticket"/>
    <field name="subject">[${object.name}] Custom Notification</field>
    <field name="body_html"><![CDATA[
        <p>Hello ${object.partner_id.name},</p>
        <p>Custom message here.</p>
        <p>Ticket: ${object.name}</p>
    ]]></field>
</record>
```

### Custom SLA Calculations

```python
class CustomSLAStatus(models.Model):
    _inherit = 'sh.helpdesk.sla.status'

    @api.depends('sh_sla_id', 'sh_ticket_id')
    def _compute_sh_deadline(self):
        # Custom deadline calculation
        for status in self:
            # Your custom logic
            pass
```

---

## Database Schema

### Core Tables

```sql
-- Main ticket table
CREATE TABLE helpdesk_ticket (
    id SERIAL PRIMARY KEY,
    name VARCHAR,
    partner_id INTEGER REFERENCES res_partner(id),
    ticket_subject VARCHAR,
    description TEXT,
    stage_id INTEGER REFERENCES helpdesk_stages(id),
    team_id INTEGER REFERENCES helpdesk_team(id),
    user_id INTEGER REFERENCES res_users(id),
    priority INTEGER REFERENCES helpdesk_priority(id),
    state VARCHAR,
    active BOOLEAN DEFAULT TRUE,
    create_date TIMESTAMP,
    write_date TIMESTAMP,
    close_date TIMESTAMP,
    cancel_date TIMESTAMP
);

-- Team table
CREATE TABLE helpdesk_team (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    team_head INTEGER REFERENCES res_users(id),
    sh_resource_calendar_id INTEGER REFERENCES resource_calendar(id)
);

-- Many2many: team members
CREATE TABLE helpdesk_team_res_users_rel (
    team_id INTEGER REFERENCES helpdesk_team(id),
    user_id INTEGER REFERENCES res_users(id),
    PRIMARY KEY (team_id, user_id)
);

-- Stages table
CREATE TABLE helpdesk_stages (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    sequence INTEGER,
    is_cancel_button_visible BOOLEAN,
    is_done_button_visible BOOLEAN
);

-- SLA Policy table
CREATE TABLE sh_helpdesk_sla (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    sh_team_id INTEGER REFERENCES helpdesk_team(id),
    sh_ticket_type_id INTEGER REFERENCES helpdesk_ticket_type(id),
    sh_sla_target_type VARCHAR,
    sh_stage_id INTEGER REFERENCES helpdesk_stages(id),
    sh_days INTEGER,
    sh_hours INTEGER,
    sh_minutes INTEGER
);

-- SLA Status (per ticket)
CREATE TABLE sh_helpdesk_sla_status (
    id SERIAL PRIMARY KEY,
    sh_ticket_id INTEGER REFERENCES helpdesk_ticket(id) ON DELETE CASCADE,
    sh_sla_id INTEGER REFERENCES sh_helpdesk_sla(id),
    sh_deadline TIMESTAMP,
    sh_status VARCHAR,
    sh_done_sla_date TIMESTAMP
);
```

### Indexes

```sql
-- Performance indexes
CREATE INDEX idx_ticket_stage ON helpdesk_ticket(stage_id);
CREATE INDEX idx_ticket_team ON helpdesk_ticket(team_id);
CREATE INDEX idx_ticket_user ON helpdesk_ticket(user_id);
CREATE INDEX idx_ticket_partner ON helpdesk_ticket(partner_id);
CREATE INDEX idx_ticket_create_date ON helpdesk_ticket(create_date);
CREATE INDEX idx_sla_status_ticket ON sh_helpdesk_sla_status(sh_ticket_id);
```

---

## Performance Considerations

### Query Optimization

```python
# Good: Use search with limit
tickets = self.env['helpdesk.ticket'].search([
    ('stage_id', '=', stage_id)
], limit=100)

# Good: Prefetch related records
tickets = self.env['helpdesk.ticket'].search([]).with_prefetch()
for ticket in tickets:
    ticket.partner_id  # Already loaded

# Bad: N+1 queries
for ticket in tickets:
    # This causes separate query per ticket
    print(ticket.partner_id.name)

# Good: Read all at once
data = tickets.read(['partner_id'])
```

### Batch Operations

```python
# Good: Batch create
records = []
for data in data_list:
    records.append(data)
self.env['helpdesk.ticket'].create(records)

# Good: Batch write
tickets.write({'stage_id': new_stage.id})
```

---

*Next: [Integrations](07_INTEGRATIONS.md)*
