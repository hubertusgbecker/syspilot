Harness Prompt Adapter Expected Outcomes
========================================

Expected outcomes for ``SYSP_REQ_UAT_HARNESS_PROMPT_ADAPTER``.


.. spec:: UAT Expected Outcomes: Harness Prompt/Command Adapter
  :id: SYSP_SPEC_UAT_HARNESS_PROMPT_ADAPTER
  :status: approved
  :links: SYSP_REQ_UAT_HARNESS_PROMPT_ADAPTER

   **TC-HPA-VSCODE — Existing VS Code prompt remains unchanged**

   *Precondition:* Reset ``F-PROMPT-VSCODE`` and retain the source prompt.

    *Action:* Install syspilot, open the generated prompt, and invoke it with
    the representative request from the test-data requirement.

   *Expected result:*

   * [ ] ``.github/prompts/syspilot.<name>.prompt.md`` exists.
   * [ ] Its description and ``agent: syspilot.<name>`` route match source.
   * [ ] Invocation starts the named Manager agent.
   * [ ] The Manager produces the fixture's observable workflow outcome for
     the representative request.

   *Traces to:* ``SYSP_US_UAT_HARNESS_PROMPT_ADAPTER`` AC-1

   ---

   **TC-HPA-OPENCODE — OpenCode command mirrors the prompt role**

   *Precondition:* Reset ``F-PROMPT-OPENCODE`` and retain every product source
   prompt, including one prompt without a description and names containing
   dots.

    *Action:* Install syspilot, inspect the generated command, and invoke it by
    its native command name with exactly the same representative request used
    in TC-HPA-VSCODE.

   *Expected result:*

   * [ ] Exactly one ``.opencode/commands/<name>.md`` exists for every product
     ``<name>.prompt.md``; removing the exact suffix preserves all dots in
     ``<name>``.
   * [ ] Its frontmatter retains the source description or, when absent, uses
     the corresponding agent description, and routes through the source
     ``agent: <name>`` value.
   * [ ] The body is exactly ``$ARGUMENTS`` plus one terminal newline and does
     not duplicate the Manager's workflow prose.
   * [ ] Native invocation supplies a multi-token request containing spaces and
     punctuation; OpenCode substitutes the complete request and the routed
     Manager records the exact received text.
   * [ ] The selected Manager and observable workflow outcome are equivalent
     to TC-HPA-VSCODE for the identical request; matching route metadata alone
     does not satisfy this check.

   *Traces to:* ``SYSP_US_UAT_HARNESS_PROMPT_ADAPTER`` AC-2

   ---

   **TC-HPA-CLAUDE — Claude Code uses no separate prompt file**

    *Precondition:* Optional experimental fixture ``F-PROMPT-CLAUDE`` is
    available and reset. Otherwise record this scenario as not executed.

   *Action:* Install syspilot, list all generated paths, and invoke the
   Manager through its native agent or Skill entry point.

   *Expected result:*

   * [ ] No syspilot prompt/command directory or separate prompt artifact is
     generated under ``.claude/``.
   * [ ] The Manager remains invocable through its installed native surface.

   *Traces to:* ``SYSP_US_UAT_HARNESS_PROMPT_ADAPTER`` AC-3

   ---

   **TC-HPA-QODER — Qoder uses no separate prompt file**

    *Precondition:* Optional experimental fixture ``F-PROMPT-QODER`` is
    available and reset. Otherwise record this scenario as not executed.

    Absence of either experimental fixture does not block the VS Code/OpenCode
    production result.

   *Action:* Install syspilot, list all generated paths, and invoke the
   Manager through Qoder's Custom Agent surface.

   *Expected result:*

   * [ ] No syspilot prompt/command directory or separate prompt artifact is
     generated under ``.qoder/``.
   * [ ] The installed Custom Agent remains directly invocable.

   *Traces to:* ``SYSP_US_UAT_HARNESS_PROMPT_ADAPTER`` AC-4
