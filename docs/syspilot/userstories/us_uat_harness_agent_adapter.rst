Harness Agent Adapter UAT
=========================

Manual acceptance scenarios for ``SYSP_SPEC_HARNESS_AGENT_ADAPTER``.


.. story:: UAT: Harness Agent Frontmatter Adapter
   :id: SYSP_US_UAT_HARNESS_AGENT_ADAPTER
   :status: approved
   :priority: mandatory
   :tags: uat, harness, agent, frontmatter, portability
   :links: SYSP_US_HARNESS_PORTABILITY

   **As a** syspilot Test Designer,
   **I want** a human tester to compare adapted agent files with their source,
   **so that** only harness-specific structure changes while agent behavior
   remains single-source.

   **Artifacts Under Test:**

   * One generated Manager agent and one generated Engineer agent for each
     production harness; Claude Code and Qoder counterparts are optional
     experimental fixtures
   * Their corresponding source ``*.agent.md`` files
   * The field mapping in ``SYSP_SPEC_HARNESS_AGENT_ADAPTER``

   **Traceability:**

   Covers ``SYSP_SPEC_HARNESS_AGENT_ADAPTER``,
   ``SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE``, and
   ``SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE``. Test data is defined in
   ``SYSP_REQ_UAT_HARNESS_AGENT_ADAPTER``; expected outcomes are defined in
   ``SYSP_SPEC_UAT_HARNESS_AGENT_ADAPTER``.

   **Acceptance Criteria:**

   1. Adapted Markdown bodies are byte-for-byte identical to their source
      except for the explicitly permitted structural binding adaptation.
   2. The Claude Code fixture contains all 13 product agents with required,
      valid, unique native ``name`` values mapped from dotted source IDs;
      supported fields are retained and fields with no native equivalent are
      dropped.
   3. OpenCode frontmatter expresses primary/subagent mode and SEND
      permissions without changing workflow prose.
   4. When the optional Qoder fixture is available, frontmatter contains only
      documented mappings and does not invent equivalents for unsupported
      fields.
   5. Representative Manager and Engineer agents load in production harnesses
      and retain their specified guidance. Claude CLI validation and
      ``claude --agent syspilot-qm`` prove discovery reaches authentication or
      model execution; an external authentication failure is recorded
      separately from, and must not be mistaken for, agent-not-found. Claude
      remains experimental until authenticated invocation and delegation pass.
