# FlowZap Code DSL — Official Syntax & Examples

**FlowZap Code** is a simple domain-specific language (DSL) for collaborative business process mapping and workflow automation. Write natural, readable code to create flowcharts, swimlane diagrams, sequence diagrams, and architecture diagrams. No technical skills needed!

- **Website:** https://flowzap.xyz
- **Syntax Guide:** [flowzap.xyz/flowzap-code](https://flowzap.xyz/flowzap-code)
- **AI Guidelines:** [flowzap.xyz/flowzap-code-guidelines.md](https://flowzap.xyz/flowzap-code-guidelines.md)
- **Templates:** [flowzap.xyz/templates](https://flowzap.xyz/templates) — 356 ready-to-use workflow, sequence, architecture and mind map templates
- **MCP Server:** [flowzap.xyz/docs/mcp](https://flowzap.xyz/docs/mcp) — for Claude Desktop, Cursor, Antigravity, Devin Desktop and any other MCP client
- **Agent Skill:** [skills.sh/flowzap-xyz/flowzap-mcp/flowzap-diagrams](https://skills.sh/flowzap-xyz/flowzap-mcp/flowzap-diagrams)
- **npm:** [flowzap-mcp](https://www.npmjs.com/package/flowzap-mcp)

---

## ⚡ Quick example: A wedding planning process
```
couple { # couple
  n1: circle label:"Start Planning"
  n2: rectangle label:"Define Budget & Priorities"
  n3: rectangle label:"Review Proposals"
  n4: rectangle label:"Make Final Decisions"
  n5: circle label:"Wedding Day!"
  n1.handle(right) -> n2.handle(left)
  n2.handle(right) -> n3.handle(left)
  n3.handle(right) -> n4.handle(left)
  n4.handle(right) -> n5.handle(left)
  n2.handle(bottom) -> planner.n6.handle(top) [label="Hires"]
}

planner { # planner
  n6: rectangle label:"Initial Consultation"
  n7: rectangle label:"Create Planning Timeline"
  n8: rectangle label:"Coordinate Vendors"
  n9: rectangle label:"Final Walkthrough"
  n6.handle(right) -> n7.handle(left)
  n7.handle(right) -> n8.handle(left)
  n8.handle(right) -> n9.handle(left)
  n7.handle(bottom) -> vendors.n10.handle(top) [label="Requests"]
}

vendors { # vendors
  n10: rectangle label:"Receive Requirements"
  n11: diamond label:"Available?"
  n12: rectangle label:"Send Proposal"
  n13: rectangle label:"Confirm Booking"
  n10.handle(right) -> n11.handle(left)
  n12.handle(right) -> n13.handle(left)
  n11.handle(right) -> n12.handle(left) [label="No"]
  n11.handle(bottom) -> n12.handle(bottom) [label="Yes"]
  n12.handle(top) -> couple.n3.handle(bottom) [label="Sends"]
}
```

Create your free account on https://flowzap.xyz. Then paste the above into your FlowZap workspace to visualize your first workflow, sequence and architecture diagram in seconds.

---

## 🤝 Contributing

- Fork & submit pull requests with new code examples or suggestions!
- Open issues for syntax questions or documentation fixes.
- See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## 📖 Documentation

- [Syntax Rules](SYNTAX.md) — includes sequence best practices, loops and the mind map rules
- [AI Coding Guidelines](flowzap-code-guidelines.md) — instructions for AI assistants generating FlowZap Code
- [Download AI Guidelines](https://flowzap.xyz/flowzap-code-guidelines.md) — drop this file into your project's `/docs/` folder for AI coding tools
- [Templates Library](https://flowzap.xyz/templates) — 356 ready-to-use templates for workflows, sequences, architecture diagrams and mind maps
- [Architecture Diagram Templates](https://flowzap.xyz/templates/architecture-diagram-templates) — 50 architecture diagram templates (Microservices, Event-Driven, CQRS, Serverless, Saga, and more)
- [n8n Workflow JSON Examples](https://flowzap.xyz/templates/n8n-workflow-json-examples) — FlowZap Code to n8n JSON export
- [Make.com Blueprint Examples](https://flowzap.xyz/templates/make-blueprint-json-examples) — FlowZap Code to Make.com JSON export

### Blog

- [FlowZap Unveils .fz: True Diagrams as Code for Sequence Diagrams and AI-Native Workflows](https://flowzap.xyz/blog/flowzap-unveils-fz-file-extension-for-true-diagrams-as-code-workflows)
- [Supercharge Your Vibe Coding Flow with FlowZap: Turn AI Sparks into Structured Magic](https://flowzap.xyz/blog/supercharge-your-vibe-coding-flow-with-flowzap)
- [Introducing the FlowZap MCP Server](https://flowzap.xyz/blog/introducing-the-flowzap-mcp-server)
- [The FlowZap Diagram Skill Is Live — Here's Why It Matters](https://flowzap.xyz/blog/new-flowzap-diagram-skill-makes-it-even-easier)
- [One Code, Three Views: Flowchart + Sequence + Architecture](https://flowzap.xyz/blog/flowzaps-game-changing-update-one-code-two-views)
- [The Top 10 Workflows for CI/CD Pipeline Configuration](https://flowzap.xyz/blog/top-10-workflows-cicd)

---

## 🤖 For AI Agents & Agentic Browsers

FlowZap provides machine-readable resources for automated discovery:

- **Syntax Schema (JSON):** [`/api/flowzap-code-schema.json`](https://flowzap.xyz/api/flowzap-code-schema.json)
- **Well-Known Discovery:** [`/.well-known/flowzap-code-schema.json`](https://flowzap.xyz/.well-known/flowzap-code-schema.json)
- **LLM Context File:** [`/llms.txt`](https://flowzap.xyz/llms.txt)
- **Full LLM Context:** [`/llms-full.txt`](https://flowzap.xyz/llms-full.txt)
- **AI Guidelines:** [`/flowzap-code-guidelines.md`](https://flowzap.xyz/flowzap-code-guidelines.md)
- **Syntax Documentation:** [`/flowzap-code`](https://flowzap.xyz/flowzap-code)
- **OpenAPI Spec:** [`/.well-known/openapi.json`](https://flowzap.xyz/.well-known/openapi.json)

### MCP Server (v2.2.1)

Install the `flowzap-mcp` npm package for any compatible coding tool:

```json
{
  "mcpServers": {
    "flowzap": {
      "command": "npx",
      "args": ["-y", "flowzap-mcp@2.2.1"]
    }
  }
}
```

Or connect the hosted endpoint, which runs nothing on your machine:

```json
{
  "mcpServers": {
    "flowzap": {
      "url": "https://flowzap.xyz/mcp"
    }
  }
}
```

**Compatible with any MCP client:** Claude Desktop, Claude Code, Cursor, Antigravity, Devin Desktop (formerly Windsurf), OpenAI Codex, Gemini CLI, GitHub Copilot, Warp Terminal, Zed Editor, Cline, Roo Code, Continue.dev.

**13 tools available:** `flowzap_validate`, `flowzap_fix`, `flowzap_create_playground`, `flowzap_get_syntax`, `flowzap_export_graph`, `flowzap_artifact_to_diagram`, `flowzap_diff`, `flowzap_apply_change`, `flowzap_compliance_check`, `flowzap_mindmap_validate`, `flowzap_mindmap_approve`, `flowzap_mindmap_template`, `flowzap_mindmap_create_playground`

### URL Parameters
- `?mode=code` on `/playground/{id}` auto-opens the code editor dialog
- `?code=<base64url>` on `/playground` loads FlowZap Code from URL (no API call needed)

---

## Training Corpus
For a comprehensive dataset of 356 real-world examples tailored for LLM training, see:
[flowzap-xyz/sequence-workflows](https://github.com/flowzap-xyz/sequence-workflows)

---

## License

See [LICENSE](LICENSE).

