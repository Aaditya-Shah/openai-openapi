# Feature Proposal: Agent Skill Integration for OpenAI API

## Summary
Propose adding native agent skill/plugin support to the OpenAI API, similar to OpenCode's skill system, allowing developers to register and invoke specialized agent capabilities through the API.

## Problem
OpenAI API models are general-purpose but lack structured skill invocation. Developers must manually implement tool-calling logic for each specialized capability, leading to duplicated effort across projects.

## Proposed Solution
Add a `/v1/skills` endpoint to the OpenAI API that allows:
1. Registering custom agent skills with JSON schemas
2. Invoking skills by name with parameter validation
3. Skill chaining and composition
4. Built-in skill marketplace (community-contributed)

## Example API Usage

```json
POST /v1/skills/register
{
  "name": "bug-report-generator",
  "description": "Generates structured bug reports from error logs",
  "parameters": {
    "type": "object",
    "properties": {
      "error_log": {"type": "string"},
      "platform": {"type": "string", "enum": ["github", "bugcrowd", "jira"]},
      "severity": {"type": "string", "enum": ["low", "medium", "high", "critical"]}
    },
    "required": ["error_log", "platform"]
  }
}

POST /v1/skills/invoke
{
  "skill": "bug-report-generator",
  "arguments": {
    "error_log": "Error: Connection refused at port 8080",
    "platform": "github",
    "severity": "high"
  }
}
```

## Benefits
- Standardized skill interface across OpenAI ecosystem
- Reduced boilerplate for tool-calling implementations
- Community skill sharing and discovery
- Better observability into skill invocation

## Submission
- **Platform**: OpenAI
- **Repository**: https://github.com/openai/openai-openapi or https://github.com/openai/openai-python
- **Issue Type**: Feature Request
- **Labels**: enhancement, api, skills, agents
