# 🐚 Windows PowerShell — Runbook

## 📝 Executive Summary
This runbook covers the transition from CMD to PowerShell. Unlike text-based CMD, PowerShell is an **object-oriented** task automation framework built on .NET. It allows for advanced data manipulation, filtering, and system administration through "cmdlets" that follow a consistent `Verb-Noun` syntax.

---

## 🌐 Core Concepts
* **Objects vs. Text:** PowerShell handles .NET objects (with properties/methods) rather than plain text, enabling precise data filtering and sorting.
* **Cmdlets:** Standardized commands using `Verb-Noun` naming (e.g., `Get-Service`, `Set-Location`).
* **Piping (`|`):** Passes entire objects between commands, allowing you to chain operations like filtering (`Where-Object`) and sorting (`Sort-Object`).
* **Cross-Platform:** PowerShell now supports Windows, Linux, and macOS.

---

## 🛠️ Practical Reference

### Cmdlet Discovery & Help
* **List Commands:** `Get-Command` (e.g., `Get-Command -Name Verb*`)
* **Get Help/Examples:** `Get-Help <Cmdlet> -examples`
* **Aliases:** `Get-Alias -Name <alias>` (e.g., `echo` is an alias for `Write-Output`)

### File & Directory Management
* **List Contents:** `Get-ChildItem` (like `ls` or `dir`)
* **Navigation:** `Set-Location <path>` (like `cd`)
* **Read Content:** `Get-Content <file>` (like `cat` or `type`)
* **Create/Remove:** `New-Item <name>`, `Remove-Item <name>`

### Data Manipulation (Piping)
* **Filter:** `Where-Object -Property <prop> -<operator> <value>`
* **Sort:** `Sort-Object -Property <prop>`
* **Operators:** `-eq` (equal), `-ne` (not equal), `-gt` (greater than), `-lt` (less than).
* **Hash Calculation:** `Get-FileHash -Path <file>`

### System & Network Analysis
* **Users:** `Get-LocalUser`
* **Processes:** `Get-Process`
* **Services:** `Get-Service`
* **Network Connections:** `Get-NetTCPConnection` (use `OwningProcess` to correlate with PID)
* **Remote Execution:** `Invoke-Command -ComputerName <Name> -ScriptBlock {<Command>}`

---

## 🛡️ Remediation & Best Practices
* **Use Verbose Syntax:** While aliases are great for quick CLI work, use full cmdlet names in scripts for readability and reliability.
* **Filter Early:** When piping, use `Where-Object` as early as possible in the pipeline to improve performance by reducing the number of objects passed.
* **Property Access:** Use `$_.Property` within script blocks (e.g., `Where-Object`) to reference the current object in the pipeline.

---

## 🧠 Key Takeaways
* **Object Power:** Because PowerShell returns objects, you don't need complex string parsing (grep/awk) to extract data; you can access properties directly.
* **Naming Convention:** The `Verb-Noun` structure makes it highly intuitive to guess commands for tasks you haven't performed before.
* **Scalability:** `Invoke-Command` makes PowerShell a critical tool for managing entire fleets of servers simultaneously rather than configuring them one by one.

---
**Task Status:** ✅ Completed