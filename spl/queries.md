## Simple queries

### search
- It searches the **`botsv3` index** for events that contain any of the following **keywords / words**:
    - `"error"`
    - `"failed"`
    - `"failure"`

- These keywords are grouped using the `OR` operator, which means the query will return events that include **at least one** of those terms in their raw log data.
- `index=botsv3`: Targets the `botsv3` index, often used in Splunk BOTS (Boss of the SOC) datasets for simulated security events.
- `("error" OR "failed" OR "failure")`: Filters events that mention **system issues, operation failures, or error messages**.
```spl
index=botsv3 ("error" OR "failed" OR "failure")
```
- Zeroing in on activity from a **specific source IP** by searching the events by ip address
- `index=botsv3`: This points to the `botsv3` index, commonly used in **Splunk's Boss of the SOC (BOTS)** datasets, which simulate a range of security scenarios.
- `src_ip="172.16.0.178"`: Filters for events where this IP appears in the `src_ip` field—meaning it's the **originating IP address** of a network connection or action.
```spl
index=botsv3 src_ip="172.16.0.178"
```
- Look for failed logins between 5 to 15 years
- EventCode is microsoft event code
```spl
index=botsv3 EventCode=4625 earliest=-15y latest=-5y
```
- Useing wild cards
```spl
index=botsv3 src_ip="172.16*"
index=botsv3 user="*admin*"
```
- High byte count indicating exfilteration or downloading large packets
```spl
index=botsv3 bytes>1000000
```
- Look for specific event codes
```spl
index=botsv3 EventCode IN (4624, 4625, 4634)
index=botsv3 NOT EventCode IN (4624, 4625, 4634)
```
- Unique values 
```spl
index=botsv3 | stats values(sourcetype) AS UniqueEvents
index=botsv3 | stats values(app)
```
- Look for http requests that hit the admin related urls
```spl
index=botsv3 sourcetype = stream:http uri_path="*admin*"
```
- Finds events in botsv3 index that contain the string amazon_aws anywhere
```spl
index=botsv3 amazon_aws
```
- Finds events in botsv3 index that contain the string amazon_aws anywhere with protocol tcp
```spl
index=botsv3 amazon_aws protocol="tcp"
```
- Filters events where the app is explicitly amazon_aws
```spl
index=botsv3 app=amazon_aws
```
- Find events with googlebot
```spl

```
-
```spl

```
-
```spl

```
-
```spl

```
-
```spl

```
