# Splunk Detection Queries

## Failed Logins
index=* EventCode=4625
| stats count by Account_Name, src_ip

## Successful Login
index=* EventCode=4624

## Brute Force Detection
index=* (EventCode=4624 OR EventCode=4625)
| stats count by EventCode, Account_Name, src_ip
