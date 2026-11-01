# SBT-DF205 Mobile and IoT Forensics – Android Forensic Investigation

## Case Reference

**SBT-DF205-C1**

## Student

**Athanasius Alekwe**
**Student ID:** 2025/FWSD/11230

## Course

**SBT-DF205 – Mobile and IoT Forensics**

## Case Overview

This repository documents the forensic examination of an Android 10 file-system extraction from a **Google Pixel 3**.

The investigation focused on preserving the supplied evidence, verifying its integrity, identifying device and user information, recovering communication artefacts, examining application data, reviewing location and photograph metadata, and correlating findings across multiple Android sources.

The analysis was performed using **Kali Linux** in a VMware virtual machine.

The original forensic evidence archive is **not included in this repository** because of its size and to maintain proper evidence-handling practice.

---

## Investigation Objectives

The examination was carried out to:

* Verify the integrity of the supplied Android evidence.
* Identify the device manufacturer, model, Android version, build and security patch.
* Identify Android user profiles and associated accounts.
* Examine relevant system settings.
* Review available Wi-Fi and Bluetooth configuration artefacts.
* Recover and analyse SMS and MMS communications.
* Examine contacts, call logs and calendar data.
* Compare AOSP communication records with Google Messages and Google Dialer.
* Examine WhatsApp and TextNow artefacts.
* Review Google Maps location information.
* Examine photograph metadata and GPS information.
* Build a UTC-normalised forensic timeline.
* Correlate findings across independent artefact sources.
* Assess whether the available evidence supports allegations of unauthorised communication or information sharing.

---

## Evidence Handling

The original evidence archive was preserved separately from the working copy.

Original evidence:

```text
Pixel 3.zip
```

SHA-256:

```text
ca6918ef8b20486b6a5ded15609ac51318f377829480f93be3ba15364a8aa00a
```

A separate working copy was created and independently hashed.

The original and working copies produced identical SHA-256 values, confirming that the working evidence was an exact copy before extraction and examination.

The archive was also tested before extraction and returned:

```text
No errors detected in compressed data
```

All forensic analysis was performed against the extracted working copy rather than the preserved original archive.

---

## Device Identification

The recovered Android build properties identified the device as:

| Property        | Value           |
| --------------- | --------------- |
| Manufacturer    | Google          |
| Model           | Pixel 3         |
| Android Version | Android 10      |
| Build ID        | QQ1A.200105.003 |
| Security Patch  | 2020-01-01      |

Two Android profiles were recovered:

* **User 0 – "This Is"**
* **User 11 – "User 2"**

Associated Google accounts included:

```text
User 0  -> thisisdfir@gmail.com
User 11 -> thisisdfirtwo@gmail.com
```

The account associations were also corroborated using Android sync and application artefacts.

---

## Key Findings

### Android Settings

Relevant stored settings included:

```text
adb_enabled = 1
development_settings_enabled = 1
wifi_on = 1
bluetooth_on = 0
airplane_mode_on = 0
location_mode = 3
location_providers_allowed = gps,network
```

Wi-Fi configuration files were present, but no saved network entries were recovered.

Bluetooth configuration and database artefacts were also present, although the available records did not provide sufficient evidence to prove a connection at a specific time.

---

## SMS and MMS Evidence

Two `mmssms.db` databases were identified.

The main Telephony Provider database contained:

```text
24 SMS records
7 MMS/PDU records
10 MMS part records
```

The separate `user_de` database contained:

```text
0 SMS records
0 PDU records
0 PART records
```

Two recovered sent SMS records included:

```text
2020-02-09 18:41:42 UTC
"I am on my way to the coffee shop. Do you want anything?"
```

and:

```text
2020-02-14 13:09:03 UTC
"Wow, the messages sync over between accounts."
```

A recovered MMS contained the text:

```text
"Here's Signal."
```

---

## Contacts and Calls

Contact artefacts associated the number:

```text
9195790479
```

and its formatting variants with:

```text
Josh Hickman
```

Multiple call records involving the same contact were recovered from both the Android call log and Google Dialer.

This provided cross-source corroboration between:

* Contacts
* AOSP Call Log
* Google Dialer

---

## Google Messages

Google Messages artefacts were examined from:

```text
bugle_db
```

Several messages matched records recovered from the Android Telephony Provider by message text, participant and timestamp.

Examples included:

```text
"I am on my way to the coffee shop. Do you want anything?"
```

and:

```text
"Wow, the messages sync over between accounts."
```

These matching records were treated as duplicate representations of the same communication events rather than separate messages.

---

## Application Evidence

The examination included artefacts from:

* Google Messages
* Google Dialer
* WhatsApp
* TextNow
* Google Maps
* Google Photos

WhatsApp configuration identified the registered number and application account information.

TextNow preferences identified the account username and email address, although the documented message-query attempt was incomplete because the original query referenced `id` instead of the database's `_id` field.

This limitation was retained in the report rather than replacing the missing evidence with assumptions.

---

## Location Evidence

Google Maps artefacts contained stored coordinates including:

```text
35.734602, -78.636654
```

Additional synchronised coordinates were recovered from Maps databases.

These records were treated as stored application location information and not automatically interpreted as proof that a particular individual physically visited the locations.

---

## Photograph Evidence

Media files were examined using ExifTool and SHA-256 hashing.

A significant photograph was:

```text
IMG_20200213_195030.jpg
```

The recovered metadata included:

```text
Date/Time Original: 2020-02-13 19:50:31
Time-zone offset:   -05:00
GPS Latitude:       35° 0' 39.31" N
GPS Longitude:      78° 52' 27.18" W
```

The local timestamp converts to:

```text
2020-02-14 00:50:31 UTC
```

SHA-256:

```text
2ed43a0a7b01a58f89c23263542282d3d5713b5cd2f22d7b3d2f56a109145ea6
```

---

## Cross-Source Correlations

The investigation identified several important correlations.

### Telephony Provider and Google Messages

Matching message text and timestamps demonstrated that both databases represented the same communication events.

### Contacts, AOSP Call Log and Google Dialer

The same phone number was associated with Josh Hickman and appeared in both call databases with matching call times and durations.

### Android Accounts and Application Records

The Google account associated with Android User 0 was also identified in Android sync, Google Photos and TextNow artefacts.

---

## Conclusion

The examination confirmed that the Pixel 3 contained multiple user profiles, account associations, communications, calls, location artefacts and photograph metadata.

Several findings were independently corroborated across Android and application databases.

However, the available artefacts did not establish that any communication was unauthorised or that confidential company information was transferred to an unauthorised party.

The final investigative assessment was therefore:

```text
UNRESOLVED
```

The evidence demonstrates communication activity and account relationships, but additional organisational, cloud, server or recipient-side evidence would be required to make a stronger attribution or policy-violation finding.

---

## Repository Structure

```text
SBT-DF205-C1/
|
|-- README.md
|-- Report.pdf
|-- Evidence_Register.csv
|-- Timeline.csv
|-- Queries.sql
|-- Handling_Log.csv
|
`-- Supporting_Evidence/
    |-- evidence_integrity.png
    |-- device_identity.png
    |-- account_association.png
    |-- android_settings.png
    |-- telephony_database_comparison.png
    |-- sms_records.png
    |-- contacts_calls_calendar.png
    |-- google_messages.png
    |-- google_dialer.png
    |-- google_maps.png
    `-- photograph_metadata.png
```

---

## Tools Used

* Kali Linux
* VMware Workstation
* SQLite3
* SHA-256 utilities
* ExifTool
* xmllint
* Linux command-line utilities including:

  * `find`
  * `grep`
  * `stat`
  * `ls`
  * `unzip`
  * `sha256sum`
  * `date`

---

## Important Notes

The original Android evidence archive and full application databases are intentionally excluded from this repository.

Only the report, reproducible queries, evidence register, timeline, handling log and selected supporting screenshots should be published.

Sensitive authentication information, passwords, tokens and verification codes should not be included in the public repository.

---

## Disclaimer

This repository was created for academic digital-forensics training as part of **SBT-DF205 – Mobile and IoT Forensics**.

The findings are based solely on the supplied forensic dataset and should not be interpreted as allegations against any real person or organisation.
