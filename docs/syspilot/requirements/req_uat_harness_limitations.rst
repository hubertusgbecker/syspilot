Harness Limitations Test Data
=============================

Test data requirements for ``SYSP_US_UAT_HARNESS_LIMITATIONS``.


.. req:: UAT Test Data: Harness Disclosed Limitations
  :id: SYSP_REQ_UAT_HARNESS_LIMITATIONS
  :status: approved
  :priority: mandatory
  :tags: uat, harness, limitations, disclosure, test-data
  :links: SYSP_US_UAT_HARNESS_LIMITATIONS

   **Required Evidence:**

   * The five limitation entries in ``SYSP_SPEC_HARNESS_LIMITATIONS``.
   * A generated OpenCode installation; generated Claude Code and Qoder
     installations are optional experimental fixtures.
   * The installed production harness version and its current native
     agent/Skill UI or execution transcript; optional Claude Code or Qoder
     observations record the exact version and test date but are not required
     for production acceptance.
   * A Manager with a two-Engineer source allowlist, two Skills sharing the
     same orchestration ``group``, and a deterministic one-hop Qoder
     Manager/Engineer pair.

   **Reference Checks:**

   * Claude Code autocomplete/direct mention behavior for the Installer and
     Bootloader.
   * Claude Code generated main-thread and nested-spawning Manager
     frontmatter, mapped Workflow bindings, and Agent-tool semantics.
   * OpenCode Skill loading plus the Installer's mutually exclusive Skill
     placement result.
   * Qoder generated Agents, Skills, Rules paths, and one-hop transcript.

   **Preconditions:**

   * Reset the relevant harness fixture before every scenario.
   * Generate all artifacts from the same source revision.
   * Preserve screenshots or text transcripts for positive UI/runtime
     observations and complete path manifests for absence checks.
