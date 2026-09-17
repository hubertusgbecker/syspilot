Harness Orchestration Adapter UAT
=================================

Manual acceptance scenarios for ``SYSP_SPEC_HARNESS_ORCHESTRATION_ADAPTER``.


.. story:: UAT: Harness Orchestration Fallback
   :id: SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER
   :status: approved
   :priority: mandatory
   :tags: uat, harness, orchestration, fallback, portability
   :links: SYSP_US_HARNESS_PORTABILITY, SYSP_US_SKILL_ORCHESTRATION

   **As a** syspilot Test Designer,
   **I want** a human tester to verify deterministic synchronous orchestration
   installation and one-hop SEND behavior,
   **so that** installation never depends on Jarvis inference or actor setup.

   **Traceability:**

   Covers ``SYSP_SPEC_HARNESS_ORCHESTRATION_ADAPTER`` and
   ``SYSP_REQ_SKILL_ORCHESTRATION_GROUP``. Test data is defined in
   ``SYSP_REQ_UAT_HARNESS_ORCHESTRATION_ADAPTER``; expected outcomes are
   defined in ``SYSP_SPEC_UAT_HARNESS_ORCHESTRATION_ADAPTER``.

   **Acceptance Criteria:**

   1. Every target installs the synchronous orchestration-group variant and no
      asynchronous variant, regardless of ``.jarvis/`` presence.
   2. OpenCode Managers complete a synchronous Manager-to-Engineer SEND and
      receive the Engineer result as production acceptance.
   3. Claude Code Manager allowlists and Workflow bindings resolve mapped
      native names. Claude remains experimental until authenticated live
      invocation and Manager-to-Engineer delegation pass; Qoder remains
      experimental until its live native invocation UAT is executed.
