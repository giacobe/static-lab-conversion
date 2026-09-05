# Cybersecurity Lab Conversion Methodology

## Purpose

Use this methodology to transform any existing cybersecurity laboratory material into a complete, reproducible, role-separated instructional lab package.

The source may be:

- a published cybersecurity lab;
- an instructor-authored handout;
- a textbook exercise;
- a container-based exercise;
- a virtual-machine exercise;
- a capture-the-flag task;
- a programming or systems-security assignment;
- a collection of notes, scripts, or configuration files.

The goal is not to copy the source instructions. The goal is to reconstruct the learning experience, expose hidden prerequisites, choose an appropriate execution architecture, preserve the security concepts, define student and instructor responsibilities, and produce a tested package that can be used and graded consistently.

## Core design principle

**Analyze the source → expose hidden prerequisites → compare relevant versions or implementations → separate instructional content from infrastructure → choose the topology and execution architecture → define artifact and state ownership → scope the lab → personalize the experience where useful → define automation boundaries → package, test, assess, and release the complete workflow.**

Do not begin by writing commands. First reconstruct the instructional model.

## Governing rules

1. Preserve the source lab's important security concepts, not necessarily its commands or deployment model.
2. Replace infrastructure that obscures the learning objective with infrastructure that makes the objective visible.
3. Automate repetitive prerequisites; leave security-relevant decisions and student work to students.
4. Represent distinct security roles as distinct machines, interfaces, accounts, processes, or explicitly documented logical roles when separation matters.
5. Treat artifact movement and mutable state as part of the instructional design.
6. Scope the lab to the smallest coherent learning narrative.
7. Never silently perform the central experiment in provisioning scripts.
8. Treat the Markdown source as authoritative and render distribution PDFs from controlled source.
9. Do not claim that a Canvas file is importable unless the exact import path has been tested in the target Canvas environment.
10. If required information is missing, identify the missing input explicitly rather than silently inventing it.

# Required inputs

Before design begins, collect as much of the following as available:

- source lab instructions, URLs, files, scripts, or repositories;
- source versions or revision history;
- target course, audience, and skill level;
- learning objectives or course outcomes;
- supported operating system and version;
- hypervisor, container runtime, cloud platform, or physical hardware;
- available CPU, memory, storage, and network resources;
- intended lab duration;
- individual or team workflow;
- student prerequisites;
- Internet availability during setup and execution;
- topics explicitly in scope and out of scope;
- expected evidence and grading method;
- accessibility or platform constraints;
- student-specific information that may personalize the lab;
- target LMS, especially Canvas, and its actual rubric workflow.

If some inputs are unknown, record assumptions in the design brief and mark them for validation.

# Workflow

## 1. Analyze the source lab

Extract:

- formal tasks and subtasks;
- sequence of activities;
- security concept demonstrated by each activity;
- assumed machines, containers, services, accounts, and networks;
- operating-system, package, privilege, and kernel assumptions;
- student-created files and artifacts;
- expected observations and failures;
- optional and advanced phases;
- cleanup and reset assumptions;
- evidence and assessment expectations.

Create `analysis/source-lab-analysis.md` with this table:

| Original task | Student action | Security concept | Hidden prerequisite | Adaptation decision |
|---|---|---|---|---|
|  |  |  |  |  |

For every source phase, choose one decision:

- retain;
- modify;
- combine;
- make an extension;
- move to another lab;
- omit.

Separate:

- content worth preserving;
- infrastructure worth replacing;
- content outside the target scope.

## 2. Expose the prepared environment

List everything the source supplies implicitly:

- operating-system image;
- users, groups, and privileges;
- package installation;
- interfaces, addresses, routes, and name resolution;
- services and daemons;
- application or database state;
- browser profiles, cookies, and trust stores;
- cryptographic tools and configuration;
- kernel capabilities and raw-packet permissions;
- shared directories, volumes, and mounts;
- snapshots and reset mechanisms;
- test data and expected outputs;
- pre-created vulnerabilities, credentials, or secrets.

Classify every item:

| Item | Automate | Prebuild | Student performs | Instructor-only | Reason |
|---|---:|---:|---:|---:|---|
|  |  |  |  |  |  |

## 3. Compare versions and implementations

When multiple source versions exist, use older material to understand the original pedagogical arc and newer material to identify technical improvements.

Compare:

- learning objectives;
- commands and APIs;
- cryptographic or protocol defaults;
- package names;
- application dependencies;
- kernel and firewall behavior;
- certificate or identity validation;
- questions and expected evidence;
- deployment architecture;
- reset and grading support.

Retain technical improvements when they support the learning objective, but do not automatically retain the newer deployment architecture.

## 4. Choose the execution architecture

Select the simplest architecture that makes the security concept visible and is supportable on the target platform.

Possible roles include:

- client;
- server;
- attacker;
- defender;
- certificate authority;
- firewall or router;
- database;
- trusted site;
- malicious site;
- sensor or monitor;
- tunnel endpoint;
- traffic source or target;
- application host.

Use separate VMs when role separation, trust boundaries, routing, or artifact ownership is pedagogically important. Use one VM when additional roles would not improve understanding. Use containers only when they reduce complexity without hiding the concept.

Document the rejected alternatives and why they were rejected.

## 5. Define topology and role boundaries

For every machine, process, account, or logical role specify:

- role;
- hostname or identifier;
- IP address or namespace;
- interfaces;
- network segment;
- routes;
- services;
- packages;
- privileges;
- student-owned actions;
- instructor-owned actions;
- reset behavior;
- Internet-access policy.

Produce:

- `design/topology.md`;
- `design/topology.mmd`;
- a design brief with the final role table.

## 6. Define artifact and state ownership

For every security-relevant artifact or state item specify:

- where it is created;
- who owns it;
- whether it may leave that location;
- where it is transferred or replicated;
- how its integrity is checked;
- whether it contains secrets;
- how it is reset or destroyed.

Produce:

- `design/artifact-ownership.md`;
- `design/state-ownership.md`.

Artifacts may include keys, certificates, packet captures, source code, payloads, policies, configuration files, browser state, database records, logs, and credentials. State may include routes, firewall tables, service state, processes, interfaces, caches, trust stores, and persistent application data.

## 7. Define the adapted learning narrative

Express the lab as a lifecycle rather than a command sequence.

Examples:

- authentication: initialize → enroll → authenticate → observe → invalidate → recover;
- firewall: provision → route → generate traffic → filter → observe → modify → recover → reset;
- packet lab: generate → capture → inspect → construct → inject → observe → compare → reset;
- web security: initialize → establish session → exploit → observe impact → defend → retest → restore;
- tunneling: create interface → route → encapsulate → transmit → decapsulate → deliver → capture → clean up.

Produce `design/student-workflow.md` with this table:

| Stage | Student action | Expected observation | Concept | Evidence | Common failure | Recovery |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |

## 8. Scope the lab deliberately

Define:

- core tasks;
- optional extensions;
- prerequisites or precursor activities;
- topics moved to another lab;
- topics omitted;
- maximum expected duration;
- stopping point for a successful submission.

Do not preserve a phase merely because it exists in the source. Remove phases that require unrelated infrastructure, introduce a different security concept, or overwhelm the target audience.

## 9. Add meaningful personalization

Choose a student-specific parameter only when it affects the experiment or improves assessment.

Possible parameters include:

- university ID or hostname;
- internal subnet;
- service port;
- packet marker;
- firewall rule number;
- account or database record;
- certificate identity;
- tunnel endpoint or port.

Document its format, validation, propagation, and grading use in `design/personalization.md` when applicable.

## 10. Define automation boundaries

Create four operational script categories:

```text
scripts/
├── provision/
├── preflight/
├── validate/
└── reset/
```

### Provision

May install packages, create users and directories, configure baseline networking, install harmless starter files, create test data, and establish baseline services.

Must not complete the central security task, silently configure the intended attack, or hide a decision students are expected to make.

### Preflight

Must check platform version, packages, hostnames, interfaces, addresses, routes, service status, privileges, kernel settings, files, ports, isolation, and personalization values.

Every failure message should state what failed, why it matters, how to correct it, and how to rerun the check.

### Validate

Should test observable outcomes and support both successful and deliberately failing states. Validation does not replace student explanations.

### Reset

Must restore configuration, routes, policies, services, application and database state, browser/session state where practical, interfaces, generated artifacts, and relevant logs.

Each reset operation must state whether it is safe during the lab, destructive, instructor-only, or suitable for returning to the initial state.

## 11. Define evidence and assessment

Each evidence item must map to:

- a learning objective;
- a student task;
- a rubric criterion;
- a validation or grading method.

Prefer evidence such as:

- command output;
- configuration excerpts;
- hashes or fingerprints;
- logs;
- packet captures;
- before/after comparisons;
- code;
- validation output;
- short explanations tied to observations.

Screenshots may supplement evidence but should not be the only proof when text or machine-readable evidence is available.

## 12. Produce the complete instructional package

The authoritative editable sources should include:

```text
analysis/source-lab-analysis.md
design/design-brief.md
design/topology.md
design/topology.mmd
design/artifact-ownership.md
design/state-ownership.md
design/student-workflow.md
design/automation-boundaries.md
design/personalization.md                 # when applicable
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

The final release should include controlled distribution files:

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

# Output specifications

## Student-facing instructions

`student/student-manual.md` is the authoritative editable source. It must include:

1. title, version, and estimated time;
2. scenario and purpose;
3. learning objectives;
4. prerequisites;
5. safety and isolation warnings;
6. topology and role assignments;
7. preparation and preflight;
8. numbered tasks;
9. expected observations;
10. required evidence;
11. conceptual questions;
12. troubleshooting appropriate for students;
13. reset and cleanup;
14. submission requirements;
15. acceptable-use and academic-integrity notes where appropriate.

It must not include instructor answers, hidden grading notes, recovery credentials, or completed solutions when discovery is the objective.

The distribution PDF must be rendered from controlled source and checked visually. The Markdown and PDF must match in task numbering, commands, evidence requirements, version, and lab identifier.

## Instructor outputs

`instructor/instructor-guide.md` must cover setup, timing, preparation, allocation, expected outcomes, recovery, reset, grading, platform differences, and extensions.

`instructor/answer-key.md` must include expected answers, acceptable variations, expected technical outcomes, common misconceptions, and full-credit evidence.

`instructor/troubleshooting.md` must use:

| Symptom | Likely cause | Diagnostic command | Correction | Recovery/reset |
|---|---|---|---|---|

`instructor/test-report.md` must record platform, hypervisor, VM specifications, network mode, Internet state, clean-start conditions, task results, timing, failures, workarounds, reset results, document review, and LMS review.

## Canvas rubric outputs

Produce three synchronized rubric representations:

- `rubric/rubric.md` — human-readable rubric;
- `rubric/rubric.csv` — one row per criterion-rating combination;
- `rubric/rubric-canvas.json` — structured source representation, not automatically assumed to be Canvas-importable.

The CSV must contain:

```text
criterion_id
criterion_order
criterion_name
criterion_description
criterion_points
learning_objective
evidence_requirement
validation_method
rating_id
rating_order
rating_name
rating_description
rating_points
grader_notes
```

Also produce `canvas/rubric-entry.md` as a copy-ready manual-entry table and `canvas/import-notes.md` documenting the tested Canvas process, limitations, and final verification responsibilities.

Do not claim generic CSV or JSON import compatibility without testing it in the target Canvas environment.

`rubric/rubric-check.md` must verify criterion counts, rating counts, stable IDs, point totals, evidence mappings, and consistency across Markdown, CSV, JSON, and Canvas entry materials.

## Submission requirements

`student/submission-requirements.md` must define exact filenames, formats, directory structure, accepted evidence, screenshot policy, required explanations, archive policy, prohibited secrets, and cleanup expectations.

A recommended structure is:

```text
submission/
├── README.md
├── answers.md
├── evidence/
├── captures/
├── configs/
├── code/
└── validation/
```

Students must not submit private keys, passwords, reusable credentials, browser cookies, instructor-only files, or unnecessary VM images.

# Quality gates

## Design gate

Do not implement until the source analysis, design brief, topology, ownership model, scope decision, workflow, automation boundaries, evidence plan, and rubric outline exist.

## Implementation gate

Do not distribute until provisioning works from a clean environment, preflight detects missing prerequisites, validation detects expected success and failure states, reset restores the baseline, documents match tested behavior, and rubric totals are correct.

## Classroom-readiness gate

Before release:

- an independent tester follows the student manual;
- no undocumented prerequisite blocks progress;
- intended security failures are distinguishable from infrastructure failures;
- Markdown and PDF agree;
- PDF layout is visually inspected;
- Canvas entry or import is tested in the target environment;
- private keys, credentials, and instructor-only content are absent from release files;
- checksums and release notes are present.

# Versioning

Use a stable lab identifier such as `firewall-exploration`, `packet-sniffing-spoofing`, `sql-injection`, `csrf`, `xss`, or `vpn-tunneling`.

Use semantic versions:

- major: changed learning workflow, topology, or assessment;
- minor: new task, platform support, or significant documentation change;
- patch: wording, typo, or nonfunctional correction.

The version must appear in editable documents, PDFs, rubrics, test reports, and release notes.

# Collaboration protocol

For a new conversion, work in this order:

1. identify and collect source materials;
2. produce the source analysis;
3. produce the design brief and scope decision;
4. produce topology and ownership models;
5. produce the student workflow and evidence plan;
6. define automation boundaries;
7. draft scripts and starter materials;
8. draft student and instructor documents;
9. produce rubric and Canvas materials;
10. test from a clean environment;
11. revise from test findings;
12. render PDFs and assemble the release;
13. run final consistency and secret checks.

The assistant should not jump directly to a final student manual when the design-stage outputs do not yet exist. It should identify missing inputs and assumptions explicitly.