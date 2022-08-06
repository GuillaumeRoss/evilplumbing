slidenumbers: true

# Protect/hunt/respond with Fleet and osquery
![inline filtered](images/fleet_logos/fleet-logo-text-white.svg)


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

For troubleshooting, questions, etc, join the #Fleet channel on the osquery Slack. : https://fleetdm.com/slack

![](images/backgrounds/E27A8193.jpg)


---

## Google Docs running note

https://bit.ly/3QaL3IP

![inline](images/qr_codes.png)


![](images/backgrounds/E27A1849.jpg)

---
# The workshop

Split in 8 "modules"

* Install Fleet
* osquery basics
* osquery SQL basics
* Fleet policies / detecting dangerous configs

![](images/backgrounds/E27A8324.jpg)

---
# The workshop

* Vulnerability identification
* Scheduled queries / gathering data for IR
* Integrations
* Finding badness using MITRE ATT&CK and osquery

![](images/backgrounds/E27A1316.jpg)

---
# Breaks

We aren't monsters, we know it's 9am on DEF CON Thursday.

## 9:45 to 10am -> Break 1 & catch-up 
## 11:30 to 11:45am -> Break 2

![](images/backgrounds/gateways 01 - desktop.jpg)

---
# Why install Fleet before basics?

We want to be sure everyone is able to get Fleet up to make it easy to query osquery later. 

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

---
* Clone the `defcon` branch of Kathy's repo:

`git clone -b defcon https://github.com/ksatter/fleet-docker.git`

* Make sure Docker is installed and running.
* Navigate to the directory `cd fleet-docker`
* Run `docker compose up`

Fleet will accessible at `fleet.traefik.me`

**More instructions in README.md in the repo's defcon branch**

---
# Install `fleetctl`

Command line tool for managing Fleet and generating osquery packages.

0. Install it from the *releases* page https://github.com/fleetdm/fleet/releases

OR 

1. Make sure node is installed and `npm` is available.
2. `sudo npm install -g fleetctl` (On Windows, replace sudo with using an admin `cmd/PowerShell`)


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

---
We also have a centralized Fleet server for everyone to use.

* https://fleet1.evil.plumbing
* Your usernames are on your desks. 
* Password for first logon: DEFCON2022workshop!
* Pre-generated installers point there

1. Log in now and set your password!
2. Generate an installer with `fleetctl` (see Add Hosts, or the shared Docs for full commands)
3. Install the package on a VM **for testing**, without REAL data


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

1. A mostly private local Fleet setup.
2. VM(s) pointing to our centralized workshop Fleet server.
3. An account to log in to https://fleet1.evil.plumbing

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

Fleet is the most widely used open source, osquery manager. Deploying osquery with Fleet enables programmable live queries, streaming logs, and effective management of osquery across 100,000+ servers, containers, and laptops. It's handy for talking to multiple devices at the same time.

![](images/backgrounds/E27A1645.jpg)

---
# Installing osquery

Two ways:

1. Regular osquery package + configure to connect to Fleet
2. With Fleet Orbit packages - pre-configured and with automatic updates 😍

You've already done #2!


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
# What happens in preview mode

1. Fleet server will be run using Docker.
2. A dockerized osquery will be run on your laptop, enrolled to the local Fleet.
3. Simulated Linux hosts will get enrolled to the local Fleet.
4. http://localhost:1337 - `admin@example.com` / `admin123#`

![](images/backgrounds/E27A2085.jpg)

---
# Test it

* Go to **Queries** and then **Create new query**

`SELECT * from osquery_info;`

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

`SELECT * FROM users;`

`SELECT * FROM crontab;`

`SELECT * FROM processes;`


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

* Select only the columns you need: `SELECT username, uid FROM users;`
* Filter with WHERE: `SELECT username, uid FROM users WHERE username = 'guillaume'; 

Which Windows machine on Fleet1 has a user called `guillaume`?

---
# SQL - wirldcards
`
* Wildcards with LIKE: `SELECT column1, column2 FROM table WHERE column2 LIKE '%potato%';`

`%` matches any sequence. `_` matches a single character. For file paths, `%` can be used in a directory: `/etc/%/*.conf`

![](images/backgrounds/E27A3247.jpg)

---
# SQL - you can get advanced!

You can also `SPLIT`, concatenate create temporary tables, essentially do almost anything you could think of doing with SQL. 

```
WITH pstree AS (  
  SELECT 0 as level, pid, name, parent, name as pparent, uid,  cast(uid as varchar(10)) puid       
  FROM processes   WHERE parent = 0  
  UNION ALL
  SELECT level+1 as level, t.pid, t.name, t.parent, pstree.pparent || '->' || t.name as pparent, t.uid, pstree.puid || '->' || t.uid as puid       
  FROM processes t   INNER join pstree on t.parent = pstree.pid )     
SELECT level, pid, name, pparent as process_chain, puid as user_chain  FROM pstree;  
```

(Query available on our centralized server) 

VERY useful to split **registry paths** on Windows!

![](images/backgrounds/E27A3406.jpg)

---
# Processes table

The previously shown query uses the processes table, cross-platform.

`SELECT * FROM processes;`

This is a snapshot of what is happening. 

* How would you find a specific process that you know is running?
* What potential issue could this create if we don't consider that this is a snapshot?


![](images/backgrounds/gateways 04 - desktop.jpg)

---
# User specific tables

* What Chrome extensions are installed? (or shell history, or Firefox extensions, etc.)

These questions require a "per-user" answer. They have a **UID** column. 

The *users* table also has a **UID** column.

![](images/backgrounds/E27A8079.jpg) 

---

### Try this

`SELECT * FROM table_with_uids WHERE table_with_uids.uid IN (SELECT uid FROM users);`

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

`SELECT users.username, table_with_uids.what_you_want, table_with_uids.what_you_want2 FROM users CROSS JOIN table_with_uids USING (uid);`

![](images/backgrounds/E27A4402.jpg)

---
# Chrome extensions example

Time to make a real one! There are a few ways to achieve it, but the previous example works too.

![](images/backgrounds/E27A5512.jpg)

---
# Chrome extensions example
`SELECT users.username, chrome_extensions.name, chrome_extensions.description FROM users CROSS JOIN chrome_extensions USING (uid);`

![](images/backgrounds/E27A7524.jpg)

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

Fleet manages flags.

`select * from osquery_flags;` to see flags currently applied on a host.

![](images/backgrounds/E27A8074.jpg)

---
# Events flags

`disable-events` must be turned off. 

OS permissions also apply, which is why you are likely getting no results now. In a regular environment, deploy osquery and grant it *full disk access* on macOS: https://fleetdm.com/docs/using-fleet/adding-hosts#grant-full-disk-access-to-osquery-on-mac-os

Then, there are settings per type of events.

![](images/backgrounds/E27A2085bw.jpg)

---
# On vanilla osqueryi

`osqueryi --audit_allow_config=true --audit_allow_sockets=true --audit_persist=true --disable_audit=false --events_expiry=1 --events_max=50000 --logger_plugin=filesystem --disable_events=false`

This will run an interactive osquery terminal with file, socket and process events enabled.

![](images/backgrounds/E27A8106.jpg)

---
# Generate activity

In another terminal on the same machine (ssh, screen, anything), run a few commands (`ping`, `ls`, `top`, whatever!)

# Go back to your running osqueryi

`select * FROM process_events;`

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

* Detection and investigation (process_events, process_open_files, etc.)
* File integrity monitoring (FIM) (file_events, ntfs_journal_events)
* Tracking the use of USB storage (hardware_events)

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

E.g., `SELECT 1 WHERE 1=1;` will always pass.

![](images/backgrounds/E27A8278.jpg)

---

We previously had a query to find users called "guillaume":

`SELECT username, uid FROM users WHERE username = 'guillaume';`

If we used this as a policy - it would pass when a user called guillaume exists, and fail otherwise.

Simplified:

`SELECT 1 FROM users WHERE username = 'guillaume';`

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
`SELECT 1 FROM alf WHERE global_state >= 1;`
`global_state` could be set to 1 or 2 meaning it's enabled, or enabled and blocking all inbound, and this would pass.

![](images/backgrounds/E27A8625.jpg)

---
# Answer - Firewall

## Windows
We do not have a table for firewall status itself, but `windows_security_center` has a firewall column!

`SELECT 1 FROM windows_security_center WHERE firewall='good';`

![](images/backgrounds/E27A8691.jpg)

---
# "Negative" policies

Policies where you want the LACK of results to be a PASS.

Ex: Laptops should not have unencrypted SSH keys.


`SELECT 1 WHERE NOT EXISTS (SELECT 1 FROM users CROSS JOIN user_ssh_keys USING (uid) WHERE encrypted='0');`

---
`SELECT 1 WHERE NOT EXISTS (SELECT 1 FROM users CROSS JOIN user_ssh_keys USING (uid) WHERE encrypted='0');`

What we are doing: 

Selecting 1 so the policy passes IF "not exists" (select 1 if there are keys that are unencrypted).

---

# Create an unencrypted SSH key on one of your VMs

If you are on Mac or Linux.

`ssh-keygen -t rsa -b 4096 -C "your_email@domain.lulz"` -> Make sure you don't overwrite your real keys. But store it in your `.ssh` (e.g., `/Users/g/.ssh/unencrypted_ssh_rsa.pub`)

That VM should start failing that policy!

![](images/backgrounds/E27A3406a.jpg)


---
# Exercise - App installed and up to date

Name some apps you think should be either:

1) NOT installed at all.
2) Installed and up to date.

i.e.: You want to be alerted if someone has an old version of the app, but not if they do not have the app at all.

---

`SELECT 1 WHERE EXISTS (SELECT 1 FROM apps a3 WHERE a3.bundle_identifier = 'org.mozilla.firefox' AND a3.bundle_short_version>='100.0') OR NOT EXISTS (SELECT 1 FROM apps a2 WHERE a2.bundle_identifier = 'org.mozilla.firefox')`

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
# On your local stack

Point automations to my Tines Webhook now - we'll look later to see the results!

URL (also on Docs for easy copy-pasta):

https://rustling-snow-6608.tines.com/webhook/0fe6acf0c2b62cdf527eeffdbcefb7a3/1ebd48b0aa19e1954791e30eda28eb69

![](images/backgrounds/E27A8550.jpg)

---
# Scheduled queries

### Why?

Things you want to keep historic data about. What's installed on our servers? What SSH keys were present on our machines?
### How

1. Create and save a query.
2. Schedule it!

![](images/backgrounds/disappear 005 - desktop.jpg)


---
# Exercise - scheduled queries

On your *LOCAL STACKS* - schedule 2-3 queries to run every 15 minutes.

**NOTE**: The logs from this will go to a centralized server, do not query confidential information!

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

---
# Logging - example
Look at `fleet-docker/fleet/default.env` 
`FLEET_LOGGING_JSON` is set to `"true"`

JSON is very easy to parse and ingest by most logging tools out there.

---
# Unwanted software

Let's make a **negative** policy. One that fails **if** something **is found**.

Assume you hit your head cartoon-style and suddenly started preferring emacs.
You want to detect any use of any package that starts with **vim** on Debian machines (such as the enrolled fake-Linux machines on your **preview**.)

**Tip**: Look at the built-in "SSH Keys encrypted" policy for ideas on making a negative policy.

![](images/backgrounds/E27A8691i.jpg)

---
# Unwanted software

`SELECT 1 WHERE NOT EXISTS (SELECT * FROM deb_packages WHERE name LIKE 'vim%');`

1. `deb_packages` table. You can query all of it and see, one of your fake machines should have `vim-tiny` and `vim-common`.
2. We use `WHERE NOT EXISTS` with `SELECT 1` to return results IF the sub-query is empty. So `1` is returned only if nothing that has a name `LIKE` `vim%` is found.

![](images/backgrounds/E27A8699.jpg)

---
# No unencrypted SSH keys

Enable this built-in policy.

![](images/backgrounds/disappear 006 - desktop.jpg)

---
# Automations

See that manage automations on the top right?

We need something to receive that webhook!

![](images/backgrounds/E27A8699.jpg)

---
# Orchestrator

Fleet works with anything that can receive a webhook.

[Tines.io](https://tines.io) is a nice one with a free trial. 

Create a free account if you want to set up automations.

**Webhook URLs are in the Google Docs -- results will go to a Slack Channel I will show!**

![](images/backgrounds/message from another world - desktop.jpg)

---


![](images/backgrounds/E27A8704.jpg)


---


![](images/backgrounds/the unreturning III - desktop.jpg)

---


![](images/backgrounds/E27A8625bw.jpg)

---

![](images/backgrounds/E27A8709.jpg)

---


![](images/backgrounds/E27A8745.jpg)

---

![](images/backgrounds/E27A8745.jpg)

---

---

![](images/backgrounds/E27A8791.jpg)


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

It's in HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging\EnableScriptBlockLogging. How would you query that?

Bonus: turn it into a policy query

^ SELECT 1 FROM registry WHERE path = 'HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging\EnableScriptBlockLogging' and data=1;

![](images/backgrounds/video rental sticker - be kind rewind - desktop.jpg)

---


![](images/backgrounds/white sands 08 4k.jpg)

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

1. Download eicar.com on your host running the Fleet preview.

wget https://secure.eicar.org/eicar.com

Warning: On Mac, put it on your home directory, **not** Downloads or Desktop. Those are protected folders the preview osquery can't read.

2. Create a query using the **yara** table and this signature. Don't look at the whole filesystem - too slow for demo purposes. Point it at the directory containing the file.

```yaml
rule eicar {
    strings:
    $s1 = "X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*" fullword ascii
    condition:
    all of them
```

3. Convert your query into a policy query

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

### This scans only running processes against a set of yara rules.

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

---
# T1053 - Scheduled tasks / Cron jobs

1. Create a query for T1053.002
2. Which Windows machine on Fleet1 has weird scheduled tasks?

---
# T1546.013 - PowerShell profile

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

---
# Thank you for your time!

* Fleet channel if you have questions later! fleetdm.com/slack

* Twitter:  @ksatter_dev @gepeto42 and @fleetctl

* https://www.pluralsight.com/authors/guillaume-ross 

Wallpapers provided by the great Rob Sheridan!

![original](images/backgrounds/fleet-wallpaper_desktop - 5120x2880.jpg)
