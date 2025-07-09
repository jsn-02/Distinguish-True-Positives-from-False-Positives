# Distinguish True Positives from False Positives

## Scenario

As a cybersecurity analyst in a Security Operations Center (SOC), you are tasked with analyzing a surge of alerts during the holiday season. Your job is to determine whether the alerts represent **True Positives (TP)** or **False Positives (FP)** to ensure security threats are addressed while avoiding unnecessary responses to harmless activity.

A specific incident involves suspicious PowerShell activity flagged on December 1, 2024. Using Elastic SIEM, you will analyze and correlate data to classify the alert and mitigate potential risks.

## Objective

To demonstrate the ability to analyze alerts using a SIEM tool, investigate anomalies, and classify them as true or false positives based on event patterns and contextual data.

### Skills Learned

- Filtering and interpreting SIEM logs
- Identifying patterns of malicious activity
- Investigating login events and PowerShell execution

### Tools Used

<img src="https://img.shields.io/badge/-Elastic%20SIEM-005571?style=for-the-badge&logo=elastic&logoColor=white"/>

## Process Breakdown

### Step 1: Set Timeframe

I logged into the Elastic SIEM platform and configured the time window to focus on the incident timeframe (December 1, 2024, from 09:00 to 09:30). This narrowed down the log data to relevant events for analysis.

![Timeframe Settings](https://github.com/user-attachments/assets/9cfa9361-aacb-4eac-8c8e-5d36fcb72de4)


### Step 2: Add Relevant Fields

I customized the SIEM table by adding key fields:
- **host.hostname**: Identifies the affected machine.
- **user.name**: Tracks the user associated with the activity.
- **process.command_line**: Reveals executed PowerShell commands.
- **event.outcome**: Indicates whether the action was successful.

![Relevant Fields](https://github.com/user-attachments/assets/707ed6a5-aa6f-4ede-bd26-53a04bc8d4c3)



### Step 3: Examine Source IP and Authentication Events

I added **source.ip** to identify the origin of the activity and filtered for specific **event.category** values, particularly **authentication**. This revealed a successful login from a suspicious IP address followed by the execution of PowerShell commands.

![Authentication Events](https://github.com/user-attachments/assets/25998a52-6175-46ea-8c29-adac349d21ab)


### Step 4: Expand the Timeframe

To establish a pattern, I broadened the timeframe (November 29 to December 1). This revealed several failed login attempts from the same source IP `10.0.255.1`, followed by a successful login and subsequent changes to the credentials.

![Timeframe Expansion](https://github.com/user-attachments/assets/b5252620-60ea-4931-8655-78c43c1fc4e4)


### Step 5: Decode PowerShell Commands

Using the PowerShell decoding tool, <a href="https://gchq.github.io/CyberChef/">CyberChef</a>, I translated the encoded commands in the **process.command_line** field to plaintext. This allowed me to understand the intent behind the activity, confirming that the PowerShell commands were used to adjust the credentials for the machines that handled Windows updates.

![Decoded Commands](https://github.com/user-attachments/assets/4bc87627-6765-48af-be6e-e866f62d4e2f)



## Summary

The investigation revealed that the flagged PowerShell activity was not an attack but an effort to fix outdated credentials on systems handling Windows updates. The actions, associated with a successful login from 10.0.255.1, were performed by unauthorized user to address a brute-force attempt. The alert was therefore classified as a False Positive, clarifying that the activity, initially suspected as malicious, was actually a defensive measure.
