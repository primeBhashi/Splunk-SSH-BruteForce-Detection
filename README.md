# SSH Brute-Force Attack Detection with Splunk SIEM

## 📌 Project Overview
This project demonstrates the setup of a home lab environment to simulate, detect, and analyze a live SSH brute-force attack. The objective is to monitor authentication logs in real-time and visualize the attack patterns using Splunk Enterprise.

## 🛠️ Tools & Technologies Used
* **SIEM:** Splunk Enterprise
* **Attacker Machine:** Kali Linux (using `Hydra` for the brute-force attack)
* **Target Machine:** Ubuntu Server
* **Logs Analyzed:** Linux Secure Auth Logs (`/var/log/auth.log`)

## 🏗️ Lab Architecture
1. **Ubuntu Target:** Configured with SSH service exposed to the local network. Splunk Universal Forwarder / Local monitoring is set up to ingest `/var/log/auth.log`.
2. **Kali Attacker:** Runs a dictionary-based SSH brute-force attack against the Ubuntu target using `hydra` and the `fasttrack.txt` wordlist.
3. **Splunk Server:** Parses the ingested logs, extracts relevant fields using Regular Expressions (Regex), and visualizes the data.

## 🚀 Attack Simulation
The following command was executed on the Kali machine to initiate the attack:
`hydra -l leo -P /usr/share/wordlists/fasttrack.txt ssh://<target-ip>`

<img width="960" height="504" alt="hydra attack" src="https://github.com/user-attachments/assets/8596e12c-3b3d-4e5a-ad8e-36762c4a8a77" />


## 🔍 Threat Detection & SPL Queries
To detect the attack, the following Splunk Search Processing Language (SPL) queries were utilized:

**1. Identifying Failed Logins:**
`source="/var/log/auth.log" "Failed password"`

<img width="960" height="504" alt="fail alerts" src="https://github.com/user-attachments/assets/9f9ded62-584c-4679-af13-cf099b7299b1" />

**2. Timechart of Attack Spikes (Monitoring volume over time):**
`source="/var/log/auth.log" "Failed password" | timechart count span=1m`

**3. Extracting and Identifying Top Attacker IPs (Regex):**
`source="/var/log/auth.log" "Failed password" | rex "from (?<attacker_ip>\d+\.\d+\.\d+\.\d+)" | top limit=10 attacker_ip`

## 📊 Splunk SOC Dashboard
A real-time dashboard was created to monitor ongoing authentication failures and visualize the source of the attacks.

<img width="960" height="504" alt="ssh brute-force monitor" src="https://github.com/user-attachments/assets/73ddd039-a2c1-4e83-bf2c-c7c3fe2c89fe" />


## 💡 Key Learnings
* Configuring live data ingestion in Splunk.
* Parsing unstructured log data and writing custom SPL queries.
* Field extraction using Regular Expressions.
* Building Real-Time SOC Dashboards for threat hunting.
