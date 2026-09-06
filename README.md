# Cybersecurity Lab Conversion

This repository contains a reusable workflow for converting cybersecurity laboratory material into complete, reproducible, assessed instructional labs.

## Governing principle

The conversion must preserve the source lab's security concepts while making infrastructure, role boundaries, artifact ownership, mutable state, automation, evidence, assessment, testing, and release status explicit.

Markdown files are editable authoring sources. PDFs are controlled distribution artifacts. A conversion is not complete when only Markdown sources exist.

## Required final deliverables

Every conversion must maintain a deliverable matrix mapping each editable source to its required distribution form, repository path, and validation status.

| Source artifact | Required distribution form | Required path | Validation |
|---|---|---|---|
| `student-manual.md` | Markdown and PDF | `student/`, `output/` | PDF matches Markdown in tasks, commands, evidence, version, and lab ID |
| `vmware-setup.md` or equivalent host setup guide | Markdown and PDF | `student/`, `output/` | Clean student-path setup test |
| `instructor-guide.md` | Markdown and PDF | `instructor/`, `output/` | Instructor review |
| `answer-key.md` | Markdown and PDF | `instructor/`, `output/` | Answer review |
| `troubleshooting.md` | Markdown and PDF | `instructor/`, `output/` | Failure-path review |
| `test-report.md` | Markdown and PDF | `instructor/`, `output/` | Test status and blockers reviewed |
| `rubric.md` | Markdown and PDF | `rubric/`, `output/` | Points, modes, and criteria validated |
| `rubric.csv` | CSV | `rubric/`, `output/` | Schema, row count, IDs, modes, and points validated |
| Canvas rubric representation | Tested target format | `canvas/` | Tested in a disposable Canvas assignment |
| `topology.mmd` | Mermaid source and rendered image | `design/`, `output/` | Source parses and rendered topology reviewed |
| Release notes | Markdown | `release/` | Version, gates, limitations, and included files checked |
| Checksums | SHA-256 text | `release/` | Recomputed after packaging |

A release check must fail if any required PDF or validated structured rubric artifact is absent.

## Repository structure

```text
.
├── README.md
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
├── output/
├── release/
└── tests/
```

## Required design-stage outputs

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

`topology.mmd` must be rendered and inspected; source existence alone is insufficient.

## Required instructional and assessment outputs

```text
student/student-manual.md
student/student-manual.pdf
student/vmware-setup.md       # required when a host hypervisor is used
student/vmware-setup.pdf
instructor/instructor-guide.md
instructor/instructor-guide.pdf
instructor/answer-key.md
instructor/answer-key.pdf
instructor/troubleshooting.md
instructor/troubleshooting.pdf
instructor/test-report.md
instructor/test-report.pdf
rubric/rubric.md
rubric/rubric.pdf
rubric/rubric.csv
rubric/rubric-canvas.json
rubric/rubric-check.md
canvas/assignment.md
canvas/module-item.md
canvas/rubric-entry.md
canvas/import-notes.md
```

The final student distribution must have one authoritative student-facing document containing setup, prerequisites, safety, tasks, evidence, submission, cleanup, reset, and attribution. A separate submission source may exist for authoring, but it must be merged before distribution and must not conflict with the authoritative manual.

## Host hypervisor requirement

Whenever a lab uses a host hypervisor, the student package must include a dedicated setup document covering:

- host operating-system prerequisites;
- supported x86_64 and ARM64 architectures where applicable;
- VMware Workstation Pro or equivalent Windows instructions;
- VMware Fusion or equivalent macOS instructions;
- VM creation, installation, cloning, and unique virtual MAC addresses;
- adapter connection and power-on settings;
- NAT and isolated/private network configuration;
- guest hostname and operating-system configuration;
- static or dynamic addressing;
- routing and forwarding requirements;
- completion checks;
- troubleshooting and reset guidance.

## Student-document requirements

The authoritative student manual must include purpose, scenario, objectives, prerequisites, safety, topology, preparation, preflight, numbered tasks, expected observations, required evidence, conceptual questions, troubleshooting, cleanup, reset, submission, and attribution.

Every student-facing manual must define and consistently use these conventions:

- OS commands appear in fenced `console` blocks;
- a leading `$` is a prompt marker and is not typed;
- placeholders use an explicit form such as `<LAN_IF>`;
- the required editor is named explicitly;
- root-owned files are opened with `sudo nano`;
- student-owned source files are opened with ordinary `nano`;
- every file students must create or modify is named explicitly;
- ownership and privilege requirements are stated;
- save, confirm, and exit keystrokes are documented;
- administrative commands are visibly marked;
- each configuration file is associated with its VM or role.

A document review or lint check must detect the required command-box convention and identify all student-modified files.

## Topology validation

Topology validation must verify:

```text
[ ] topology.mmd exists
[ ] Mermaid syntax parses
[ ] rendered topology image exists
[ ] all required VMs are present
[ ] all assigned addresses are present
[ ] roles match the design brief
[ ] NAT and the experiment network are distinct
[ ] private-LAN gateway behavior is shown correctly
[ ] the diagram does not imply unauthorized Internet experimentation
```

## Rubric requirements

Every criterion must declare one scoring mode:

```text
objective
qualitative
hybrid
```

Objective criteria use measurable checks and explicit thresholds. Qualitative criteria use anchored explanations. Hybrid criteria separate observable implementation evidence from interpretation.

The CSV must contain one row per criterion-rating combination, stable criterion and rating IDs, criterion points, scoring mode, learning-objective mapping, evidence requirement, validation method, rating description, rating points, and grader notes.

The rubric validator must fail on:

1. malformed rows or unexpected column counts;
2. missing or duplicate criterion IDs;
3. missing or duplicate rating IDs;
4. incorrect rating counts;
5. nonnumeric points;
6. rating points greater than criterion points;
7. criterion totals that do not equal the assignment total;
8. hybrid component totals that do not equal criterion totals;
9. disagreement among Markdown, CSV, JSON, Canvas, and assignment points;
10. objective criteria using unanchored qualitative language;
11. qualitative criteria lacking explanation anchors;
12. missing evidence or validation mappings.

At minimum:

```text
sum(criteria.points) = assignment.total_points
```

Validation errors must identify the exact file, row, criterion ID, and field.

## Canvas requirements

Select one canonical Canvas workflow before implementation: manual entry, institution-specific import, API payload, QTI, or another tested format. Test it in a disposable assignment before release.

`canvas/import-notes.md` must describe the tested workflow and must not claim generic CSV or JSON import compatibility without evidence. If manual entry is required, label the artifact clearly as manual-entry guidance rather than a direct import file.

## Script boundaries

Provisioning may install prerequisites and create the workspace but must not complete the central student task. Preflight must provide actionable failure messages. Validation must check observable outcomes without replacing explanations. Reset must restore the baseline and document destructive behavior.

## Release gates

A package may not be called final or classroom-approved until all required gates pass:

- design review;
- clean-environment provisioning;
- actionable preflight;
- observable validation success and failure checks;
- reset testing;
- PDF generation and visual inspection;
- Markdown/PDF consistency review;
- rubric schema and cross-representation validation;
- Canvas workflow testing;
- independent student-path testing;
- platform and architecture testing required by the design;
- security and privacy review;
- checksums generated after packaging.

Each release must include a machine-readable status record identifying passed, pending, and failed gates.

## Versioning and release

Use semantic versions and include the lab ID and version in source documents, PDFs, rubrics, test reports, and release notes. Release notes must identify supported platforms, tested environments, included files, known limitations, reset limitations, Canvas limitations, security warnings, and changes from the previous release.

