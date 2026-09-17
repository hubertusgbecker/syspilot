Harness Deterministic Installation Entry
========================================


.. spec:: Harness Deterministic Installation Entry
   :id: SYSP_SPEC_HARNESS_ONE_TIME_ENABLEMENT
   :status: approved
   :tags: agent-v2, harness, bootstrap, enablement
   :links: SYSP_REQ_HARNESS_ONE_TIME_ENABLEMENT, SYSP_REQ_HARNESS_NATIVE_UPDATE, SYSP_SPEC_SETUP_DUTIES, SYSP_SPEC_SETUP_WORKFLOW

   **Behavior:**

   Initial installation pre-places no Setup file. From the target Git
   repository root, the user runs the remote PEP 723 Installer once for each
   harness used:

   .. list-table::
      :header-rows: 1
      :widths: 20 80

      * - Harness
        - Initial command harness value
      * - VS Code GitHub Copilot
        - ``--harness vscode``
      * - OpenCode
        - ``--harness opencode``
      * - Claude Code
        - Experimental adapter/UAT only; not a production CLI value

   The common command shape is::

      uv run --no-project "https://raw.githubusercontent.com/<owner>/<repository>/<branch>/syspilot/installer.py" install --harness <harness> [--repository <repository>] [--branch <branch>]

   The command creates only the complete selected native target, including
   Setup, plus ``.syspilot/installer.py`` and missing-only documentation
   bootstrap files. A repository using multiple production harnesses runs the
   command separately for each harness.

   **Update flow:** Re-run the identical remote command for the selected
   harness, or invoke installed Setup. Setup directly runs ``uv run
   --no-project .syspilot/installer.py install`` with the same inputs; it never
   delegates to the Installer agent.

   **Source fidelity:** The explicit repository and branch govern the remote
   script and every product fetch. The runtime refreshes its stable local copy
   from the same resolved revision.
