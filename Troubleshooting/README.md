# Troubleshooting

This folder contains incident notes for problems that affected the homelab and were
investigated or resolved. Each document describes one specific symptom or failure.

## Recommended Format

Troubleshooting documents should capture:

1. **Summary:** What failed and where?
2. **Environment:** Which nodes, networks, or services were involved?
3. **Symptoms:** What was observed?
4. **Investigation:** What checks were performed, and what did they show?
5. **Root cause:** What actually caused the problem?
6. **Resolution:** What change restored service?
7. **Validation:** How was the result confirmed?
8. **Prevention:** What should be checked next time?

Do not use this folder as a replacement for runbooks. A troubleshooting document records
what happened; a runbook explains the repeatable procedure for deploying or operating a
system.

## Incidents

- [Sandbox VMs Received APIPA Addresses](sandbox-apipa.md)
- [Proxmox Failed After SSD Removal](proxmox-ssd-removal.md)
