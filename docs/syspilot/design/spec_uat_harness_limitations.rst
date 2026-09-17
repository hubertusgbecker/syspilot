Harness Limitations Expected Outcomes
=====================================

Expected outcomes for ``SYSP_REQ_UAT_HARNESS_LIMITATIONS``.


.. spec:: UAT Expected Outcomes: Harness Disclosed Limitations
  :id: SYSP_SPEC_UAT_HARNESS_LIMITATIONS
  :status: approved
  :priority: mandatory
  :tags: uat, harness, limitations, disclosure, expected-outcomes
  :links: SYSP_REQ_UAT_HARNESS_LIMITATIONS

    Claude Code and Qoder scenarios below are optional experimental evidence.
    If either fixture is unavailable, record its scenarios as not executed;
    they do not block the mandatory OpenCode production scenario.

   **TC-HL-CLAUDE-DIRECT — Direct invocation cannot be hidden**

    *Precondition:* The optional Claude Code fixture is available; install the
    adapted Setup and Installer agents and record the harness version.

   *Action:* Search for or directly ``@``-mention each installed agent, then
   inspect the generated frontmatter and limitation text.

   *Expected result:*

   * [ ] Both agents can be directly addressed despite the Installer's
     workflow-level invocation guardrail.
   * [ ] No unsupported ``user-invocable`` substitute appears in generated
     Claude Code frontmatter.
   * [ ] ``SYSP_SPEC_HARNESS_LIMITATIONS`` explicitly discloses this gap and
     its naming/Soul-text mitigation.

   *Traces to:* ``SYSP_US_UAT_HARNESS_LIMITATIONS`` AC-1

   ---

  **TC-HL-CLAUDE-ALLOWLIST — Main-thread and nested SEND boundaries**

    *Precondition:* The optional Claude Code fixture is available; install the
    two-Engineer Manager fixture.

  *Action:* Inspect the main-thread Manager's adapted frontmatter and
  Workflow, compare every mapped target with generated native names, then
  inspect a nested-spawning subagent's Agent tool declaration.

   *Expected result:*

   * [ ] No source ``agents:`` field or invented field is present.
   * [ ] The main-thread Manager has an explicit
     ``Agent(syspilot-engineer-a, syspilot-engineer-b)`` tools entry and its
     structurally adapted Workflow names those same resolvable targets.
   * [ ] Bare ``Agent`` on a nested spawner is recognized as unrestricted and
     is not reported as an enforced per-target allowlist.
   * [ ] The limitation accurately identifies only the nested enforcement
     boundary.

   *Traces to:* ``SYSP_US_UAT_HARNESS_LIMITATIONS`` AC-2

   ---

    **TC-HL-OPENCODE-METADATA — Source metadata remains installation-effective**

   *Precondition:* Prepare two source orchestration Skills sharing one
   ``group`` and select one through the Installer in an OpenCode fixture.

   *Action:* Inspect installed frontmatter, open the Skill picker, and list
   installed orchestration-group directories.

   *Expected result:*

   * [ ] The selected Skill omits source ``group``, ``tools``, and
     ``triggers`` from native frontmatter and loads without a frontmatter error.
   * [ ] Exactly one orchestration-group Skill directory is installed,
     proving source ``group`` metadata drove mutual exclusion before adaptation.
   * [ ] The limitation states that unsupported keys are omitted without
     removing Installer-side semantics.

   *Traces to:* ``SYSP_US_UAT_HARNESS_LIMITATIONS`` AC-3

   ---

   **TC-HL-QODER-NESTING — Qoder coverage stops at one hop**

    *Precondition:* The optional Qoder fixture is available; install the
    deterministic Manager/Engineer pair and record the harness version.

   *Action:* Run the single-hop workflow and review the limitation text; do
   not require the Engineer to spawn another agent.

   *Expected result:*

   * [ ] The Manager completes one Engineer hop and returns its result.
   * [ ] No test result claims that nested Custom Agents are supported.
   * [ ] The limitation explicitly says nesting depth is undocumented and
     identifies the flat-hop boundary.

   *Traces to:* ``SYSP_US_UAT_HARNESS_LIMITATIONS`` AC-4

   ---

   **TC-HL-QODER-RULES — Rules remain disclosed future scope**

    *Precondition:* The optional Qoder fixture is available; install syspilot
    and capture a complete generated path manifest.

   *Action:* Search generated output for Qoder Rule artifacts, then verify
   installed Agents and Skills remain discoverable.

   *Expected result:*

   * [ ] No Qoder Rule artifact is generated or registered.
   * [ ] Syspilot Agents and Skills remain discoverable and invocable.
   * [ ] The limitation identifies Settings-UI registration as relevant only
     to a future Rules-distribution scope.

   *Traces to:* ``SYSP_US_UAT_HARNESS_LIMITATIONS`` AC-5

   ---

   **Testability Note:**

   Negative vendor-capability claims can change between harness versions.
   Every run must record the tested version and date. If a newer harness adds
   a native capability, the corresponding scenario is a design-review
   trigger, not an automatic UAT failure against obsolete assumptions.
