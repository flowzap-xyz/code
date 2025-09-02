# FlowZap DSL Syntax Guide

> _Last updated: September 2025 | Official docs: https://flowzap.xyz/flowzap-code

**FlowZap Code** is a straightforward domain-specific language (DSL) designed for describing business process diagrams visually. Built for real business users—no programming background required.

---


## 1. Organize Your Process with Lanes

Think of lanes as rows in your diagram, each representing a team, department, or role. Every lane groups together related steps.

**Syntax:**
laneName {
...nodes...
...edges...
}


**Guidelines:**
- Use clear names that make sense for your process (e.g., `Sales`, `Accounting`, `HR`)
- Lane names can be any descriptive identifier
- Comments can be added only after the opening brace: `Sales { # internal SLA 48h }`
- Brackets {} are only used to separate lanes

---


## 2. Add Steps with Nodes

Inside each lane, list every step in your process. Each step is called a **node**.

**Syntax:**
nodeId: shape label:"Description"


**Node Shapes:**
- `circle` — Start/end points
- `rectangle` — Process steps
- `diamond` — Decision points
- `taskbox` — Detailed tasks (optional, with additional attributes)


**Examples:**
n1: circle label:"Start"
n2: rectangle label:"Enter Data"
n3: diamond label:"Is Valid?"
n4: taskbox owner:"Alice" description:"Validate Input" system:"FormApp"


**Nodes Rules:**
- Use sequential IDs (n1, n2, etc.) within each lane for easy post editions
- Keep labels concise but descriptive
- Respect the syntax and the absence of spaces around colons (no extra spaces)
- Notice that Node labels use the colon sign (:)
- Don't insert comments

---


## 3. Connect Steps with Edges

After your nodes, describe how each step connects to the next.

**Syntax:**
node.handle(direction) -> node.handle(direction) [label="Text"]


**Directions:**
- `right` — Main flow (left to right within lanes)
- `left` — Backward flow
- `top`/`bottom` — For decisions, branches, or loops


**Edge Rules:**
- Add the edges after the node desciptions, within a defined lane
- Label all decision outcomes: `[label="Yes"]`, `[label="No"]`
- Always use equals sign (=) for label assignment, never colon (:)
- Keep edge labels short and clear
- Labels can be left empty: `[label=""]` or omitted entirely
- Notice that Edge labels use the equal sign (=)
- Don't insert comments


**Example:**
n1.handle(right) -> n2.handle(left)
n3.handle(right) -> n4.handle(left) [label="Approved"]



---


## 4. Connect Across Lanes

When a step jumps from one lane to another, add the lane name before the node.

**Syntax:**
n3.handle(bottom) -> targetLane.n5.handle(top) [label="Send for Review"]

text

**Cross-Lane Rules:**
- Use when responsibility transfers between actors/systems
- Edge definition must live inside the braces of the **origin lane**
- Clearly show handoffs between lanes
- Minimize crossing lines for better readability



---


## 5. Complete Example

Here's a full FlowZap Code sample demonstrating all concepts:

couple { # Couple planning wedding
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

planner { # Wedding planner
n6: rectangle label:"Initial Consultation"
n7: rectangle label:"Create Planning Timeline"
n8: rectangle label:"Coordinate Vendors"
n9: rectangle label:"Final Walkthrough"

n6.handle(right) -> n7.handle(left)
n7.handle(right) -> n8.handle(left)
n8.handle(right) -> n9.handle(left)
n7.handle(bottom) -> vendors.n10.handle(top) [label="Requests"]
}

vendors { # Vendor management
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


---

## 6. Tips & Best Practices

**Node Management:**
- Keep IDs short: use `n1`, `n2`, etc.
- Use descriptive labels that clearly explain the step or decision

**AI Assistance:**
- Ask your favorite AI helper to draft FlowZap Code templates
- Refer to the latest instructions [flowzap.xyz/flowzap-code](https://flowzap.xyz/flowzap-code) and [flowzap.xyz/examples](https://flowzap.xyz/examples) in your prompts
- Use natural language descriptions to generate initial code structures

**Layout & Design:**
- Use the Auto-Layout checkbox for initial organization (stacks actors end/or systems like BPMN diagrams)
- Manual adjustments often improve clarity and flow
- Minimize crossing lines between lanes for optimal clarity

**Code Quality:**
- Maintain consistent formatting for readability
- Comments only allowed after lane braces: `Sales { # internal SLA 48h }`
- Lines beginning with `//` are ignored and not preserved when regenerating code
- Respect syntax spacing—avoid extra spaces around operators

**Workflow Tips:**
- Start with main flow (left to right) before adding branches
- Group related steps in appropriate lanes
- Test your code by pasting into FlowZap and clicking "Update Diagram"

---

**Ready to start?** Write or paste your FlowZap Code into the editor and click "Update Diagram." Your process comes to life—**no technical skills required!**

For more examples and details, visit [flowzap.xyz/flowzap-code](https://flowzap.xyz/flowzap-code) and [flowzap.xyz/examples](https://flowzap.xyz/examples).