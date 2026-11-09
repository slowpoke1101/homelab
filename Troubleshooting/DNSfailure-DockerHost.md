# Troubleshooting: DNS Failure on Ubuntu Docker Host After AdGuard Shutdown

## Overview

While performing maintenance on my Ubuntu Docker host VM, I discovered that stopping my AdGuard Home container appeared to break DNS resolution for the host itself.

This behavior was unexpected because the host's `/etc/resolv.conf` appeared to point to my OPNsense router rather than the AdGuard container.

The issue ultimately traced back to a legacy configuration artifact from an earlier stage of my homelab journey, combined with a subtle syntax error in `resolv.conf`.

---

## Environment

### Host

- Ubuntu Server Docker VM
- Hosted on Proxmox
- Runs self-hosted services in Docker
- DNS infrastructure provided by OPNsense

### Background

Several months earlier, this Docker host was migrated from a Raspberry Pi to a virtual machine.

At the time:

- I did not yet have OPNsense
- I had limited networking knowledge
- AdGuard Home served as the primary DNS service
- The host was likely configured to depend on its own AdGuard container for DNS resolution

As the homelab evolved and OPNsense was introduced, the original DNS design was largely forgotten.

---

# Problem

Whenever the AdGuard container was stopped:

```bash
ping google.com
```

failed with:

```text
Temporary failure in name resolution
```

However:

```bash
ping 1.1.1.1
```

worked successfully.

This indicated:

- Layer 3 connectivity was functioning
- Routing was functioning
- DNS resolution was failing

---

# Initial Hypothesis

My first assumption was that the Ubuntu host still depended on the AdGuard container for DNS.

Possible causes included:

- Host DNS pointed at localhost
- AdGuard bound directly to port 53
- systemd-resolved misconfiguration
- Netplan DNS override
- Docker networking issue

---

# Investigation

## Step 1: Check Netplan

Examined network configuration:

```yaml
network:
  version: 2
  ethernets:
    enp6s18:
      dhcp4: true
```

Result:

- DHCP enabled
- No static DNS override present

Netplan appeared healthy.

---

## Step 2: Examine DNS Resolver Configuration

Checked:

```bash
cat /etc/resolv.conf
```

Output:

```text
nameservers 10.4.4.1
```

At the time this appeared normal and was not immediately recognized as problematic.

---

## Step 3: Test DNS Server Directly

Tested:

```bash
dig @10.4.4.1 google.com
```

Result:

Successful DNS response.

This proved:

- OPNsense DNS was functioning
- Network connectivity was functioning
- DNS infrastructure itself was healthy

---

## Step 4: Test Default Resolver

Tested:

```bash
dig google.com
```

Output:

```text
communications error to ::1#53: connection refused
communications error to 127.0.0.1#53: connection refused
```

Additional testing:

```bash
host google.com
```

produced similar results.

This suggested the host was attempting to use a local DNS resolver that was no longer available after AdGuard was stopped.

---

## Step 5: Investigate systemd-resolved

Checked the resolver service and discovered:

```bash
systemctl status systemd-resolved
```

showed the service was disabled.

Started the service and verified it was listening on:

```text
127.0.0.53:53
127.0.0.54:53
```

Verification:

```bash
resolvectl status
```

showed:

```text
DNS Servers: 10.4.4.1
```

which matched the DHCP-provided DNS server from OPNsense.

Despite this, DNS resolution was still failing.

---

# Root Cause Discovery

After re-examining the resolver configuration, I noticed:

```text
nameservers 10.4.4.1
```

The entry contained:

```text
nameservers
```

instead of the valid keyword:

```text
nameserver
```

The correct syntax is:

```text
nameserver 10.4.4.1
```

A single extra letter prevented applications from correctly interpreting the resolver configuration.

---

# Resolution

Corrected `/etc/resolv.conf`:

```text
nameserver 10.4.4.1
```

Immediately after the correction:

```bash
ping google.com
```

succeeded.

```bash
host google.com
```

succeeded.

```bash
dig google.com
```

succeeded.

DNS functionality was fully restored.

---

# Lessons Learned

## 1. Follow Evidence, Not Assumptions

The initial assumption was that Docker, AdGuard, or systemd-resolved were responsible.

Testing revealed:

- Network connectivity existed
- DNS server functionality existed
- Resolver configuration was the actual issue

---

## 2. Verify Configuration Syntax

A one-character typo:

```text
nameservers
```

vs.

```text
nameserver
```

caused a complete DNS failure.

Simple syntax errors can produce symptoms that appear significantly more complex than their actual root cause.

---

## 3. Legacy Configurations Create Long-Term Technical Debt

This issue originated from an earlier stage of the homelab when:

- Networking knowledge was limited
- AdGuard existed before OPNsense
- DNS architecture evolved over time

The original design decisions remained hidden until maintenance exposed them.

---

## 4. Direct Testing Helps Isolate Layers

The troubleshooting sequence:

```bash
ping 1.1.1.1
```

↓

```bash
dig @10.4.4.1 google.com
```

↓

```bash
dig google.com
```

quickly isolated the problem to local name resolution rather than general networking.

---

# Final State

Current DNS path:

```text
Ubuntu Docker Host
        ↓
OPNsense (10.4.4.1)
        ↓
Upstream DNS
```

The Docker host no longer depends on the AdGuard container for DNS resolution.

---

# Key Takeaway

What initially appeared to be a container dependency, routing issue, or resolver service problem ultimately traced back to a single-character typo in a legacy configuration file.

The most important lesson was not the typo itself, but the troubleshooting process:

- Form a hypothesis
- Test individual layers
- Validate assumptions
- Follow observable evidence
- Continue narrowing the scope until the root cause is identified

In this case, a potentially complex DNS outage was resolved through systematic troubleshooting and careful examination of configuration details.
