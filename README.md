# Jenkins Idle Job Report Script

This script generates a report of idle Jenkins jobs that have been inactive for a long period of time. The report is produced in **HTML format** for easy viewing and analysis.

---

## Overview

- **Purpose:** Identify Jenkins jobs that have been idle for extended durations.
- **Output:** HTML report listing these idle jobs.
- **Use Case:** Helps Jenkins administrators monitor and clean up unused or stale jobs to optimize system resources.

---

## Post-Build Script Approval

After running the script in Jenkins, the following methods and static methods must be approved manually in:


### Methods to approve:

| Class/Package                                   | Method              | Type          |
|------------------------------------------------|---------------------|---------------|
| `hudson.model.Item`                            | `getFullName`       | Instance      |
| `hudson.model.ItemGroup`                       | `getAllItems`       | Instance      |
| `hudson.model.Job`                             | `getLastBuild`      | Instance      |
| `hudson.model.Run`                             | `getTimeInMillis`   | Instance      |
| `jenkins.model.Jenkins`                        | `getRootUrl`        | Instance      |
| `jenkins.model.Jenkins`                        | `getInstanceOrNull` | Static        |
| `org.codehaus.groovy.runtime.DefaultGroovyMethods` | `round`             | Static        |
| `java.lang.Double`                             | (constructor/methods)| (used implicitly) |
| `org.codehaus.groovy.runtime.DefaultGroovyMethods` | `toDouble`          | Static        |
| `java.lang.Number`                             | (constructor/methods)| (used implicitly) |

---

## How to Approve Scripts in Jenkins

1. Navigate to **Manage Jenkins** from the Jenkins dashboard.
2. Click on **In-process Script Approval**.
3. Review the pending signatures.
4. Approve the required methods and signatures listed above.
5. Once approved, rerun the script to generate the report successfully.

---

## Notes

- Script approval is a security feature in Jenkins to prevent unauthorized or unsafe Groovy script execution.
- Ensure you understand the implications of approving these methods before proceeding.
- It is recommended to perform this operation under the guidance of your Jenkins administrator if you do not have sufficient privileges.

---


**Author:** JANATH KUMAR
