Harness Target Matrix Expected Outcomes
=======================================

Expected outcomes for ``SYSP_REQ_UAT_HARNESS_TARGET_MATRIX``.


.. spec:: UAT Expected Outcomes: Harness Target Matrix
   :id: SYSP_SPEC_UAT_HARNESS_TARGET_MATRIX
   :status: approved
   :priority: mandatory
   :tags: uat, harness, matrix, portability, expected-outcomes
   :links: SYSP_REQ_UAT_HARNESS_TARGET_MATRIX

   **TC-HTM-DIRS — Project-scoped agent and Skill destinations**

    *Precondition:* Reset the VS Code and OpenCode fixtures. Reset each optional
    Claude Code or Qoder experimental fixture only when available.

   *Action:* Run the syspilot installation entry point once per production
   fixture and the deterministic adapter once per experimental fixture, then
   list the generated agent and Skill files.

   *Expected result:*

   * [ ] VS Code agents are under ``.github/agents/*.agent.md`` and Skills
     under ``.github/skills/<name>/SKILL.md``.
   * [ ] When the optional Claude Code fixture is available, agents are under
     ``.claude/agents/*.md`` and Skills under
     ``.claude/skills/<name>/SKILL.md``; otherwise the check is not executed.
   * [ ] OpenCode agents are under ``.opencode/agents/*.md`` and Skills under
     ``.opencode/skills/<name>/SKILL.md``.
   * [ ] When the optional Qoder fixture is available, agents are under
     ``.qoder/agents/*.md`` and Skills under
     ``.qoder/skills/<name>/SKILL.md``; otherwise the check is not executed.
   * [ ] No syspilot artifact is written to another harness's destination.

   *Traces to:* ``SYSP_US_UAT_HARNESS_TARGET_MATRIX`` AC-1

   ---

   **TC-HTM-PROMPTS — Prompt and command destinations**

    *Precondition:* The production fixtures contain the completed TC-HTM-DIRS
    installation; optional experimental fixtures are included only when
    available.

   *Action:* List prompt or command artifacts in each fixture and compare
   them with the source Manager prompts.

   *Expected result:*

   * [ ] VS Code has ``.github/prompts/*.prompt.md`` for source prompts.
   * [ ] OpenCode has ``.opencode/commands/<name>.md`` for source Manager
     prompts.
   * [ ] Each available optional Claude Code or Qoder fixture has no separate
     syspilot prompt or command artifact; unavailable fixtures remain not
     executed and do not block production acceptance.

   *Traces to:* ``SYSP_US_UAT_HARNESS_TARGET_MATRIX`` AC-2

   ---

   **TC-HTM-NATIVE — Native copied artifacts load without config edits**

   *Precondition:* Reset ``F-MATRIX-OPENCODE`` and record checksums of personal
   and global OpenCode configuration files, if present.

   *Action:* Install syspilot, restart or reload OpenCode as its normal
   discovery flow requires, and open its agent/Skill picker.

   *Expected result:*

   * [ ] OpenCode lists the representative syspilot agent and Skill from
     its project-scoped destination.
   * [ ] Recorded personal/global configuration checksums are unchanged.
   * [ ] The tester performed no manual personal/global configuration edit.

   *Traces to:* ``SYSP_US_UAT_HARNESS_TARGET_MATRIX`` AC-3

   ---

   **TC-HTM-REGRESSION — VS Code target remains unchanged**

   *Precondition:* Reset ``F-MATRIX-VSCODE`` and make the pre-change VS Code
   installation manifest available.

   *Action:* Install syspilot and compare all generated ``.github/`` paths
   with the reference manifest.

   *Expected result:*

   * [ ] The generated path set exactly matches the reference manifest.
   * [ ] Agent filenames retain ``*.agent.md`` and prompt filenames retain
     ``*.prompt.md``.
   * [ ] No non-VS Code harness directory is created.

   *Traces to:* ``SYSP_US_UAT_HARNESS_TARGET_MATRIX`` AC-4

   ---

   **Testability Note:**

   TC-HTM-NATIVE requires access to each actual harness. Static file
   inspection can verify Claude Code and Qoder paths but cannot establish that
   either harness loads them; their absent live evidence is reported as not
   executed and keeps both experimental.
