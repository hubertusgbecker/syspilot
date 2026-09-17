Harness Limitations UAT
=======================

Manual acceptance scenarios for ``SYSP_SPEC_HARNESS_LIMITATIONS``.


.. story:: UAT: Harness Disclosed Limitations
   :id: SYSP_US_UAT_HARNESS_LIMITATIONS
   :status: approved
   :priority: mandatory
   :tags: uat, harness, limitations, disclosure, portability
   :links: SYSP_US_HARNESS_PORTABILITY

   **As a** syspilot Test Designer,
   **I want** a human tester to verify every accepted harness limitation is
   accurate, visible, and isolated,
   **so that** unsupported behavior is never silently presented as parity and
   does not block supported behavior on other harnesses.

   **Traceability:**

   Covers ``SYSP_SPEC_HARNESS_LIMITATIONS`` and
   ``SYSP_REQ_HARNESS_LIMITATION_DISCLOSURE``. Test data is defined in
   ``SYSP_REQ_UAT_HARNESS_LIMITATIONS``; expected outcomes are defined in
   ``SYSP_SPEC_UAT_HARNESS_LIMITATIONS``.

   **Acceptance Criteria:**

   Claude Code and Qoder scenarios use optional experimental fixtures and do
   not block production acceptance; unavailable checks remain not executed.

   1. Claude Code's Installer/Bootloader direct-invocation limitation is
      disclosed and observable without affecting hidden-agent behavior in
      other harnesses.
   2. Claude Code main-thread Managers use explicit native Agent target
      restrictions, while nested-spawn behavior is disclosed without falsely
      claiming per-target enforcement for bare ``Agent``.
   3. OpenCode native Skill frontmatter omits unsupported ``group``, ``tools``,
      and ``triggers`` fields while source ``group`` metadata still drives
      Installer-side mutual exclusion.
   4. Qoder's undocumented nesting depth is disclosed and acceptance is
      limited to a flat Manager-to-Engineer hop.
   5. Qoder Rules are not shipped, and their Settings-UI registration gap is
      disclosed as future scope without affecting Agents or Skills.
