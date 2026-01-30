# Troubleshooting Guide - SH All-In-One Helpdesk

Solutions to common issues and problems.

---

## Table of Contents

1. [Installation Issues](#installation-issues)
2. [Configuration Issues](#configuration-issues)
3. [Ticket Issues](#ticket-issues)
4. [Email Issues](#email-issues)
5. [SLA Issues](#sla-issues)
6. [Portal Issues](#portal-issues)
7. [Performance Issues](#performance-issues)
8. [Integration Issues](#integration-issues)
9. [Error Messages](#error-messages)
10. [Getting Help](#getting-help)

---

## Installation Issues

### Module Not Found in Apps List

**Symptoms:**
- Module doesn't appear when searching in Apps
- "Module not found" error

**Solutions:**

1. **Verify module location:**
   ```bash
   ls -la /path/to/addons/sh_all_in_one_helpdesk/
   # Should see __manifest__.py
   ```

2. **Check addons path in odoo.conf:**
   ```ini
   addons_path = /usr/lib/python3/dist-packages/odoo/addons,/your/custom/addons
   ```

3. **Update apps list:**
   - Enable developer mode
   - Apps → Update Apps List

4. **Restart Odoo:**
   ```bash
   sudo systemctl restart odoo
   ```

### Dependency Error: html2text

**Symptoms:**
```
ModuleNotFoundError: No module named 'html2text'
```

**Solution:**
This error should not occur in the current version. The module includes a bundled `lib/html2text.py`. If you see this:

1. Verify the lib folder exists:
   ```bash
   ls sh_all_in_one_helpdesk/lib/
   # Should show: __init__.py, html2text.py
   ```

2. Check import in mail_compose.py:
   ```python
   # Should be:
   from ..lib.html2text import html2text
   # NOT:
   import html2text
   ```

### Missing Module Dependencies

**Symptoms:**
```
Module X is not installed
```

**Solution:**
Install required dependencies first:
```
Apps → Search for: mail, portal, sale_management, purchase, account, hr_timesheet, project, crm
→ Install each one
```

### Database Constraint Errors

**Symptoms:**
```
IntegrityError: duplicate key value violates unique constraint
```

**Solution:**
1. Check for conflicting data
2. Try upgrading instead of reinstalling:
   ```bash
   ./odoo-bin -d database -u sh_all_in_one_helpdesk --stop-after-init
   ```

---

## Configuration Issues

### Default Team/User Not Working

**Symptoms:**
- New tickets not auto-assigned
- Default team blank on tickets

**Solutions:**

1. **Verify settings saved:**
   - Helpdesk → Configuration → Settings
   - Check Default Team and Default User are set
   - Click Save

2. **Check company context:**
   - Ensure settings are for correct company
   - Multi-company: each company needs own settings

### Stages Not Showing

**Symptoms:**
- Stage dropdown empty
- Kanban columns missing

**Solutions:**

1. **Check stages exist:**
   - Helpdesk → Configuration → Stages
   - Should have at least: New, In Progress, Done

2. **Check sequence numbers:**
   - Stages with sequence 0 may not display correctly
   - Ensure unique, positive sequence numbers

3. **Create default stages:**
   - If missing, create manually or reinstall module

### Categories Not Visible

**Symptoms:**
- Category field not showing on ticket form

**Solution:**
Enable in settings:
- Helpdesk → Configuration → Settings
- Check "Enable Category"
- Save

---

## Ticket Issues

### Cannot Create Tickets

**Symptoms:**
- Create button doesn't work
- Error on save

**Solutions:**

1. **Check required fields:**
   - Partner (Customer) is required
   - Subject is required

2. **Verify permissions:**
   - User must have Support User role or higher
   - Settings → Users → [user] → Helpdesk section

3. **Check team exists:**
   - At least one team must be configured

### Tickets Not Visible

**Symptoms:**
- User cannot see tickets
- "No records found"

**Solutions:**

1. **Check user role:**
   - Support User: sees own tickets only
   - Team Leader: sees team tickets
   - Manager: sees all tickets

2. **Check team membership:**
   - User must be team member or team head
   - Helpdesk → Configuration → Teams

3. **Check filters:**
   - Clear all filters
   - Remove search terms

### Stage Transition Blocked

**Symptoms:**
- Cannot move ticket to certain stage
- "Access Denied" on stage change

**Solution:**
Check stage group restrictions:
1. Helpdesk → Configuration → Stages
2. Edit the target stage
3. Check "Group Access" field
4. Add user's group or remove restriction

### Ticket Number Not Generated

**Symptoms:**
- Tickets have blank names
- Names show "New" instead of sequence

**Solution:**
Check sequence configuration:
1. Settings → Technical → Sequences
2. Find "helpdesk.ticket.sequence"
3. Verify it exists and has correct prefix

If missing, create:
```xml
<record id="helpdesk_ticket_sequence" model="ir.sequence">
    <field name="name">Helpdesk Ticket</field>
    <field name="code">helpdesk.ticket.sequence</field>
    <field name="prefix">HD</field>
    <field name="padding">5</field>
</record>
```

---

## Email Issues

### Emails Not Sending

**Symptoms:**
- No email notifications
- Stage change emails not received

**Solutions:**

1. **Check outgoing mail server:**
   - Settings → Technical → Outgoing Mail Servers
   - Test connection

2. **Check email templates linked:**
   - Helpdesk → Configuration → Stages
   - Edit stage → verify Email Templates field

3. **Check recipient email:**
   - Customer must have valid email
   - User must have valid email

4. **Check mail queue:**
   - Settings → Technical → Emails → Emails
   - Look for failed emails

### Customer Not Receiving Updates

**Symptoms:**
- Customer doesn't get notifications
- Only agent gets emails

**Solutions:**

1. **Check follower status:**
   - Open ticket
   - Verify customer is a follower
   - Or enable "Auto Add Customer as Follower" in settings

2. **Check message type:**
   - Use "Send Message" not "Log Note"
   - Notes are internal only

3. **Verify customer email:**
   - Open customer contact
   - Ensure email is set

### Email Template Variables Not Working

**Symptoms:**
- Email shows `${object.name}` literally
- Variables not replaced

**Solution:**
Check template syntax:
```html
<!-- Correct Qweb syntax -->
<t t-out="object.name"/>

<!-- Or Jinja-style (older) -->
${object.name}
```

---

## SLA Issues

### SLA Not Calculating

**Symptoms:**
- No SLA deadline on tickets
- SLA status blank

**Solutions:**

1. **Verify SLA policy exists:**
   - Helpdesk → Configuration → SLA Policies
   - Create policy for team

2. **Check policy matches ticket:**
   - Policy team = ticket team
   - Policy ticket type = ticket type (or blank for all)

3. **Verify working hours:**
   - Team must have working hours calendar
   - Helpdesk → Configuration → Teams → edit team

### SLA Deadline Wrong

**Symptoms:**
- Deadline seems off
- Not accounting for weekends

**Solutions:**

1. **Check working calendar:**
   - Verify team's working hours calendar
   - Check calendar has correct days/hours

2. **Check timezone:**
   - User timezone setting
   - Server timezone
   - Calendar timezone

3. **Review SLA time settings:**
   - Days + Hours + Minutes all contribute
   - 1 day = 1 working day, not 24 hours

### SLA Status Not Updating

**Symptoms:**
- SLA shows wrong status
- Status doesn't change when stage changes

**Solution:**
Manually trigger update:
```python
# In Odoo shell or via action
ticket.change_sh_status()
```

Or check the `write` method is triggering status update.

---

## Portal Issues

### Customer Can't Access Portal

**Symptoms:**
- Login fails for customer
- No portal access

**Solutions:**

1. **Grant portal access:**
   - Contacts → select customer
   - Action → Grant Portal Access
   - Enter email

2. **Check portal user group:**
   - User should have "Portal" group
   - Not internal user group

3. **Verify email sent:**
   - Check email queue
   - Customer needs activation email

### Customer Sees Wrong Tickets

**Symptoms:**
- Customer sees other customers' tickets
- Tickets missing from portal

**Solutions:**

1. **Check portal user level:**
   - User's Helpdesk portal access level
   - Settings → Users → [user] → Helpdesk section

2. **Verify partner matching:**
   - Ticket partner_id must match portal user's partner_id

### Portal Attachments Fail

**Symptoms:**
- Cannot upload files
- "File too large" error

**Solution:**
Increase file size limit:
- Helpdesk → Configuration → Settings
- Increase "Portal File Size Limit"

---

## Performance Issues

### Slow Ticket List Loading

**Symptoms:**
- Ticket list takes long to load
- Timeout errors

**Solutions:**

1. **Reduce visible fields:**
   - Remove computed fields from list view
   - Use summary views

2. **Add database indexes:**
   ```sql
   CREATE INDEX idx_ticket_stage ON helpdesk_ticket(stage_id);
   CREATE INDEX idx_ticket_user ON helpdesk_ticket(user_id);
   ```

3. **Archive old tickets:**
   - Set tickets to inactive after closure
   - Use auto-close feature

### Dashboard Slow

**Symptoms:**
- Dashboard takes long to render
- Counters don't update

**Solutions:**

1. **Reduce dashboard stages:**
   - Only show essential stages in dashboard
   - Company settings → Dashboard Filter/Table stages

2. **Limit date range:**
   - Use shorter default period
   - Monthly instead of yearly

### Search Performance

**Symptoms:**
- Search takes too long
- Timeouts on search

**Solutions:**

1. **Use specific filters:**
   - Avoid wildcard searches
   - Filter by date range first

2. **Index frequently searched fields:**
   ```sql
   CREATE INDEX idx_ticket_subject ON helpdesk_ticket USING gin(to_tsvector('english', ticket_subject));
   ```

---

## Integration Issues

### Sales Order Integration Not Working

**Symptoms:**
- No "Create Ticket" button on SO
- Ticket not linking to SO

**Solutions:**

1. **Verify sub-module loaded:**
   - Check sh_helpdesk_so folder exists
   - Module should auto-load with main module

2. **Check sale_management installed:**
   - Apps → Sales
   - Must be installed

### Timesheet Not Recording

**Symptoms:**
- Timer starts but no entry created
- Hours not appearing

**Solutions:**

1. **Check project configuration:**
   - Company settings → Default Project
   - Must have valid project

2. **Verify employee link:**
   - User must have employee record
   - HR → Employees

3. **Check timesheet module:**
   - hr_timesheet must be installed

### CRM Link Missing

**Symptoms:**
- No "Create Lead" button
- Lead count not showing

**Solution:**
Verify CRM installed:
- Apps → CRM
- Install if missing

---

## Error Messages

### "Access Denied"

**Cause:** User lacks permission

**Solution:**
- Check user's helpdesk group
- Verify record rules for user's role
- Ensure user is in correct team

### "Constraint Error"

**Cause:** Data validation failed

**Solution:**
- Check required fields
- Verify unique constraints
- Check related records exist

### "KeyError: 'field_name'"

**Cause:** Code references non-existent field

**Solution:**
- Upgrade module to get latest fields
- Check for customization conflicts

### "RecursionError"

**Cause:** Infinite loop in computed fields

**Solution:**
- Check custom computed fields
- Review depends decorators
- Look for circular references

### "TransactionRollbackError"

**Cause:** Database conflict

**Solution:**
- Retry the operation
- Check for concurrent edits
- Review database locks

---

## Diagnostic Commands

### Check Module Status

```bash
# Odoo shell
env['ir.module.module'].search([('name', '=', 'sh_all_in_one_helpdesk')]).state
```

### List Installed Modules

```bash
./odoo-bin shell -d database
>>> env['ir.module.module'].search([('state', '=', 'installed')]).mapped('name')
```

### Check Ticket Count

```python
>>> env['helpdesk.ticket'].search_count([])
```

### View Error Logs

```bash
# Linux
tail -f /var/log/odoo/odoo.log | grep -i error

# Docker
docker logs -f container_name 2>&1 | grep -i error
```

### Test Email

```python
>>> env['mail.mail'].create({
...     'subject': 'Test',
...     'body_html': 'Test email',
...     'email_to': 'test@example.com'
... }).send()
```

---

## Getting Help

### Before Asking for Help

Gather this information:

```yaml
diagnostic_info:
  odoo_version: "18.0"
  module_version: "18.0.0.0"
  python_version: "3.x.x"
  database_type: "PostgreSQL x.x"
  deployment: "Docker / Native / Hosted"
  error_message: "Full error text"
  steps_to_reproduce: "1. Go to... 2. Click..."
  expected_behavior: "What should happen"
  actual_behavior: "What actually happens"
```

### Support Channels

1. **Documentation:** Review this guide
2. **Odoo Forums:** community.odoo.com
3. **GitHub Issues:** Report bugs
4. **Original Author:** Softhealer Technologies

### Reporting Bugs

Include:
- Odoo version
- Module version
- Full error traceback
- Steps to reproduce
- Screenshots if applicable

---

## Troubleshooting Checklist

```yaml
general_troubleshooting:
  - id: TS-001
    check: "Odoo service running"
    command: "systemctl status odoo"
    status: pending

  - id: TS-002
    check: "Database accessible"
    command: "psql -U odoo -d database -c 'SELECT 1'"
    status: pending

  - id: TS-003
    check: "Module installed"
    location: "Apps → search 'helpdesk'"
    status: pending

  - id: TS-004
    check: "User has permissions"
    location: "Settings → Users → Helpdesk section"
    status: pending

  - id: TS-005
    check: "Email server configured"
    location: "Settings → Technical → Outgoing Mail"
    status: pending

  - id: TS-006
    check: "At least one team exists"
    location: "Helpdesk → Configuration → Teams"
    status: pending

  - id: TS-007
    check: "Stages configured"
    location: "Helpdesk → Configuration → Stages"
    status: pending

  - id: TS-008
    check: "Company settings complete"
    location: "Helpdesk → Configuration → Settings"
    status: pending
```

---

*Next: [CHECKLISTS.md](CHECKLISTS.md)*
