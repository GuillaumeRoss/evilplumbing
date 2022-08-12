slidenumbers: true

# Protect/hunt/respond with Fleet and osquery
![inline filtered](images/fleet_logos/fleet-logo-text-white.svg)

^ Welcome everyone! Please take a seat and grab one of the cards in front of you.

---
# Who we are

We work at Fleet. We like security. And open source.

**Guillaume** -> Head of security @ Fleet - @gepeto42.

**Kathy** -> Developer Advocate @ Fleet - @ksatter_dev 

Find us on the #Fleet channel in the osquery Slack!

![](images/backgrounds/disappear 001 - desktop.jpg)

---

# Pre-requisites

* Docker
  - Mac and Windows: Desktop
  - Linux: Installed from Docker and not your distro's packages
*  nodeJS (https://nodejs.org/en/download/)
  - Mac: `brew install npm` works too
  - Linux: Recent Ubuntu LTS `apt-get install npm` works too
  - Windows: get it straight from node

![](images/backgrounds/disappear 003 - desktop.jpg)

---
# Communications

## Slack

For troubleshooting, questions, etc, join the #Fleet channel on the osquery Slack.

## https://fleetdm.com/slack

![](images/backgrounds/E27A8193.jpg)


---

## Google Docs running note

https://bit.ly/3QaL3IP


![inline](images/qr_codes.png)



![](images/backgrounds/E27A1849.jpg)

---
# The workshop

Split in 8 "modules"

* Installing Fleet
* osquery basics
* osquery SQL basics
* Fleet policies / detecting dangerous configs

![](images/backgrounds/E27A8324.jpg)

---
# The workshop

* Vulnerability detection
* Scheduled queries / gathering data for IR
* Integrations
* Finding badness using MITRE ATT&CK and osquery

![](images/backgrounds/E27A1316.jpg)

---
# A few terms

* **osquery** - the open source agent running on endpoints
* **Fleet** - the open source server used to manage osquery
* **Orbit** - open source runtime for osquery, by Fleet, to make packaging and configuring it easier (including automatic updates)

![](images/backgrounds/dont fear the reaper 04 destkop.jpg)

---
# Breaks

We aren't monsters, we know it's 9am on DEF CON Thursday. 

Even though **Guillaume**'s brain is still in Eastern timezone mwahaha.

## 9:45 to 10am -> Break 1 & catch-up 
## 11:30 to 11:45am -> Break 2

![](images/backgrounds/gateways 01 - desktop.jpg)

---
# Why install Fleet before basics?

We want to be sure everyone is able to get Fleet up to make it easy to use osquery later. 

DO NOT HESITATE to ask questions in person or on Slack - you need a Fleet instance to work!

![](images/backgrounds/disappear 007 - desktop.jpg)

---
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                 │
│          ###                                                                    │
│           #  #    #  ####  #####   ##   #      #      # #    #  ####            │
│           #  ##   # #        #    #  #  #      #      # ##   # #    #           │
│           #  # #  #  ####    #   #    # #      #      # # #  # #                │
│           #  #  # #      #   #   ###### #      #      # #  # # #  ###           │
│           #  #   ## #    #   #   #    # #      #      # #   ## #    #           │
│          ### #    #  ####    #   #    # ###### ###### # #    #  ####            │
│                                                                                 │
│                       #######                                                   │
│                       #       #      ###### ###### #####                        │
│                       #       #      #      #        #                          │
│                       #####   #      #####  #####    #                          │
│                       #       #      #      #        #                          │
│                       #       #      #      #        #                          │
│                       #       ###### ###### ######   #                          │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```
## Installing Fleet

![](images/backgrounds/E27A8783.jpg)

---
# Local setup

This setup is local and private to you **except** for status logs and scheduled queries.
THIS SETUP IS FOR THE WORKSHOP ONLY NEVER USE THIS "FOR REAL" 


```
┌─────────────────────────────────────────────┐                                
│Docker setup for workshop                    │                                
│ ┌─────┐ ┌─────┐ ┌─────┐  ┌─────┐  ┌─────┐   │       ┌───────────────────────┐
│ │Fleet│ │mysql│ │redis│  │proxy│  │beat │─ ─│─ ─ ─ ─▶graylog1.evil.plumbing │
│ └─────┘ └─────┘ └─────┘  └─────┘  └─────┘   │       └───────────────────────┘
└─────────────────────────────────────────────┘                                
```


![](images/backgrounds/disappear 006 - desktop.jpg)

---
* Clone the `defcon` branch of Kathy's repo:

`git clone -b defcon https://github.com/ksatter/fleet-docker.git`

* Make sure Docker is installed and running.
* Navigate to the directory `cd fleet-docker`
* Run `docker compose up`

Fleet will accessible at `fleet.traefik.me`

**More instructions in README.md in the repo's defcon branch**

![](images/backgrounds/white sands 08 4k.jpg)

---
![](images/backgrounds/mind melt 06 desktop.jpg)

# Install `fleetctl`

Command line tool for managing Fleet and generating osquery packages.

0. Install it from the *releases* page https://github.com/fleetdm/fleet/releases **OR**
1. Make sure node is installed and `npm` is available.
2. `sudo npm install -g fleetctl` (On Windows, replace sudo with using an admin `cmd/PowerShell`)
3. `fleetctl --version` to check that it's working


---
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                 │
│                                                                                 │
│                ______           __             ___                __            │
│               / ____/__  ____  / /__________ _/ (_)___  ___  ____/ /            │
│              / /   / _ \/ __ \/ __/ ___/ __ `/ / /_  / / _ \/ __  /             │
│             / /___/  __/ / / / /_/ /  / /_/ / / / / /_/  __/ /_/ /              │
│             \____/\___/_/ /_/\__/_/   \__,_/_/_/ /___/\___/\__,_/               │
│                                                                                 │
│                              _____      __                                      │
│                             / ___/___  / /___  ______                           │
│                             \__ \/ _ \/ __/ / / / __ \                          │
│                            ___/ /  __/ /_/ /_/ / /_/ /                          │
│                           /____/\___/\__/\__,_/ .___/                           │
│                                              /_/                                │
│                                                                                 │
│                                                                                 │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```

![](images/backgrounds/horrorvision vintage 09 desktop.jpg)

# Centralized class setup

---
```
   ┌───────────────────────┐     ┌───────────────────────┐
   │                       │     │                       │
   │                       │     │                       │
   │ fleet1.evil.plumbing  │────▶│graylog1.evil.plumbing │
   │                       │     │                       │
   │                       │     │                       │
   └───────────▲───────────┘     └───────────────────────┘
               │                                          
        ┌──────┴──────┐                                   
┌───────┼─────────────┼───────┐                           
│       │             │       │                           
│  ┌─────────┐   ┌─────────┐  │                           
│  │ osquery │   │ osquery │  │                           
│  └─────────┘   └─────────┘  │                           
│    Pre-built installers     │                           
└─────────────────────────────┘                           
```

![](images/backgrounds/horrorvision vintage 04 desktop.jpg)

---
We also have a centralized Fleet server for everyone to use.

* https://fleet1.evil.plumbing
* Your usernames are on your desks. 
* Password for first logon: DEFCON2022workshop!
* Pre-generated installers point there

1. Log in now and set your password!
2. Generate an installer with `fleetctl` (see Add Hosts, or the shared Docs for full commands)
3. Install the package on a VM **for testing**, without REAL data

![](images/backgrounds/wanderers 04 desktop.jpg)

---
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│      __________  _________  ______   _____ __  ______________________________   │
│     / ____/ __ \/ ____/   |/_  __/  / ___// / / / ____/ ____/ ____/ ___/ ___/   │
│    / / __/ /_/ / __/ / /| | / /     \__ \/ / / / /   / /   / __/  \__ \\__ \    │
│   / /_/ / _, _/ /___/ ___ |/ /     ___/ / /_/ / /___/ /___/ /___ ___/ /__/ /    │
│   \____/_/ |_/_____/_/  |_/_/     /____/\____/\____/\____/_____//____/____/     │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```

You now have:

1. A mostly private local Fleet setup
2. VM(s) pointing to our centralized workshop Fleet server
3. An account to log in to https://fleet1.evil.plumbing

![](images/backgrounds/mind melt 03 desktop.jpg)

---
# osquery basics

* Open source, cross-platform agent
* Makes your machine look like an SQL DB
* ~300 different tables

What's running? Who's logged in? What devices are connected? What's connecting? Huge breadth of data.

**Bookmark this** https://osquery.io/schema/

^ When looking at the schema, see you can filter the tables based on which operating system support them. Some tables, for example, the **registry** table, only works on Windows for apparent reasons, and others work on multiple OSes.

![](images/backgrounds/E27A1600.jpg)

---
# How osquery works

* `osqueryi`: terminal/interactive version (or `orbit shell` if installed via orbit)
* `osqueryd` - daemon
* Configured with local files or over TLS
* Logs locally, over TLS (Fleet!) or other outputs.
* If using osquery over TLS, real-time queries can be performed.

![](images/backgrounds/disappear 002 - desktop.jpg)

---

# What is Fleet?

* [https://github.com/fleetdm/fleet](https://github.com/fleetdm/fleet)

Fleet is the most widely used open source osquery manager

* live queries
* policies
* management of osquery at scale

![](images/backgrounds/E27A1645.jpg)

---
# Installing osquery

Two ways:

1. Regular osquery package + configure to connect to Fleet
2. With Fleet Orbit packages - pre-configured and with automatic updates 😍

You've already done #2!

![](images/backgrounds/E27A8791.jpg)

---
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│              ____  ____  _________    __ __    ____________  _________          │
│             / __ )/ __ \/ ____/   |  / //_/   /_  __/  _/  |/  / ____/          │
│            / __  / /_/ / __/ / /| | / ,<       / /  / // /|_/ / __/             │
│           / /_/ / _, _/ /___/ ___ |/ /| |     / / _/ // /  / / /___             │
│          /_____/_/ |_/_____/_/  |_/_/ |_|    /_/ /___/_/  /_/_____/             │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```

Resuming in 15m.

If anyone had issues installing Fleet or using `fleetctl` to make packages, perfect time to catch up!

![](images/backgrounds/gateways 02 - desktop.jpg)


---
# osquery SQL basics

Try these queries either:

1. On your own Fleet.
2. On the centralized Fleet, targeting one of your VMs (to avoid 80 people querying everyone's machines non stop).

![](images/backgrounds/E27A1747.jpg)

---
# Target a single device with a query

![inline](images/target_single_device.png)


---
# Test it

* Go to **Queries** and then **Create new query**

```sql
SELECT * from osquery_info;
```

Run it on one of your VMs. Did you get data back?

![](images/backgrounds/E27A2138.jpg)

---
# SQL in osquery

* Uses SQLite
* Standard SQL

https://osquery.readthedocs.io/en/stable/introduction/sql/



![](images/backgrounds/E27A2199.jpg)

---
# SQL in osquery

Mostly READING from tables. Sometimes just a single table.

```sql
SELECT * FROM users;
```

```sql
SELECT * FROM crontab;
```

```sql
SELECT * FROM processes;
```


^ By the way - using uppercase for SELECT and FROM here is just to make the queries more legible. It would work without that.

![](images/backgrounds/disappear 004 - desktop.jpg)

---
# Exercise: SQL

1. Look at the osquery schema.
2. Find a way to query a few of those:
  - Is disk encryption enabled? 
  - What Linux kernel modules are loaded? (feel free to query Linux VMs that are not your own if needed)
  - Does the file `/etc/hosts` exist on the system?
  - Are USB devices present?
  - How long have systems been up?

![](images/backgrounds/pop skullture satanic - desktop.jpg)

---
# Exercise: SQL - What did you come up with?

Time to look at your examples!

^ `select * from disk_encryption;` (or bitlocker), `kernel_modules`, `SELECT * FROM file WHERE path='/etc/hosts';`

![](images/backgrounds/gateways 03 - desktop.jpg)

---
# SQL - filter what you need only

* Select only the columns you need: 

```sql
SELECT username, uid FROM users;
```
* Filter with WHERE: 

```sql
SELECT username, uid FROM users WHERE username = 'guillaume';
``` 

Which Windows machine on Fleet1 has a user called `guillaume`?

---
# SQL - wildcards

* Wildcards with LIKE: 

```sql
SELECT column1, column2 FROM table WHERE column2 LIKE '%potato%';
```

`%` matches any sequence. `_` matches a single character. For file paths, `%` can be used in a directory: `/etc/%/*.conf`

![](images/backgrounds/E27A3247.jpg)


---
# SQL - you can get advanced!

You can also `SPLIT`, concatenate create temporary tables, essentially do almost anything you could think of doing with SQL. 

See our query to detect log4j being used in Java: https://fleetdm.com/securing/detect-log4j-with-osquery-and-fleet

```sql
SELECT key, data, split(path, '\',3) as keyyouwant FROM 
registry WHERE key like 'HKEY_LOCAL_MACHINE\System\CurrentControlSet\control\LSA' 
and name='LMCompatibilityLevel';
```

![](images/backgrounds/E27A3406.jpg)

^ This could be particularly useful if you were also using wildcards, for example, looking at all users under HKEY_USERS

---
# User specific tables

* What Chrome extensions are installed? (or shell history, or Firefox extensions, etc.)

These questions require a "per-user" answer. They have a **UID** column. 

The *users* table also has a **UID** column.

![](images/backgrounds/E27A8079.jpg) 

---

### Try this

```sql
SELECT * FROM table_with_uids WHERE 
table_with_uids.uid IN (SELECT uid FROM users);
```

Use one of these:
* `chrome_extensions`
*  `firefox_addons` 
* `safari_extensions`

![](images/backgrounds/E27A3522.jpg)

---
# Results

What did you get?

Could you tell precisely what user was related to each entry?

How would you do it? 

![](images/backgrounds/E27A3543.jpg)

---
# List the actual users?
```sql
SELECT users.username, table_with_uids.what_you_want, 
table_with_uids.what_you_want2 
FROM users CROSS JOIN table_with_uids USING (uid);
```

![](images/backgrounds/E27A4402.jpg)


---
# Chrome extensions example
```sql
SELECT users.username, chrome_extensions.name, 
chrome_extensions.description 
FROM users 
CROSS JOIN chrome_extensions USING (uid);
```

![](images/backgrounds/E27A7524.jpg)



---
# Processes table

```sql
SELECT * FROM processes;
```

This is a snapshot of what is happening. 

* How would you find a specific process that you know is running?
* What potential issue could this create if we don't consider that this is a snapshot?


![](images/backgrounds/gateways 04 - desktop.jpg)

---

# Events table

* *file_events* (macOS and Linux)
* *ntfs_journal_events* (Windows)
* *powershell_events* (Windows + requires script block logging)
* *process_events* (macOS and Linux)
* *socket_events*  (macOS and Linux)

And many more!

![](images/backgrounds/E27A7845.jpg)

---
# Try one of the _events tables

Pick one that works with the OS of your host enrolled to Fleet.

Results?

![](images/backgrounds/E27A8029.jpg)

---
# The *osquery_flags* table

osquery flags: configuration - can be passed via TLS, in local config files, or at startup

To see flags currently applied on a host:

```sql
select * from osquery_flags;
``` 


![](images/backgrounds/E27A8074.jpg)

---
# Events flags

`disable-events` must be turned off. 

In a regular macOS environment, grant osquery *full disk access*:

https://fleetdm.com/docs/using-fleet/adding-hosts#grant-full-disk-access-to-osquery-on-mac-os


![](images/backgrounds/E27A2085bw.jpg)

---
# On Fleet

1) On Fleet1 - I configured it already 
2) On your own machines... add this to global agent config and restart agents:

```yaml
    audit_persist: true
    disable_audit: false
    disable_events: false
    audit_allow_sockets: true
```

![](images/backgrounds/E27A8106.jpg)

---
# Generate activity

In another terminal on the same machine (ssh, screen, anything), run a few commands (`ping`, `ls`, `top`, whatever!)

# Query again

```sql
SELECT * FROM process_events;
```

^ Does everyone have process event results? 

![](images/backgrounds/E27A8134.jpg)

---
# Events and Fleet

* Fleet does not "store" all osquery data (it's not a SIEM!)
* Send them to something such as Firehose to ingest in a SIEM, Graylog, Elastic, Splunk, etc.
* https://fleetdm.com/docs/using-fleet/osquery-logs#firehose

![](images/backgrounds/E27A8184.jpg)

---
# Use cases for events

* Detection and investigation (`process_events`, `process_open_files`, etc.)
* File integrity monitoring (FIM) (`file_events`, `ntfs_journal_events`)
* Tracking the use of USB storage (`hardware_events`)

![](images/backgrounds/E27A8190.jpg)



---
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ ___      _          _   _                _                                      │
│|   \ ___| |_ ___ __| |_(_)_ _  __ _   __| |__ _ _ _  __ _ ___ _ _ ___ _  _ ___  │
│| |) / -_)  _/ -_) _|  _| | ' \/ _` | / _` / _` | ' \/ _` / -_) '_/ _ \ || (_-<  │
│|___/\___|\__\___\__|\__|_|_||_\__, | \__,_\__,_|_||_\__, \___|_| \___/\_,_/__/  │
│                               |___/                 |___/                       │
│              __ _                                                               │
│ __ ___ _ _  / _(_)__ _ ___                                                      │
│/ _/ _ \ ' \|  _| / _` (_-<                                                      │
│\__\___/_||_|_| |_\__, /__/                                                      │
│                  |___/                                                          │
└─────────────────────────────────────────────────────────────────────────────────┘
```

# Using Fleet policies

![](images/backgrounds/ghost in the machine - desktop.jpg)



---
# Policies

Policies in Fleet are queries that *pass* or *fail*.

If results are returned, that's a *pass*.

```sql
SELECT 1 WHERE 1=1;
``` 

This policy passes for sure since 1 is always equal to 1 (unless you ask Terrence Howard)

![](images/backgrounds/E27A8278.jpg)

---

We previously had a query to find users called "guillaume":

```sql
SELECT username, uid FROM users WHERE username = 'guillaume';
```

If we used this as a policy - it would pass when a user called guillaume exists, and fail otherwise.

Simplified:

```sql
SELECT 1 FROM users WHERE username = 'guillaume';
```

![](images/backgrounds/E27A8193.jpg)

---
# Exercise - Policies

Create a few policies for things we have previously queried for.

Examples:

Is a specific Chrome extension installed?
Is disk encryption enabled?
Has the machine been rebooted in the last 30 days?
Is the machine running a specific version of an app or OS or newer?

**If creating them on Fleet1, prefix policy name with your username/number**



![](images/backgrounds/E27A8271.jpg)


---
# Exercise - Firewall policy

Can you write a policy to check if the firewall is enabled?

Pick the OS you want.

**Tips**:

* Look through the osquery schema
* Use `SELECT 1 FROM` to only return results if a specific condition is passed. (Look at the built-in queries for examples)

![](images/backgrounds/E27A8570.jpg)

---
# Answer - Firewall

## macOS

```sql
SELECT 1 FROM alf WHERE global_state >= 1;
```

`global_state` could be set to 1 or 2 meaning it's enabled, or enabled and blocking all inbound, and this would pass.

![](images/backgrounds/E27A8625.jpg)

---
# Answer - Firewall

## Windows
We do not have a table for firewall status itself, but `windows_security_center` has a firewall column!

```sql
SELECT 1 FROM windows_security_center WHERE firewall='good';
```

![](images/backgrounds/E27A8691.jpg)


---
# Exercise - Antivirus OK - Windows

Create a policy that will check that the AV is working alright (as well as AV can work 😂) in the sense that it is:

* Running
* Up to date

![](images/backgrounds/the unreturning III - desktop.jpg)

---
# Exercise - Antivirus OK - Windows

What'd you come up with?

I'll go configure it on Fleet1

![](images/backgrounds/E27A8704.jpg)

^ SELECT 1 from windows_security_center wsc CROSS JOIN windows_security_products wsp WHERE antivirus = 'Good' AND type = 'Antivirus' AND signatures_up_to_date=1; - who didn't use the built-in one?


---
# "Negative" policies

Policies where you want the LACK of results to be a PASS.

Ex: Laptops should not have unencrypted SSH keys.

```sql
SELECT 1 WHERE NOT EXISTS 
(SELECT 1 FROM users CROSS JOIN user_ssh_keys USING (uid) WHERE encrypted='0');
```

What we are doing: 

Selecting 1 so the policy passes IF "not exists" (select 1 if there are keys that are unencrypted).

![](images/backgrounds/E27A2085.jpg)

---

# Create an unencrypted SSH key on one of your VMs

If you are on Mac or Linux.

`ssh-keygen -t rsa -b 4096 -C "your_email@domain.lulz"` -> Make sure you don't overwrite your real keys. But store it in your `.ssh` (e.g., `/Users/g/.ssh/unencrypted_ssh_rsa.pub`)

That VM should start failing that policy!

![](images/backgrounds/E27A3406a.jpg)

---
# Unwanted software

```sql
SELECT 1 WHERE NOT EXISTS 
(SELECT * FROM deb_packages WHERE name LIKE 'vim%');
```

1. `deb_packages` table. You can query all of it and see, one of your fake machines should have `vim-tiny` and `vim-common`.
2. We use `WHERE NOT EXISTS` with `SELECT 1` to return results IF the sub-query is empty. So `1` is returned only if nothing that has a name `LIKE` `vim%` is found.

![](images/backgrounds/E27A8699.jpg)


---
# Exercise - App installed and up to date

Name some apps you think should be either:

1) NOT installed at all.
2) Installed and up to date.

You want to be alerted if someone has an old version of the app, but not if they do not have the app at all.

![](images/backgrounds/horrorvision vintage 09 desktop.jpg)




---

```sql
SELECT 1 WHERE EXISTS 
(SELECT 1 FROM apps a3 WHERE a3.bundle_identifier = 'org.mozilla.firefox' 
  AND a3.bundle_short_version>='100.0')
 OR NOT EXISTS 
(SELECT 1 FROM apps a2 WHERE a2.bundle_identifier = 'org.mozilla.firefox')
```

![](images/backgrounds/E27A8338.jpg)



---
```
┌───────────────────────────────────────────────────────────────┐
│  _   _   _   _   _   _   _   _   _   _   _   _   _   _   _    │
│ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \   │
│( V | u | l | n | e | r | a | b | i | l | i | t | i | e | s )  │
│ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/   │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

1. Enabled by default. No scanning needed
2. Grabs inventory of software -> apps/programs, tools installed via package managers (brew for example), etc.
3. Matches that against NVD database of vulnerabilities.


![](images/backgrounds/E27A8388.jpg)


---
# Vulnerability automations

1. Requires admin privileges to setup (so check it out on your local deployment).
2. Integrates with:
  * Webhooks (anything)
  * Jira
  * Zendesk
  * More soon...
  
![](images/backgrounds/E27A8466.jpg)

---
# Orchestrator

Fleet works with anything that can receive a webhook.

[Tines.io](https://tines.io) is a nice one with a free trial. 

Create a free account if you want to set up automations.

**Webhook URLs are in the Google Docs -- results will go to a Slack Channel I will show!**

![](images/backgrounds/message from another world - desktop.jpg)


---
# On your local stack

Point automations to my Tines Webhook now - we'll look later to see the results!

URL (also on Docs for easy copy-pasta):

https://rustling-snow-6608.tines.com/webhook/0fe6acf0c2b62cdf527eeffdbcefb7a3/1ebd48b0aa19e1954791e30eda28eb69

![](images/backgrounds/E27A8550.jpg)

---


# Scheduled queries

For things you want to keep track of over time and/or use with a SIEM/centralized logs

```
┌────────────────────────────────────────────────────────────────────────────────────────────────┐
│        _____      __             __      __         __                         _               │
│       / ___/_____/ /_  ___  ____/ /_  __/ /__  ____/ /  ____ ___  _____  _____(_)__  _____     │
│       \__ \/ ___/ __ \/ _ \/ __  / / / / / _ \/ __  /  / __ `/ / / / _ \/ ___/ / _ \/ ___/     │
│      ___/ / /__/ / / /  __/ /_/ / /_/ / /  __/ /_/ /  / /_/ / /_/ /  __/ /  / /  __(__  )      │
│     /____/\___/_/ /_/\___/\__,_/\__,_/_/\___/\__,_/   \__, /\__,_/\___/_/  /_/\___/____/       │
│                                                         /_/                                    │
└────────────────────────────────────────────────────────────────────────────────────────────────┘
```

![](images/backgrounds/E27A5512.jpg)

---
# Scheduled queries

### How

1. Create and save a query.
2. Schedule it!

![](images/backgrounds/disappear 005 - desktop.jpg)


---
# Exercise - scheduled queries

On your *LOCAL STACKS* - schedule 2-3 queries to run every 15 minutes.

**NOTE**: The logs from this will go to a centralized server, do not query confidential information!

![](images/backgrounds/E27A8745.jpg)

---

# Scheduled queries - recorded where?

https://fleetdm.com/docs/using-fleet/osquery-logs

Many options including: 
* Filesystem (grab it from there with Filebeat for example)
* Snowflake
* Splunk
* Firehose
* Kinesis
* Kafka
* More!

![](images/backgrounds/video rental sticker - vhs - desktop.jpg)

---
# Logging - example

Open `fleet-docker/filebeat/filebeat.yml`

It is simply pointed to log files:

```
  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    - /fleet/logs/*.log
 #   - /var/log/*.log
    - /tmp/*.log
```

![](images/backgrounds/E27A8709.jpg)


---
# Logging - example
Look at `fleet-docker/fleet/default.env` 
`FLEET_LOGGING_JSON` is set to `"true"`

JSON is very easy to parse and ingest by most logging tools out there.


![](images/backgrounds/E27A8625bw.jpg)

---
# Hunting

1. In the real world, I highly recommend sending *_events* to your centralized logs/SIEM etc. You will likely need some historical data.
2. Let's use MITRE ATT&CK for some ideas.

![](images/backgrounds/E27A8791.jpg)

---
# MITRE ATT&CK For Linux

I am using the Linux Matrix - as everyone has Linux machines simulated in the **preview**!

https://attack.mitre.org/matrices/enterprise/linux/

**Execution**: Do you see something in there we could look at (excluding events)?

^ Suggest Scheduled Job Creation - And let's make a scheduled query for cronjobs!

![](images/backgrounds/E27A8794.jpg)

---
# MITRE ATT&CK for all OSes

**Privilege escalation**: *T1543* - Create or modify system process
https://attack.mitre.org/techniques/T1543/

Pick an OS and make an example!

^ macOS: select * from launchd; Windows: select * from registry where blablabla path like HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\Windows\load

![](images/backgrounds/white sands 06 4k.jpg)

--- 
# PowerShell Script Block Logging + osquery

One of the most valuable ways to dig into PowerShell shenanigans. Requires enabling events

First, check if script block logging is enabled!

HKLM\Microsoft\Windows\PowerShell\ScriptBlockLogging\EnableScriptBlockLogging. How would you query that?

Bonus: turn it into a policy query

^ SELECT 1 FROM registry WHERE path = 'HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging\EnableScriptBlockLogging' and data=1;

![](images/backgrounds/video rental sticker - be kind rewind - desktop.jpg)

---
# See all current network connections

How would you look at a snapshot of all network connections?

Then, how would you filter on an IP address IOC? (e.g., 8.8.8.8)

![](images/backgrounds/E27A3522a.jpg)

---
# Yara

A great way to look for "freeware"!

1. Can pre-load Yara rules and configure them in the **yara** configuration section (not great for opsec)
2. Can point to URLs
3. Can specify the signature in the query
4. If events are enabled, can run constantly!

For workshop purposes we'll go **inline**.

![](images/backgrounds/disappear 008 - desktop.jpg)

---
# Yara

* Download eicar.com on your host running the Fleet preview.

wget https://secure.eicar.org/eicar.com

Warning: On Mac, put it on your home directory, **not** Downloads or Desktop. Those are protected folders the preview osquery can't read.

* Create a query using the **yara** table and this signature. Don't look at the whole filesystem - too slow for demo purposes. Point it at the directory containing the file.

![](images/backgrounds/radio boneyard 02 desktop.jpg)

---

```yaml
rule eicar {
    strings:
    $s1 = "X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*" fullword ascii
    condition:
    all of them
```

* Convert your query into a policy query

![](images/backgrounds/white sands 02 4k.jpg)

---
# Yara

The query:

```sql
SELECT * FROM yara WHERE path like '/root/%%' AND sigrule IN (
    'rule eicar {
    strings:
    $s1 = "X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*" fullword ascii
    condition:
    all of them
}'
) AND matches='eicar';
```
![](images/backgrounds/white sands 01 4k.jpg)

---
# Yara - policy

Make it a negative query so the policy passes if nothing is found!

```sql
SELECT 1 WHERE NOT EXISTS (SELECT * FROM yara WHERE path like '/root/%%' AND sigrule IN (
    'rule eicar {
    strings:
    $s1 = "X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*" fullword ascii
    condition:
    all of them
}'
) AND matches='eicar');
```

![](images/backgrounds/wander 017 cosmic dunes 4k.jpg)

---
# Yara - production grade

In production, you'd likely not scan the filesystem for malware like this, but either use:

**yara_events**

OR

Match a set of rules against **processes**. Load a standard library locally, then look up new things either live or via URL (opsec warnings go here).

![](images/backgrounds/betamax logo glitched - desktop.jpg)

---
# Yara - processes

```sql
SELECT * FROM yara WHERE path IN (SELECT DISTINCT path FROM processes) AND
```

-> Then add your signature group, signature URLs.

### This scans only running processes against a set of Yara rules.

![](images/backgrounds/pop skullture gothmas 2021 - desktop.jpg)

---
# Yara - processes
Let's try it with eicar. nothing will match since eicar is not running.

```sql
SELECT * FROM yara WHERE path IN (SELECT DISTINCT path FROM processes) AND sigrule IN (
    'rule eicar {
    strings:
    $s1 = "X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*" fullword ascii
    condition:
    all of them
}'
) AND matches='eicar';
```

Notice it was pretty quick to run! Good query to schedule with a decent yara db.

![](images/backgrounds/E27A2890.jpg)



---

# MITRE ATT&CK and osquery for hunting

## https://attack.mitre.org/

We'll pick a few techniques and see how we can hunt for them.

![](images/backgrounds/E27A8691i.jpg)

---
# T1053 - Scheduled tasks / Cron jobs

1. Create a query for T1053.002
2. Which Windows machine on Fleet1 has weird scheduled tasks?

![](images/backgrounds/radio boneyard 03 desktop.jpg)

---
# T1562.002 

Find proof two of the machines starting with `dcwin` had their event logs fooled with.

* System, Microsoft-Windows-Sysmon/Operational, Microsoft-Windows-PowerShell/Operational
* Wiping and remote PS exec
* Auditpol
* Times 

^ We could check out what is happening in event logs with `SELECT * FROM windows_eventlog WHERE channel='System' LIMIT 5;` or check out sysmon logs: `SELECT * FROM windows_eventlog WHERE channel='Microsoft-Windows-Sysmon/Operational';`  BUT You should also look for remote PS execution with Suspend -> SELECT * FROM windows_eventlog WHERE channel='Microsoft-Windows-PowerShell/Operational' AND eventid='4104' AND data LIKE "%%SuspendThread%%" ;

![](images/backgrounds/oblivion 017b - darkness gathering \(alt\) desktop.jpg)

---

## Which machine had its logs wiped most recently?
## Which machine had its event log messed with in more complicated ways? How?

^ dcwin2 had its logs wiped.  `SELECT * FROM windows_eventlog WHERE channel='System' AND eventid='104';` - dcwin3 had its even log messed with - it is the one that has not logged recently..

![](images/backgrounds/horrorvision 03 desktop.jpg)

---

Log clearing:

```sql
SELECT * FROM windows_eventlog WHERE channel='System' AND eventid='104';
```

AuditPol fun:

```sql
SELECT * FROM windows_eventlog WHERE 
channel='Microsoft-Windows-Sysmon/Operational' 
AND eventid='1' AND data LIKE "%%auditpol%%";
```
phant0m:

```sql
SELECT * FROM windows_eventlog WHERE 
channel='Microsoft-Windows-PowerShell/Operational' 
AND eventid='4104' AND data LIKE "%%SuspendThread%%"
```

![](images/backgrounds/wanderers 02 desktop.jpg)

---
# Exercise - Prove Internet exposure

All the `dcwin*` machines have RDP exposed to the Internet. Some of the Linux ones have SSH.

Prove it with osquery!

![](images/backgrounds/radio boneyard 06 desktop.jpg)

---

# Internet exposure

## How'd you do it?

A few ways.. but my favorite for such popular ports: just look for connections!

* Exclude RFC1918 IPs

![](images/backgrounds/oblivion 013b desktop.jpg)

---
# If we have time left...

## Open use case time!

Suggest use cases in Slack. The one that gets the most emoji reactions is the one we'll look at next!

![](images/backgrounds/be kind rewind glitched - desktop.jpg)

---
# What we did!

1. Installed Fleet
2. Learned how to query osquery
3. Created Fleet policies
4. Integrated policies and vulnerabilities with webhooks
5. Scheduled tasks and sent results to a centralized logging system
6. Hunted for threats

Have fun using Fleet in the real world!

![](images/backgrounds/E27A8699.jpg)

---
# Thank you for your time!

* FREE SWAG SHIPPED HOME! 

* Fleet channel if you have questions later! fleetdm.com/slack

* Twitter:  @ksatter_dev @gepeto42 and @fleetctl

* https://www.pluralsight.com/authors/guillaume-ross 

* IF YOU HAVE AN ISC2 CERT: https://www.be-represented.org/ 

Wallpapers provided by the great Rob Sheridan!

![original](images/backgrounds/fleet-wallpaper_desktop - 5120x2880.jpg)


---


![](images/backgrounds/mind melt 06 desktop.jpg)
