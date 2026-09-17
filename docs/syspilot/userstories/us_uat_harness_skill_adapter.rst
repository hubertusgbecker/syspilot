Harness Skill Adapter UAT
=========================

Manual acceptance scenarios for ``SYSP_SPEC_HARNESS_SKILL_ADAPTER``.


.. story:: UAT: Harness Skill Frontmatter Adapter
   :id: SYSP_US_UAT_HARNESS_SKILL_ADAPTER
   :status: approved
   :priority: mandatory
   :tags: uat, harness, skill, frontmatter, portability
   :links: SYSP_US_HARNESS_PORTABILITY

   **As a** syspilot Test Designer,
   **I want** a human tester to compare adapted Skills with their source,
   **so that** Instructions and Rules remain single-source and harmless
   syspilot metadata does not prevent native Skill discovery.

   **Traceability:**

   Covers ``SYSP_SPEC_HARNESS_SKILL_ADAPTER``,
   ``SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE``, and
   ``SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE``. Test data is defined in
   ``SYSP_REQ_UAT_HARNESS_SKILL_ADAPTER``; expected outcomes are defined in
   ``SYSP_SPEC_UAT_HARNESS_SKILL_ADAPTER``.

   **Acceptance Criteria:**

   1. Adapted Skill bodies are byte-for-byte identical to their source.
   2. ``description`` remains available for native automatic discovery and
      directory names remain the manual invocation names.
   3. Native frontmatter omits unsupported ``group``, ``tools``, and
      ``triggers`` fields, while source ``group`` metadata still drives
      Installer mutual exclusion before adaptation.
   4. Representative Skills load and can be manually invoked in OpenCode
      without frontmatter errors; equivalent Claude Code and Qoder checks use
      optional experimental fixtures and do not block production UAT.
