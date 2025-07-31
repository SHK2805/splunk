### SPL
- This folder containes SPL (Splunk Search Processing Language)
- **Splunk SPL (Search Processing Language)** is the powerhouse behind Splunk’s data exploration and analysis. 
- It’s a flexible, command-driven language designed to sift through massive datasets and extract meaningful insights.

### What SPL Is
- **Purpose**: Used to search, filter, transform, and visualize data in Splunk.
- **Structure**: Commands are chained using pipes (`|`), similar to UNIX shell scripting.
- **Versions**: SPL and SPL2 (SPL2 is newer and more structured, but SPL is still widely used).

### Core Components
- **Search Terms**: Keywords or field-value pairs (e.g., `status=404`).
- **Commands**: Actions like `stats`, `eval`, `table`, `top`, `sort`.
- **Functions**: Calculations like `avg()`, `sum()`, `count()`.
- **Clauses**: Modifiers like `BY`, `AS`, `WHERE`.

### Example SPL Query
```spl
index=web_logs status=404 | stats count BY uri | sort -count
```
- This search:
    - Looks for 404 errors in `web_logs`
    - Counts how many times each URI triggered a 404
    - Sorts the results by count in descending order

### 📚 Useful Resources
- [Splunk SPL Syntax Guide](https://docs.splunk.com/Documentation/Splunk/9.4.2/SearchReference/UnderstandingSPLsyntax)
- [Splunk Cheat Sheet for SPL & Regex](https://www.splunk.com/en_us/blog/learn/splunk-cheat-sheet-query-spl-regex-commands.html)
- [Splexicon: SPL Glossary](https://docs.splunk.com/Splexicon:SPL)

### Dataset
- [botsv3](https://github.com/splunk/botsv3)


### index
- In Splunk, an **index** is essentially a **repository where your data gets stored and organized** after it's ingested. Think of it as a specialized filing system for logs, events, and metrics that Splunk can search through efficiently.

##### What Happens When Data Is Indexed
- Splunk takes **raw machine data** (e.g. logs, metrics, alerts) and breaks it into **events**.
- These events are stored in **indexes**, which are made up of:
  - **Raw data files** (compressed)
  - **Index files** (called `tsidx`) that help Splunk search quickly
  - **Metadata** about the events

##### Why Use Multiple Indexes?
- **Access control**: You can restrict who sees what data by assigning roles to specific indexes.
- **Retention policies**: Set different data lifespans for different indexes (e.g. keep security logs for 5 years, app logs for 1).
- **Performance tuning**: Separate high-volume data (like VPC Flow Logs) from low-volume data (like IAM changes).
- **Search optimization**: Narrowing your search to a specific index speeds things up.

##### Example
```spl
index=security_logs sourcetype=aws:cloudtrail | stats count BY eventName
```
- This searches only the `security_logs` index for AWS CloudTrail events.


### search vs where
##### `search` Command
- **Purpose**: Filters events based on keyword matches or field-value pairs.
- **Behavior**: Works like a **free-text filter** or **field match**, similar to the initial search bar.
- **Efficiency**: Faster and less resource-intensive because it uses indexed fields and raw event text.

#### ✅ Example:
```spl
index=botsv3 | search src_ip="172.16.0.178"
```
This finds events where `src_ip` equals that value—simple and fast.

##### `where` Command
- **Purpose**: Evaluates **logical expressions** and supports **field-to-field comparisons**.
- **Behavior**: Works like a conditional filter after fields are extracted.
- **Efficiency**: Slightly slower, as it operates post-indexing and requires field extraction.

##### Example:
```spl
index=botsv3 | where bytes_in > bytes_out
```
This compares two fields—something `search` can’t do.

---

##### Key Differences

| Feature               | `search`                          | `where`                             |
|----------------------|-----------------------------------|-------------------------------------|
| Field-to-field logic | ❌ Not supported                  | ✅ Supported                        |
| Performance          | ⚡ Faster                         | 🐢 Slightly slower                  |
| Use case             | Keyword/field match               | Conditional logic & comparisons     |
| Case sensitivity     | ❌ Case-insensitive by default     | ✅ Case-sensitive unless handled    |

---

##### When to Use What
- Use `search` for **quick filtering** or **indexed field matches**.
- Use `where` for **complex logic**, **numeric comparisons**, or **field relationships**.


### eval
- **eval** Command in Splunk lets you create new fields, transform existing ones, and perform calculations or logic using expressions.
- It calculates values using mathematical, string, or boolean expressions.
- It Creates new fields or overwrites existing ones.
- it Supports functions like if(), len(), match(), round(), and even cryptographic ones like md5().
- In fraud detection, eval is your go-to for:
    - Scoring risk based on multiple signals.
    - Flagging anomalies like mismatched device profiles.
    - Creating derived fields for dashboards (e.g., eval fraud_ratio=fraud_count/total_count).