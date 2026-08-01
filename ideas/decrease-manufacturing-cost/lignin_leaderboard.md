---
title: "Lignin Leaderboard"
excerpt: "A governed Databricks lakehouse linking Pseudomonas Putida genome reference data with feedstock and product cost to determine promising production targets."
date: 2026-07-31
status: "Under development"
mission: decrease-manufacturing-cost
header:
  teaser: /assets/images/projects/from-gene-to-cost-teaser.jpg   # TODO: add a teaser image (optional)
# tags let you group projects later:
tags:
  - databricks
  - bioprocess
  - lakehouse
---

*Status: under development.*

A bioreactor makes a product — a metabolite, a monoclonal antibody, and so on —
by growing cells. How well it does that depends on the cell (usually genetically
modified) and on conditions in the reactor: feed rate, oxygen availability,
stirring. All of these cost money in substrate and electricity, and all of it
happens in a regulatorily stringent environment.

So understanding how a bioprocess *really* performs means understanding it as
part of a whole system: the producer cell and how it responds to its
environment, the physical conditions in the reactor, the economics, and the
regulatory playing field.

**The goal:** build a reference implementation for a governed Databricks
lakehouse that integrates this data and produces insight none of these sources
could give alone.

The same pattern applies to chemical processes, and to linking patients with
genetic variants to clinical trials. Bioprocess is simply where I'm starting.

<!-- TODO as this develops, consider adding:
     - An architecture diagram (drop in assets/images/projects/ and reference it)
     - A "what's built so far" section with concrete progress
     - A link to the repo, if/when it's public
     - The specific insight the integrated data produces, with an example
-->

*Glycolysis pathway diagram by [author], via Wikimedia Commons, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*