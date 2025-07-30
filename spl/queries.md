## Simple queries

- ### search
- It searches the **`botsv3` index** for events that contain **any of the following keywords**:
    - `"error"`
    - `"failed"`
    - `"failure"`

- These keywords are grouped using the `OR` operator, which means the query will return events that include **at least one** of those terms in their raw log data.
- `index=botsv3`: Targets the `botsv3` index, often used in Splunk BOTS (Boss of the SOC) datasets for simulated security events.
- `("error" OR "failed" OR "failure")`: Filters events that mention **system issues, operation failures, or error messages**.
```spl
index=botsv3 ("error" OR "failed" OR "failure")
```