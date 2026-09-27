A few points are worth taking to your System Architect as explicit discussion topics rather than trying to settle them in the Business Architecture document:

What exactly constitutes L5 telemetry?
I would expect this to include platform telemetry plus carefully governed AI interaction signals, but System Architecture should define what is technically observable and retainable.
How should prompt/response data be handled?
This needs a clear architecture and governance decision around privacy, customer isolation, sensitive information, retention, and whether raw conversations or derived signals should be used. I deliberately kept this at the business level rather than prescribing a technical solution.
What is the boundary between “L5 proposal” and autonomous implementation?
My recommendation at the business level is Discover → Evaluate → Propose → Human/Platform Governance → Controlled Evolution. Whether later stages can be automated is a System Architecture and governance decision.
What exactly is a “skill”?
This is important because L5 depends on the concept. Is a skill a reusable AI capability, a tool, a workflow, a domain-analysis capability, or potentially a software component? I would ask the System Architect to establish a precise technical definition.
How do we distinguish L3, L4, and L5 architecturally?
This will probably be the most important discussion. My business distinction is:
L3: AI decides how to investigate/recommend.
L4: AI decides how to pursue a defined goal.
L5: AI helps determine what new capabilities the platform should have.

That last distinction is particularly important: L5 is not simply “more autonomous L4.” It changes the object of AI's responsibility from accomplishing work with the platform to helping evolve the platform itself. This is the point I would strongly preserve when you discuss the model with System Architecture.