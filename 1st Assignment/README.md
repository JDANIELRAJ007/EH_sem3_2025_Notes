# 🛰️ Assignment 17: Bash Script for Auto Ping and Logging  
**Author:** Daniel Raj J  
**Course:** EH Semester 3 - 2025  

---

## 📌 Objective

To develop a Bash script that automatically pings a domain every 5 minutes and logs the response time with timestamps into a CSV file. This script can be used to monitor network health and detect possible downtime or latency issues.

---

## 🧪 Methodology: How the Task Was Approached

- Chose `google.com` as the target for reliable uptime.
- Wrote a Bash script that:
  - Pings the domain once every 5 minutes using `ping -c 1`.
  - Extracts response time using `grep` and `awk`.
  - Adds a timestamp with `date`.
  - Appends the result into a CSV log file.
- Used `sleep 300` to repeat the operation every 5 minutes.

---

## 🖼️ Screenshots

### Terminal Running the Script

![WhatsApp Image 2025-07-30 at 21 26 38_f4cc9126](https://github.com/user-attachments/assets/47c64d7b-591f-4c0c-97b2-6e865fe38921)


## 🔍 Findings

- The average ping time to `google.com` was around 24–26 ms.
- Occasionally, the server would not respond, which was captured as `"timeout"`.
- The CSV log allows us to visualize connection reliability over time.

---

## 🧠 Conclusion: What Was Learned

- Bash scripting is effective for quick automation tasks like monitoring.
- Output can be used for trend analysis, alerts, or historical tracking.
- This setup can be extended to multiple servers, domains, or IoT devices.

---

## 🧾 Code: All Scripts and Configuration

### `auto_ping_log.sh`

```bash
#!/bin/bash

DOMAIN="google.com"
OUTPUT="ping_log.csv"

if [ ! -f "$OUTPUT" ]; then
    echo "Timestamp,Response Time (ms)" > "$OUTPUT"
fi

while true; do
    TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
    RESPONSE=$(ping -c 1 $DOMAIN | grep 'time=' | awk -F'time=' '{print $2}' | awk '{print $1}')
    
    if [ -z "$RESPONSE" ]; then
        RESPONSE="timeout"
    fi

    echo "$TIMESTAMP,$RESPONSE" >> "$OUTPUT"
    sleep 300
done
