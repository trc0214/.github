# GitHub Governance Repository Agent Guidelines

## Scope and precedence

Apply instructions in this order:

1. User instructions for the current task.
2. This repository's `AGENTS.md` and more specific in-repo rules.
3. Governing workspace development rules, when available.
4. Tool defaults.

This repository owns account-wide GitHub defaults for repositories under `trc0214`. It is governance infrastructure, not an application source repository. Do not add project-specific build, runtime, dependency, deployment, or architecture rules here.

## Governance responsibility

- Keep reusable GitHub-supported defaults here when they should apply account-wide, including Discussion category forms, Issue/PR templates, and supported community-health files.
- Repository-local supported files override these defaults. Do not duplicate a universal template into every repository unless GitHub inheritance cannot satisfy a concrete requirement.
- Keep `trc0214/project-template` responsible for files that must physically exist in generated repositories, especially `AGENTS.md`, `README.md`, `.gitignore`, `.env.example`, `.editorconfig`, and `.gitattributes`.
- When changing a shared template, consider both UI-based use and API/CLI/automation paths before declaring the workflow enforced.

## Label governance

- Use GitHub Labels as a lightweight visual classification/indexing layer for Issues, pull requests, and Discussions. Labels do not replace GitHub lifecycle state, Discussion decisions, or AI provenance.
- Prefer GitHub's standard repository labels before inventing a custom taxonomy: `bug`, `documentation`, `duplicate`, `enhancement`, `good first issue`, `help wanted`, `invalid`, `question`, and `wontfix`.
- Account-wide forms should apply stable labels automatically when the meaning is unambiguous: Bug Report -> `bug`; Implementation -> `enhancement`; Ideas Discussion -> `enhancement`.
- Add `documentation` when documentation is a material part of the tracked work. Other standard labels should be added only when their normal GitHub meaning is actually true.
- Do not encode `open`, `closed`, `merged`, `approved`, AI model identity, `Drafted By`, reviewer identity, or other provenance/state already represented by GitHub or the durable record into Labels.
- Create repository-specific custom Labels only when a repeated, durable classification need cannot be expressed by the standard set. Avoid one-off labels and prefix taxonomies that merely duplicate titles, form fields, or native GitHub state.

## Branching and pull requests

- Keep `main` stable. Do not make planned governance changes directly on `main`.
- Use one short-lived task branch per task. When an Issue exists, prefer `task/<issue-number>-<short-name>`; otherwise use a concise `task/<short-name>`.
- The task branch identifies the work, not the current agent. If governance work changes owner while incomplete, continue on the same task branch after verifying its state.
- Keep one active writer per task branch at a time. Use a temporary `experiment/<agent>/<task>` branch only when deliberate isolation is justified for a risky or alternative implementation.
- Merge governance changes through a focused pull request after reading back the changed files and checking that no unrelated account-wide behavior was modified.
- Treat published shared task history as durable: do not force-push or rebase it by default. For multi-AI task branches, default to a merge commit into `main` so original commit SHAs and AI provenance remain available unless the repository explicitly chooses another strategy.

## AI attribution

- GitHub account identity records the operator account and must not be used to infer the actual AI contributor when explicit attribution is required.
- For AI provenance, record the provider/agent and exact model when the runtime exposes it, for example `ChatGPT / GPT-5.6 Sol`. If the exact model is unavailable, write `<provider/agent> / model unknown`; never guess a model from product name, writing style, branch name, or GitHub account.
- Every Issue must identify the human or AI agent/model that produced the initial Issue content.
- Standard Bug/Implementation Issue Forms require the `Drafted By` field.
- If an Issue is created through Blank issue, GitHub CLI, API, or automation without the standard Issue Form, put `Drafted By: <human-or-agent/model>` at the top of the Issue body before creation. Do not create an Issue without initial attribution.
- Treat the initial `Drafted By` as immutable provenance; later handoff, revision, or implementation must not overwrite it.
- If another AI materially changes scope, acceptance criteria, reproduction, impact, dependencies, or other Issue-defining content, add one concise Issue comment beginning with `AI-Contributor: <agent/model>` and `Role: Planning`, `Role: Revision`, or `Role: Synthesis`. Routine wording edits do not require attribution.
- Every pull request with material implementation, test, refactor, configuration, or governance changes must include `Implemented By: <human-or-agent/model>`. If multiple agents materially contributed, list each agent/model with a concise role.
- Every material AI commit must add `AI-Agent: <agent/model>` as a commit-message footer. This is the commit-level provenance used when multiple agents share one task branch or Git identity.
- Task branch names do not encode agent identity and do not replace model-level provenance.
- AI pull-request reviews must begin with `AI-Reviewer: <agent/model>`.

## Handoff

If governance work changes owner while still incomplete, continue on the same task branch and leave concise durable context in the linked Issue or PR:

- `<from-agent/model> → <to-agent/model>`
- completed work
- remaining work
- verification state
- `Last Verified Commit: <SHA>` only when the current HEAD is not fully verified
- known risks only when they exist

Do not duplicate information already recoverable from Git, the Issue, or the PR. The receiving agent must inspect the branch, recent commits, diff, and checks before continuing. If a published commit is bad, prefer `git revert` or selective reuse of good commits over rewriting shared history.
