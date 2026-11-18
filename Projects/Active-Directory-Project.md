# Active Directory Help Desk Lab

## Overview

The purpose of this project is to build a small Active Directory environment that simulates a basic enterprise domain. The goal is not to become an Active Directory administrator, but to create a realistic environment for learning and practicing common IT support and help desk tasks.

The environment is designed to provide hands-on experience with user management, authentication, group membership, permissions, file sharing, Group Policy, workstation administration, and basic troubleshooting. By building the environment from the ground up, each component can be understood individually before being combined into a functioning Windows domain.

The lab is intentionally small and simple, focusing on technologies commonly encountered in entry-level IT support roles rather than advanced enterprise topics.

---

# Objectives

- Learn the fundamentals of Active Directory.
- Understand how domain authentication works.
- Practice common help desk administration tasks.
- Gain experience with user, group, and computer management.
- Learn how permissions are applied and enforced.
- Become familiar with Group Policy.
- Build confidence troubleshooting domain-joined Windows systems.

---

# Environment

## Hypervisor

- Proxmox VE

## Network

- Sandbox VLAN (VLAN 50)

## Virtual Machines

- Windows Server 2022 Domain Controller (DC01)
- Windows 10 Workstation
- Windows 11 Workstation

## Domain

- `lab.hub`

---

# Project Phases

## Phase 1 - Infrastructure Deployment

Establish the Active Directory environment and confirm domain functionality.

### Goals

- Deploy Windows Server 2022
- Deploy Windows 10 workstation
- Deploy Windows 11 workstation
- Install VirtIO drivers
- Configure static addressing
- Install Active Directory Domain Services
- Promote the server to a Domain Controller
- Configure DNS functionality
- Join workstations to the domain
- Authenticate with domain user accounts

### Status

✅ Complete

---

## Phase 2 - Active Directory Fundamentals

Learn the core organizational structure of Active Directory.

### Goals

- Create Organizational Units (OUs)
- Create security groups
- Create domain users
- Understand the relationship between:
  - Users
  - Groups
  - OUs
  - Computers
- Manage group membership
- Perform account administration tasks such as password resets

### Status

🟡 In Progress

### Completed Examples

- Created Organizational Units
- Created domain users
- Created security groups
- Added users to groups
- Reset domain user passwords

---

## Phase 3 - Permissions and Resource Access

Learn how Active Directory controls access to resources.

### Goals

- Create SMB file shares
- Configure share permissions
- Configure NTFS permissions
- Grant access through security groups
- Verify permission inheritance
- Troubleshoot access-denied conditions

### Status

🟡 In Progress

### Completed Examples

- Created IT security group
- Added user membership
- Created shared folder
- Granted access via security group
- Verified access denial for unauthorized users

---

## Phase 4 - Group Policy

Learn how centralized configuration is applied to users and computers.

### Goals

- Create Group Policy Objects (GPOs)
- Link GPOs to Organizational Units
- Understand User vs Computer policies
- Apply and verify policies
- Use `gpupdate` and `gpresult`
- Troubleshoot policy application

### Status

🟡 In Progress

### Completed Examples

- Created and linked first GPO
- Tested Control Panel restriction policy
- Verified policy enforcement on a domain workstation
- Began learning GPO scope and processing behavior

---

## Phase 5 - Help Desk Operations

Practice realistic support tasks commonly performed in entry-level IT roles.

### Goals

- Reset passwords
- Unlock accounts
- Disable and enable users
- Modify group membership
- Troubleshoot failed logins
- Join computers to the domain
- Verify DNS functionality
- Troubleshoot Group Policy issues
- Troubleshoot permissions issues

### Status

🟡 In Progress

---

## Phase 6 - Scenario-Based Practice

Use the lab environment to simulate real help desk tickets.

### Example Scenarios

- User cannot access shared drive
- User forgot password
- User account locked out
- New employee onboarding
- Employee termination/offboarding
- Workstation not receiving Group Policy
- Workstation unable to join the domain
- Incorrect group membership
- DNS-related login failures

### Status

🔲 Planned

---

# Progress Summary

Successfully deployed a functional Active Directory environment consisting of a Windows Server 2022 Domain Controller and Windows 10 and Windows 11 workstations. Both clients were joined to the domain and tested using domain-based authentication.

Active Directory Domain Services and DNS were configured through the domain promotion process, allowing centralized authentication and management of domain resources.

The lab currently includes Organizational Units, users, security groups, shared folders, permissions management, and initial Group Policy testing. Access control has been validated by granting and denying access to shared resources based on security group membership.

Current focus is on understanding how Active Directory objects, Organizational Units, group membership, permissions, and Group Policy interact to support common enterprise administration and help desk workflows.

The next stage of the project will focus on common help desk tasks, troubleshooting exercises, and simulated ticket scenarios to build practical experience with day-to-day Active Directory administration.
