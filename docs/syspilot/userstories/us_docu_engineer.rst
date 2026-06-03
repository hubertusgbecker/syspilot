Documentation Engineer Agent
=============================


.. story:: Documentation Engineer Agent
   :id: SYSP_US_DOCU
   :status: draft
   :priority: mandatory
   :tags: agent-v2, engineer, docu, documentation-engineer
   :links: SYSP_US_AGENT_ARCH

   **As a** syspilot user,
   **I want** my agentic managers to have a Documentation Engineer agent (syspilot.docu) that keeps
   the copilot-instructions, README, and other project documentation in sync with the codebase,
   **so that** all documentation reflects the current state of the project at all times.

   **Context:**

   The Documentation Engineer updates two categories of documentation:

   * **Internal docs**: ``copilot-instructions.md`` and context files read by AI agents
   * **External docs**: README, methodology, architecture, workflows, naming conventions

   The boundary between this story and ``SYSP_US_DOC_EXTERNAL`` / ``SYSP_US_DOC_INTERNAL``
   is intentional: those stories define *quality requirements for the artifacts themselves*
   (what the content must cover). This story defines the *agent responsible for updating them*
   (who performs the work and when).

   **Acceptance Criteria:**

   1. Given codebase changes, When the Documentation Engineer runs, Then it identifies doc gaps
   2. Given internal docs, When updating, Then copilot-instructions.md reflects current project state
   3. Given external docs, When updating, Then README and methodology are current
   4. Given redundant documentation, When detected, Then it is removed or consolidated
