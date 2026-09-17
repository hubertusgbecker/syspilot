Harness Portability
====================

Behavioral equivalence of syspilot's methodology across AI coding harnesses.


.. story:: Harness-Portable Methodology
   :id: SYSP_US_HARNESS_PORTABILITY
   :status: approved
   :priority: mandatory
   :tags: agent-v2, harness, portability
   :links: SYSP_US_AGENT_ARCH, SYSP_US_SKILL_ARCH

   **As a** syspilot user working in a production-supported harness other than
   VS Code Copilot Chat (OpenCode),
   **I want to** invoke syspilot's agents and skills and receive the same
   guidance and results as a VS Code Copilot Chat user does today,
   **so that** my choice of AI coding harness does not lock me out of the
   methodology.

   **Context:**

   syspilot's agents and skills today are expressed exclusively as VS Code
   custom-agent files bound to Copilot Chat's tool and model conventions.
   This story requires that the methodology content — what each agent does,
   what each skill teaches — be authored once and installed, without
   behavioral drift, onto every supported harness.

   Each target harness's actual current extension/plugin/hook mechanism is
   researched at design time (Level 2), rather than assumed from prior
   knowledge. Full equivalence is the goal for every harness, but it is not
   a precondition for shipping this change: a harness whose own mechanism
   cannot cleanly support a given capability is documented as a known,
   honestly-disclosed limitation rather than blocking the rest of the CR.
   Partial success (e.g. full equivalence on production-supported harnesses
   while another remains explicitly experimental) is an acceptable outcome.
   Claude Code and Qoder remain experimental until each completes live native
   invocation together with clean-install, update, and rollback acceptance.

   **Acceptance Criteria:**

   1. Given a production-supported harness (OpenCode or VS Code GitHub Copilot), When I invoke a syspilot Manager agent, Then I receive the same Soul/Duties/Workflow-driven behavior as on any other production-supported harness
   2. Given a production-supported harness, When I consult a syspilot Skill, Then its Instructions and Rules are applied identically to how they are applied on VS Code Copilot Chat
   3. Given the methodology content (agent and skill definitions) is authored once in ``syspilot/``, When it is installed onto any target harness, Then no harness-specific fork of the behavioral content is maintained — only deterministic structural/frontmatter adapters differ per harness
   4. Given a target harness's own extension/plugin mechanism cannot cleanly support full behavioral equivalence for a given capability, When that limitation is discovered at design time, Then it is documented as a known, honestly-disclosed limitation for that harness rather than blocking support on the other harnesses
   5. Given a change is made for harness portability, When existing VS Code
      Copilot Chat users invoke agents, Skills, or commands, Then they see no
      regression in methodology behavior
   6. Given a harness has not completed clean-install, update, native
      invocation, and rollback acceptance, When support is documented, Then
      that harness is labeled experimental rather than production-supported
   7. Given a production-supported harness, When a syspilot workflow invokes
      another agent or resolves a shipped Skill resource, Then the installed
      artifact uses that harness's native orchestration binding and a stable
      harness-neutral shared path without forking the semantic methodology
