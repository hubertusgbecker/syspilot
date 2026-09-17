Harness-Native Install
======================

Installing and updating syspilot through each harness's own mechanism.


.. story:: Harness-Native Installation
   :id: SYSP_US_HARNESS_INSTALL
   :status: approved
   :priority: mandatory
   :tags: agent-v2, harness, installer
   :links: SYSP_US_SETUP, SYSP_US_INSTALLER

   **As a** syspilot user on any supported harness,
   **I want to** install and update syspilot through that harness's own
   native install/extension mechanism,
   **so that** I never have to hand-edit personal or global configuration
   files to use syspilot.

   **Context:**

   Every supported harness has its own native surface for registering
   agents, commands, or skills (for example: VS Code custom-agent files
   read by Copilot Chat). Installation is run separately for each harness
   used in a repository and targets that harness's native project files.
   The production matrix for this change is VS Code GitHub Copilot and
   OpenCode. Claude Code and Qoder remain experimental until each completes
   clean-install, repeat-update, live native invocation, and rollback UAT.

   **Acceptance Criteria:**

   1. Given I am on a production-supported harness, When I run one remote
      PEP 723 ``uv run --no-project`` installer command from the project root
      with an explicit harness, Then syspilot's complete files for only that
      harness plus the shared deterministic runtime and documentation become
      available without pre-placing Setup or editing personal/global
      configuration by hand
   2. Given a new version of syspilot is published upstream, When I re-run the
      same deterministic command or invoke the installed Setup entry point,
      Then I get the selected upstream behavior, consistent with
      ``SYSP_US_SETUP``
   3. Given a supported harness whose native mechanism requires a minimal one-time host-level enablement step (e.g. enabling an extension) that cannot be avoided, When that step exists, Then it is a documented, one-time, harness-standard action — never a hand-edited configuration file
   4. Given an existing VS Code Copilot Chat installation, When this CR is
      implemented, Then its installation and update flow and its native files
      continue to work without regression
   5. Given I use more than one supported harness in a repository, When I
      install syspilot, Then I run the explicit install command separately for
      each harness I use and each run updates only its selected harness target
      plus shared runtime and documentation files
   6. Given a target contains unrelated content or filesystem links, When an
      installation succeeds or rolls back, Then all writes remain confined to
      declared mutable paths inside that target and unrelated or concurrent
      project content is preserved
