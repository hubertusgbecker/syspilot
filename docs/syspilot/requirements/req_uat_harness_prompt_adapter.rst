Harness Prompt Adapter Test Data
================================

Test data requirements for ``SYSP_US_UAT_HARNESS_PROMPT_ADAPTER``.


.. req:: UAT Test Data: Harness Prompt and Command Adapter
  :id: SYSP_REQ_UAT_HARNESS_PROMPT_ADAPTER
  :status: approved
  :links: SYSP_US_UAT_HARNESS_PROMPT_ADAPTER

   **Required Artifacts:**

   * A source Manager prompt with a known ``description`` and
     ``agent: syspilot.<name>`` route.
   * Generated VS Code and OpenCode installations from the same source
     revision; generated Claude Code and Qoder fixtures are optional
     experimental evidence.
   * Installed copies of VS Code and OpenCode and a project file-listing tool;
     installed Claude Code and Qoder copies are optional until each completes
     its independent production acceptance gate.
   * One representative multi-token user request containing spaces and
     punctuation, plus a deterministic Manager receiver that records the exact
     request and observable outcome; use it unchanged for the paired VS Code
     and OpenCode native invocations.

   **Fixture Preparation:**

   .. list-table:: Prompt Fixtures
      :header-rows: 1
      :widths: 25 40 35

      * - Fixture
        - Initial State
        - Native Invocation
      * - ``F-PROMPT-VSCODE``
        - Empty ``.github/`` project harness directory
        - Prompt command routed to custom agent
      * - ``F-PROMPT-OPENCODE``
        - Empty ``.opencode/`` project harness directory
        - ``/<command-name>``
      * - ``F-PROMPT-CLAUDE``
        - Optional experimental empty ``.claude/`` project harness directory
        - ``@agent-name`` or installed Skill command
      * - ``F-PROMPT-QODER``
        - Optional experimental empty ``.qoder/`` project harness directory
        - Custom Agent invocation

   Each fixture SHALL be reset before its scenario, and the tester SHALL
    record the complete generated path list after installation. For
    ``F-PROMPT-VSCODE`` and ``F-PROMPT-OPENCODE``, the tester SHALL submit the
    same request text, record the selected Manager, and compare the observable
    workflow outcome and exact received request rather than treating matching
    routing metadata or fabricated metadata tuples as behavioral evidence.
