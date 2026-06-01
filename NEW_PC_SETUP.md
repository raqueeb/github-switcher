# GitHub Account Switcher - New Windows PC Setup

## Overview

This document contains everything you need to set up GitHub account switching on a new Windows PC. Follow each step carefully.

---

## Step 1: Copy Key Files

On your current PC, navigate to your `.ssh` folder and copy these 4 files to your Google Drive or USB:

```
C:\Users\YourName\.ssh\
```

Required files:
- `id_ed25519_aiwithr` (private key)
- `id_ed25519_aiwithr.pub` (public key)
- `id_ed25519_raqueeb` (private key)
- `id_ed25519_raqueeb.pub` (public key)

---

## Step 2: On New PC - Create SSH Folder

Open PowerShell as Administrator:

```powershell
# Create .ssh folder
mkdir $env:USERPROFILE\.ssh
```

---

## Step 3: Create Key Files

Create each file with the exact content below:

### File 1: id_ed25519_aiwithr (Private Key)

Create this file: `C:\Users\YourName\.ssh\id_ed25519_aiwithr`

```
-----BEGIN OPENSSH PRIVATE KEY-----
COPY_THE_ACTUAL_PRIVATE_KEY_CONTENT_HERE_FROM_YOUR_CURRENT_PC
-----END OPENSSH PRIVATE KEY-----
```

**To get your actual key content, run this on your current PC:**

```powershell
notepad $env:USERPROFILE\.ssh\id_ed25519_aiwithr
```

Copy the entire content and paste it into the new file.

---

### File 2: id_ed25519_aiwithr.pub (Public Key)

Create this file: `C:\Users\YourName\.ssh\id_ed25519_aiwithr.pub`

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAREPLACE_WITH_YOUR_ACTUAL_PUBLIC_KEY user1@gmail.com
```

**To get your actual public key, run this on your current PC:**

```powershell
cat $env:USERPROFILE\.ssh\id_ed25519_aiwithr.pub
```

Copy the output and replace the line above.

---

### File 3: id_ed25519_raqueeb (Private Key)

Create this file: `C:\Users\YourName\.ssh\id_ed25519_raqueeb`

```
-----BEGIN OPENSSH PRIVATE KEY-----
COPY_THE_ACTUAL_PRIVATE_KEY_CONTENT_HERE_FROM_YOUR_CURRENT_PC
-----END OPENSSH PRIVATE KEY-----
```

**To get your actual key content, run this on your current PC:**

```powershell
notepad $env:USERPROFILE\.ssh\id_ed25519_raqueeb
```

Copy the entire content and paste it into the new file.

---

### File 4: id_ed25519_raqueeb.pub (Public Key)

Create this file: `C:\Users\YourName\.ssh\id_ed25519_raqueeb.pub`

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAREPLACE_WITH_YOUR_ACTUAL_PUBLIC_KEY user2@gmail.com
```

**To get your actual public key, run this on your current PC:**

```powershell
cat $env:USERPROFILE\.ssh\id_ed25519_raqueeb.pub
```

Copy the output and replace the line above.

---

## Step 4: Set File Permissions

After creating all files, run this in PowerShell:

```powershell
# Remove inherited permissions
icacls $env:USERPROFILE\.ssh\id_ed25519_aiwithr /inheritance:r /grant:r "$env:USERNAME:R"
icacls $env:USERPROFILE\.ssh\id_ed25519_raqueeb /inheritance:r /grant:r "$env:USERNAME:R"

# Verify permissions (should show: YourName:(R))
icacls $env:USERPROFILE\.ssh\id_ed25519_aiwithr
icacls $env:USERPROFILE\.ssh\id_ed25519_raqueeb
```

---

## Step 5: Clone the Switcher Script

```powershell
# Clone the repository
git clone git@github.com:raqueeb/github-switcher.git
cd github-switcher
```

---

## Step 6: Initialize the Switcher

```powershell
# Start with aiwithr account
.\switch.ps1 a

# Or start with raqueeb account
.\switch.ps1 r
```

---

## Step 7: Verify Setup

Test authentication:

```powershell
# Check current account
.\switch.ps1

# Test SSH connection
ssh -T git@github.com
```

Expected output:
- For aiwithr: `Hi aiwithr! You've successfully authenticated...`
- For raqueeb: `Hi raqueeb! You've successfully authenticated...`

---

## Quick Commands Reference

```powershell
.\switch.ps1           # Show current account
.\switch.ps1 a         # Switch to aiwithr
.\switch.ps1 r         # Switch to raqueeb
ssh -T git@github.com  # Check which account is active
```

---

## Troubleshooting

### "Permissions are too open"
Run the `icacls` commands from Step 4 again.

### "Permission denied (publickey)"
1. Verify keys exist: `ls $env:USERPROFILE\.ssh\`
2. Verify GitHub has your public key added
3. Run: `ssh -T git@github.com`

### "No such identity"
1. Start ssh-agent: `eval $(ssh-agent -s)`
2. Add keys: 
   ```powershell
   ssh-add $env:USERPROFILE\.ssh\id_ed25519_aiwithr
   ssh-add $env:USERPROFILE\.ssh\id_ed25519_raqueeb
   ```

---

## File Locations After Setup

```
C:\Users\YourName\.ssh\
├── id_ed25519_aiwithr         # aiwithr private key
├── id_ed25519_aiwithr.pub     # aiwithr public key
├── id_ed25519_raqueeb         # raqueeb private key
├── id_ed25519_raqueeb.pub     # raqueeb public key
├── config                     # Created by switch.ps1
└── known_hosts                # Existing or new
```

---

*Last updated: May 31, 2026*
