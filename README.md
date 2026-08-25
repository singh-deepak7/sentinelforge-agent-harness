# SentinelForge

SentinelForge is a human-governed autonomous production incident response agent built with TrueForge.

The project is being developed for the WeMakeDevs Agent Harness Hackathon.

## Overview

Production incidents often require engineers to manually correlate information across logs, metrics, deployment history, configuration changes, and other operational systems before they can identify a likely root cause.

SentinelForge is designed to coordinate this investigation through an AI agent while keeping sensitive production-changing actions under explicit human control.

The goal is to enable an incident-response workflow that can:

1. Understand a reported production incident.
2. Gather evidence from available operational tools.
3. Correlate evidence across multiple systems.
4. Distinguish confirmed facts from hypotheses.
5. Validate root-cause hypotheses using safe diagnostic capabilities.
6. Propose the safest supported remediation.
7. Require explicit human approval before sensitive production changes.
8. Apply an approved remediation through controlled tools.
9. Verify service recovery before declaring the incident resolved.

## Why TrueForge

SentinelForge uses TrueForge as the agent harness responsible for coordinating the incident-response agent and its capabilities.

As the project evolves, TrueForge will provide the orchestration layer through which SentinelForge can use:

- MCP-based operational tools
- Reusable incident-response skills
- Controlled diagnostic capabilities
- Human approval gates
- Subagents for specialized investigation
- Persistent incident sessions

The agent is intentionally designed so that reasoning alone is not treated as sufficient evidence for production actions.

## Safety Principles

SentinelForge follows several core operational safety principles:

- Prefer read-only investigation before remediation.
- Never fabricate logs, metrics, deployments, configuration changes, infrastructure state, or tool results.
- Never claim to have inspected a system unless an available tool actually provided that information.
- Clearly separate confirmed evidence from assumptions and hypotheses.
- Do not treat temporal correlation alone as proof of causation.
- Explicitly identify missing evidence or unavailable tooling.
- Prefer reversible remediation over destructive actions.
- Require explicit human approval before sensitive production-changing operations.
- Verify service health after remediation before declaring an incident resolved.

Sensitive actions may include:

- Rollbacks
- Service restarts
- Configuration changes
- Deployments
- Infrastructure modifications
- Resource deletion
- Code merges

## Planned Architecture

SentinelForge is being developed incrementally during the hackathon.

The planned architecture is:

```text
                    ┌─────────────────────────┐
                    │        TrueForge        │
                    │                         │
                    │ SentinelForge Incident  │
                    │       Commander         │
                    └────────────┬────────────┘
                                 │
               ┌─────────────────┼─────────────────┐
               │                 │                 │
               ▼                 ▼                 ▼
        Observability       Deployment        Diagnostic /
            MCP            Intelligence         Validation
                                MCP                MCP
               │                 │                 │
               └─────────────────┼─────────────────┘
                                 │
                                 ▼
                         Evidence Synthesis
                                 │
                                 ▼
                        Root-Cause Hypothesis
                                 │
                                 ▼
                       Controlled Validation
                                 │
                                 ▼
                        Remediation Proposal
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │    HUMAN APPROVAL     │
                     └───────────┬───────────┘
                                 │
                              Approved
                                 │
                                 ▼
                         Remediation MCP
                                 │
                                 ▼
                         Recovery Verification
                                 │
                                 ▼
                         Incident Resolution
```

> The diagram represents the target architecture. Individual capabilities are being added incrementally through reviewed pull requests.

## Planned Capabilities

### Incident Observability

Retrieve operational evidence such as:

- Incident details
- Service metrics
- Application logs
- Post-remediation health indicators

### Deployment Intelligence

Investigate recent production changes including:

- Deployment history
- Deployment details
- Configuration changes
- Differences between known-good and current configurations

### Controlled Validation

Validate root-cause hypotheses using safe diagnostic and validation capabilities before remediation is proposed.

### Human-Governed Remediation

SentinelForge will propose remediation based on collected evidence but will require explicit human approval before sensitive production-changing operations are executed.

### Recovery Verification

After an approved remediation is applied, SentinelForge will verify service health and relevant operational metrics before declaring the incident resolved.

### TrueForge Skills

Reusable incident-response procedures will be represented as TrueForge Skills so investigation methodology is explicit and repeatable.

### Subagent Investigation

Specialized investigation tasks may be delegated to focused subagents, such as observability and deployment investigators, while the Incident Commander coordinates the overall response.

### Persistent Incident Sessions

Incident context will be maintained through TrueForge sessions so investigations can continue without losing relevant operational context.

## Repository Structure

The repository is organized around independently evolving agent capabilities:

```text
sentinelforge-agent-harness/
├── agents/
│   └── sentinelforge-incident-commander.json
├── docs/
├── mcp/
├── services/
├── shared/
├── skills/
├── scripts/
├── tests/
├── .env.example
├── .gitignore
├── .nvmrc
├── package.json
└── README.md
```

Additional directories will be populated as their corresponding capabilities are implemented.

## Incident Commander

The initial agent definition is located at:

```text
agents/sentinelforge-incident-commander.json
```

The Incident Commander is responsible for coordinating investigation and recovery while enforcing the project's operational safety principles.

The baseline agent intentionally starts without production connectors, sandbox access, or subagents. These capabilities will be introduced incrementally as they are implemented and reviewed.

## Development

### Prerequisites

Node.js 22 or newer is required.

If you use `nvm`:

```bash
nvm use
```

Verify your environment:

```bash
node --version
npm --version
```

### Install Dependencies

At the current foundation stage:

```bash
npm install
```

Additional package-specific installation instructions will be documented as services and MCP servers are introduced.

## Running TrueForge

Start TrueForge locally:

```bash
npx @truefoundry/trueforge
```

Then open the local TrueForge interface:

```text
http://localhost:8790
```

The SentinelForge agent and its connectors will be configured through TrueForge as their corresponding capabilities are implemented.

## Environment Configuration

Copy the environment template when local configuration is required:

```bash
cp .env.example .env
```

Never commit `.env` or real credentials to the repository.

The current environment template contains:

```text
OPENAI_API_KEY=
SENTINELFORGE_SIMULATOR_URL=http://localhost:3010
```

## Development Workflow

Substantive changes are developed through feature branches and GitHub pull requests.

The expected workflow is:

```text
Feature Branch
      │
      ▼
GitHub Pull Request
      │
      ▼
Qodo Code Review
      │
      ▼
Address / Evaluate Findings
      │
      ▼
Follow-up Review
      │
      ▼
Merge
```

This allows implementation decisions and review feedback to remain visible in the repository history.

## Project Status

SentinelForge is under active development during the WeMakeDevs Agent Harness Hackathon.

Current foundation:

- [x] Repository structure
- [x] Baseline SentinelForge Incident Commander
- [x] Human-approval safety requirements
- [x] Development and review workflow
- [ ] Incident Observability MCP
- [ ] Deployment Intelligence MCP
- [ ] Controlled validation
- [ ] Production remediation
- [ ] Shared production simulator
- [ ] Recovery verification
- [ ] Incident-response Skill
- [ ] Sandbox diagnostics
- [ ] Subagent investigation
- [ ] Persistent-session demonstration
- [ ] End-to-end incident-response demo

The checklist will be updated as capabilities are implemented through reviewed pull requests.

## Qodo Code Review Evidence

SentinelForge uses Qodo to review substantive implementation changes before they are merged.

Representative reviewed pull requests and the corresponding findings, engineering decisions, fixes, and follow-up reviews will be documented here as development progresses.

## AI-Assisted Development Disclosure

AI coding assistants are used during development for implementation assistance, debugging, architecture discussions, testing, and documentation.

AI-generated suggestions are reviewed, understood, and validated by the project author before being incorporated into the project.

## License

This project is licensed under the APACHE License.