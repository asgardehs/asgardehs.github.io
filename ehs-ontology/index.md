---
layout: page
title: AsgardEHS Ontology
description: An OWL/Turtle ontology for environmental health & safety regulatory routing across federal frameworks (OSHA, EPA, DOT, NRC, MSHA, NFPA, ANSI).
permalink: /ehs-ontology/
---

_A practitioner-built ontology for multi-agency EHS compliance routing._

The AsgardEHS Ontology models the three-axis routing problem
(`HazardType` × `ActionContext` × `ContextualCondition`) across the federal
EHS compliance landscape. Given a workplace situation, it surfaces which
regulatory frameworks fire — and why.

## Get the ontology

The canonical resolvable IRI is **`https://w3id.org/asgardehs/ehs`**. Importing
that URL always returns the current release.

| Version | Type   | Download                                      |
| ------- | ------ | --------------------------------------------- |
| latest  | live   | [ehs.ttl](ehs.ttl)                            |
| 3.3.0   | frozen | [snapshots/ehs-3.3.0.ttl](snapshots/ehs-3.3.0.ttl) |
| 3.2.0   | frozen | [snapshots/ehs-3.2.0.ttl](snapshots/ehs-3.2.0.ttl) |

For citations in publications, prefer the version IRI
(e.g. `https://w3id.org/asgardehs/ehs/3.3.0`) over the moving `ehs.ttl`.

## Cite

> Bick, Adam J. *The Compliance Routing Problem — A Practitioner-Built
> Ontology for Multi-Agency EHS Navigation.* 2026. Submitted to EngrXiv.

The paper validates the routing model through nine worked scenarios spanning
chemical spills, transport accidents, multi-hazard interactions, and
NPDES/stormwater compliance.

## License

[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) — share and
adapt with attribution; derivative work must use the same license.

## Source & development

Source repository: [github.com/asgardehs/ehs-ontology](https://github.com/asgardehs/ehs-ontology).

Issues, pull requests, and the [CHANGELOG](https://github.com/asgardehs/ehs-ontology/blob/main/CHANGELOG.md)
live there. Releases follow semantic versioning; each release is published
both as a moving `ehs.ttl` pointer and as an immutable
`snapshots/ehs-X.Y.Z.ttl` file.

## Contact

[asgardehs@proton.me](mailto:asgardehs@proton.me)
