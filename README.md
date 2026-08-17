# QANEX - Setup & Usage Guide

QANEX is a QA agent for generating detailed SWMS test cases from Jira
requirements. This guide explains how to set up QANEX in VS Code and how
to use it after installation.


------------------------------------------------------------------------

## 1. Prerequisites

Before using QANEX, make sure the following are available in your
development environment:

-   Visual Studio Code
-   GitHub access
-   Access to the SWMS Jira project
-   Access to SWMS Confluence documentation
-   Access to QMetry test cases
-   Access to the SWMS QE Phase 2 UI repository
-   The MCP/tool integrations required by the QANEX agent
-   Python available in the environment used for Excel generation
-   `openpyxl` Python package for `.xlsx` generation

The QANEX specification references these tool integrations:

-   Atlassian MCP server
-   QMetry knowledge base: https://github.com/SyscoCorporation/SWMS_AI_MCP
-   GitHub MCP

------------------------------------------------------------------------

## 2. Install QANEX

QANEX is configured as a VS Code agent/prompt file.

Create the QANEX agent file:

``` text
QANEX.agent.md
```

Place it in the VS Code prompts/agents directory:

``` text
C:\Users\<USERNAME>\AppData\Roaming\Code\User\prompts\agents\
```

The file must contain the QANEX agent configuration and instructions
provided with this project.

------------------------------------------------------------------------

## 3. Agent Configuration

The beginning of `QANEX.agent.md` defines the agent metadata.

The important configuration is:

``` yaml
---
name: "QANEX"
description: "Use when generating, designing, or writing test cases for a Jira ticket in SWMS."
argument-hint: "Jira ticket ID (e.g. SMOD-1234)"
tools:
  - com.atlassian/atlassian-mcp-server/*
  - qmetry-knowledge-base/*
  - github/*
user-invocable: true
---
```

This makes QANEX available as a user-invocable agent and gives it access
to the tools required by its workflow.

------------------------------------------------------------------------

## 4. Configure Required Connections

QANEX depends on external systems to retrieve requirements and
supporting information.

Make sure the corresponding connections/authentication are configured in
your VS Code/agent environment.

### Jira / Atlassian

QANEX needs access to:

-   Jira issues
-   Jira descriptions
-   Acceptance criteria
-   Labels
-   Components
-   Priorities
-   Status
-   Linked issues
-   Attachments and embedded references

The QANEX workflow uses Jira information as the primary requirement
source.

### Confluence

QANEX needs access to relevant SWMS Confluence pages for:

-   Business rules
-   Functional flows
-   Navigation paths
-   Screen names
-   Validation rules
-   Field constraints
-   Data creation guidance

The agent also references the SWMS UI navigation guidance page specified
in the agent configuration.

### QMetry

QANEX uses QMetry to search existing test cases.

This is used to:

-   Understand existing test patterns
-   Follow established QA conventions
-   Identify already-covered scenarios
-   Avoid unnecessary duplicate test cases

### GitHub

QANEX uses the relevant repository when implementation details
need to be confirmed.

Repository:

``` text
https://github.com/SyscoCorporation/swms-qe-phase2-ui.git
```

Codebase searches should be used when necessary to clarify
implementation-dependent acceptance criteria rather than
indiscriminately searching the entire repository.

------------------------------------------------------------------------

## 5. Verify the Installation

After placing `QANEX.agent.md` in the agents directory and configuring
the required connections:

1.  Open VS Code.
2.  Open the Chat/Agent interface.
3.  Check that **QANEX** is available as a user-invocable agent.
4.  If it does not appear, verify that the agent file is located in the
    correct prompts/agents directory.
5.  Verify that the required MCP/tool connections are available.
6.  Restart or reload VS Code if the agent list has not refreshed.

------------------------------------------------------------------------

## 6. Using QANEX

QANEX accepts a Jira ticket ID as the primary input.

### Recommended Prompt Format

The simplest recommended format is:

``` text
Generate test cases for <JIRA-TICKET-ID>
```

For a specific scope:

``` text
Generate test cases for <JIRA-TICKET-ID>, only for <SCENARIO/AC/FEATURE>
```

Examples:

``` text
Generate test cases for OPCOF-6568
```

``` text
Generate test cases for OPCOF-6568, only for AC2
```

``` text
Generate test cases for OPCOF-6568, only for field validation
```

------------------------------------------------------------------------

## 7. What QANEX Does Automatically

After receiving the Jira ticket, QANEX follows its configured workflow.

### Step 1: Load SWMS Domain Context

QANEX first loads SWMS terminology, acronyms, and cross-reference
guidance.

### Step 2: Read the Jira Ticket

QANEX retrieves the ticket and analyzes:

-   Summary
-   Description
-   Acceptance criteria
-   Labels
-   Components
-   Priority
-   Status
-   Linked issues
-   Attachments
-   Embedded screenshots/references

### Step 3: Read Linked Jira Issues

Linked issues are checked for additional:

-   Requirements
-   Dependencies
-   Constraints
-   Business behavior

### Step 4: Read Confluence References

Relevant Confluence documentation is checked for:

-   Business rules
-   Functional flows
-   Navigation
-   UI screens
-   Validation behavior
-   Field constraints
-   Data setup instructions

### Step 5: Validate Implementation When Required

QANEX may inspect the SWMS QE Phase 2 UI repository to confirm
implementation-dependent behavior.

The agent can search for:

-   Symbols
-   SQL/table names
-   API routes
-   UI labels
-   Validation logic
-   Enum values
-   Error messages

### Step 6: Search Existing QMetry Test Cases

Existing test cases are reviewed to prevent duplicate coverage and
maintain established QA patterns.

### Step 7: Analyze Coverage

QANEX determines:

-   Which scenarios are required
-   Which scenarios can be combined
-   Which scenarios need separate test cases
-   Which negative/boundary cases apply

### Step 8: Generate Test Cases

The final output is generated as one continuous Markdown table.

### Step 9: Export to Excel

QANEX generates an Excel workbook containing the same test cases.

------------------------------------------------------------------------

## 8. Excel Generation Requirement

The QANEX environment must have `openpyxl` available.

Check:

``` bash
pip show openpyxl
```

If it is not installed:

``` bash
pip install openpyxl -q
```

QANEX then uses `openpyxl` to generate and format the workbook.

------------------------------------------------------------------------

## 9. Troubleshooting

### QANEX does not appear in VS Code

Check:

``` text
C:\Users\<USERNAME>\AppData\Roaming\Code\User\prompts\agents\
```

Confirm that:

``` text
QANEX.agent.md
```

exists in that directory.

Then reload/restart VS Code.

------------------------------------------------------------------------

### Jira information cannot be retrieved

Check:

-   Atlassian connection
-   Jira permissions
-   Jira project access
-   MCP tool availability

QANEX requires access to the Jira issue before generating test cases.

------------------------------------------------------------------------

### Confluence information cannot be retrieved

Check:

-   Atlassian/Confluence connection
-   Confluence permissions
-   Access to the referenced SWMS documentation

------------------------------------------------------------------------

### Existing QMetry cases cannot be searched

Check that the QMetry knowledge-base integration is available and
authenticated.

------------------------------------------------------------------------

### GitHub implementation validation fails

Check:

-   GitHub authentication
-   Repository access
-   Repository availability

Repository:

``` text
https://github.com/SyscoCorporation/swms-qe-phase2-ui.git
```

Implementation validation should only be required when the Jira
requirement needs code-level clarification.

------------------------------------------------------------------------

### Excel generation fails

Check that Python is available:

``` bash
python --version
```

Then check:

``` bash
pip show openpyxl
```

Install it if required:

``` bash
pip install openpyxl -q
```

------------------------------------------------------------------------

## 10. Typical End-to-End Usage

A typical QE workflow is:

``` text
1. Open VS Code.
2. Open the QANEX agent.
3. Provide the Jira ticket ID.
4. Specify partial scope if full coverage is not required.
5. Allow QANEX to retrieve the Jira requirements and supporting references.
6. Review the generated test cases.
7. Review the generated Excel file.
8. Use the test cases for the required QA/QMetry workflow.
```

------------------------------------------------------------------------


## 11. Before Using QANEX

Use this checklist:

-   [ ] `QANEX.agent.md` is installed in the VS Code agents directory.
-   [ ] Atlassian/Jira access is configured.
-   [ ] Confluence access is configured.
-   [ ] QMetry access is configured.
-   [ ] GitHub access is configured.
-   [ ] Repository is accessible.
-   [ ] Python is available.
-   [ ] `openpyxl` is installed.
-   [ ] QANEX is visible as a user-invocable agent.
-   [ ] The Jira ticket ID is available before starting the test-case
    generation request.

------------------------------------------------------------------------

## 12. Reference

The complete behavior, workflow, test-case rules, coverage rules, and
output requirements are defined in the QANEX agent specification in
`QANEX.agent.md`.
