Harness Skill Adapter Expected Outcomes
=======================================

Expected outcomes for ``SYSP_REQ_UAT_HARNESS_SKILL_ADAPTER``.


.. spec:: UAT Expected Outcomes: Harness Skill Frontmatter Adapter
   :id: SYSP_SPEC_UAT_HARNESS_SKILL_ADAPTER
   :status: approved
   :priority: mandatory
   :tags: uat, harness, skill, frontmatter, expected-outcomes
   :links: SYSP_REQ_UAT_HARNESS_SKILL_ADAPTER

   **TC-HSA-BODY — Skill body remains single-source**

   *Precondition:* Source and generated copies of both selected Skills are
   available from the same revision.

   *Action:* Remove only YAML frontmatter and compare each generated Markdown
   body byte-for-byte with its source.

   *Expected result:*

   * [ ] Every generated Instructions/Rules body exactly matches its source.
   * [ ] No harness-specific instruction is inserted into the body.

   *Traces to:* ``SYSP_US_UAT_HARNESS_SKILL_ADAPTER`` AC-1

   ---

   **TC-HSA-DISCOVERY — Description and directory invocation name**

    *Precondition:* Install both Skills in a clean OpenCode fixture using the
    original directory names. Include clean Claude Code and Qoder fixtures only
    when those optional experimental fixtures are available.

   *Action:* Inspect generated YAML, open each harness's Skill picker, and
   manually invoke each Skill by its directory name.

   *Expected result:*

   * [ ] Every copy retains the source ``description`` unchanged.
   * [ ] OpenCode lists or resolves the Skill using its source directory name.
   * [ ] Manual OpenCode invocation loads the selected Skill's instructions.
   * [ ] Available optional Claude Code and Qoder fixtures satisfy the same
     checks; unavailable fixtures remain not executed and do not block
     production acceptance.

   *Traces to:* ``SYSP_US_UAT_HARNESS_SKILL_ADAPTER`` AC-2

   ---

    **TC-HSA-METADATA — Unsupported native metadata is omitted**

    *Precondition:* The selected metadata-rich Skill is generated for OpenCode
    and for each available optional Claude Code or Qoder fixture.

    *Action:* Parse each generated YAML block, record the source ``group``
    before adaptation, and install a second Skill from the same group.

   *Expected result:*

   * [ ] ``group``, ``tools``, and ``triggers`` are absent from every adapted
     native frontmatter block where the harness does not support them.
   * [ ] The Installer uses the source ``group`` value before adaptation and
     replaces the previously installed member of that group.
   * [ ] Exactly one Skill from the group remains installed even though the
     adapted ``SKILL.md`` does not retain the ``group`` key.
   * [ ] OpenCode and each available optional experimental fixture load the
     Skill without unsupported-field errors.

   *Traces to:* ``SYSP_US_UAT_HARNESS_SKILL_ADAPTER`` AC-3

   ---

   **TC-HSA-RUNTIME — Representative Skill instructions are available**

    *Precondition:* Both Skills are installed and manually invocable in
    OpenCode; optional Claude Code and Qoder fixtures are included only when
    available.

   *Action:* Invoke each Skill and ask the active agent to identify the first
   instruction and one rule from the loaded Skill text.

   *Expected result:*

   * [ ] Both OpenCode/Skill combinations load without frontmatter errors;
     available experimental combinations are recorded separately.
   * [ ] The identified instruction and rule match the source text.

   *Traces to:* ``SYSP_US_UAT_HARNESS_SKILL_ADAPTER`` AC-4

   ---

   **TC-HSA-RESOURCES — Harness-neutral scripts and templates**

   *Precondition:* Install change-launcher and impact from the same revision
   into separate clean VS Code and OpenCode fixtures.

   *Action:* Inspect native and shared paths, then invoke each Skill's
   deterministic resource command from the target root.

   *Expected result:*

   * [ ] Each fixture contains native ``SKILL.md`` files only under its selected
     harness and no unselected harness tree.
   * [ ] Both fixtures contain
     ``.syspilot/templates/change-document.md`` and the required scripts below
     ``.syspilot/skills/<name>/`` with selected-revision bytes.
   * [ ] Change-launcher resolves the shared template and script; impact runs
     ``uv run --no-project
     .syspilot/skills/syspilot.impact-python/scripts/get_need_links.py``.
   * [ ] The OpenCode execution reads no ``.github`` path.

   *Traces to:* ``SYSP_US_UAT_HARNESS_SKILL_ADAPTER`` AC-4

   ---

   **Testability Note:**

   Description-based automatic invocation is model-selected and cannot be
   made deterministic from a fixed prompt. This chain verifies description
   preservation and manual invocation; automatic selection should be
   observed exploratorily and must not be reported as a guaranteed pass.
