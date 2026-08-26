# Lab 01 — Linux Fundamentals & Security

## Objective

The goal of this lab was to practice Linux administration and basic security investigation on my Ubuntu home server.

I practiced identifying the system, managing file permissions, inspecting processes and services, identifying listening network ports, and investigating SSH authentication logs.

## Environment

- Ubuntu Server 26.04 LTS
- Linux home server
- SSH remote access
- Docker
- Tailscale

## Skills Practiced

- Linux command-line navigation
- Users and groups
- Linux file permissions
- chmod
- Processes and services
- TCP/UDP listening ports
- SSH
- Docker containers
- Linux system logs
- Basic authentication investigation

## Commands Used

### System Information

`whoami` — identified the current user.

`hostname` — displayed the server hostname.

`uname -a` — displayed Linux kernel and system information.

`cat /etc/os-release` — identified the Ubuntu version.

### Users and Permissions

`id` — displayed my user ID and group memberships.

`groups` — displayed the groups my account belongs to.

`pwd` — displayed my current directory.

`ls -la` — displayed files and their permissions.

`chmod 600 investigation.txt` — restricted a test investigation file so only the owner could read and modify it.

I learned:

- `r = 4` — read
- `w = 2` — write
- `x = 1` — execute
- `600 = rw-------`
- `644 = rw-r--r--`
- `755 = rwxr-xr-x`

## Processes and Services

I used `ps aux` to inspect running processes.

I used `ps aux | grep ssh` to locate SSH-related processes.

I inspected the SSH service with:

`systemctl status ssh`

The SSH service was active and running.

## Network Investigation

I used:

`sudo ss -tulpn`

to identify listening network ports and the processes associated with them.

I identified:

- TCP 22 — SSH
- TCP/UDP 53 — AdGuard Home / DNS
- TCP 80 — AdGuard Home
- TCP 3000 — AdGuard Home
- TCP 8096 — Jellyfin

I also learned:

- `0.0.0.0` means a service is listening on all IPv4 interfaces.
- `127.0.0.1` means a service is listening only on the local machine.

## SSH Log Investigation

I inspected SSH logs using:

`sudo journalctl -u ssh --no-pager`

I filtered successful authentication events using:

`grep "Accepted"`

I filtered failed authentication events using:

`grep "Failed"`

During the investigation, I identified both successful SSH authentication and a failed password attempt.

I learned to investigate:

1. Who attempted to log in?
2. What IP address did the connection originate from?
3. When did it happen?
4. Was authentication successful or unsuccessful?
5. Is the activity expected?

## What I Learned

This lab helped me understand how Linux permissions, processes, services, network ports, and authentication logs work together.

I practiced a basic SOC investigation workflow:

**Observe → Filter → Identify → Investigate → Determine whether activity is expected**

## Lab Evidence

The following screenshot contains redacted evidence from the hands-on lab. Network addresses and unnecessary identifying information were removed before publishing.

![Lab 01 Linux Security Evidence](redacted.png)

## Next Steps

Continue building the cybersecurity home lab and practice networking, Windows security, log analysis, and SOC investigation techniques.
