Harness Prompt Adapter UAT
==========================

Manual acceptance scenarios for ``SYSP_SPEC_HARNESS_PROMPT_ADAPTER``.


.. story:: UAT: Harness Prompt and Command Adapter
   :id: SYSP_US_UAT_HARNESS_PROMPT_ADAPTER
   :status: approved
   :priority: mandatory
   :tags: uat, harness, prompt, command, portability
   :links: SYSP_US_HARNESS_PORTABILITY

   **As a** syspilot Test Designer,
   **I want** a human tester to verify each harness's invocation surface,
   **so that** prompt files are generated only where a native prompt or
   command format exists.

   **Traceability:**

   Covers ``SYSP_SPEC_HARNESS_PROMPT_ADAPTER`` and
   ``SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE`` and
   ``SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE``. Test data is defined in
   ``SYSP_REQ_UAT_HARNESS_PROMPT_ADAPTER``; expected outcomes are defined in
   ``SYSP_SPEC_UAT_HARNESS_PROMPT_ADAPTER``.

   **Acceptance Criteria:**

   1. VS Code prompt files and ``agent:`` routing remain unchanged, and a
      representative request invokes the expected Manager behavior.
   2. OpenCode receives a thin native command for each source Manager prompt,
      and the same representative request invokes equivalent Manager behavior.
   3. When the optional Claude Code experimental fixture is available, it
      receives no separate prompt file and invokes the Manager through its
      native agent or Skill surface; absence does not block production UAT.
   4. When the optional Qoder experimental fixture is available, it receives
      no separate prompt file and invokes the Manager through its native
      Custom Agent surface; absence does not block production UAT.
