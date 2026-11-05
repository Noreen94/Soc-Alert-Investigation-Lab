# SOC Alert Investigation Lab

## Project Overview

A hands-on Security Operations Centre (SOC) investigation project using Splunk Enterprise to analyse simulated authentication logs, identify suspicious login activity and document security investigation findings.

The project demonstrates practical SOC Level 1 skills, including log analysis, alert investigation, threat identification, evidence gathering and incident reporting.

## Scenario

A fictional organisation, **FinTrust Solutions**, recorded repeated failed authentication attempts against the employee account `j.smith`.

The investigation focused on identifying the source of the activity, reviewing authentication patterns and assessing whether the account may have been compromised.

**Incident details:**
- Date: 15 October 2026
- Affected account: `j.smith`
- Suspicious source IP: `198.51.100.23`
- Event type: Authentication activity
- Investigation status: Suspicious activity identified; compromise unconfirmed.

## Investigation Findings

Analysis of the authentication logs identified:

- 10 consecutive failed login attempts against `j.smith`.
- Failed attempts occurred between 09:14:02 and 09:16:23.
- All 10 failures originated from `198.51.100.23`.
- A successful login occurred at 09:17:01 from the same IP address.
- The recorded location for the suspicious activity was Unknown.

The pattern may indicate password guessing or brute-force activity. However, the available evidence does not establish whether the successful login was authorised or whether the account was compromised.

## Tools and Technologies

- Splunk Enterprise
- Search Processing Language (SPL)
- GitHub
- CSV authentication logs
- Markdown documentation

## Splunk Investigation Queries

### 1. Failed vs Successful Login Attempts

```spl
index=soc_lab source_ip="198.51.100.23"
| stats count AS attempts by status
| table status attempts
```

### 2. Failed Login Attempts by Username

```spl
index=soc_lab status=failed
| stats count AS failed_attempts by username
| sort - failed_attempts
```

### 3. Login Activity from Suspicious IP

```spl
index=soc_lab source_ip="198.51.100.23"
| table _time username source_ip status device location
| sort _time
```

### 4. Login Attempts Over Time

```spl
index=soc_lab
| timechart span=5m count by status
```

## Dashboard and Visual Evidence

The Splunk dashboard contains visualisations and tables showing authentication activity.

### Failed vs Successful Login Attempts

![Failed vs successful login attempts](screenshots/Failed%20vs%20successful%20login%20attempts.png)

### Failed Login Attempts by Username

![Failed login attempts by username](screenshots/failed%20login%20attempts%20by%20username.png)

### Login Activity from Suspicious IP

![Login activity from suspicious IP](screenshots/login%20activity%20from%20suspicious%20IP.png)

### Login Attempts Over Time

![Login attempts over time](screenshots/Login%20attempts%20overtime.gif)

## Investigation Report

The investigation report documents the initial observations, evidence reviewed, authentication timeline, potential security concerns, findings and recommended escalation.

[View the Initial Security Investigation](investigation/initial-analysis.md)

## Recommended Response

- Escalate the suspicious authentication activity for further investigation.
- Verify the login activity with the account owner through an approved channel.
- Review available identity and authentication logs.
- Check whether multi-factor authentication was completed.
- If compromise is confirmed, follow the organisation's account containment and credential-reset procedures.

## Key Learning Outcomes

This project demonstrates practical experience in:

- Searching and analysing authentication logs using SPL.
- Identifying repeated failed login attempts.
- Investigating suspicious source IP addresses.
- Comparing failed and successful authentication events.
- Building Splunk dashboards and visualisations.
- Documenting findings and recommending incident response actions.

## Disclaimer

This project uses synthetic training data and fictional organisation details. The IP addresses and user accounts are included for educational purposes. No real organisation or individual is represented.

---

**Author:** Noreen Marowa  
**Project:** SOC Alert Investigation Lab
