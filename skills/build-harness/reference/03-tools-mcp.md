# Module 3: Tools & MCP

## Core Idea

So far, we've defined who the agents are and what their rules are. But agents can so far only think; they can't yet do anything.

An agent that only reads and writes text is like someone with no access to the actual tools of the trade. It has the knowledge, but not the hands.

**Tools** give an agent hands. They let it:
- Read and write files
- Call APIs
- Inspect external systems
- Validate things
- Use external services

## What a Tool Is

A tool is a function the agent can call. It decides for itself when it needs which tool, based on its task and the context.

From the agent's point of view, a tool looks like this:

```
Name:         read_record
Description:  Reads a record from the system of record
Input:        { "id": "..." }
Output:       The content as structured data
```

The agent reads the description and decides: "I need this information to check whether X already exists. I'll call this tool."

The description is what matters most. A poorly described tool gets used incorrectly by the agent, or not at all.

## What MCP Is

MCP stands for **Model Context Protocol**, an open standard that defines how tools get exchanged between systems.

Think of it as a power outlet:
- In the past, every AI provider had its own tool API, incompatible with all the others
- MCP is the standardized outlet: a tool that speaks MCP works with any agent that understands MCP

That means an MCP server someone else built (e.g. for Figma, GitHub, a database) works instantly in your own harness. You don't have to build it yourself.

```
Agent (Claude, GPT, Gemini...)
    ↕ MCP
MCP Server (Figma, Filesystem, GitHub, internal system...)
    ↕
External System
```

## Two Kinds of Tools

**1. Built-in Tools**
Available directly in the agent system: reading/writing files, running code, web search. These don't need to be configured; they're just there.

**2. MCP Servers**
External servers that provide specialized tools, for exactly the system this field works with. These need to be configured: which servers are available.

## What Tools Does This Harness Need?

Go through the roles from Module 2 and ask for each: what does it need to read? What does it need to write? What external system does it need to access?

| Role | Needs | Tool |
|---|---|---|
| *(from Module 2, role 1)* | *(read/write/check what)* | *(Filesystem / MCP server / Custom)* |
| *(from Module 2, role 2)* | … | … |
| Reviewer | Ability to read everything the others produced | Filesystem (usually enough) |

Two or three tool categories usually cover almost everything; the rest is optional or very specific.

## How Tools Get Configured

Tools aren't defined in the skill files; they're configured in the harness and made available to the agents.

```json
// harness/tools/mcp.config.json
{
  "mcpServers": {
    "<name>": {
      "command": "<mcp-server-command>",
      "description": "What this server is needed for"
    }
  }
}
```

## Tool Descriptions Are Context

This is the most important point in this module: **tools aren't just functions, they're context.**

A good description tells the agent when to use the tool. A bad description leads to the agent ignoring the tool or misusing it.

Bad:
```
Name: get_file
Description: Gets a file
```

Good:
```
Name: get_record
Description: Reads a record from the system of record.
Use this before creating something new, to check whether it already exists.
Returns the full content of the requested record.
```

## Questions For You

- For each role from Module 2: what does it need to read/write/check?
- Which existing tools/systems are already used day-to-day that the agent should have access to? (From the intake, concretize here)
- Does an MCP server already exist for that (official or community), or does something custom need to be built?

## What Gets Built

- `harness/tools/mcp.config.json`: which MCP servers are active
- `harness/tools/tool-descriptions.md`: what each tool does and when a role should use it

## Notes For Running This

- If it's unclear whether an MCP server exists, say so openly. Don't guess or invent one. Research it if possible, or flag it in `HANDOUT.md` as an open item.
- Start small: two well-described tools beat ten half-finished ones.
