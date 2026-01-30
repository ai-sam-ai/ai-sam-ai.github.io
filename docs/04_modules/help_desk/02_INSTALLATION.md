# Installation Guide - SH All-In-One Helpdesk

Complete installation and deployment instructions for system administrators.

---

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Pre-Installation Checklist](#pre-installation-checklist)
3. [Installation Methods](#installation-methods)
4. [Post-Installation Steps](#post-installation-steps)
5. [Docker Deployment](#docker-deployment)
6. [Upgrade Instructions](#upgrade-instructions)
7. [Uninstallation](#uninstallation)

---

## System Requirements

### Odoo Version
```
Required: Odoo 18.0 (Community or Enterprise)
```

### Python Version
```
Required: Python 3.10+
```

### Python Dependencies
```python
# Required (usually included with Odoo)
beautifulsoup4>=4.11.0   # HTML parsing (already in most Odoo installs)

# Note: html2text is bundled locally in lib/ folder
# No external html2text package required
```

### Odoo Module Dependencies
```yaml
Required Modules (install first if not present):
  - mail            # Core messaging
  - portal          # Customer portal
  - product         # Product management
  - resource        # Working hours/calendars
  - sale_management # Sales integration
  - purchase        # Purchase integration
  - account         # Invoicing integration
  - hr_timesheet    # Time tracking
  - project         # Task integration
  - crm             # CRM integration
```

### Disk Space
```
Module size: ~5 MB
Database overhead: ~10-50 MB (depending on usage)
```

---

## Pre-Installation Checklist

### AI-Parseable Checklist

```yaml
pre_installation_checklist:
  system_requirements:
    - id: REQ-001
      task: "Verify Odoo 18.0 is installed"
      command: "odoo --version"
      expected: "18.0"
      status: pending

    - id: REQ-002
      task: "Verify Python 3.10+"
      command: "python3 --version"
      expected: "3.10.x or higher"
      status: pending

    - id: REQ-003
      task: "Verify beautifulsoup4 installed"
      command: "pip3 show beautifulsoup4"
      expected: "Version: 4.x.x"
      status: pending

  odoo_dependencies:
    - id: DEP-001
      task: "Install mail module"
      location: "Apps → mail"
      status: pending

    - id: DEP-002
      task: "Install portal module"
      location: "Apps → portal"
      status: pending

    - id: DEP-003
      task: "Install sale_management module"
      location: "Apps → sale_management"
      status: pending

    - id: DEP-004
      task: "Install purchase module"
      location: "Apps → purchase"
      status: pending

    - id: DEP-005
      task: "Install account module"
      location: "Apps → Invoicing"
      status: pending

    - id: DEP-006
      task: "Install hr_timesheet module"
      location: "Apps → Timesheets"
      status: pending

    - id: DEP-007
      task: "Install project module"
      location: "Apps → Project"
      status: pending

    - id: DEP-008
      task: "Install crm module"
      location: "Apps → CRM"
      status: pending

  access_requirements:
    - id: ACC-001
      task: "Confirm admin access to Odoo"
      status: pending

    - id: ACC-002
      task: "Confirm access to addons directory"
      status: pending

    - id: ACC-003
      task: "Confirm ability to restart Odoo service"
      status: pending
```

---

## Installation Methods

### Method 1: Standard Odoo Installation

#### Step 1: Copy Module to Addons Path

```bash
# Find your Odoo addons path
# Common locations:
# - /usr/lib/python3/dist-packages/odoo/addons
# - /opt/odoo/addons
# - ~/odoo/custom_addons

# Copy the module
cp -r sh_all_in_one_helpdesk /path/to/odoo/addons/
```

#### Step 2: Update Odoo Configuration

Ensure addons path includes module location:

```ini
# /etc/odoo/odoo.conf
[options]
addons_path = /usr/lib/python3/dist-packages/odoo/addons,/path/to/custom/addons
```

#### Step 3: Restart Odoo Service

```bash
# Linux systemd
sudo systemctl restart odoo

# Linux init.d
sudo /etc/init.d/odoo restart

# Manual
./odoo-bin -c /etc/odoo/odoo.conf
```

#### Step 4: Update Apps List

1. Log in to Odoo as administrator
2. Enable developer mode: `Settings` → `Activate Developer Mode`
3. Navigate to: `Apps`
4. Click: `Update Apps List`
5. Click: `Update` in dialog

#### Step 5: Install Module

1. Search for: `All In One Helpdesk`
2. Click module card
3. Click `Install`
4. Wait for completion

---

### Method 2: Command Line Installation

```bash
# Navigate to Odoo directory
cd /opt/odoo

# Install via CLI
./odoo-bin -d your_database -i sh_all_in_one_helpdesk --stop-after-init
```

---

### Method 3: Docker Installation

See [Docker Deployment](#docker-deployment) section below.

---

## Post-Installation Steps

### Verification Checklist

```yaml
post_installation_verification:
  - id: VER-001
    task: "Helpdesk menu appears in main menu"
    location: "Main menu bar"
    status: pending

  - id: VER-002
    task: "Configuration submenu accessible"
    location: "Helpdesk → Configuration"
    status: pending

  - id: VER-003
    task: "Default stages created"
    location: "Helpdesk → Configuration → Stages"
    expected: "New, In Progress, Done, Closed, Cancel"
    status: pending

  - id: VER-004
    task: "Ticket sequence created"
    location: "Settings → Technical → Sequences"
    expected: "helpdesk.ticket.sequence"
    status: pending

  - id: VER-005
    task: "Security groups created"
    location: "Settings → Technical → Groups"
    expected: "Support User, Team Leader, Support Manager"
    status: pending
```

### Initial Configuration

After installation, complete these steps:

1. **Configure Company Settings**
   ```
   Helpdesk → Configuration → Settings
   ```

2. **Create Support Team**
   ```
   Helpdesk → Configuration → Teams → Create
   ```

3. **Assign User Permissions**
   ```
   Settings → Users → Edit → Helpdesk section
   ```

4. **Test Ticket Creation**
   ```
   Helpdesk → Tickets → Create
   ```

See [Configuration Guide](03_CONFIGURATION.md) for detailed setup.

---

## Docker Deployment

### Docker Compose Configuration

Add the module to your Docker Compose setup:

```yaml
# docker-compose.yml
services:
  odoo:
    image: odoo:18.0
    volumes:
      # Mount your custom modules
      - ./custom_addons/sh_all_in_one_helpdesk:/mnt/extra-addons/sh_all_in_one_helpdesk:ro
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo
```

### Dockerfile with Dependencies

```dockerfile
FROM odoo:18.0

USER root

# Install Python dependencies
RUN pip3 install --no-cache-dir --break-system-packages \
    beautifulsoup4>=4.11.0

# Update addons path
RUN sed -i 's|addons_path = /mnt/extra-addons|addons_path = /mnt/extra-addons,/mnt/custom-addons|' /etc/odoo/odoo.conf

USER odoo
```

### Mount Points for SAM AI Docker

If using SAM AI Docker container (102-saas-host-docker):

```yaml
volumes:
  # Module is mounted from host repository
  - D:/github_repos/30_samai_saas_host_management:/mnt/dev/30-host-only:ro
```

The module is then available at:
```
/mnt/dev/30-host-only/sh_all_in_one_helpdesk
```

---

## Upgrade Instructions

### From Previous Versions

1. **Backup Database**
   ```bash
   pg_dump -U odoo your_database > backup_before_upgrade.sql
   ```

2. **Replace Module Files**
   ```bash
   rm -rf /path/to/addons/sh_all_in_one_helpdesk
   cp -r new_sh_all_in_one_helpdesk /path/to/addons/sh_all_in_one_helpdesk
   ```

3. **Upgrade Module**
   ```bash
   ./odoo-bin -d your_database -u sh_all_in_one_helpdesk --stop-after-init
   ```

   Or via UI:
   ```
   Apps → search "All In One Helpdesk" → Upgrade
   ```

4. **Restart Odoo**
   ```bash
   sudo systemctl restart odoo
   ```

### Version-Specific Notes

#### Upgrading from Odoo 13/14/15/16/17 to 18

This module was migrated from earlier Odoo versions. Key changes:

- `external_dependencies` removed (html2text bundled locally)
- OWL 2.0 compatible JavaScript
- Python 3.10+ required
- Updated security rules syntax

---

## Uninstallation

### Standard Uninstall

1. Navigate to: `Apps`
2. Search: `All In One Helpdesk`
3. Click module
4. Click `Uninstall`

**Warning:** This will delete all helpdesk data including:
- All tickets
- All teams
- All stages
- All SLA policies
- All related records

### Data Preservation

To preserve data before uninstalling:

```sql
-- Export tickets
COPY (SELECT * FROM helpdesk_ticket) TO '/tmp/helpdesk_tickets.csv' CSV HEADER;

-- Export teams
COPY (SELECT * FROM helpdesk_team) TO '/tmp/helpdesk_teams.csv' CSV HEADER;
```

---

## Troubleshooting Installation

### Common Issues

#### Issue: Module Not Found in Apps List

**Solution:**
```bash
# Verify module in addons path
ls -la /path/to/addons/sh_all_in_one_helpdesk/

# Update apps list
./odoo-bin -d database -u base --stop-after-init
```

#### Issue: Dependency Error

**Error:** `ModuleNotFoundError: No module named 'html2text'`

**Solution:** This is fixed in current version. Ensure you have latest module with bundled `lib/html2text.py`.

#### Issue: Access Rights Error

**Error:** `Access Denied` after installation

**Solution:**
1. Log in as admin
2. Settings → Users → Edit user
3. Add to "Support Manager" group

#### Issue: Missing Menu

**Solution:**
1. Clear browser cache
2. Refresh page (Ctrl+F5)
3. Log out and log back in

---

## Installation Checklist - Complete

```yaml
complete_installation_checklist:
  phase_1_preparation:
    - "[ ] Verified Odoo 18.0 running"
    - "[ ] Verified Python 3.10+"
    - "[ ] Installed beautifulsoup4"
    - "[ ] Have admin credentials"
    - "[ ] Backup created"

  phase_2_dependencies:
    - "[ ] mail module installed"
    - "[ ] portal module installed"
    - "[ ] sale_management installed"
    - "[ ] purchase installed"
    - "[ ] account installed"
    - "[ ] hr_timesheet installed"
    - "[ ] project installed"
    - "[ ] crm installed"

  phase_3_installation:
    - "[ ] Module copied to addons path"
    - "[ ] Odoo config updated"
    - "[ ] Odoo service restarted"
    - "[ ] Apps list updated"
    - "[ ] Module installed via Apps"

  phase_4_verification:
    - "[ ] Helpdesk menu visible"
    - "[ ] Configuration accessible"
    - "[ ] Default stages exist"
    - "[ ] Can create test ticket"
    - "[ ] Users can be assigned roles"

  phase_5_initial_setup:
    - "[ ] Company settings configured"
    - "[ ] First team created"
    - "[ ] User permissions assigned"
    - "[ ] Test ticket created and resolved"
```

---

*Next: [Configuration Guide](03_CONFIGURATION.md)*
