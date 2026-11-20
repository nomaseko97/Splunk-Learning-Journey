# My Splunk Learning Journey 🔍

## Introduction

I recently started learning **Splunk Enterprise** as part of my journey to develop practical skills in IT monitoring, log analysis, cloud computing, and cybersecurity.

Before starting, I had no experience working with Splunk. My goal was to understand how organisations collect, search, and analyse machine-generated data to monitor activities, identify problems, and make informed decisions.

Through hands-on practice, I explored the Splunk interface, uploaded log data, wrote searches using **Search Processing Language (SPL)**, and analysed different patterns in user login activities.

This repository documents my practical learning experience, the SPL queries I used, the challenges I encountered, and the knowledge I gained.

---

## Getting Started with Splunk

I began by installing **Splunk Enterprise** on my Windows computer and exploring the Search & Reporting application.

To practise, I used a CSV dataset named `splunk_practice_login_logs.csv`, containing 120 fictional banking application login events.

The dataset included the following fields:

- `timestamp` — Date and time of each login attempt
- `user` — Username
- `action` — Activity performed
- `status` — Whether the login succeeded or failed
- `src_ip` — Source IP address
- `application` — Application associated with the event
- `environment` — Environment where the activity occurred
- `message` — Description of the event

I uploaded the dataset into the `main` index using the `csv` sourcetype.

During this process, I learned about indexes, sources, sourcetypes, hosts, timestamps, and events.

### Challenges I Encountered

My first challenge was that Splunk initially displayed an empty data preview.

After investigating the warning messages, I discovered that Splunk had paused indexing because my computer had insufficient available disk space.

I also encountered another issue where Splunk displayed only 90 events instead of the expected 120. I discovered that my search time range was set to **Last 24 hours**. Changing it to **All time** allowed me to retrieve all 120 events.

These challenges helped me understand the importance of troubleshooting, checking system resources, and selecting appropriate time ranges when searching logs.

---

## Exploring and Searching Log Data

After successfully uploading the data, I started learning SPL (Search Processing Language).

I learned that SPL allows me to search, filter, organise, and analyse events stored in Splunk.

### 1. Retrieving All Events

My first search was to retrieve all the records from the uploaded dataset.

```spl
index="main" source="splunk_practice_login_logs.csv"
```

**Result:** 120 events.

This search helped me understand how indexes and sources work together to locate specific data in Splunk.

### 2. Investigating Successful Logins

Next, I filtered the dataset to identify successful login attempts.

```spl
index="main" source="splunk_practice_login_logs.csv" status="success"
| table timestamp user action status src_ip application environment message
```

**Result:** 66 successful login events.

I used the `table` command to organise the results into selected columns, making the information easier to read and analyse.

### 3. Investigating Failed Logins

I then searched for unsuccessful login attempts.

```spl
index="main" source="splunk_practice_login_logs.csv" status="failure"
| table timestamp user action status src_ip application environment message
```

**Result:** 54 failed login events.

This exercise showed me how Splunk can help identify unsuccessful login activity, which may be useful when investigating authentication problems or potential security concerns.

### 4. Searching with Wildcards and Removing Duplicates

I wanted to identify usernames beginning with the letter T without displaying repeated usernames.

```spl
index="main" source="splunk_practice_login_logs.csv" user="t*"
| dedup user
| table user
```

**Result:**

- thabo
- tumelo

Through this exercise, I learned how wildcards (`*`) help match values and how the `dedup` command removes duplicate results based on a selected field.

---

## Analysing Data Using SPL Commands

After learning how to search and filter events, I explored commands that help summarise data and identify patterns.

### 5. Identifying the Most Frequent Users

```spl
index="main" source="splunk_practice_login_logs.csv"
| top user
```

I used the `top` command to identify usernames appearing most frequently in the login events.

**Key finding:** Lerato appeared in 25 events, representing approximately 20.83% of the total 120 events.

This exercise helped me understand that `top` calculates the frequency and percentage of values within the search results.

### 6. Identifying the Least Frequent Users

```spl
index="main" source="splunk_practice_login_logs.csv"
| rare user
```

I used the `rare` command to identify usernames appearing least frequently.

**Key finding:** Anele, Nomsa, and Sipho each appeared in 13 events.

This helped me understand that several values can share the same lowest frequency, meaning there may not always be one uniquely rare value.

I also learned that rare activity does not automatically indicate suspicious behaviour.

### 7. Counting Events by Login Status

```spl
index="main" source="splunk_practice_login_logs.csv"
| stats count by status
```

This search grouped the login events according to their status.

**Results:**

| Login Status | Number of Events |
|---|---:|
| Successful | 66 |
| Failed | 54 |
| **Total** | **120** |

This exercise helped me understand how `stats count by` summarises events according to a specific field.

### 8. Counting and Sorting Events by Source IP

```spl
index="main" source="splunk_practice_login_logs.csv"
| stats count by src_ip
| sort - count
```

I used the `stats` command to count events associated with each IP address and the `sort` command to arrange the results from highest to lowest.

**Key finding:** The IP address `203.0.113.30` appeared most frequently, with 29 events.

I learned that multiple events associated with one IP address do not necessarily represent different users because several login attempts or usernames may share the same IP address.

---

## Skills and Knowledge Gained

Through these practical exercises, I developed a better understanding of:

- Installing and navigating Splunk Enterprise
- Uploading and indexing log files
- Understanding hosts, indexes, sources, sourcetypes, and events
- Searching data using SPL
- Filtering events using field values and keywords
- Using wildcards to search for matching values
- Removing duplicate results using `dedup`
- Displaying selected fields using `table`
- Identifying frequent values using `top`
- Identifying less frequent values using `rare`
- Summarising events using `stats count by`
- Sorting results using `sort`
- Understanding event counts and percentages
- Troubleshooting indexing and search time-range issues

---

## Key Takeaways

One of my biggest lessons was understanding that **collecting data is only the beginning**. The real value comes from being able to search, organise, analyse, and interpret that data.

I also learned that different SPL commands can produce similar-looking results but serve different purposes. For example, `top` and `stats count by` can both show event frequencies, while `top` also calculates percentages automatically.

Another important lesson was understanding that results need context. A username appearing frequently or an IP address occurring multiple times does not automatically mean something suspicious is happening.

These exercises helped me build confidence in working with Splunk and strengthened my understanding of log analysis and monitoring.

---

## Next Steps

As I continue my Splunk learning journey, I plan to explore:

- Custom field extraction
- Working with different log formats
- Timestamp and event-boundary configuration
- Saved searches and reports
- Dashboard creation using Splunk Dashboard Studio
- Monitoring and alerting
- More advanced log analysis and security use cases

## Conclusion

Starting Splunk with no previous experience has been a valuable learning experience.

What initially seemed complicated is becoming easier to understand through practical exercises, troubleshooting, and consistent learning.

I'm looking forward to building on these foundations and applying Splunk in real-world cloud computing, IT operations, and cybersecurity environments.

**This repository will continue to document my progress as I develop my practical Splunk skills.**

---

*Tools used: Splunk Enterprise, Search Processing Language (SPL), Windows, CSV practice logs, and GitHub.*
