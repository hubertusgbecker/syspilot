Harness Skill Adapter Test Data
===============================

Test data requirements for ``SYSP_US_UAT_HARNESS_SKILL_ADAPTER``.


.. req:: UAT Test Data: Harness Skill Frontmatter Adapter
   :id: SYSP_REQ_UAT_HARNESS_SKILL_ADAPTER
   :status: approved
   :priority: mandatory
   :tags: uat, harness, skill, frontmatter, test-data
   :links: SYSP_US_UAT_HARNESS_SKILL_ADAPTER

   **Required Artifacts:**

   * One source Skill that declares ``group``, ``tools``, and ``triggers``
     and one source Skill that declares only required metadata.
   * Generated copies under the VS Code and OpenCode native Skill directories;
     Claude Code and Qoder generated copies are optional experimental fixtures.
   * A YAML parser, byte-level body comparison tool, and installed OpenCode
     harness; installed Claude Code and Qoder harnesses are optional until
     their independent production acceptance gates complete.
   * Installed production-harness fixtures containing shared
     ``.syspilot/templates/change-document.md``, shared non-native Skill
     resources below ``.syspilot/skills/``, and no unselected harness tree.

   **Reference Data:**

   * The source directory name, complete YAML mapping, and Instructions/Rules
     body for each selected Skill.
   * OpenCode's recognized fields: ``name``, ``description``, ``license``,
     ``compatibility``, and ``metadata``.
   * The expectation that Installer mutual exclusion reads ``group`` before
     adaptation and placement; adapted native frontmatter omits ``group``,
     ``tools``, and ``triggers``, and no target harness is expected to enforce
     ``group`` itself.

   **Preconditions:**

   * Generate all copies from the same source revision.
   * Reset each harness project and preserve each source directory name.
   * Split frontmatter from the Markdown body before byte comparison.
   * Invoke change-launcher and impact path resolution from OpenCode without a
     ``.github`` directory and record every resolved shared resource path.
