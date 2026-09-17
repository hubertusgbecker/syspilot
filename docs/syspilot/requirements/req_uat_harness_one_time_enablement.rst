Harness One-Time Enablement Test Data
=====================================

Test data requirements for ``SYSP_US_UAT_HARNESS_ONE_TIME_ENABLEMENT``.


.. req:: UAT Test Data: Harness One-Time Enablement
   :id: SYSP_REQ_UAT_HARNESS_ONE_TIME_ENABLEMENT
   :status: approved
   :priority: mandatory
   :tags: uat, harness, bootstrap, update, test-data
   :links: SYSP_US_UAT_HARNESS_ONE_TIME_ENABLEMENT

   **Fixtures:**

   * Two clean production Git projects for VS Code GitHub Copilot and
     OpenCode, with no pre-existing syspilot files or harness directory.
   * Separate Claude Code and Qoder experimental fixtures for static adapter
     checks; neither fixture counts as production acceptance.
   * A reachable raw URL for ``syspilot/installer.py`` at upstream revision
     ``R1``.
   * Upstream revision ``R1`` and a later revision ``R2`` with one
     unmistakable, non-behavioral marker change in a product agent body.

   **Tools and Records:**

   * Installed copies of VS Code GitHub Copilot and OpenCode. Claude CLI
     authentication remains an external prerequisite for its pending live UAT.
   * A checksum manifest of any existing personal/global configuration files
     for each harness, captured before every scenario.
   * A generated-file manifest captured after bootstrap and after update.
   * A process-command record for Setup and Installer, including child process
     executable and arguments, plus an operating-system temporary-directory
     snapshot for ``syspilot-checkpoints``.

   **Preconditions:**

   * Reset the matching clean fixture before each clean-install scenario.
   * For TC-HOTE-UPDATE, install both production fixtures from ``R1`` first, then
     make ``R2`` the current upstream revision.
   * Do not preinstall any other syspilot agent or Skill in a clean fixture.
   * ``uv`` and Git are available. No virtual environment is activated, no
     Python packages are installed globally for the fixture, and direct
     ``python``, ``python3``, ``pip``, ``pip3``, and ``sphinx-build`` command
     shims fail the scenario if invoked.
   * Configure injected failures at each post-checkpoint phase, including
     after commit creation but before checkpoint deletion, while preserving an
     uninjected success path.
