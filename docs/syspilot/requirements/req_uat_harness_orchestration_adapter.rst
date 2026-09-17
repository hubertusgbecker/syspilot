Harness Orchestration Adapter Test Data
=======================================

Test data requirements for
``SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER``.


.. req:: UAT Test Data: Harness Orchestration Fallback
  :id: SYSP_REQ_UAT_HARNESS_ORCHESTRATION_ADAPTER
  :status: approved
  :priority: mandatory
  :tags: uat, harness, orchestration, fallback, test-data
  :links: SYSP_US_UAT_HARNESS_ORCHESTRATION_ADAPTER

   **Fixtures:**

   * ``F-ORCH-VSCODE`` and ``F-ORCH-OPENCODE``: clean production targets.
   * ``F-ORCH-JARVIS-PRESENT``: production target containing ``.jarvis/`` to
     prove that installation remains synchronous.
   * ``F-ORCH-CLAUDE`` and ``F-ORCH-QODER``: experimental static adapter
     fixtures whose results do not establish production support.

   **Required Artifacts and Tools:**

   * Both mutually exclusive orchestration Skills from the source product.
   * Deterministic VS Code, OpenCode, and Claude generated variants whose
     designated binding sections map SEND to ``runSubagent``, native Task, and
     native Agent respectively.
   * A Manager fixture whose Workflow SENDs one deterministic request to one
     Engineer fixture, and an Engineer whose response contains a fixed token
     ``ORCHESTRATION-RESULT``.
   * Installed OpenCode for production live UAT and authenticated Claude Code
     for the experimental invocation/delegation promotion gate; Qoder is
     required only when its pending experimental live UAT is executed.
   * A file-listing tool and access to each harness's task/subagent execution
     transcript.

   Every fixture SHALL be reset before its scenario. Manager and Engineer
   bodies SHALL be identical across harnesses except for frontmatter. The
   orchestration Skill's semantic definitions, rules, inputs, outputs, and
   ordering SHALL be identical; only its designated native-binding section may
   differ.
