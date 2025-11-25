# flowzap-code-guidelines.md

> Instructions for AI coding assistants (Cursor, Windsurf, Warp, Google AI Studio, etc.) on how to generate valid FlowZap Code to be saved in `.fz` files and rendered by FlowZap.xyz.

***

## Core output rules

- Always output **only** raw FlowZap Code, with no Markdown fences, no explanations, and no surrounding text.
- Do not prepend or append comments like “Here is your code” or ``` fences; return the code body exactly as-is.
- Use UTF‑8 plain text only; no emojis, no non-printable characters.

---

## High-level structure of a FlowZap diagram

- A FlowZap diagram is a plain-text description composed of lanes, nodes, edges, and optional loop fragments.
- Each lane groups related steps for one actor or system (e.g., `User`, `Application`, `Server`).
- Nodes define the steps or decisions; edges define the directional flow between nodes.
- Loops describe repeating behavior, such as retries, and can involve nodes across lanes.

---

## Global constraints (very important)

- Node IDs must follow the pattern `n1`, `n2`, `n3`, … and must be globally unique across **all** lanes in a single diagram.
- Only four shapes are allowed for nodes: `circle`, `rectangle`, `diamond`, `taskbox`.
- Node attributes use `key:"value"` with a colon, while edge labels use `label="value"` with an equals sign inside square brackets.
- Do not generate any Mermaid, PlantUML, or other diagram syntaxes such as `graph TD`, `sequenceDiagram`, `participant`, or `-->`.
- Do not invent new attribute keys; only use `label`, `owner`, `description`, `system`, and similar English keys described here.

---

## Lanes (participants / swimlanes)

- Each lane starts with a lane identifier followed by an opening brace and a single display-label comment.

```
User { # User
...
}
```

- Lane IDs are simple identifiers like `User`, `Application`, `Server`, `flow`, `process`, `ops`.
- Exactly one comment is allowed per lane, immediately after the opening brace, using `# Display Label`.
- Do not place comments anywhere else in the code except this one allowed lane display label.

**Do / Don’t for lanes**

**Do:**

- `User { # User`
- `flow { # Flow`

**Don’t:**

- `// User` or comments outside the opening brace
- Multiple `#` comments in the same lane block

---

## Nodes (steps, decisions, tasks)

- Each node is defined on its own line with the format: `nX: shape label:"Text"` plus optional attributes.

```
n1: circle label:"Start"
n2: rectangle label:"Enter username and password"
n3: diamond label:"Valid format?"
n4: taskbox owner:"Alice" description:"Deploy" system:"CI"
```

- Allowed shapes: `circle` (start/end), `rectangle` (process/action), `diamond` (decision), `taskbox` (assigned task with owner/system).
- Use colon (`:`) between the node ID and the shape, and colon again for all node attributes (e.g., `label:"..."`, `owner:"..."`).
- Node labels must be enclosed in double quotes, e.g., `label:"Start"`, not `label="Start"`.
- Keep node labels concise but descriptive, typically under 50 characters.

**Do / Don’t for nodes**

**Do:**

- `n1: circle label:"Start"`
- `n3: diamond label:"Is valid?"`
- `n1: taskbox owner:"Alice" description:"Deploy" system:"CI"`

**Don’t:**

- `n1: circle label="Start"` (wrong separator)
- `n3 diamond "Is valid?"` (missing colons)
- Using unknown attributes like `priority:"high"` (unless explicitly documented)

---

## Edges (connections between nodes)

- Edges always connect a `.handle(direction)` of one node to a `.handle(direction)` of another node.

```
n1.handle(right) -> n2.handle(left)
n2.handle(bottom) -> Application.n3.handle(top) [label="Send"]
```

- Basic format for edges: `sourceNode.handle(direction) -> targetNode.handle(direction) [label="Text"]`
- Directions must be one of: `left`, `right`, `top`, `bottom`.
- For cross-lane edges, prefix the target with the lane name, e.g., `Application.n6.handle(top)`.
- Edge labels use **equals** with brackets, e.g., `[label="Yes"]`, never colon: `[label:"Yes"]`.
- Edge labels are optional; if omitted, just write the edge without the `[label="..."]` part.

**Do / Don’t for edges**

**Do:**

- `n1.handle(right) -> n2.handle(left) [label="Go"]`
- `n3.handle(bottom) -> laneB.n5.handle(top)`
- `n2.handle(bottom) -> Application.n6.handle(top) [label="Send"]`

**Don’t:**

- `n1.handle(right) -> n2.handle(left) [label:"Go"]` (wrong equals sign)
- `n3.handle(bottom) -> n5` (missing lane in cross-lane edge)
- `n2 -> n6 [label="Send"]` (no handles)

---

## Cross-lane connections

- When connecting from a node in one lane to a node in another, prefix the target node with the lane ID.

```
n3.handle(bottom) -> Application.n6.handle(top) [label="Send credentials"]
n8.handle(top) -> User.n2.handle(bottom) [label="No - Error"]
```

- Keep the edge definition inside the braces of the **origin** lane.
- Never omit the lane prefix for cross-lane targets; always write `LaneId.nX`.

---

## Loops (LOOP fragments)

- Use the `loop` keyword to describe repeating behavior, such as retries.

```
loop [retry until valid format] n2 n3 n7 n8
loop [retry up to 3 times] n2 n3 n4
```

- Basic format: `loop [condition] n1 n2 n3 ...` where the condition is plain text and the following tokens are node IDs.
- Conditions should be short and descriptive, such as `[retry until valid format]` or `[repeat 3 times]`.
- List the nodes involved in the loop in execution order; they may be in one or multiple lanes.
- Do not nest loops; keep them flat and defined after the relevant nodes.

---

## Minimal complete templates

Use these as skeletons when the user gives a vague prompt; adapt labels and edges to the requested flow.

**Single-lane minimal flow**

```
process { # Process
n1: circle label:"Start"
n2: rectangle label:"Step"
n3: circle label:"End"
n1.handle(right) -> n2.handle(left)
n2.handle(right) -> n3.handle(left)
}
```

**Two-lane user–app interaction**

```
user { # User
n1: circle label:"Start"
n2: rectangle label:"Submit"
n1.handle(right) -> n2.handle(left)
n2.handle(bottom) -> app.n3.handle(top) [label="Send"]
}
app { # App
n3: rectangle label:"Process"
n4: circle label:"Done"
n3.handle(right) -> n4.handle(left)
}
```

**Decision branch**

```
flow { # Flow
n1: rectangle label:"Check"
n2: diamond label:"OK?"
n3: rectangle label:"Fix"
n4: rectangle label:"Proceed"
n1.handle(right) -> n2.handle(left)
n2.handle(bottom) -> n3.handle(top) [label="No"]
n2.handle(right) -> n4.handle(left) [label="Yes"]
}
```

**Simple loop**

```
flow { # Flow
n1: rectangle label:"Validate"
n2: diamond label:"Valid?"
n1.handle(right) -> n2.handle(left)
loop [retry until valid] n1 n2
}
```

**Taskbox example**

```
ops { # Ops
n1: taskbox owner:"Alice" description:"Deploy" system:"CI"
}
```


---

## Full example: multi-lane authentication flow

Use this pattern as a reference for typical “auth” or “login” flows.

```
User { # User
n1: circle label:"Start"
n2: rectangle label:"Enter username and password"
n3: rectangle label:"Submit form"
n4: rectangle label:"Receive confirmation"
n5: circle label:"Access granted"
n1.handle(right) -> n2.handle(left)
n2.handle(right) -> n3.handle(left)
n3.handle(bottom) -> Application.n6.handle(top) [label="Send credentials"]
n4.handle(right) -> n5.handle(left)
loop [retry until valid format] n2 n3 n7 n8
}
Application { # Application
n6: rectangle label:"Receive request"
n7: rectangle label:"Validate format"
n8: diamond label:"Valid format?"
n9: rectangle label:"Forward to server"
n14: rectangle label:"Forward confirmation to client"
n6.handle(right) -> n7.handle(left)
n7.handle(right) -> n8.handle(left)
n8.handle(right) -> n9.handle(left) [label="Yes"]
n9.handle(bottom) -> Server.n10.handle(top) [label="Authenticate"]
n8.handle(top) -> User.n2.handle(bottom) [label="No - Error"]
n14.handle(top) -> User.n4.handle(bottom) [label="Success"]
}
Server { # Server
n10: rectangle label:"Check database"
n11: diamond label:"Valid credentials?"
n12: rectangle label:"Generate session token"
n13: rectangle label:"Return error"
n10.handle(right) -> n11.handle(left)
n11.handle(right) -> n12.handle(left) [label="Yes"]
n11.handle(bottom) -> n13.handle(bottom) [label="No"]
n12.handle(top) -> Application.n14.handle(bottom) [label="Token"]
n13.handle(top) -> Application.n6.handle(bottom) [label="Error 401"]
}
```


---

## Common mistakes to avoid

- Do **not** use `label="Text"` on nodes; node labels must be `label:"Text"`.
- Do **not** use `label:"Text"` inside edge brackets; edges must be `[label="Text"]`.
- Do **not** omit lane prefixes on cross-lane edges (e.g., `n3 -> n5` is invalid when crossing lanes).
- Do **not** introduce other diagram syntaxes (Mermaid, UML, JSON, YAML, XML).
- Do **not** reuse node IDs (`n1`, `n2`, …) anywhere in the diagram; each ID must be unique globally.
- Do **not** add additional comments besides the `# Display Label` right after each lane’s opening brace.
- Do **not** use loops outside of Lane braces. Loops should only appear inside a Lane's braces `{...}`

---

## LLM prompting tips (for human developers)

- Always include an instruction like: `Refer to docs/flowzap-code-guidelines.md and output ONLY valid FlowZap Code for this flow.`
- Provide a short, clear description of the flow: actors, key steps, decisions, and loops (e.g., login with retry, checkout with payment gateway).
- If the LLM output contains Markdown, explanations, or invalid syntax, re-prompt: `Remove all Markdown and explanations; output only FlowZap Code following the guidelines.`
- For updates, paste the existing FlowZap Code and say: `Modify this code in place: add [new step/branch/loop] while keeping IDs unique and syntax valid.`

---

## Final reminder for AI agents

- Your job is to generate **only** valid FlowZap Code text that follows all syntax and structural rules above so it can be saved as a `.fz` file and rendered by FlowZap without manual fixes.

