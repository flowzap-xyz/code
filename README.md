# FlowZap Code DSL — Official Syntax & Examples

**FlowZap Code** is a simple domain-specific language (DSL) for collaborative business process mapping and workflow automation. Write natural, readable code to create flowcharts, swimlane diagrams, and process documentation. No technical skills needed!

- **Website:** https://flowzap.xyz
- **Syntax Guide:** [flowzap.xyz/flowzap-code](https://flowzap.xyz/flowzap-code)
- **Examples:** [https://flowzap.xyz/examples](https://flowzap.xyz/examples)
- **API:** [https://flowzap.xyz/docs/agent-api](https://flowzap.xyz/docs/agent-api)

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
  n7.handle(bottom) -> vendors.n10.handle(top) [label:"Requests"]
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


Create your free account on https://flowzap.xyz. Then paste the above into your FlowZap workspace to visualize your first process diagram.

---

## 🤝 Contributing

- Fork & submit pull requests with new code examples or suggestions!
- Open issues for syntax questions or documentation fixes.
- See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## 📖 Documentation

- [Syntax Rules](SYNTAX.md)
- [Common Use Cases & Examples](https://flowzap.xyz/examples)
- [LLM Workflow Mapping Patterns](https://flowzap.xyz/blog/common-workflow-mapping-requests-to-llms)
- [7 Critical BPMN Mapping Indicators](https://flowzap.xyz/blog/7-critical-indicators-for-business-process-mapping-implementation)
- [Agent API Reference](https://flowzap.xyz/docs/agent-api)

---

## License

See [LICENSE](LICENSE).

