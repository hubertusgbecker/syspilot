Harness Target Matrix UAT
=========================

Manual acceptance scenarios for ``SYSP_SPEC_HARNESS_TARGET_MATRIX``.


.. story:: UAT: Harness Target Matrix
   :id: SYSP_US_UAT_HARNESS_TARGET_MATRIX
   :status: approved
   :priority: mandatory
   :tags: uat, harness, matrix, portability
   :links: SYSP_US_HARNESS_PORTABILITY, SYSP_US_HARNESS_INSTALL

   **As a** syspilot Test Designer,
   **I want** a human tester to verify production destinations and static
   experimental adapter destinations,
   **so that** generated artifacts use native locations without changing the
   established VS Code installation.

   **Artifacts Under Test:**

   * The implemented harness-target selection derived from
     ``SYSP_SPEC_HARNESS_TARGET_MATRIX``
   * A syspilot source distribution containing agents, Skills, and prompts
   * Clean production fixtures for VS Code and OpenCode, plus static
     experimental fixtures for Claude Code and Qoder

   **Traceability:**

   Covers ``SYSP_SPEC_HARNESS_TARGET_MATRIX``,
   ``SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE``, and
   ``SYSP_REQ_HARNESS_NATIVE_INSTALL``. Test data is defined in
   ``SYSP_REQ_UAT_HARNESS_TARGET_MATRIX``; expected outcomes are defined in
   ``SYSP_SPEC_UAT_HARNESS_TARGET_MATRIX``.

   **Acceptance Criteria:**

   1. **Project-scoped agent and Skill destinations.** Install syspilot into
      each production fixture and run deterministic adaptation for each
      available experimental fixture; verify files are written only to the
      selected harness's documented project directories. Claude Code and
      Qoder fixtures are optional and do not block production acceptance.
   2. **Prompt/command destinations.** Verify VS Code receives prompt files,
      OpenCode receives command files, and Claude Code and Qoder receive no
      separate prompt/command files.
   3. **Native file-copy distribution.** Verify VS Code and OpenCode load the
      copied project artifacts without a manual edit to personal or global
      configuration. Record Claude Code and Qoder live loading as pending,
      not passed, until executed.
   4. **VS Code mapping unchanged.** Compare a VS Code-only installation with
      the pre-change reference manifest and verify its paths and file set are
      unchanged.
