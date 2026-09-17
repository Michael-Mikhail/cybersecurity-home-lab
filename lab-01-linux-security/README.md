# Lab 01 — Linux SSH Security Investigation

## Objective

Investigate SSH authentication activity on an Ubuntu Server and determine whether observed login attempts were authorized or suspicious.

This lab demonstrates basic Linux security monitoring and SOC investigation skills using real SSH authentication logs.

---

## Lab Environment

* Ubuntu Server
* OpenSSH
* Linux system logs / `journalctl`
* Internal home-lab network

---

## Scenario

SSH authentication activity was observed on the Ubuntu home server.

The goal was to:

* Confirm the SSH service was running
* Review authentication logs
* Identify successful and failed login attempts
* Identify usernames and source IP addresses
* Create an event timeline
* Determine whether the activity appeared malicious or legitimate

---

## Evidence Collected

### Event 1 — Successful SSH Login

**Date/Time:** 2026-08-14 19:11:09 UTC
**Source IP:** `10.0.0.20`
**Username:** `michael`
**Result:** Successful authentication

```text
Accepted password for michael from 10.0.0.20 port 54107 ssh2
```

This shows that the valid account `michael` successfully authenticated to the SSH server.

---

### Event 2 — Failed SSH Login

**Date/Time:** 2026-09-16 15:30:36 UTC
**Source IP:** `10.0.0.16`
**Username Attempted:** `michaal`
**Result:** Failed authentication — invalid username

```text
Failed password for invalid user michaal from 10.0.0.16 port 56005 ssh2
```

The username `michaal` does not exist on the server.

The authentication attempt therefore failed before a normal user session could be established.

---

### Event 3 — Successful SSH Login

**Date/Time:** 2026-09-17 16:55:19 UTC
**Source IP:** `10.0.0.16`
**Username:** `michael`
**Result:** Successful authentication

```text
Accepted password for michael from 10.0.0.16 port 62448 ssh2
```

The SSH authentication succeeded.

A Linux session was then opened for the user:

```text
pam_unix(sshd:session): session opened for user michael(uid=1000)
```

---

## Investigation Timeline

| Date       | Source      | Username  | Result                    |
| ---------- | ----------- | --------- | ------------------------- |
| 2026-08-14 | `10.0.0.20` | `michael` | Successful login          |
| 2026-09-16 | `10.0.0.16` | `michaal` | Failed — invalid username |
| 2026-09-17 | `10.0.0.16` | `michael` | Successful login          |

---

## Analyst Interpretation

The September 16 failed authentication attempt used the invalid username `michaal`.

The same internal source IP, `10.0.0.16`, later successfully authenticated using the valid username `mi
