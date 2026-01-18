### **THE DEPLOYMENT GATE**

**Status:** MANDATORY
**Severity:** SEV-1 PREVENTION

#### **1. COMPLEXITY AUDIT (The "ReDoS" Check)**

*Most outages aren't volume; they are complexity.*

* [ ] **No Unbounded Regex:** Is the regex linear-time safe? (Verify with a linter or migrate to `re2`).
* [ ] **The "Evil String" Test:** Have you fuzz-tested the endpoint with large payloads (1MB+) and special characters?
* [ ] **Loop Safety:** Confirm no nested loops rely on unfiltered user input for iteration counts.

#### **2. BLAST RADIUS CONTROL**

*If this breaks, what else dies?*

* [ ] **Sandboxed Logic:** Is this new logic isolated? (e.g., Does the WAF rule have a hard timeout like `cpu_limit: 10ms`?)
* [ ] **Feature Flagged:** Is the change wrapped in a flag that can be toggled **OFF** instantly without a code rollback?
* [ ] **Canary Release:** Are we routing 1% of traffic first? (Never hit 100% on Day 1).

#### **3. THE ESCAPE HATCH**

*When the phone screams at 3 AM, can you fix it in 30 seconds?*

* [ ] **Hard Kill Switch:** Is there a single config switch to disable this feature globally?
* [ ] **Sheddability:** If this causes latency, will the system automatically drop these requests to save the core business logic?

#### **4. OBSERVABILITY**

*You cannot fix what you cannot see.*

* [ ] **Latency Histograms:** Are p99 alerts configured specifically for this component?
* [ ] **Resource Saturation:** Will we get paged if CPU/Memory deviates >20% from baseline?

#### **5. THE "SENIOR ENGINEER" VETO**

* [ ] **Simplicity:** If you had to explain this logic to a Junior Engineer at 3 AM, would they understand it? If no, rewrite it.
* [ ] **Necessity:** Does this *need* to be a regex? Could it be a simple string match?

---

**FAILED A CHECK?**
**DO NOT DEPLOY.**

*Watch the full autopsy of why this matters:*
**[youtube.com/@SystemDesignAutopsy](https://www.youtube.com/watch?v=846QiREkloo)**