# RVTMA - Key Advantages

## Overview

RVTMA (RVTools Migration Analyzer) turns an RVTools export into a migration plan for VMware vSphere to OpenShift Virtualization. It reads the spreadsheet your team already produces, runs the analysis in the browser, and hands back the deliverables a migration actually needs. No agents on the guests, no cluster access, nothing leaves the tab.

## What you get out of it

### The numbers people ask for first

- A **migration time and cost estimate** with a per-VM breakdown and 1x / 2x / 4x / 8x parallelization scenarios, so you can show best case and worst case.
- **Cluster sizing** and an **OpenShift Virtualization subscription estimate** (cores, sockets, nodes, right-sized against efficient, with an SKU comparison).
- A **wave plan** grouped by ecosystem, capped by complexity, packable into real maintenance windows and exportable as an `.ics` calendar.

### Risk, up front

- A per-VM **Risk Register** consolidating the blockers (USB / GPU / serial passthrough, RDMs, snapshots, old hardware, missing VMware Tools, near-full datastores, multi-NIC).
- A **Risk Report** with verified Red Hat references, and **Blast Radius** failure-domain analysis.

### Artifacts the technical team runs from

- **NetworkAttachmentDefinition (NAD)** and **NodeNetworkConfigurationPolicy (NNCP)** YAML generated from the VMware network layout, with VLAN IDs pulled from the VMkernel data.
- A **Wave Execution Playbook** (Excel, up to 24 sheets) with a per-VM checklist and the source hosts, datastores and VLANs scoped to each wave.

### Speed, scale, and honesty

- What was hours of manual work per environment happens in minutes, repeatably, with the efficiency and parallelization sliders under your control.
- Tested in the field on real estates of 15,000 VMs.
- The classifications it makes (workload type, database VMs, OS support tier, criticality, environment) are best-effort from what the export carries, and labeled as such. RVTools is host-side inventory, it cannot see inside the guest, so a VM with no signal in the export gets no classification. RVTMA does not claim certainty the data cannot give.

## Why it fits, by audience

- **Migration and infrastructure teams**: the artifacts they execute from (the playbook, the NAD / NNCP YAML, the host and datastore inventory scoped to each wave), built from data they already have.
- **Project managers**: data-driven time estimates, a wave schedule that packs into maintenance windows, risk visibility, and deliverables in English, Portuguese and Spanish.
- **Executives and sponsors**: what it costs, how long it takes, what the subscription looks like, and where the risk sits, with no license to buy just to find out.

## No infrastructure, private by design

Everything runs in your browser. No backend, no telemetry, no account. The customer data in the export never leaves the tab. Run it from the container (which bundles the PDF toolchain so reports render out of the box), or open the page directly for a quick look at the analysis.

## Realistic expectations

RVTMA is a planning tool, not a migration engine, and it is honest about the difference.

**What it does well:** reads and normalizes the export, maps the infrastructure, sizes the target, estimates time and cost, plans the waves, generates the OpenShift artifacts, and surfaces the risks.

**What needs a human:**

- **Dependencies** are inferred from naming, not from actual network connections. Validate them with the application teams.
- **Workload, database and environment classification** is signal-dependent (name, folder, resource pool, custom attribute, vCluster Platform column). An export with no such signal gives no classification. Confirm with the customer.
- **Criticality scoring** uses heuristics. Review it with the business owners.
- **Time estimates** are conservative by design. Treat them as a defensible starting point, not a guarantee.

**Known limits:** it needs an RVTools export as input (it does not connect to vCenter), dependency mapping is inference-based, very large files can be slow in some browsers, and some views need specific RVTools tabs to be present. It plans migrations, it does not perform them.

Built by **Andre Rocha**. Combined with your own knowledge of the estate and proper validation, RVTMA takes the manual grind out of migration planning and leaves you with a plan you can defend.
