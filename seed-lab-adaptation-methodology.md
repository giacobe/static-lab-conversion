# SEED Lab Adaptation Methodology

## Purpose

Use this methodology to transform an existing SEED Lab into a modern, role-separated, non-containerized instructional lab without merely copying its commands or reproducing every original phase.

The goal is to preserve the original security concepts while making hidden infrastructure explicit, choosing a topology that reflects security roles, and producing a complete teaching package.

## Core design principle

**Analyze SEED → expose hidden prerequisites → compare versions → replace unnecessary infrastructure complexity → choose topology from security roles → define artifact ownership → scope the lab → personalize the experience → package and test the complete workflow.**

Do not begin by writing commands. First reconstruct the instructional model.

## When to use this methodology

Use it when adapting an existing SEED Lab or similar prepared-environment lab into:

- student-built Ubuntu VMs;
- a non-containerized environment;
- a role-separated topology;
- a current software baseline;
- a lab with setup, preflight, validation, reset, grading, and instructor materials.

This methodology is appropriate for labs such as firewalling, packet sniffing/spoofing, web security, VPN tunneling, PKI, and related network-security topics.

## Required inputs

Before designing the lab, collect:

- the original SEED Lab instructions;
- the latest relevant SEED version or version-difference notes;
- the target course level and prerequisites;
- available hypervisor and VM resources;
- supported Ubuntu release;
- intended lab duration;
- topics explicitly in scope and out of scope;
- whether students work individually or in teams;
- grading and evidence requirements;
- whether Internet access is available during setup and disabled during execution.

## Workflow

### 1. Analyze the original lab

Extract:

- formal tasks;
- sequence of activities;
- security concept demonstrated by each task;
- assumed machines, containers, services, and networks;
- student-created files and artifacts;
- expected observations and failures;
- optional and advanced phases;
- cleanup and reset assumptions.

Create a table:

| Original task | Student action | Security concept | Hidden prerequisite | Keep, modify, or omit |
|---|---|---|---|---|
|  |  |  |  |  |

Do not assume every original phase belongs in the adapted lab.

### 2. Expose the prepared environment

List everything the original environment supplies implicitly:

- operating-system image;
- user accounts and privileges;
- package installation;
- network interfaces and routes;
- hostnames and name resolution;
- services and daemons;
- application/database state;
- browser profiles and cookies;
- cryptographic tools and configuration;
- kernel capabilities or raw-packet permissions;
- shared directories and mounted volumes;
- snapshots, reset mechanisms, and cleanup.

Classify each item:

| Item | Automate | Prebuild | Student performs | Instructor-only |
|---|---:|---:|---:|---:|
|  |  |  |  |  |

### 3. Compare historical and current versions

Use older SEED material to understand the original pedagogical arc. Use newer SEED material to identify technical improvements such as:

- current cryptographic commands;
- stronger algorithms and key sizes;
- SANs and modern certificate validation;
- updated package names;
- changed firewall or kernel behavior;
- revised application dependencies;
- improved questions and expected evidence.

Preserve improved technical content when appropriate, but do not automatically preserve the newer deployment architecture.

Separate:

- **content worth retaining**;
- **infrastructure worth replacing**;
- **content that no longer fits the course scope**.

### 4. Choose the execution architecture

Select the simplest architecture that makes the security concept visible.

Prefer separate VMs when role separation is pedagogically important. Use one VM only when multiple roles would not obscure the concept.

Possible roles include:

- certificate authority;
- client;
- server;
- attacker;
- firewall/router;
- database;
- trusted web site;
- malicious web site;
- tunnel endpoint;
- traffic source or target.

Reject containerization when it adds lifecycle, networking, or filesystem complexity without supporting a learning objective.

### 5. Design the topology from security roles

For every VM, specify:

- role;
- hostname;
- IP address;
- interfaces;
- network segment;
- routes;
- services;
- packages;
- student-owned actions;
- reset behavior.

Use a topology that makes trust boundaries and traffic paths obvious.

Example specification:

| VM | Role | Interface/network | Address | Services | Student-owned actions |
|---|---|---|---|---|---|
|  |  |  |  |  |  |

### 6. Define artifact and state ownership

For every security-relevant artifact or state item, specify:

- where it is created;
- who owns it;
- whether it may leave that machine;
- where it is transferred;
- how its integrity is checked;
- how it is reset or destroyed.

Use this table:

| Artifact/state | Created on | Owner | May leave? | Destination | Integrity/evidence |
|---|---|---|---:|---|---|
|  |  |  |  |  |  |

Make prohibited transfers explicit. Examples include private keys, firewall recovery credentials, browser session state, database secrets, and tunnel endpoint secrets.

Artifact movement should be visible when the movement itself teaches the concept. Prefer explicit tools such as `scp` over invisible shared folders when appropriate.

### 7. Define the adapted learning narrative

Express the lab as a lifecycle, not a command sequence.

Examples:

- PKI: create → request → approve → issue → transfer → deploy → trust → validate → clean up
- Firewall: provision → route → generate traffic → filter → observe → modify → recover → reset
- Packet lab: generate → capture → inspect → construct → inject → observe → compare → reset
- Web security: initialize → establish session → exploit → observe impact → defend → retest → restore
- VPN: create interface → route → encapsulate → transmit → decapsulate → deliver → capture → clean up

For each stage define:

- student action;
- expected observation;
- concept demonstrated;
- evidence required;
- common failure;
- recovery procedure.

### 8. Scope the lab deliberately

Preserve the smallest coherent subset of the original lab that supports a complete learning narrative.

Explicitly decide what to:

- retain in the core lab;
- convert into an optional extension;
- move to another lab;
- omit entirely.

Do not preserve a phase merely because it exists in SEED. Remove phases that require a different topology, introduce unrelated infrastructure, or overwhelm the intended learning objective.

### 9. Add meaningful personalization

Choose one student-specific parameter that affects the experiment and appears in validation.

Possible parameters:

- university ID or hostname;
- internal subnet;
- service port;
- packet marker;
- firewall rule number;
- account or database record;
- tunnel endpoint or port.

Propagate it through relevant configuration, artifacts, test commands, and grading evidence. Do not use personalization merely as decoration.

### 10. Define automation boundaries

Create four operational script categories:

1. **Provision/install** — packages, baseline files, users, permissions.
2. **Preflight** — verify prerequisites, addresses, routes, services, and versions.
3. **Validate** — check student-created results and expected security behavior.
4. **Reset/cleanup** — restore services, files, databases, routes, trust stores, and interfaces.

Automate repetitive infrastructure work. Leave security-relevant decisions, attack implementation, rule writing, certificate issuance, code changes, or protocol construction to students.

### 11. Define evidence and assessment

Require evidence that demonstrates understanding, not screenshots alone.

Possible evidence:

- command output;
- packet captures;
- configuration excerpts;
- hashes or fingerprints;
- before/after comparisons;
- logs;
- browser or client results;
- validation tables;
- short explanations of why behavior changed.

Every major task should have:

- expected result;
- required evidence;
- conceptual question;
- grading criterion;
- recovery hint.

### 12. Package the complete teaching product

Produce both student-facing and instructor-facing materials.

Recommended repository:

```text
lab-name/
├── README.md
├── student/
│   ├── instructions.md
│   ├── questions.md
│   └── submission-requirements.md
├── instructor/
│   ├── setup.md
│   ├── answer-key.md
│   ├── troubleshooting.md
│   └── test-report.md
├── scripts/
│   ├── install-prerequisites.sh
│   ├── configure-role.sh
│   ├── preflight.sh
│   ├── validate.sh
│   └── reset.sh
├── configs/
├── starter-code/
├── precursor/
├── rubric/
└── canvas/
```

### 13. Test from a clean state

Perform three tests:

1. **Happy path:** a competent student follows the instructions.
2. **Expected failure:** the student makes the security-relevant mistake the lab is meant to reveal.
3. **Recovery path:** the student can repair or reset the environment.

Test on the exact Ubuntu release, browser, hypervisor, and package versions students will use.

Record every undocumented assumption. Convert each one into an instruction, preflight check, prerequisite, or instructor note.

## Design rules

1. Preserve learning objectives, not implementation details.
2. Use newer SEED versions for technical improvements, not as mandatory architecture.
3. Choose topology from trust and traffic roles.
4. Make ownership and prohibited artifact movement explicit.
5. Do not let setup scripts perform the central security task.
6. Separate expected security failures from infrastructure failures.
7. Keep the core lab small enough to finish reliably.
8. Make reset possible after every destructive or stateful experiment.
9. Personalize only when the parameter improves identity, engagement, or grading.
10. Treat student and instructor documentation as separate products.
11. Prefer reproducibility over cleverness.
12. Validate behavior, not merely file presence.

## Standard design brief

Before implementation, produce this brief:

```text
Lab title:
SEED source/version(s):
Primary security claim:
Secondary concepts:
Target audience:
Expected duration:
Prerequisites:
Ubuntu release:
Hypervisor:
VM count:
Student/team model:
In-scope tasks:
Out-of-scope tasks:
VM roles:
Network segments and addresses:
Security-relevant artifacts:
Artifact ownership rules:
Student-specific parameter:
Automated setup:
Student-owned work:
Preflight checks:
Validation checks:
Reset strategy:
Required evidence:
Grading structure:
Known compatibility risks:
Optional extensions:
```

## Collaboration protocol for future work

When asked to adapt a new SEED Lab, proceed in this order:

1. Examine the original and current SEED instructions.
2. Produce the task/concept/hidden-prerequisite analysis.
3. Identify the pedagogical arc and candidate topology.
4. Ask only for missing constraints that materially affect the design; otherwise make a stated assumption.
5. Produce the design brief.
6. Review scope and VM roles before writing implementation details.
7. Define artifact ownership and automation boundaries.
8. Draft the student workflow and evidence requirements.
9. Implement provisioning, preflight, validation, and reset.
10. Produce instructor materials, rubric, and Canvas package.
11. Test the complete lab and revise based on failures.

Do not jump directly from a SEED URL to final student instructions.

## Quality gate

The adaptation is ready for implementation only when:

- the primary security claim is stated in one sentence;
- each VM has a clear pedagogical role;
- hidden prerequisites are documented;
- student-owned security actions are separated from automation;
- artifact ownership is explicit;
- core and optional phases are distinguished;
- reset and recovery are defined;
- evidence and grading are specified;
- the lab can be completed without undocumented instructor knowledge.
