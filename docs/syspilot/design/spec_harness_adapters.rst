Harness Adapter Design
======================

Structural/frontmatter adapters that let the single-source agent and skill
content (Soul/Duties/Workflow content per SYSP_SPEC_AGENT_ARCH_SOUL,
Skill Instructions/Rules) run unmodified on OpenCode in addition to VS Code
GitHub Copilot. Claude Code and Qoder mappings are retained as experimental
targets pending live native invocation UAT. Findings below are based on each
harness's own current documentation, fetched 2026-09-16 — not on assumptions
carried over from prior knowledge or from ``superpowers``.

.. important::

    Content (Soul/Duties/Workflow prose, Skill Instructions/Rules prose) is
    never forked per harness. The complete methodology body bytes after the
    YAML frontmatter delimiter, including terminal-newline state, are preserved
    unchanged except for deterministic identity and tool-binding references in
    Workflow or the orchestration Skill's formally designated native-binding
    section. Those structural binding adaptations preserve all other bytes and
    semantics; they are not content drift. Only those bindings, frontmatter,
    filename, and target directory may differ, per
    SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE.


.. spec:: Harness Target Matrix
   :id: SYSP_SPEC_HARNESS_TARGET_MATRIX
   :status: approved
   :tags: agent-v2, harness, matrix
   :links: SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE, SYSP_REQ_HARNESS_NATIVE_INSTALL

   **Researched native mechanisms (2026-09-16):**

   .. list-table::
      :header-rows: 1
      :widths: 14 20 20 20 26

      * - Harness
        - Agent (subagent) files
        - Skill files
        - Prompt / command files
        - Native distribution
      * - VS Code Copilot Chat
        - ``.github/agents/*.agent.md``
        - ``.github/skills/<name>/SKILL.md``
        - ``.github/prompts/*.prompt.md``
        - Installer copies files directly (existing mechanism, unchanged)
      * - Claude Code
        - ``.claude/agents/*.md`` (project) or ``~/.claude/agents/*.md`` (user)
        - ``.claude/skills/<name>/SKILL.md`` (project) or
          ``~/.claude/skills/<name>/SKILL.md`` (user)
        - Skills subsume commands; a Skill folder invoked as ``/<name>``
          *is* the prompt entry point — no separate prompt file format
        - File copy into the above directories (team-shared via ``git``);
          optionally packaged as a **Plugin** (``.claude-plugin/plugin.json``
          + ``agents/`` + ``skills/`` dirs) installed via
          ``/plugin marketplace add <org>/<repo>`` then
          ``/plugin install syspilot@<marketplace>``
      * - OpenCode
        - ``.opencode/agents/*.md`` (project) or
          ``~/.config/opencode/agents/*.md`` (global). OpenCode also reads
          ``.claude/agents/`` is **not** documented for agents (Claude-compat
          is documented for Skills only — see next column)
        - ``.opencode/skills/<name>/SKILL.md`` (project),
          ``~/.config/opencode/skills/<name>/SKILL.md`` (global) — OpenCode
          additionally reads ``.claude/skills/`` and ``.agents/skills/`` at
          both project and global scope for cross-tool interop
        - ``.opencode/commands/<name>.md`` (project) or
          ``~/.config/opencode/commands/<name>.md`` (global)
        - File copy only; no marketplace/package-manager mechanism was found
          for agents/skills/commands in OpenCode's own docs
      * - Qoder
        - ``.qoder/agents/<name>.md`` (project) or ``~/.qoder/agents/<name>.md``
          (user)
        - ``.qoder/skills/<name>/SKILL.md`` (project) or
          ``~/.qoder/skills/<name>/SKILL.md`` (user)
        - No separate prompt/command file format documented; Skills are
          invoked as ``/<skill-name>``, same pattern as Claude Code
        - File copy only; a ``skills add`` third-party marketplace CLI
          (``npx skills add ... -a qoder``) exists for *community* skills,
          not for first-party project distribution

.. spec:: Harness Agent Frontmatter Adapter
  :id: SYSP_SPEC_HARNESS_AGENT_ADAPTER
  :status: approved
  :tags: agent-v2, harness, adapter, agent, frontmatter
  :links: SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE, SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE, SYSP_SPEC_AGENT_ARCH_FRONTMATTER, SYSP_SPEC_HARNESS_TARGET_MATRIX

   **Field mapping** (source: syspilot's own ``.agent.md`` frontmatter,
   SYSP_SPEC_AGENT_ARCH_FRONTMATTER):

   .. list-table::
      :header-rows: 1
      :widths: 16 28 28 28

      * - syspilot field
        - Claude Code
        - OpenCode
        - Qoder
      * - ``description``
        - ``description`` (required)
        - ``description`` (required)
        - ``description`` (required)
      * - Source dotted agent ID
        - Required ``name`` identity. Replace every ``.`` in
          ``syspilot.<role>`` with ``-`` to produce
          ``syspilot-<role>``. The result contains only lowercase letters and
          hyphens. Filename mapping may remain independently target-specific;
          Claude identity comes only from frontmatter ``name``. A file without
          ``name`` is skipped as documentation, not loaded as an agent.
        - Retained as the native dotted agent identity.
        - Filename-derived identity; no separate field documented.
      * - Soul/Duties/Workflow body
        - Markdown body (system prompt) — unchanged
        - Markdown body (system prompt) — unchanged
        - Markdown body (system prompt) — unchanged
      * - ``user-invocable``
        - No equivalent field. Every subagent file can always be
          ``@``-mentioned by the user; there is no frontmatter flag to hide
          one from direct invocation. **Limitation** — see
          SYSP_SPEC_HARNESS_LIMITATIONS.
        - No first-class hide flag; ``mode: subagent`` plus ``hidden: true``
          removes an agent from the ``@`` autocomplete menu only — the model
          can still invoke it via the Task tool regardless.
        - No equivalent field documented.
      * - ``agents`` (SEND allowlist)
        - For a Manager that runs as the main thread, emit an explicit
          ``Agent(syspilot-role-a, syspilot-role-b)`` entry in ``tools`` after
          mapping every source allowlist ID to its Claude native ``name``.
          Preserve other required tools through an approved native mapping or
          Claude inheritance. For a nested subagent that must spawn further
          subagents, Claude's documented nested Agent-tool semantics apply;
          ``tools: Agent`` permits unrestricted spawn and therefore does not
          falsely claim per-type enforcement.
        - ``permission.task`` glob patterns on the *calling* agent's own
          frontmatter or ``opencode.json`` can allow/deny/ask which subagent
          names it may invoke via the Task tool — closest native equivalent
          of ``agents:``.
        - No equivalent field documented; orchestration is "automatic or
          manual ``/agent-name``" with no declared allowlist.
      * - ``name`` / ``agent`` (Jarvis session identity)
        - Jarvis session identity remains inapplicable. This does not remove
          Claude's required native ``name`` derived from the source dotted
          agent ID (see SYSP_SPEC_HARNESS_ORCHESTRATION_ADAPTER).
        - Not applicable — omitted.
        - Not applicable — omitted.
      * - ``tools``
        - ``tools:`` list where required by the target role. Main-thread
          Manager SEND restrictions use ``Agent(<native-name>, ...)``;
          ``Agent`` without arguments permits unrestricted spawn under
          Claude's documented semantics. Setup needs only direct
          command-execution capability and no ``Agent`` tool.
        - ``permission.task`` / built-in Task tool, gated by the agent's
          ``mode``.
        - Not documented beyond a fixed built-in tool list (Bash, Edit,
          Write, Glob, Grep, Read, WebFetch, WebSearch); no confirmed
          subagent-spawning tool name.
      * - ``version``
        - No equivalent; omitted (only ``syspilot.setup`` uses it, and the
          Setup/Bootloader concept differs per harness — see
          SYSP_SPEC_HARNESS_ONE_TIME_ENABLEMENT).
        - Omitted.
        - Omitted.

  **Adapter rule:** the Installer derives each canonical source agent ID from
  ``syspilot.<role>.agent.md`` and emits the Claude native identity
  ``syspilot-<role>`` as required frontmatter ``name``. Across all 13 product
  agents, generated names SHALL be valid and unique. The Installer otherwise
  writes identical Markdown body bytes (Soul/Duties/Workflow, including the
  Tailoring preflight sentence and exact EOF-newline state) except for the
  structural binding adaptation defined below. Fields with no native
  equivalent are dropped, never approximated with a same-named field having
  different semantics.

  **Structural binding adaptation:** for Claude output only, replace dotted
  agent IDs with their mapped native names solely where Workflow prose binds
  SEND/RECEIVE/RESPOND to a target identity or where the designated
  orchestration native-binding section binds an Agent tool target. The
  transformation is deterministic and boundary-aware; ordinary prose,
  examples unrelated to orchestration binding, headings, duties, ordering,
  and all remaining bytes stay unchanged. Every resulting allowlist and
  binding reference SHALL resolve to exactly one generated Claude ``name``.
  This target-native wiring is structural adaptation, not methodology-content
  drift.

  **Claude user invocation:** installed user-facing agents are invoked as
  ``claude --agent syspilot-setup``, ``claude --agent syspilot-pm``,
  ``claude --agent syspilot-qm``, and ``claude --agent syspilot-cm``.
  Dotted source IDs and source/display filenames are not Claude invocation
  identities.

   **OpenCode exact mapping:** a source Manager emits ``mode: primary`` and an
   Engineer emits ``mode: subagent``. For a nonempty parsed source ``agents``
   allowlist, emit the parser-valid map shape below: the deny-all entry is
   always present and each source allowlist value becomes one explicit allow
   entry. With an empty or absent allowlist, omit ``permission.task`` entirely.
   Setup therefore emits as ``primary`` without Installer task permission;
   Installer emits as ``subagent`` and is not part of installation control
   flow.

   .. code-block:: yaml

      mode: primary
      permission:
        task:
          "*": deny
          "syspilot.<allowed-agent>": allow


.. spec:: Harness Skill Frontmatter Adapter
   :id: SYSP_SPEC_HARNESS_SKILL_ADAPTER
   :status: approved
   :tags: agent-v2, harness, adapter, skill, frontmatter
   :links: SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE, SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE, SYSP_SPEC_SKILL_ARCH_FRONTMATTER, SYSP_SPEC_HARNESS_TARGET_MATRIX

   **Field mapping** (source: syspilot's own ``SKILL.md`` frontmatter,
   SYSP_SPEC_SKILL_ARCH_FRONTMATTER):

   .. list-table::
      :header-rows: 1
      :widths: 16 28 28 28

      * - syspilot field
        - Claude Code
        - OpenCode
        - Qoder
      * - ``name``
        - Directory name is the invocation name; frontmatter ``name`` is
          display-only for personal/project skills (only matters for
          plugin-packaged skills). Kept for readability, not load-bearing.
        - ``name`` (recognized and retained).
        - Not documented as a distinct field; directory name is the
          invocation name (``/log-analyzer``-style), same pattern.
      * - ``description``
        - ``description`` (recommended; used for auto-invocation matching)
        - ``description`` (required)
        - Used for both automatic and manual (``/name``) triggering.
      * - ``group`` (Installer mutual-exclusion key)
        - Omitted from native frontmatter. The Installer reads the source value
          before adaptation to enforce SYSP_SPEC_INSTALLER_SKILL_MUTEX.
        - Omitted from native frontmatter; the source value still drives
          Installer mutual exclusion before adaptation.
        - Omitted from native frontmatter; the same source-metadata rule
          applies.
      * - ``tools`` (Skill's own suggested toolset)
        - Omitted; Claude Code's ``allowed-tools`` has different semantics.
        - Omitted because it is not a recognized OpenCode Skill field.
        - Omitted because no native equivalent is documented.
      * - ``triggers``
        - Omitted; Claude Code relies on the natural-language ``description``.
        - Omitted because OpenCode has no equivalent field.
        - Omitted; Qoder uses description-based matching.
      * - Instructions / Rules body
        - Markdown body — unchanged. ``## Instructions`` / ``## Rules``
          headings are plain Markdown headings with no special meaning to
          Claude Code, but remain useful structure for the model and for
          humans.
        - Markdown body — unchanged except for the orchestration Skill's
          designated native Task-binding section, same as above otherwise.
        - Markdown body — unchanged, same as above.

   **Adapter rule:** the Skill's Markdown body (Frontmatter-adjacent
   Instructions/Rules content) is written byte-for-byte identical across all
   four harnesses except for the orchestration Skill's designated native
   binding section. Only recognized frontmatter fields are emitted.
   syspilot-internal fields with no harness-native meaning (``group``,
   ``tools``, ``triggers``) are omitted from native frontmatter; the
   deterministic adapter retains their installation semantics where needed,
   including Skill-group mutual exclusion.

   Non-``SKILL.md`` resources are copied to
   ``.syspilot/skills/<source-directory>/`` and product templates to
   ``.syspilot/templates/``. Installed Skill instructions reference only those
   harness-neutral shared paths. In particular, change-launcher resolves
   ``.syspilot/templates/change-document.md`` and its shared script, and impact
   resolves ``.syspilot/skills/syspilot.impact-python/scripts/get_need_links.py``.


.. spec:: Harness Prompt/Command Adapter
  :status: implemented
  :id: SYSP_SPEC_HARNESS_PROMPT_ADAPTER
  :links: SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE, SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE, SYSP_SPEC_AGENT_ARCH_PROMPT, SYSP_SPEC_HARNESS_TARGET_MATRIX
  :tags: agent-v2, harness, adapter, prompt

   **Behavior:**

   * **VS Code** — unchanged: ``syspilot.<name>.prompt.md`` with
     ``agent: syspilot.<name>`` frontmatter, per SYSP_SPEC_AGENT_ARCH_PROMPT.
   * **Claude Code** — no separate prompt-file concept exists; a Manager
     agent's own Skill/subagent entry point already serves as the
     user-invocation surface (``@agent-name`` or, if the Manager is
     packaged as a Skill with ``context: fork``, ``/name``). The Installer
     SHALL NOT generate a Claude Code prompt file — there is nothing to
     adapt it to.
   * **OpenCode** — ``.opencode/commands/<name>.md`` (or global
     ``~/.config/opencode/commands/<name>.md``) mirrors each product prompt.
     Convert the source filename by removing only the exact terminal
     ``.prompt.md`` suffix and appending ``.md``; all dots in ``<name>`` are
     preserved (for example, ``syspilot.cm.prompt.md`` becomes
     ``syspilot.cm.md``). Emit ``description`` from prompt frontmatter, falling
     back to the corresponding source agent description when absent, and emit
     the prompt's ``agent`` value. The command body consists only of
     ``$ARGUMENTS`` followed by one terminal newline. OpenCode replaces that
     documented placeholder with the complete native command request, so the
     selected agent receives the caller's full argument text without copied
     prompt or agent prose.
   * **Qoder** — no separate command-file concept documented; same as
     Claude Code, the Custom Agent file itself is the entry point.

   **Consequence:** the Prompt File architecture element
   (SYSP_SPEC_AGENT_ARCH_PROMPT) is VS Code- and OpenCode-specific; Claude
   Code and Qoder fold this role into the agent file itself. This is a
   structural difference the adapter absorbs, not a content fork.


.. spec:: Harness Orchestration Fallback
  :id: SYSP_SPEC_HARNESS_ORCHESTRATION_ADAPTER
  :status: approved
  :tags: agent-v2, harness, orchestration, adapter
  :links: SYSP_REQ_SKILL_ORCHESTRATION_GROUP, SYSP_REQ_HARNESS_BEHAVIORAL_EQUIVALENCE, SYSP_REQ_HARNESS_CONTENT_SINGLE_SOURCE, SYSP_SPEC_INSTALLER_ORCHESTRATION_SELECT, SYSP_SPEC_HARNESS_TARGET_MATRIX

   **Finding:** none of Claude Code, OpenCode, or Qoder expose a Jarvis-
   equivalent persistent, cross-session, named-actor messaging registry.
   Claude Code has its own cross-session ``SendMessage``/agent-teams
   mechanism, but it is a different, incompatible protocol, not a Jarvis
   integration.

   **Semantic source and adapter ownership:** the product
   ``syspilot.orchestration-subagent`` Skill owns one canonical
   SEND/RECEIVE/RESPOND vocabulary: definitions, caller/callee inputs,
   synchronous ordering, result propagation, terminal RESPOND behavior, and
   rules. Its native-binding section is explicitly adapter-owned. The Installer
   selects that section solely from the explicit harness value, replaces it as
   a structured section rather than by free-form text substitution, and rejects
   a source with a missing, duplicate, or malformed binding boundary before
   checkpoint creation.

   Every installation deterministically selects this synchronous variant. No
   ``.jarvis/`` detection, asynchronous variant choice, or actor creation
   participates. The generated binding section maps the semantic operations to
   each harness's native primitive; byte identity is required outside that
   section, while semantic equivalence rather than byte identity is required
   inside it:

   .. list-table::
      :header-rows: 1
      :widths: 20 40 40

      * - Harness
        - SEND primitive
        - Notes
      * - Claude Code
        - Built-in ``Agent`` tool (subagent invocation)
        - A main-thread Manager maps its source allowlist to
          ``Agent(<native-name>, ...)``; a nested spawning subagent follows
          Claude's documented Agent-tool semantics, where bare ``Agent`` is
          unrestricted
      * - OpenCode
        - Built-in Task tool, gated by ``permission.task``
        - Manager agents set ``mode: primary`` and grant task permission to
          named Engineer subagents
      * - Qoder
        - Documented only as "the subagent approach"; no further detail
          confirmed
        - Treat as flat (Manager invokes one Engineer at a time, no
          documented nesting) until independently verified — see
          SYSP_SPEC_HARNESS_LIMITATIONS

   **Production binding contracts:**

   * **VS Code:** SEND invokes
     ``runSubagent("syspilot.<agent>", "<task>")`` and blocks for its return;
     RECEIVE is the task that started the current agent; RESPOND is normal
     terminal output captured by the caller.
   * **OpenCode:** SEND invokes native Task with the exact allowed
     ``syspilot.<agent>`` and task, gated by the caller's ``permission.task``;
     RECEIVE is the task that started the current agent; RESPOND is the normal
     Task result returned to the caller. The installed OpenCode Skill SHALL
     contain this Task mapping and SHALL NOT instruct the model to call
     ``runSubagent``.
   * **Claude Code:** SEND invokes the built-in Agent tool with the mapped
     ``syspilot-<agent>`` native identity and blocks for its result; RECEIVE is
     the task that started the current agent; RESPOND is normal output returned
     to the caller. Main-thread Manager restrictions use explicit
     ``Agent(<native-name>, ...)`` entries; nested subagents follow Claude's
     documented Agent-tool behavior.

   The adapter never derives binding choice from installed directories or
   rewrites the shared semantic definitions. VS Code and OpenCode generated
   Skills are allowed to differ only in frontmatter and this designated
   binding section.


.. spec:: Harness Disclosed Limitations
  :id: SYSP_SPEC_HARNESS_LIMITATIONS
  :status: approved
  :tags: agent-v2, harness, limitation, disclosure
  :links: SYSP_REQ_HARNESS_LIMITATION_DISCLOSURE, SYSP_SPEC_HARNESS_AGENT_ADAPTER, SYSP_SPEC_HARNESS_SKILL_ADAPTER, SYSP_SPEC_HARNESS_ORCHESTRATION_ADAPTER

   **Disclosed, accepted limitations** (each scoped to one harness; none
   blocks support on the other harnesses, per
   SYSP_REQ_HARNESS_LIMITATION_DISCLOSURE AC-2):

   * **Claude Code — nested allowlist enforcement boundary.** A Manager
     running as the main thread can restrict Agent targets with
     ``Agent(<native-name>, ...)``. For a nested subagent that itself spawns
     another subagent, documented bare ``Agent`` semantics permit unrestricted
     spawn; Workflow target bindings remain authoritative guidance but are not
     misrepresented as harness-enforced per-type restrictions.
   * **OpenCode — Skill frontmatter is a strict six-field allowlist.**
     Source-only ``group``, ``tools``, and ``triggers`` fields are omitted
     from adapted native frontmatter. The Installer reads source ``group``
     metadata before adaptation to enforce mutual exclusion, so omission does
     not remove installation semantics.
   * **Qoder — subagent-to-subagent nesting is undocumented.** Qoder's own
     docs describe Custom Agent scheduling only as "the subagent approach"
     without detailing whether a Custom Agent can itself invoke another
     Custom Agent, or to what depth. Until independently verified, the
     Manager→Engineer SEND pattern is treated as a single flat hop on
     Qoder.
   * **Qoder — experimental support only.** The mappings in this document are
     retained for continued development, but Qoder is not advertised as
     production-supported until clean install, repeat update, native
     invocation, and rollback UAT are complete.
   * **Claude Code — experimental support only.** Static discovery and parsing
     must pass for all 13 native names, but Claude Code is not
     production-supported until authenticated live invocation and delegation,
     plus clean-install, repeat-update, and rollback UAT, are complete.
   * **Qoder — Rules require Settings-UI registration.** Qoder's Rules
     mechanism (project convention injection, distinct from Agents/Skills)
     documents rule *type* selection (Always Apply / Model Decision /
     Specific Files) via the IDE Settings UI, not purely by dropping a file
     with the right frontmatter. syspilot does not ship Rules as part of
     its installation scope, so this does not block SYSP_REQ_HARNESS_NATIVE_INSTALL;
     it is disclosed here as a related gap should a future CR extend
     syspilot to Rules-based distribution on Qoder.
