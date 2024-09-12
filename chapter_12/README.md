# Chapter 12: Building Towards a Proactive Secure Future

## Ingesting STIX2

There are a lot of feeds for STIX2 that are publicly available. You can find a list in this Reddit post: [https://www.reddit.com/r/cybersecurity/comments/q38qvz/looking_for_free_stixtaxii_threat_intelligence/](https://www.reddit.com/r/cybersecurity/comments/q38qvz/looking_for_free_stixtaxii_threat_intelligence/).

For our example we'll use one from The Computer Incident Response Center Luxembourg (CIRCL). The feed can be found [here](https://www.circl.lu/doc/misp/feed-osint/). For example, have a look at [this](https://www.circl.lu/doc/misp/feed-osint/0fadc113-6e22-4524-96b1-7b8fc98fa64c.json) feed.

You can copy the JSON data in ChatGPT and ask it to explain it to you.
```
Explain the STIX2 signature below:
[Signature pasted here]
```

Here's the response:
```
This JSON object represents a STIX (Structured Threat Information eXpression) report within a MISP (Malware Information Sharing Platform) format. It provides structured information about an OSINT (Open Source Intelligence) event, detailing a specific attack involving BazarBackdoor, Ryuk ransomware, and Cobalt Strike. Here's a breakdown:
Event Metadata:

    Analysis Level (2): Indicates the level of confidence or assessment regarding the event (usually ranges from 0 to 3 in MISP).
    Date (2020-11-09): Date of the event report.
    Info: A brief description of the event, mentioning how the BazarBackdoor was used to deploy Ryuk ransomware, via Cobalt Strike.
    Threat Level ID (1): This likely maps to a scale indicating the severity or priority of the threat. In many cases, "1" represents a high-level threat.
    Orgc (CIRCL): CIRCL (Computer Incident Response Center Luxembourg) is the organization that reported this event.
    Tags:
        misp-galaxy
        ="Ryuk ransomware": This tag identifies Ryuk ransomware as part of the event.
        misp-galaxy
        ="Cobalt Strike": This tag indicates the use of Cobalt Strike, a known post-exploitation tool.
        type:OSINT: The event is based on open-source intelligence.
        osint
        ="perpetual": The information is valid indefinitely.
        osint
        ="50": Represents a 50% certainty about the provided information.
        tlp:white: Traffic Light Protocol tag specifying that the information is public and can be shared freely.

Attributes (Indicators of Compromise - IoCs):

    Domains (under Network Activity category): The listed domains (e.g., check1domains.com, sweetmonsterr.com) are associated with malicious activity related to the BazarBackdoor or Ryuk ransomware.
    Link (under External Analysis category): A reference to an external analysis on the attack: Advanced Intel.

Key Aspects:

    Purpose: The report helps cybersecurity professionals and organizations recognize malicious domains related to BazarBackdoor and the Ryuk ransomware attack vector.
    Indicators to Block: The domains mentioned should be considered for blocking or further investigation in network defense systems.

This signature aids in detecting and preventing attacks that may exploit similar infrastructure or tactics.
```

From there you have a choice to turn this into a Sigma rule.
```
Turn this into a Sigma rule yml
```

Here's the response:
```yaml
title: Detection of Malicious Domains Related to BazarBackdoor and Ryuk Ransomware
id: 0fadc113-6e22-4524-96b1-7b8fc98fa64c
description: |
  Detect network communication to domains associated with BazarBackdoor and Ryuk ransomware, as identified in OSINT report from CIRCL.
author: CIRCL
date: 2020-11-09
status: experimental
logsource:
  category: network
  product: any
detection:
  selection:
    dns_query:
      - 'check1domains.com'
      - 'sweetmonsterr.com'
      - 'qascker.com'
      - 'remotessa.com'
      - 'havemosts.com'
      - 'unlockwsa.com'
      - 'sobcase.com'
      - 'zhameharden.com'
      - 'mixunderax.com'
      - 'bugsbunnyy.com'
      - 'fastbloodhunter.com'
      - 'serviceboosterr.com'
      - 'servicewikii.com'
      - 'secondlivve.com'
      - 'luckyhunterrs.com'
      - 'wodemayaa.com'
      - 'hybriqdjs.com'
      - 'gunsdrag.com'
      - 'gungameon.com'
      - 'servicemount.com'
      - 'servicesupdater.com'
      - 'service-boosterr.com'
      - 'serviceupdatter.com'
      - 'dotmaingame.com'
  condition: selection
fields:
  - dns_query
  - src_ip
  - dest_ip
  - timestamp
level: high
tags:
  - attack.t1071.001
  - attack.t1071.004
  - ransomware.ryuk
  - cobaltstrike
  - osint
references:
  - https://www.advanced-intel.com/post/anatomy-of-attack-inside-bazarbackdoor-to-ryuk-ransomware-one-group-via-cobalt-strike
  - https://www.circl.lu/
```

You can save it and then import it into Opensearch Dashboards.

## Creating custom alerts

1. Go to OpenSearch Dashboards > Alerting.
2. Click Create Monitor and define a new monitor for DNS queries.
- Set Name: Malicious Domain DNS Query.
- Choose a query type: "Per query monitor".
- Use "Extraction query editor." As an alternative you can use the "Visual editor".
- Write a query to match DNS queries from your logs, e.g.,:
```json
{
  "query": {
    "bool": {
      "must": [
        { "match": { "dns_query": "check1domains.com" } },
        { "match": { "dns_query": "sweetmonsterr.com" } }
      ]
    }
  }
}

```
- Set Trigger Condition: Create a trigger to fire when the query matches.
4. Save the monitor.

## Alert correlation

You know how you can filter records based on a bunch of criteria? Well what if you could filter on the basis of two or more records in order to get an alert. In other words, have criteria that apply across multiple records but within a time window. If I see SSH connections for an IP and also attempts to connect to port 80 (HTTP) within 5 minutes, I want an alert. This can be a useful alert in case someone is probbing for example your router (from within usually).

1. Go to OpenSearch Dashboards > Alerting.
2. Click Correlations
3. Click Create correlation rule.
4. Name it: SSH and HTTP connection attempts.
5. Set "index" for "Data source 1" as logstash (you can use the alias for this, just click on any), and log type as "network".
6. Then the field is `dest_port` with value 22, and `dest_ip` with value `192.168.0.1`.
7. Set "index" for "Data source 2" as logstash (you can use the alias for this, just click on any), and log type as "network".
8. Then the field is `dest_port` with value 80, and `dest_ip` with value `
9. Then, add alert trigger and give it a name.
10. Click on "Create correlation rule".

To find out if it works, attempt to connect to the IP via http and even be explict about the port `http://192.168.0.1:80`. Then open (Putty)[https://www.putty.org/] or terminal if you are on Linux or Mac, and attempt to connect to 192.168.0.16 via SSH.

```bash
ssh 192.168.0.1
```

It doesn't matter if the IP has these services available. The NetFlow record will be created and the alert will be triggered.

Give it a minute and then you should see in the Overview under Security Analytics a new alert.

## Anomaly detection

While looking at time series to detect anomalies is useful, automating the process is even better.

1. Go to OpenSearch Dashboards > Anomaly Detection.
2. Click Create detector.
3. Give it a name ("AnomalyBytes") and description: "Anomaly detection for total bytes".
4. Set the index pattern to "logstash-*" or just the alias "logstash".
5. Add filter: `event_type.keyword:flow`.
6. Set the time field to `@timestamp`.
7. Click "Next".
8. Set feature name as: Bytes.
9. Set the field to `flow.total_bytes`.
10. Click "Next" and create the detector.

From there the detector will initialize. You have also the option to hit the "Historical Analysis" tab to have it run against the historical data to find anomalies. Notice that you have not setup any alerts for the anomaly detector. This means that you can see for anomalies in the dashboard but won't be notified by them. To be notified you have to setup alerts.

## Setting up alerts

1. Go to OpenSearch Dashboards > Notifications.
2. Click on "Channels" and then "Create Channel".
3. Name the channel. Then, choose the type of channel you want to create, e.g., "Email".

TO BE CONTINUED...