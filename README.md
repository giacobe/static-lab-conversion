# Cybersecurity Lab Conversion

This repository contains a reusable workflow for converting existing cybersecurity laboratory instructions into complete, reproducible, assessed instructional labs.

The source material may come from any provider or format, including:

- published cybersecurity labs;
- instructor-authored exercises;
- textbook activities;
- container- or VM-based labs;
- capture-the-flag tasks;
- programming assignments;
- systems-security exercises;
- scripts, notes, or configuration files.

The conversion process preserves the source lab's important security concepts while making its hidden infrastructure, role boundaries, artifact ownership, automation, evidence, assessment, and release requirements explicit.

## What this repository produces

A completed conversion should include:

- source-lab analysis;
- design brief and scope decision;
- topology and role specification;
- artifact and mutable-state ownership model;
- student workflow;
- automation boundaries;
- provision, preflight, validation, and reset scripts;
- student-facing Markdown instructions;
- student-facing PDF instructions;
- instructor guide, answer key, troubleshooting guide, and test report;
- Canvas-ready assignment and rubric materials;
- machine-readable rubric data;
- release notes and checksums.

The Markdown files are the editable sources. PDFs are controlled distribution artifacts. Rubric CSV and JSON files are structured representations and should not be assumed to be directly importable into Canvas without testing.

## Methodology

The governing workflow is documented in:

```text
cybersecurity-lab-conversion-methodology.md
```

Use that document as the primary context when starting a new conversion in a fresh AI session or when asking a collaborator to extend this repository.

A useful bootstrap instruction is:

> Use `cybersecurity-lab-conversion-methodology.md` as the governing workflow. Convert the supplied cybersecurity lab material into a complete, role-separated, reproducible instructional package. First analyze the source and produce the design-stage documents. Do not jump directly to final student instructions. Preserve security concepts while replacing unnecessary infrastructure complexity. Produce the required editable sources, PDFs, validation, reset, rubric, Canvas, and release artifacts.

## Repository structure

```text
.
├── cybersecurity-lab-conversion-methodology.md
├── analysis/
├── design/
├── student/
├── instructor/
├── scripts/
│   ├── provision/
│   ├── preflight/
│   ├── validate/
│   └── reset/
├── configs/
├── starter-code/
├── validation/
├── rubric/
├── canvas/
├── release/
└── tests/
```

## Required editable outputs

The design phase should produce:

```text
analysis/source-lab-analysis.md
design/design-brief.md
design/topology.md
design/topology.mmd
design/artifact-ownership.md
design/state-ownership.md
design/student-workflow.md
design/automation-boundaries.md
design/personalization.md       # when applicable
```

The instructional and assessment phase should produce:

```text
student/student-manual.md
student/submission-requirements.md
instructor/instructor-guide.md
instructor/answer-key.md
instructor/troubleshooting.md
instructor/test-report.md
rubric/rubric.md
rubric/rubric.csv
rubric/rubric-canvas.json
rubric/rubric-check.md
canvas/assignment.md
canvas/module-item.md
canvas/rubric-entry.md
canvas/import-notes.md
```

## Release outputs

A release directory should contain the exact distribution artifacts:

```text
release/
├── <lab-id>-student-instructions.pdf
├── <lab-id>-student-instructions.md
├── <lab-id>-instructor-guide.pdf
├── <lab-id>-rubric.pdf
├── <lab-id>-rubric.csv
├── <lab-id>-rubric-canvas.json
├── checksums.sha256
└── RELEASE_NOTES.md
```

`RELEASE_NOTES.md` must identify the lab version, supported platform, tested environment, included files, known limitations, reset limitations, Canvas limitations, security warnings, and changes from the previous release.

## Conversion workflow

### 1. Analyze the source

Identify the original tasks, learning concepts, assumed infrastructure, artifacts, expected observations, failure modes, and reset assumptions. Decide whether each source phase is retained, modified, combined, made optional, moved elsewhere, or omitted.

### 2. Expose hidden prerequisites

Document operating systems, packages, privileges, interfaces, routes, services, application state, browser state, trust stores, databases, kernel capabilities, shared storage, and reset mechanisms that the source provides implicitly.

### 3. Compare versions or implementations

Use older versions to understand the original learning arc and newer versions to identify technical improvements. Preserve improvements that support learning, but do not automatically preserve a containerized or otherwise opaque deployment architecture.

### 4. Select the architecture

Choose the smallest topology that makes the security concept visible. Use distinct VMs, interfaces, accounts, processes, or logical roles when trust boundaries, routing, ownership, or attacker/defender separation matter.

### 5. Define ownership and movement

For each security-relevant artifact and mutable state item, specify its origin, owner, permitted movement, integrity check, secret status, and reset behavior.

### 6. Define the learning lifecycle

Describe what students create, transfer, configure, observe, deliberately break, defend, validate, and reset. A good lab is an experiment with an observable before/after result, not merely a command checklist.

### 7. Scope and personalize

Retain the smallest coherent lab narrative. Add a meaningful student-specific parameter only when it affects the experiment or improves assessment.

### 8. Automate the right things

Automate repetitive prerequisites and recovery. Leave security-relevant decisions, attack implementation, rule writing, protocol construction, certificate issuance, code changes, and explanations to students when those are the learning objectives.

### 9. Package and test

Produce editable Markdown, rendered PDFs, instructor materials, rubrics, Canvas materials, scripts, and release metadata. Test the complete workflow from a clean environment, including expected failures and reset.

## Script boundaries

Use four script categories:

```text
scripts/
├── provision/
├── preflight/
├── validate/
└── reset/
```

Provisioning must not silently complete the central student task. Preflight should provide actionable failure messages. Validation should check observable outcomes without replacing explanations. Reset should restore the baseline and document destructive behavior.

## Student document requirements

`student/student-manual.md` is the authoritative editable source and must include:

- purpose and scenario;
- learning objectives;
- prerequisites;
- safety and isolation warnings;
- topology and roles;
- preparation and preflight;
- numbered tasks;
- expected observations;
- required evidence;
- conceptual questions;
- troubleshooting;
- reset and cleanup;
- submission requirements.

It must not contain instructor answers, hidden grading notes, recovery credentials, or completed solutions where discovery is intended.

The distribution PDF must match the Markdown in task numbering, commands, evidence requirements, version, and lab identifier. Check code blocks, tables, diagrams, page breaks, links, headers, footers, and readability before release.

## Canvas rubric requirements

Provide three synchronized representations:

```text
rubric/rubric.md
rubric/rubric.csv
rubric/rubric-canvas.json
```

Also provide:

```text
canvas/rubric-entry.md
canvas/import-notes.md
rubric/rubric-check.md
```

The CSV must use stable criterion and rating IDs and include one row per criterion-rating combination. At minimum, include criterion name, description, points, learning-objective mapping, evidence requirement, validation method, rating name, rating description, rating points, and grader notes.

`canvas/rubric-entry.md` must be copy-ready for manual Canvas entry. `canvas/import-notes.md` must document the actual tested Canvas process and must not claim generic CSV or JSON import compatibility without evidence.

The rubric check must verify:

```text
sum(criteria.points) = assignment.total_points
```

It must also check IDs, rating counts, evidence mappings, and consistency across all rubric representations.

## Quality gates

### Design gate

Do not implement until source analysis, design brief, topology, ownership model, scope decision, student workflow, automation boundaries, evidence plan, and rubric outline exist.

### Implementation gate

Do not distribute until provisioning works from a clean environment, preflight detects missing prerequisites, validation detects expected success and failure states, reset restores the baseline, and documents match tested behavior.

### Classroom-readiness gate

Before release:

- an independent tester follows the student instructions;
- undocumented prerequisites are removed or documented;
- intended failures are distinguishable from infrastructure failures;
- the Markdown and PDF agree;
- the PDF has been visually inspected;
- Canvas entry or import is tested in the target environment;
- private keys, credentials, cookies, and instructor-only content are absent;
- release files are checksummed.

## Versioning

Use a stable lab identifier, such as:

```text
firewall-exploration
packet-sniffing-spoofing
sql-injection
csrf
xss
vpn-tunneling
```

Use semantic versions:

- major: changed topology, learning workflow, or assessment;
- minor: added task, platform support, or significant documentation;
- patch: wording, typo, or nonfunctional correction.

Include the version in source documents, PDFs, rubrics, test reports, and release notes.

## Security and privacy

All converted labs must clearly define:

- network isolation requirements;
- acceptable-use boundaries;
- whether Internet access is permitted;
- credentials used only for the lab;
- secrets and private artifacts that must not be submitted;
- cleanup requirements;
- whether the lab may affect systems outside the isolated environment.

Use intentionally local or reserved names and addresses when appropriate. Do not direct students to attack public systems.

## Starting a new conversion

Provide the assistant or collaborator with:

1. the source instructions, files, URLs, or repository;
2. the target course and audience;
3. the desired platform and resource limits;
4. the intended duration;
5. the topics to retain and omit;
6. grading and evidence requirements;
7. personalization requirements;
8. Canvas constraints;
9. any existing repository conventions.

Then request the design-stage outputs first. Review those outputs before requesting scripts, student instructions, PDFs, rubrics, or release files.