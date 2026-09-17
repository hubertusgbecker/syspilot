Harness Orchestration Adapter Expected Outcomes
===============================================

Expected outcomes for ``SYSP_REQ_UAT_HARNESS_ORCHESTRATION_ADAPTER``.


.. spec:: UAT Expected Outcomes: Harness Orchestration Fallback
  :id: SYSP_SPEC_UAT_HARNESS_ORCHESTRATION_ADAPTER
  :status: approved
  :priority: mandatory
  :tags: uat, harness, orchestration, fallback, expected-outcomes
  :links: SYSP_REQ_UAT_HARNESS_ORCHESTRATION_ADAPTER

   **TC-HOA-SYNC — Every target selects the synchronous variant**

   *Precondition:* Reset the production and experimental orchestration fixtures,
   including ``F-ORCH-JARVIS-PRESENT``.

   *Action:* Install syspilot in each fixture and inspect the installed
   orchestration-group Skill.

   *Expected result:*

   * [ ] Each fixture contains ``syspilot.orchestration-subagent``.
   * [ ] No fixture contains ``syspilot.orchestration-jarvis``, including the
     fixture with a pre-existing ``.jarvis/`` directory.
   * [ ] Exactly one Skill with ``group: orchestration`` is installed.

   *Traces to:* ``SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER`` AC-1

   ---

   **TC-HOA-BINDING — Deterministic native binding generation**

   *Precondition:* Install the same source revision into clean VS Code and
   OpenCode fixtures and retain source plus generated orchestration Skills.

   *Action:* Parse the designated binding section and compare all content
   outside frontmatter and that section byte-for-byte.

   *Expected result:*

   * [ ] VS Code maps SEND to ``runSubagent`` and contains no OpenCode Task
     instruction in its binding section.
   * [ ] OpenCode maps SEND to native Task and contains no ``runSubagent``
     instruction in its binding section.
   * [ ] RECEIVE, RESPOND, synchronous ordering, result propagation, and every
     rule outside the binding section are byte-identical to the semantic source.
   * [ ] A missing, duplicate, or malformed binding boundary fails before
     checkpoint creation.

   *Traces to:* ``SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER`` AC-2

   ---

   **TC-HOA-NATIVE — OpenCode one-hop SEND**

   *Precondition:* Complete TC-HOA-SYNC for OpenCode and install the
   deterministic Manager and Engineer fixtures.

   *Action:* Invoke the Manager and instruct it to execute its one SEND step.
   Inspect the transcript and final Manager response.

   *Expected result:*

   * [ ] OpenCode uses its permitted Task primitive to invoke the named
     Engineer.
   * [ ] The Manager returns ``ORCHESTRATION-RESULT`` from the Engineer before
     terminating.

   *Traces to:* ``SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER`` AC-2

   ---

   **TC-HOA-CLAUDE — Claude Code native binding and delegation**

   *Precondition:* Complete Claude static adapter checks; install a Manager and
   its allowed Engineer under their mapped native names. Authenticate Claude
   Code for the live portion, or record authentication as externally blocked.

   *Action:* Inspect the Manager tools/Workflow bindings, invoke the Manager as
   the main thread, and instruct it to perform one SEND to the Engineer.

   *Expected result:*

   * [ ] The main-thread Manager's explicit ``Agent(<native-name>, ...)``
     targets and every Workflow binding resolve to generated Claude names.
   * [ ] Authenticated execution uses the mapped Engineer identity and returns
     ``ORCHESTRATION-RESULT`` to the Manager.
   * [ ] Agent-not-found is a failure. Authentication failure is an external
     blocked result that proves neither delegation nor production support.
   * [ ] Static parsing or discovery alone does not mark Claude
     production-supported.

   *Traces to:* ``SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER`` AC-3

   ---

   **TC-HOA-QODER — Qoder pending live UAT**

   *Precondition:* Complete static adapter checks for Qoder.

   *Action:* Record whether authenticated live native invocation has been
   executed.

   *Expected result:*

   * [ ] Qoder remains experimental until its Manager-to-Engineer live
     invocation returns ``ORCHESTRATION-RESULT`` and is recorded.

   *Traces to:* ``SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER`` AC-3

   ---

   **Testability Note:** Missing authentication or harness availability is
   recorded as not executed, never converted into production acceptance.
