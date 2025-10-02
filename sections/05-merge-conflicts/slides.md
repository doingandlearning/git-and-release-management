# Section 5: Dealing with Merge Conflicts

---

## Learning Objectives
- <span class="fragment">Deliberately create and resolve merge conflicts step-by-step</span>  
- <span class="fragment">Learn strategies to prevent conflicts before they occur</span>  
- <span class="fragment">Build resilience for when collaboration inevitably causes overlaps</span>  

<!-- Introduce this section as both a practical skill (hands-on) and a mindset (don’t panic). -->

---

## What Are Merge Conflicts?

**Definition**
- <span class="fragment">When Git cannot automatically merge changes because the same lines were modified differently</span>  
- <span class="fragment">Conflict markers show the problem inside files</span>  
- <span class="fragment">Resolution = manually choosing what to keep</span>  

---

**Why Conflicts Happen**
- <span class="fragment">Parallel development on same files</span>  
- <span class="fragment">Same lines changed in different ways</span>  
- <span class="fragment">Git can’t decide automatically</span>  
- <span class="fragment">Human judgment required</span>  

<!-- Stress: conflicts are not errors, they are Git asking for human input. -->

---

## Understanding Conflict Markers

```text
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name
````

* <span class="fragment">`<<<<<<< HEAD`: Start of your changes</span>
* <span class="fragment">`=======`: Separator</span>
* <span class="fragment">`>>>>>>> branch-name`: End of incoming changes</span>

---

**Example**

```python
def calculate_total(items):
<<<<<<< HEAD
    return sum(item.price for item in items) * 1.1  # 10% tax
=======
    return sum(item.price for item in items) * 1.15  # 15% tax
>>>>>>> feature/tax-update
```

<!-- Demo this live: create two branches, edit the same line, merge. -->

---

## Types of Merge Conflicts

* <span class="fragment">**Content Conflicts**: same lines, different edits</span>
* <span class="fragment">**File Conflicts**: deleted vs modified, renamed differently</span>
* <span class="fragment">**Binary Conflicts**: non-text files, must pick one</span>

<!-- Emphasise binary conflicts can’t be merged line-by-line. -->

---

## Conflict Resolution Strategies

* <span class="fragment">**Accept Current Changes**</span>
* <span class="fragment">**Accept Incoming Changes**</span>
* <span class="fragment">**Manual Resolution**</span>

---

## Step-by-Step Conflict Resolution

**Step 1: Identify**

```bash
git status
git diff
```

---

**Step 2: Open the File**

```bash
code filename.py
```

---

**Step 3: Resolve**

* <span class="fragment">Find markers</span>
* <span class="fragment">Understand both changes</span>
* <span class="fragment">Remove markers after deciding</span>

---

**Step 4: Stage**

```bash
git add filename.py
```

---

**Step 5: Commit**

```bash
git commit
```

<!-- Walk participants through this slowly in a live demo. -->

---

## Prevention Strategies

**Communication**

* <span class="fragment">Coordinate work, use feature branches, pull often</span>

---

**Workflow**

* <span class="fragment">Pull before push</span>
* <span class="fragment">Prefer rebase for smaller conflicts</span>
* <span class="fragment">Use `.gitignore` wisely</span>

---

**Code Organisation**

* <span class="fragment">Modular design</span>
* <span class="fragment">Avoid editing same files</span>
* <span class="fragment">Clear interfaces</span>

<!-- Prevention = fewer conflicts. Highlight human & process factors. -->

---

## Advanced Conflict Resolution

**Rebase**

```bash
git checkout feature
git rebase main
git rebase --continue
git rebase --abort
```

---

## Common Conflict Scenarios

* <span class="fragment">**Same Function, Different Implementation**</span>
* <span class="fragment">**Different Variable Names**</span>
* <span class="fragment">**File Deletion Conflict**</span>

<!-- Use these scenarios to spark discussion: “What would you do?” -->

---

## Troubleshooting

* <span class="fragment">**Unmerged paths** → `git add resolved-files && git commit`</span>
* <span class="fragment">**Binary conflicts** → `git checkout --ours file` or `--theirs`</span>
* <span class="fragment">**Too many conflicts** → `git merge --abort`</span>

<!-- Emphasise: you can always abort and retry. -->

---

## Best Practices

**Before**

* <span class="fragment">Read both changes</span>
* <span class="fragment">Communicate with authors</span>
* <span class="fragment">Backup via branch</span>

---

**During**

* <span class="fragment">Don’t rush</span>
* <span class="fragment">Test frequently</span>
* <span class="fragment">Document reasoning</span>

---

**After**

* <span class="fragment">Review & test thoroughly</span>
* <span class="fragment">Commit clearly</span>
* <span class="fragment">Share with team</span>

<!-- Position conflict resolution as a team responsibility, not just individual. -->

---

## AI Assistant Integration

* <span class="fragment">Ask AI to explain conflict markers</span>
* <span class="fragment">Ask AI to suggest safe resolution steps</span>
* <span class="fragment">Ask AI for command references</span>

**Prompt Examples**

* “Explain these conflict markers”
* “Safest way to resolve this conflict?”
* “How do I abort a rebase with conflicts?”

<!-- Stress AI can help explain, but decisions remain human. -->

---

## Team Collaboration

* <span class="fragment">Notify team about conflicts</span>
* <span class="fragment">Discuss tricky merges</span>
* <span class="fragment">Review resolutions</span>
* <span class="fragment">Document patterns</span>

<!-- Merge conflicts are opportunities to improve collaboration. -->

---

## Key Takeaways

* <span class="fragment">Conflicts are normal in collaboration</span>
* <span class="fragment">Understand before resolving</span>
* <span class="fragment">Test after resolution</span>
* <span class="fragment">Communicate with team</span>
* <span class="fragment">Prevention beats resolution</span>
* <span class="fragment">Tools and AI assist, humans decide</span>
* <span class="fragment">Practice builds resilience</span>

<!-- End on reassurance: “Don’t fear conflicts, master them.” -->
