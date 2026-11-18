# AI Infrastructure Agent Lab

## Project Overview

This AI Infrastructure Agent Lab is an experimental environment designed to evaluate the practical capabilities of locally hosted large language models (LLMs) when given controlled access to real infrastructure within an isolated sandbox network.

Unlike typical AI demonstrations that focus on code generation or web application development, this project explores whether a local AI agent can perform tasks commonly associated with systems administration, infrastructure engineering, platform operations, and technical troubleshooting.

This project uses a locally hosted Ollama instance integrated with Visual Studio Code and an agent-capable extension. The AI operates within a dedicated sandbox environment where it can safely interact with virtual machines, services, networking components, and intentionally disposable infrastructure without impacting production systems.

The long-term objective is to determine how effectively a local AI model can:

- Understand infrastructure documentation
- Execute administrative tasks
- Deploy and configure services
- Troubleshoot failures
- Recreate existing environments
- Generate realistic training scenarios
- Create and manage self-contained practice laboratories

This project is not intended to replace human administration. Instead, it serves as a learning platform to evaluate the strengths and limitations of autonomous AI systems operating in real-world infrastructure environments.

---

# Objectives

### Primary Objectives

- Evaluate local AI models as infrastructure assistants.
- Measure the AI's ability to perform administrative tasks using available tools.
- Assess the reliability of AI-driven infrastructure changes within a controlled environment.
- Compare AI-generated solutions against manual implementations.

### Secondary Objectives

- Create repeatable training environments.
- Generate troubleshooting scenarios for hands-on learning.
- Produce Capture-the-Flag (CTF) and security-focused lab exercises.
- Explore the future role of AI agents in infrastructure and operations workflows.

---

# Planned Phases

## Phase 1 - Environment Preparation

### Goal

Establish a safe and isolated environment for AI experimentation.

### Tasks

- Deploy dedicated AI workstation VM.
- Install and configure Ollama.
- Configure VS Code integration.
- Select and test local language models.
- Validate agent functionality.
- Restrict all activities to the sandbox VLAN.
- Create disposable target VMs for testing.

### Success Criteria

The AI can interact with the workstation and perform basic file and terminal operations.

---

## Phase 2 - Tool Usage Evaluation

### Goal

Determine what actions the AI can perform using available tools.

### Tasks

- Create, edit, and delete files.
- Execute terminal commands.
- Review command output.
- Perform simple administrative tasks.
- Install software packages.
- Configure basic services.

### Success Criteria

The AI can successfully complete small administrative tasks with minimal guidance.

---

## Phase 3 - Infrastructure Deployment

### Goal

Evaluate the AI's ability to deploy infrastructure components.

### Tasks

- Deploy Docker.
- Create Docker Compose configurations.
- Deploy NGINX.
- Deploy Grafana.
- Deploy monitoring services.
- Validate service availability.
- Troubleshoot deployment failures.

### Success Criteria

The AI can reliably deploy and validate basic infrastructure services.

---

## Phase 4 - Environment Replication

### Goal

Determine whether the AI can recreate an existing environment using documentation.

### Tasks

- Provide architecture documentation.
- Provide service inventories.
- Provide network diagrams.
- Provide deployment requirements.
- Instruct the AI to recreate selected portions of the homelab.

### Success Criteria

The AI creates a functional approximation of the documented environment without direct step-by-step instructions.

---

## Phase 5 - Autonomous Troubleshooting

### Goal

Evaluate the AI's ability to diagnose and repair infrastructure issues.

### Tasks

- Introduce service failures.
- Create networking issues.
- Break application configurations.
- Simulate storage and monitoring failures.
- Observe diagnostic and remediation behavior.

### Success Criteria

The AI identifies root causes and proposes or executes corrective actions.

---

## Phase 6 - AI-Generated Training Labs

### Goal

Use the AI to create unknown learning environments.

### Tasks

- Generate Linux troubleshooting challenges.
- Generate networking scenarios.
- Generate Active Directory exercises.
- Generate Docker deployment labs.
- Store answer keys separately from the student environment.

### Success Criteria

The environment creator (AI) knows the solution while I do not.

---

## Phase 7 - AI-Generated Security Labs

### Goal

Use the AI to create realistic offensive and defensive security exercises within the sandbox environment.

### Tasks

- Create intentionally vulnerable systems.
- Place flags within target environments.
- Create privilege-escalation paths.
- Create multi-system investigation scenarios.
- Maintain hidden solution documentation.

### Success Criteria

The AI can generate novel challenges where solutions are unknown to the me prior to investigation.

---

## End State Vision

A fully isolated AI-assisted infrastructure laboratory where a locally hosted LLM can:

- Deploy systems
- Configure services
- Troubleshoot failures
- Recreate documented environments
- Create training exercises
- Generate security challenges

while allowing the myself to evaluate both AI capabilities and their own infrastructure, troubleshooting, and security skills through hands-on practice.

---


