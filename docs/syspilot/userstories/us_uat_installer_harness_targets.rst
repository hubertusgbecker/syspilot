Installer Harness Targets UAT
=============================

User Acceptance Test Story for the ``harness-interop`` change request's
Installer implementation — each production invocation explicitly selects VS
Code GitHub Copilot or OpenCode and writes only that harness's adapted files.
Claude Code and Qoder remain experimental adapter fixtures.


.. story:: UAT: Installer Harness Targets
   :id: SYSP_US_UAT_INSTALLER_HARNESS_TARGETS
   :status: approved
   :priority: mandatory
   :tags: uat, installer, harness, regression, critical
   :links: SYSP_US_HARNESS_PORTABILITY, SYSP_US_HARNESS_INSTALL, SYSP_US_INSTALLER

   **As a** syspilot Test Designer,
   **I want** self-contained test scenarios that let a human verify the
   Installer writes correctly adapted agent/Skill files to the explicitly
   selected harness's native directory and leaves unselected targets untouched,
   **so that** ``SYSP_SPEC_INSTALLER_HARNESS_TARGETS`` and the frontmatter
   adapter specs are confirmed correctly implemented before this CR reaches
   ``development``.

   **Context:**

   ``SYSP_SPEC_INSTALLER_HARNESS_TARGETS``, ``SYSP_SPEC_HARNESS_AGENT_ADAPTER``,
   and ``SYSP_SPEC_HARNESS_SKILL_ADAPTER`` were designed and are now
   implemented by the deterministic runtime. This chain exercises each
   harness in a separate run.

   **Artifacts Under Test:**

   * ``syspilot/agents/syspilot.installer.agent.md`` — Step 4
   * ``docs/syspilot/design/spec_installer.rst`` —
     ``SYSP_SPEC_INSTALLER_HARNESS_TARGETS``
   * ``docs/syspilot/design/spec_harness_adapters.rst`` —
     ``SYSP_SPEC_HARNESS_TARGET_MATRIX``, ``SYSP_SPEC_HARNESS_AGENT_ADAPTER``,
     ``SYSP_SPEC_HARNESS_SKILL_ADAPTER``, ``SYSP_SPEC_HARNESS_PROMPT_ADAPTER``
   * Isolated target project fixtures for explicit ``vscode``, ``opencode``,
     ``claude``, and experimental ``qoder`` adapter checks

   **Traceability:**

   Covers ``SYSP_REQ_HARNESS_NATIVE_INSTALL``,
   ``SYSP_REQ_HARNESS_PORTABILITY_NO_REGRESSION``,
   ``SYSP_REQ_HARNESS_INSTALL_NO_REGRESSION``,
   ``SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE``,
   ``SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE``.
   Test data is defined in ``SYSP_REQ_UAT_INSTALLER_HARNESS_TARGETS``;
   expected outcomes in ``SYSP_SPEC_UAT_INSTALLER_HARNESS_TARGETS``.

   **Acceptance Criteria:**

   1. **VS Code-only project — no regression.**
      *Precondition:* A target project with an existing syspilot
      installation.
      *Action:* Run the Installer update with ``--harness vscode``.
      *Expected result:* Output under ``.github/`` is identical to a run of
      the Installer before this CR — no new top-level harness directory is
      created anywhere in the project — traces to
      ``SYSP_REQ_HARNESS_PORTABILITY_NO_REGRESSION``,
      ``SYSP_REQ_HARNESS_INSTALL_NO_REGRESSION``.

   2. **Claude Code experimental adapter — adapted files generated.**
      *Precondition:* An isolated deterministic adapter fixture.
      *Action:* Generate Claude Code output without treating it as a
      production CLI acceptance run.
      *Expected result:* ``.claude/agents/*.md`` and
      ``.claude/skills/<name>/SKILL.md`` exist for every product agent and
      Skill; Skill bodies remain byte-for-byte identical and agent bodies
      differ only at authorized structural binding locations; every agent has
      a valid unique ``syspilot-<role>`` native ``name``; frontmatter omits
      ``user-invocable``, source ``agents``, Jarvis ``agent``, and ``version`` —
      traces to ``SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE``,
      ``SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE``.

   3. **OpenCode selected — command files and native Skill frontmatter
      generated.**
      *Precondition:* A clean target Git repository.
      *Action:* Run a fresh install with ``--harness opencode``.
      *Expected result:* ``.opencode/agents/*.md``, ``.opencode/skills/<name>/SKILL.md``,
      and ``.opencode/commands/<name>.md`` (one per Manager-style agent) all
      exist; installed Skill frontmatter omits source-only ``group``/``tools``/
      ``triggers`` keys while preserving body bytes — traces to
      ``SYSP_REQ_HARNESS_NATIVE_INSTALL``, ``SYSP_SPEC_HARNESS_SKILL_ADAPTER``.

   4. **Qoder experimental adapter — no production claim.**
      *Precondition:* An isolated deterministic adapter fixture.
      *Action:* Generate Qoder output as an experimental test target.
      *Expected result:* ``.qoder/agents/<name>.md`` and
      ``.qoder/skills/<name>/SKILL.md`` exist; no command or prompt file of
      any kind is written under ``.qoder/`` — traces to
      ``SYSP_SPEC_HARNESS_PROMPT_ADAPTER``.

   5. **Unselected harnesses remain untouched.**
      *Precondition:* A clean target project.
      *Action:* Run a fresh install with ``--harness vscode``.
      *Expected result:* Only ``.github/`` plus shared runtime/docs are
      populated; ``.claude/``, ``.opencode/``, and ``.qoder/`` are not created.

   6. **Orphan cleanup and run summary apply to the selected harness
      directories.**
      *Precondition:* A target project with a prior syspilot install that
      included ``.claude/`` output, where one previously-installed product
      agent has since been retired from upstream.
      *Action:* Run the deterministic Claude adapter update fixture.
      *Expected result:* The retired agent's file is removed from
      ``.claude/agents/``; ``.github/`` is untouched; the run summary table
      includes the selected Claude directories with correct counts — traces to
      ``SYSP_SPEC_INSTALLER_HARNESS_TARGETS`` Step 4 (Report).
