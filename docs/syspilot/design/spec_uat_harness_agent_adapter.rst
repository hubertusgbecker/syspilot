Harness Agent Adapter Expected Outcomes
=======================================

Expected outcomes for ``SYSP_REQ_UAT_HARNESS_AGENT_ADAPTER``.


.. spec:: UAT Expected Outcomes: Harness Agent Frontmatter Adapter
  :id: SYSP_SPEC_UAT_HARNESS_AGENT_ADAPTER
  :status: approved
  :priority: mandatory
  :tags: uat, harness, agent, frontmatter, expected-outcomes
  :links: SYSP_REQ_UAT_HARNESS_AGENT_ADAPTER

   **TC-HAA-BODY — Agent body remains single-source**

   *Precondition:* All generated Manager and Engineer files and their source
   files are available from the same revision.

   *Action:* Remove only each YAML frontmatter block and perform byte-level
   comparisons of the remaining Markdown bodies.

   *Expected result:*

   * [ ] Every adapted body exactly matches its source body outside the
     deterministic structural binding locations authorized by
     ``SYSP_SPEC_HARNESS_AGENT_ADAPTER``.
   * [ ] Only dotted target identities in Workflow/orchestration bindings are
     replaced by Claude native names; Soul, Duties, headings, ordering,
     ordinary prose, Tailoring text, and all other bytes are unchanged.

   *Traces to:* ``SYSP_US_UAT_HARNESS_AGENT_ADAPTER`` AC-1

   ---

   **TC-HAA-CLAUDE — Claude Code field mapping**

    *Precondition:* All 13 generated Claude Code product agents and their
    source files are available.

   *Action:* Parse each YAML block and compare its keys with the Claude Code
   row in the reference field set.

   *Expected result:*

   * [ ] Every file contains required ``name`` and ``description`` fields;
     every name equals its dotted source ID with periods replaced by hyphens.
   * [ ] Exactly 13 names exist, all are unique, and each contains only
     lowercase letters and hyphens.
   * [ ] A main-thread Manager with a source allowlist has an explicit
     ``Agent(<native-name>, ...)`` tools entry containing exactly the mapped
     target set plus any independently approved/inherited tools.
   * [ ] A nested spawning subagent follows documented Claude Agent-tool
     semantics; bare ``Agent`` is treated as unrestricted, never as a
     per-target allowlist.
   * [ ] Every Workflow and orchestration binding target resolves to exactly
     one of the 13 generated names.
   * [ ] ``user-invocable``, source ``agents``, Jarvis ``agent``, and
     ``version`` are absent.
   * [ ] No replacement field with different semantics was introduced.

   *Traces to:* ``SYSP_US_UAT_HARNESS_AGENT_ADAPTER`` AC-2

   ---

   **TC-HAA-OPENCODE — OpenCode mode and task permissions**

   *Precondition:* Generated OpenCode Manager and Engineer agents are
   available.

   *Action:* Parse their YAML and compare Manager task permissions with the
   Manager's source ``agents:`` allowlist.

   *Expected result:*

   * [ ] The Manager is ``mode: primary`` and its ``permission.task`` grants
     the named Engineer targets.
   * [ ] ``permission.task`` is a YAML map containing ``"*": deny`` and one
     explicit ``<source-allowlist-agent>: allow`` entry for every allowed
     target, rather than a list or combined allow value.
   * [ ] An empty source allowlist produces ``mode: subagent`` and no
     ``permission.task`` map; a nonempty allowlist produces ``mode: primary``.
   * [ ] Setup is ``mode: primary`` because it is user-invocable, has no
     ``permission.task`` map, and grants no Installer delegation.
   * [ ] Jarvis-only ``name`` and ``agent`` identity fields are absent.
   * [ ] Both bodies still pass TC-HAA-BODY.
   * [ ] Invoking Setup records a direct ``uv run --no-project
     .syspilot/installer.py install`` child process with the explicit inputs;
     no Agent, Task, ``runSubagent``, SEND, or Installer-agent invocation is
     recorded.

   *Traces to:* ``SYSP_US_UAT_HARNESS_AGENT_ADAPTER`` AC-3

   ---

   **TC-HAA-QODER — Qoder unsupported fields are omitted**

    *Precondition:* Optional experimental Qoder Manager and Engineer agents are
    available. Otherwise record this scenario as not executed.

   *Action:* Parse their YAML and compare all keys with the documented Qoder
   mapping.

   *Expected result:*

   * [ ] ``description`` is present and unchanged.
   * [ ] ``user-invocable``, ``agents``, ``name``, ``agent``, and ``version``
     are absent.
   * [ ] No undocumented SEND-tool or allowlist key was invented.

   *Traces to:* ``SYSP_US_UAT_HARNESS_AGENT_ADAPTER`` AC-4

   ---

   **TC-HAA-RUNTIME — Representative agents load with original guidance**

    *Precondition:* Install the generated Manager and Engineer in a clean
    OpenCode project. Include Claude Code and Qoder projects only when their
    optional experimental fixtures are available.

  *Action:* Invoke each agent by its native mechanism and ask it to state
  its role, duties, first workflow step, and terminal workflow step from its
  loaded instructions. In the Claude fixture, run
  ``claude plugin validate .claude/agents`` only when that CLI version accepts
  an agents directory as a validation target, then run
  ``claude --agent syspilot-qm``. Also record that user-facing Setup, PM, QM,
  and CM identities are respectively ``syspilot-setup``, ``syspilot-pm``,
  ``syspilot-qm``, and ``syspilot-cm``.

   *Expected result:*

   * [ ] OpenCode loads both agents without a frontmatter error and each
     response reflects the source role, duties, RECEIVE-first step, and
     RESPOND-terminal step.
   * [ ] Claude validation, when supported for this target shape, reports no
     malformed or undiscoverable agent file.
   * [ ] ``claude --agent syspilot-qm`` does not report agent-not-found and
     reaches authentication or model execution. An authentication failure is
     recorded as an external blocked result, not a discovery failure and not
     an authenticated pass.
   * [ ] Claude remains experimental until authenticated native invocation and
     Manager-to-Engineer delegation both pass and are recorded.
   * [ ] Available Qoder fixtures are recorded separately; unavailable Qoder
     evidence remains not executed and does not block production acceptance.

   **OpenCode native configuration check:** in an isolated OpenCode fixture,
   run ``opencode debug config`` from the project root and parse the resolved
   agent configuration. The command must complete without a parser error and
   show the specified Setup and representative Manager/Engineer modes and Task
   permission maps, including Setup's absent task permission. Record the
   OpenCode version; if the command is unavailable, report the runtime check as
   unavailable rather than passing it from static inspection.

   *Traces to:* ``SYSP_US_UAT_HARNESS_AGENT_ADAPTER`` AC-5

   ---

   **Testability Note:**

   Static comparison proves content identity and field mapping. It cannot
   prove model behavior is identical; TC-HAA-RUNTIME checks instruction
   availability, while stochastic response equivalence remains unsuitable
   for a deterministic pass/fail assertion.
