# Cybersecurity Lab Conversion Methodology

This methodology governs the conversion of existing cybersecurity laboratory material into complete, reproducible, assessed instructional labs.

## 1. Analyze the source

Identify the source tasks, learning concepts, assumed infrastructure, artifacts, expected observations, failure modes, privilege requirements, and reset assumptions. Compare relevant source versions and record what is retained, modified, combined, optional, relocated, or omitted.

## 2. Expose hidden prerequisites

Document operating systems, supported architectures, host software, hypervisors, VM or container requirements, packages, privileges, interfaces, routes, services, application state, kernel capabilities, shared storage, and reset mechanisms.

When a host hypervisor is used, produce a dedicated student setup document before writing task instructions. It must explain host prerequisites, VM creation, installation, cloning, unique MAC addresses, adapter settings, network isolation, guest configuration, and completion checks for every supported host platform.

## 3. Select the architecture

Choose the smallest topology that makes the security concept visible. Use distinct VMs, interfaces, accounts, processes, or logical roles when trust boundaries, routing, ownership, or attacker/defender separation matter. Do not preserve containerization merely because the source used containers.

The design must identify:

- every VM or role;
- operating system and architecture;
- host platform and hypervisor;
- interface count and purpose;
- addressing and route behavior;
- Internet access policy;
- isolation boundary;
- expected visibility limitations;
- required reset state.

## 4. Produce and validate the design package

The design gate requires:

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

`topology.mmd` is not complete until it parses, renders, and is inspected against the design brief. Validate VM names, addresses, roles, network boundaries, gateways, and Internet implications.

## 5. Define ownership and movement

For every security-relevant artifact, specify origin, owner, permitted movement, integrity check, secret status, and reset behavior. For every mutable state item, specify baseline owner, permitted student changes, instructor-controlled baseline, destructive actions, and reset method.

## 6. Define the learning lifecycle

Describe what students create, transfer, configure, observe, deliberately break, defend, validate, and reset. Each task must identify:

- objective;
- prerequisite state;
- student-owned work;
- commands or interfaces used;
- expected observation;
- evidence to save;
- interpretation or question;
- cleanup or state transition.

A lab is an experiment with an observable before/after result, not merely a command checklist.

## 7. Define the authoritative student distribution

The final student distribution must contain one authoritative document. It must combine setup, prerequisites, safety, topology, tasks, expected observations, evidence requirements, submission requirements, troubleshooting, cleanup, reset, and attribution.

A modular submission source may be retained for authoring, but it must be merged into the authoritative manual before distribution. The distribution must not contain conflicting student-facing instructions.

## 8. Enforce instructional style

Every student-facing document must define:

- fenced `console` command blocks;
- the meaning of the leading `$` prompt marker;
- placeholder syntax;
- required editor and editor keystrokes;
- `sudo nano` for root-owned files;
- ordinary `nano` for student-owned files;
- exact paths for every created or modified file;
- file ownership and privilege requirements;
- VM or role association for configuration files.

Minimum nano guidance:

```text
Ctrl+O, then Enter: save
Ctrl+X: exit nano
Ctrl+C: cancel the current prompt or operation
```

The document review checklist must confirm that all student-modified files are named and that OS commands use the required command-box convention.

## 9. Maintain a deliverable matrix

Every conversion must maintain a matrix mapping each editable source to its distribution form, required path, and validation status.

At minimum, required PDF counterparts are:

```text
student/student-manual.pdf
student/vmware-setup.pdf       # when a hypervisor is used
instructor/instructor-guide.pdf
instructor/answer-key.pdf
instructor/troubleshooting.pdf
instructor/test-report.pdf
rubric/rubric.pdf
```

A deliverable is incomplete when a required distribution counterpart is missing. The release check must fail when any required PDF or validated structured rubric artifact is absent.

## 10. Define rubric scoring modes

Every rubric criterion must declare `objective`, `qualitative`, or `hybrid`.

### Objective

Use observable and measurable requirements such as file existence, successful parsing, command exit status, PCAP readability, address scope, expected packet presence, transcript content, and required screenshots. Use threshold-based ratings.

### Qualitative

Use anchored descriptions for packet-field analysis, filter explanations, source-versus-transmitter interpretation, protocol comparison, security implications, and other interpretive work.

### Hybrid

Separate implementation evidence from explanation. The component points must sum exactly to the criterion total and be preserved in Markdown, CSV, JSON, and Canvas representations.

## 11. Validate rubric data before packaging

The validator must reject malformed CSV rows, unexpected column counts, missing or duplicate IDs, incorrect rating counts, nonnumeric points, excessive rating points, inconsistent criterion totals, inconsistent assignment totals, invalid hybrid component totals, cross-file disagreement, unanchored objective language, unanchored qualitative criteria, and missing evidence or validation mappings.

Report the exact file, row, criterion ID, rating ID, and field for each failure.

At minimum, verify:

```text
sum(criteria.points) = assignment.total_points
```

## 12. Select and test Canvas representation

Before implementation, select the target Canvas workflow. It may be manual entry, institution-specific import, API payload, QTI, or another format. Do not describe CSV or JSON as directly importable without testing.

Test the selected representation in a disposable assignment. Record the actual procedure, limitations, preserved IDs, preserved points, and any manual corrections in `canvas/import-notes.md`.

## 13. Define automation boundaries

Provisioning may install prerequisites, create directories, create a virtual environment, install packages, and copy starter files. It must not complete security-relevant student decisions or attack logic.

Preflight must detect and explain missing prerequisites, wrong interfaces, incorrect routes, bad isolation, unsupported architectures, and missing privileges.

Validation may inspect source syntax, PCAP readability, expected addresses, file presence, rubric consistency, and observable outcomes. It must not replace student explanations or inject the required experiment for the student.

Reset may stop lab-owned processes, remove confirmed temporary artifacts, clear lab-owned state, and restore documented configuration. It must protect source code and submitted evidence unless explicitly confirmed.

## 14. Package and test

The implementation gate requires:

- provisioning from a clean environment;
- actionable preflight failure messages;
- validation of both success and expected failure states;
- reset testing;
- complete student-path testing;
- PDF generation;
- visual inspection of PDFs;
- Markdown/PDF consistency review;
- rubric schema validation;
- cross-representation point validation;
- Canvas workflow testing;
- platform and architecture testing required by the design.

## 15. Use explicit release status

Every package must include a status record such as:

```yaml
lab_id: example-lab
version: 0.1.0
status: implementation
 gates:
  design: passed
  implementation: pending
  pdf_review: pending
  rubric_validation: pending
  canvas_test: pending
  independent_student_test: pending
  checksums: pending
```

Allowed statuses are:

```text
design
implementation
testing
release-candidate
classroom-approved
```

The package must not be labeled `release-candidate` or `classroom-approved` while a required gate is pending or failed.

## 16. Release artifacts

A release directory must contain the exact distribution artifacts:

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

`RELEASE_NOTES.md` must identify the lab version, supported platforms, tested environments, included files, known limitations, reset limitations, Canvas limitations, security warnings, and changes from the previous release.

## 17. Final consistency audit

Before packaging, compare all artifacts for agreement on:

- lab ID and version;
- VM names, roles, and addresses;
- duration and task numbering;
- edited file names;
- commands and placeholders;
- safety rules;
- evidence requirements;
- assignment total;
- rubric criterion IDs, names, points, modes, ratings, and evidence mappings;
- Canvas representation;
- PDF and Markdown content.

The audit must produce a report and must fail the release check when discrepancies remain.

## 18. Quality gates

### Design gate

Source analysis, design brief, topology source and rendering, ownership models, student workflow, automation boundaries, evidence plan, and rubric outline exist and have been reviewed.

### Implementation gate

Required editable sources, scripts, starter code, authoritative student manual, instructor materials, rubric representations, Canvas materials, and required PDFs exist.

### Classroom-readiness gate

An independent tester completes the student workflow; undocumented prerequisites are removed; intended failures are distinguishable from infrastructure failures; PDFs match Markdown; Canvas entry or import is tested; secrets and instructor-only material are absent; checksums are generated after final packaging.

