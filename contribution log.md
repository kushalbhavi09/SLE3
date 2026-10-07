# Contribution Log

## My Contribution

- Wrote the original SLE-2 code myself: BFS, DFS, the timing driver and the flame sampler.
- Decided how to split the system into containers, especially keeping **Profiling & Metrics** separate from the **Search Engine**.
- Kept **Memory / Visited Set** as its own container so it can be reused by other algorithms (e.g. A*).
- Reviewed the component breakdown (Frontier, Neighbor Generator, Goal Test, Path Reconstructor) against my actual SLE-2 code.
- Wrote the **Design Decisions** section based on what I actually built.
- Wrote the **Conclusion** from my own understanding and experience.
- Checked the final document and prepared the GitHub submission.

## AI Contribution

**Tool used:** Claude (Anthropic)

- Helped design the C4 diagrams (Context, Container, Component).
- Wrote the diagram-generation script.
- Mapped my SLE-2 files (`route_finding.py`, `driver.py`, `flame_sampler.py`) onto C4 containers and components.
- Drafted the explanation text for each C4 level.
- Helped lay out the document in the required template format.
- Helped create the README and this contribution log.
