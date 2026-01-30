# Quick Start Guide - SH All-In-One Helpdesk

Get your helpdesk system running in 15 minutes.

---

## Prerequisites Checklist

```
[ ] Odoo 18.0 installed and running
[ ] Admin access to Odoo instance
[ ] Module sh_all_in_one_helpdesk available in addons path
[ ] Required dependencies installed (mail, portal, sale_management, etc.)
[ ] Email server configured (optional but recommended)
```

---

## Step 1: Install the Module (2 minutes)

1. **Access Apps Menu**
   - Log in as administrator
   - Navigate to: `Apps` menu

2. **Find the Module**
   - Click "Update Apps List" (if first time)
   - Search for: `All In One Helpdesk`

3. **Install**
   - Click the module card
   - Click `Install` button
   - Wait for installation to complete

**Verification:**
```
[ ] Module appears as "Installed" in Apps
[ ] New "Helpdesk" menu appears in main menu bar
```

---

## Step 2: Basic Configuration (5 minutes)

### 2.1 Access Helpdesk Settings

1. Navigate to: `Helpdesk` → `Configuration` → `Settings`

### 2.2 Configure Company Defaults

| Setting | Recommended Value | Purpose |
|---------|-------------------|---------|
| Enable Category | ✓ Checked | Organize tickets by category |
| Enable Sub-Category | ✓ Checked | More granular classification |
| Enable Customer Rating | ✓ Checked | Collect feedback |
| Auto-Close Tickets | Optional | Auto-close after N days inactive |
| Display Multi-User | ✓ Checked | Assign multiple agents |
| Auto Add Customer as Follower | ✓ Checked | Customer receives updates |

### 2.3 Set Default Stages

| Stage Type | Select From Dropdown |
|------------|---------------------|
| New/Draft Stage | "New" or "Draft" |
| Resolved Stage | "Done" or "Resolved" |
| Closed Stage | "Closed" |
| Cancel Stage | "Cancelled" |
| Re-open Stage | "Re-opened" |

**Save Settings**

**Verification:**
```
[ ] Settings saved without errors
[ ] Default stages are set
```

---

## Step 3: Create Your First Team (3 minutes)

1. Navigate to: `Helpdesk` → `Configuration` → `Teams`

2. Click `Create`

3. Fill in:
   | Field | Value |
   |-------|-------|
   | Team Name | "Support Team" (example) |
   | Team Head | Select a user (leader) |
   | Team Members | Select support agents |
   | Working Hours | Select company calendar |

4. Click `Save`

**Verification:**
```
[ ] Team created successfully
[ ] Team head assigned
[ ] At least one team member added
```

---

## Step 4: Create Ticket Stages (2 minutes)

Default stages are created automatically. Verify or customize:

1. Navigate to: `Helpdesk` → `Configuration` → `Stages`

2. Verify these stages exist:
   ```
   [ ] New (sequence: 1)
   [ ] In Progress (sequence: 2)
   [ ] Done (sequence: 3)
   [ ] Closed (sequence: 4)
   [ ] Cancelled (sequence: 5)
   ```

3. Optional: Add email templates to stages for automatic notifications

---

## Step 5: Assign User Permissions (2 minutes)

1. Navigate to: `Settings` → `Users & Companies` → `Users`

2. For each support user:
   - Edit user
   - Under "Helpdesk" section, select:
     - `Support User` - Basic agent
     - `Team Leader` - Team supervisor
     - `Support Manager` - Full admin

3. Save each user

**Verification:**
```
[ ] At least one Support Manager exists
[ ] Support agents have Support User role
[ ] Team leaders have Team Leader role
```

---

## Step 6: Create Your First Ticket (1 minute)

1. Navigate to: `Helpdesk` → `Tickets`

2. Click `Create`

3. Fill in minimum required fields:
   | Field | Value |
   |-------|-------|
   | Customer | Select or create contact |
   | Subject | "Test Ticket - Please Ignore" |
   | Description | "This is a test ticket" |
   | Team | Select your team |
   | Assigned To | Select an agent |

4. Click `Save`

**Verification:**
```
[ ] Ticket created with auto-generated number (e.g., HD00001)
[ ] Ticket appears in kanban view under "New" stage
[ ] Assigned user can see the ticket
```

---

## Quick Start Complete!

You now have a working helpdesk system. Next steps:

### Recommended Next Actions

1. **Configure Email Integration**
   - Set up incoming email to auto-create tickets
   - See: [Configuration Guide](03_CONFIGURATION.md)

2. **Set Up SLA Policies**
   - Define response/resolution time targets
   - See: [Admin Guide](05_ADMIN_GUIDE.md)

3. **Customize Categories**
   - Add ticket categories for your business
   - Navigate to: `Helpdesk` → `Configuration` → `Categories`

4. **Enable Customer Portal**
   - Allow customers to submit/track tickets online
   - See: [Configuration Guide](03_CONFIGURATION.md)

5. **Train Your Team**
   - Share [User Guide](04_USER_GUIDE.md) with support agents

---

## Quick Reference - Daily Operations

### Create Ticket
```
Helpdesk → Tickets → Create
```

### View All Tickets
```
Helpdesk → Tickets → (switch to List/Kanban view)
```

### My Assigned Tickets
```
Helpdesk → Tickets → Filter: "My Tickets"
```

### Resolve Ticket
```
Open ticket → Click "Resolve" button
```

### Close Ticket
```
Open ticket → Click "Close" button
```

---

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| New Ticket | Alt + C (in ticket list) |
| Save | Ctrl + S |
| Edit | Ctrl + E |
| Search | Ctrl + K |

---

## Troubleshooting Quick Start Issues

### Module Won't Install
- Check Python dependencies: `beautifulsoup4` must be installed
- Check Odoo logs for specific error messages
- Ensure all dependent modules are available

### Menu Doesn't Appear
- Clear browser cache
- Log out and log back in
- Verify user has Helpdesk permissions

### Can't Create Tickets
- Ensure at least one team exists
- Verify user has Support User role or higher

---

*Next: [Installation Guide](02_INSTALLATION.md) for detailed setup*
