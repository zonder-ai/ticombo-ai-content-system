# Implementation Guide for Ticombo Two-Agent Content System

## Overview

This system uses two AI agents to efficiently generate content:

1. **Research Agent**: Uses a more affordable AI model to gather and organize information
2. **Article Writer Agent**: Uses a premium AI model to transform research into polished content

This approach significantly reduces costs by:
- Using a cheaper model for research-intensive tasks
- Reducing the number of research API calls
- Focusing the expensive model only on writing tasks

## How to Set Up the System

### Step 1: Configure the Research Agent

1. Use a cost-effective model (Claude Instant, GPT-3.5 Turbo, etc.)
2. Apply the `research-agent-prompt.md` as the system prompt
3. Enable access to the Tavily research tool
4. Configure the agent to output markdown format

### Step 2: Configure the Article Writer Agent

1. Use your premium model (Claude 3 Opus, GPT-4, etc.)
2. Apply the `article-writer-prompt.md` as the system prompt
3. No need to enable the Tavily research tool for this agent
4. Configure the agent to output JSON format

### Step 3: Create the Workflow

1. Input: Send the Target Article Structure and entity name to the Research Agent
2. Middleware: Collect the markdown research document from the Research Agent
3. Output: Send the research document and Target Article Structure to the Article Writer Agent
4. Final: Collect and use the JSON output from the Article Writer Agent

## Sample Workflow

```
User Input (Entity: "FC Barcelona") + Target Structure
     |
     V
Research Agent (Uses Tavily)
     |
     V
Markdown Research Document
     |
     V
Article Writer Agent (No Tavily calls)
     |
     V
Final JSON Article
```

## Tips for Optimal Performance

1. **Provide Clear Initial Instructions**: Include the entity name and any specific focus areas
2. **Review Research Output**: Occasionally check the research document to ensure quality
3. **Maintain the Target Structure**: Pass the same Target Structure to both agents
4. **Cache Research**: For frequently requested entities, consider caching research to avoid redundant API calls
5. **Batch Processing**: For multiple similar entities (e.g., teams in the same league), consider batching research tasks

## Troubleshooting

- **Insufficient Content**: If the final article is too short, adjust the Research Agent prompt to gather more detailed information
- **Inconsistent Information**: Ensure the Target Structure is identical for both agents
- **Missing Sections**: Verify that the Research Agent is covering all sections in the Target Structure

## Cost Optimization

- The Research Agent can use a model with 1/3 to 1/10 the cost of premium models
- Tavily API calls are reduced by focusing research queries
- The system can be further optimized by implementing caching for frequently requested entities