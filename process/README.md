# README

A step-by-step plan to parse **process.md** and push each step as an item into a GitHub Projects “Table” (Projects V2) using **GitHub CLI** (gh).

## 1. Automate Process Markdown to GitHub Projects

Markdown (`process.md`) defines **PHASE** headings, each with a set of tasks underneath. Each task must become a row (item) in a GitHub Project table, with a “Phase” column indicating which phase it came from.

## 2. Requirements

* Must Use **GitHub Projects V2**
* Project Table will have at least two fields:
  * **Title** (the task name)
  * **Phase** (single-select with options “PHASE 1”, …, “PHASE 6”)
* `gh` CLI v2+ installed and authenticated
* Python script to parse the MD and shell out to `gh`

## 3. Functionality

1. **Create** a new GitHub Project via CLI
2. **Define** a “Phase” single-select field and its options
3. **Write** a parser script that:
   * Reads `process.md`
   * Extracts each phase header and its bullet/numbered tasks
   * Calls `gh project item add …` for each task, setting the Phase field
4. **Run** the script

## 4. Execution

### A. Create the Project

```bash
# Replace <OWNER> with GitHub user/org
gh project create "Process Roadmap" \
  --owner <OWNER> \
  --public \
  --body "Imported from process.md" \
  --format json > project.json

# Grab its ID for future commands
PROJECT_ID=$(jq -r .id project.json)
echo "Project ID is $PROJECT_ID"
```

### B. Create the “Phase” Field & Options

```bash
# Create a single-select field called "Phase"
gh project field create \
  --project-id "$PROJECT_ID" \
  --name "Phase" \
  --type single_select \
  --format json > phase_field.json

FIELD_ID=$(jq -r .id phase_field.json)

# Add options for PHASE 1 through PHASE 6
for PH in "PHASE 1" "PHASE 2" "PHASE 3" "PHASE 4" "PHASE 5" "PHASE 6"; do
  gh project field option add \
    --project-id "$PROJECT_ID" \
    --field-id "$FIELD_ID" \
    --name "$PH"
done
```

### C. Parser + Import Script (`import_process.py`)

```python
#!/usr/bin/env python3
import re, subprocess, json

# 1. Read project ID from env
PROJECT_ID = subprocess.check_output([
    "gh", "project", "view", "Process Roadmap",
    "--json", "id", "-q", ".id"
]).strip().decode()

# 2. Read the process.md
with open("process.md", "r") as f:
    lines = f.readlines()

current_phase = None
for line in lines:
    # Detect phase headers: ## **PHASE 1: …**
    m = re.match(r"^## \*\*(PHASE \d+):", line)
    if m:
        current_phase = m.group(1)
        continue

    # Detect tasks: either numbered "**Task**" or bullets "* Task"
    # Adjust this regex if file has different list styles
    t = re.match(r"^\s*(?:\d+\.\s+\*\*(.+?)\*\*|[\*\-\+]\s+(.+))$", line)
    if t and current_phase:
        task = t.group(1) or t.group(2)
        task = task.strip()
        print(f"Adding: [{current_phase}] {task}")
        # Create the item with the Phase field set
        subprocess.run([
            "gh", "project", "item", "add",
            "--project-id", PROJECT_ID,
            "--title", task,
            "--field", f"Phase={current_phase}"
        ])
```

1. Make it executable: `chmod +x import_process.py`
2. Run it in the same folder as `process.md`:

   ```bash
   ./import_process.py
   ```

This will iterate through **PHASE 1**…**PHASE 6**, pick up each task, and call `gh project item add` to insert it into table with the correct “Phase” value.

___