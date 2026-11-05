# Initial Security Investigation

## 1. Investigation Overview

**Organisation:** FinTrust Solutions (fictional)  
**Affected account:** j.smith  
**Date:** 15 October 2026  
**Investigation type:** Suspicious authentication activity  
**Status:** Under investigation

## 2. Initial Observations

Review of the supplied authentication logs identified:

- Ten consecutive failed login attempts against the j.smith account.
- The failed attempts occurred between 09:14:02 and 09:16:23.
- All ten failed attempts originated from the same unfamiliar IP address: 198.51.100.23.
- A successful login occurred at 09:17:01 from the same IP address.
- The recorded location for these events was Unknown.

## 3. Potential Security Concerns

The repeated failed attempts followed by a successful login may indicate:

- Password guessing or brute-force activity.
- Potential unauthorised access.
- Possible compromise of the employee account.

These are potential explanations and have not yet been confirmed.

## 4. Evidence Required

Further investigation should establish:

1. Whether the employee recognises the login activity.
2. Whether multi-factor authentication was completed.
3. Whether the successful login used a recognised device.
4. Whether any unusual activity occurred after authentication.
5. Whether additional accounts were targeted.

## 5. Recommended Initial Actions

- Escalate the alert for further investigation.
- Verify the activity with the account owner through an approved channel.
- Review available identity and authentication logs.
- If compromise is confirmed, follow the organisation's account containment and credential-reset procedures.

## 6. Preliminary Assessment

**Priority:** Requires investigation.

The authentication pattern is suspicious, but the available evidence is insufficient to confirm account compromise.

## 7. Investigation Status

Initial log review completed. Further evidence collection required.

## 8. Data Disclaimer

This investigation uses synthetic training data and fictional organisation details. It does not represent a real security incident.





## 9. Detailed Splunk Investigation

Splunk Enterprise was used to investigate the authentication events associated with the affected account and suspicious source IP address.

### 9.1 Failed Authentication Analysis

The following SPL query was used to identify failed authentication attempts:

```spl
index=soc_lab status=failed
| stats count by username source_ip
| sort - count
```

**Result:** The account `j.smith` recorded 10 failed authentication attempts associated with source IP `198.51.100.23`.

### 9.2 Source IP Investigation

The following query was used to review all authentication events originating from the suspicious IP:

```spl
index=soc_lab source_ip="198.51.100.23"
| table _time username source_ip status device location
| sort _time
```

**Result:** Eleven authentication events were identified:
- 10 failed login attempts.
- 1 successful login.
- All events involved the account `j.smith`.
- The recorded device was Windows 10.
- The recorded location was Unknown.

### 9.3 Successful Authentication Review

The successful authentication event was isolated using:

```spl
index=soc_lab source_ip="198.51.100.23" status=success
| table _time username source_ip status device location
```

**Result:** A successful login occurred at 09:17:01 on 15 October 2026, following the repeated failed attempts.

## 10. Authentication Event Timeline

| Time (15 October 2026) | Event |
|---|---|
| 09:14:02 | First failed login |
| 09:14:18–09:16:23 | Nine additional failed logins |
| 09:17:01 | Successful login from the same IP |

The failed attempts occurred over approximately 2 minutes and 21 seconds. A successful authentication followed 38 seconds after the final recorded failure.

## 11. Investigation Findings and Assessment

The investigation identified a suspicious authentication pattern involving repeated failed login attempts followed by a successful login from the same source IP address.

This behaviour is consistent with possible password-guessing or brute-force activity.

However, the available dataset does not establish:
- Whether the successful login was authorised.
- Whether the correct credentials were obtained through malicious activity.
- Whether the account was subsequently misused.
- The identity or physical location of the source.

The activity therefore remains suspicious but unconfirmed.

**Recommended escalation:** Refer the case for further investigation and verification with the account owner.


