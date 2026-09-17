Harness Target Matrix Test Data
===============================

Test data requirements for ``SYSP_US_UAT_HARNESS_TARGET_MATRIX``.


.. req:: UAT Test Data: Harness Target Matrix
   :id: SYSP_REQ_UAT_HARNESS_TARGET_MATRIX
   :status: approved
   :priority: mandatory
   :tags: uat, harness, matrix, portability, test-data
   :links: SYSP_US_UAT_HARNESS_TARGET_MATRIX

   **Description:**

   The following fixtures and references SHALL be available to the human
   tester.

   .. list-table:: Target Fixtures
      :header-rows: 1
      :widths: 25 40 35

      * - Fixture
        - Initial State
        - Scenarios
      * - ``F-MATRIX-VSCODE``
        - Clean project with ``.github/`` as its harness signal
        - TC-HTM-DIRS, TC-HTM-PROMPTS, TC-HTM-REGRESSION
      * - ``F-MATRIX-CLAUDE``
        - Experimental static adapter fixture with an empty ``.claude/``
          directory
        - TC-HTM-DIRS, TC-HTM-PROMPTS
      * - ``F-MATRIX-OPENCODE``
        - Clean project with an empty ``.opencode/`` directory
        - TC-HTM-DIRS, TC-HTM-PROMPTS, TC-HTM-NATIVE
      * - ``F-MATRIX-QODER``
        - Experimental static adapter fixture with an empty ``.qoder/``
          directory
        - TC-HTM-DIRS, TC-HTM-PROMPTS

   **Reference Data:**

   * The complete directory and distribution table in
     ``SYSP_SPEC_HARNESS_TARGET_MATRIX``
   * A pre-change VS Code installation manifest listing every path under
     ``.github/``
   * One representative Manager agent, Engineer agent, Skill, and Manager
     prompt from the syspilot source distribution

   **Tooling and Preconditions:**

   * VS Code and OpenCode are installed and can open their matching fixture.
     Claude Code and Qoder are optional until their pending live UAT runs.
   * The current syspilot installer or equivalent implemented installation
     entry point is available.
   * The tester can list project files, compare manifests, and inspect each
     harness's loaded agents, Skills, and commands.
   * Each fixture is reset to its stated initial state before its scenario.
