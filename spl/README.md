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
