# Migration Waves

A wave is one night's work for the cutover team. RVTMA reads an RVTools export and cuts the estate into waves you can hand to that team, in an order that does not start with the hard ones.

This is what it decides for you, what it deliberately leaves alone, and how to drive it.

The waves appear in two places, the Wave Execution Playbook and the Migration Plan, and they describe the same waves. Numbering and labels differ between the two; which VMs ride together does not. There is a test that runs both generators over thirty-two setting combinations and fails if that stops being true, because it stopped being true once already.

## What a wave is

Waves are built from four things, in this order:

1. **Folder path**, which becomes Datacenter, Department and Ecosystem. The default reads `/Datacenter/Department/Ecosystem`; change it under Folder Structure on the estimator if your vCenter is organised differently.
2. **Cluster.** A wave never spans two clusters.
3. **Complexity**, computed per VM: USB, GPU or serial passthrough makes it complex; so do 4+ NICs, 4+ RDMs or 3+ snapshots. One RDM, one snapshot or 3 NICs makes it medium. Everything else is simple.
4. **A per-tier cap.** 40 simple, 25 medium, 10 complex by default, configurable in Settings. An ecosystem over its cap splits into numbered subwaves.

VMs with 3 or more NICs are pulled into their own wave regardless. They need manual bridge and NAD review, so they cannot ride the automated MTV flow the rest of the ecosystem uses.

Waves come out in **crawl-walk-run** order: every simple wave, then every medium, then every complex. The trade is real and worth knowing: an ecosystem with VMs in more than one tier does not migrate atomically in time.

## What it excludes, and why

| Excluded by default | Why | Change it? |
| --- | --- | --- |
| Powered-off VMs | Usually decommissioned or forgotten. Migrating them spends the team's night on nothing. | Settings toggle |
| Guest not running | Tools or the guest OS need remediation first; it may not boot cleanly after the move. | Settings toggle |
| Templates | Recreated on the OpenShift side as DataVolumes or VM templates, not migrated. | No, always excluded |

None of them disappear. They land on the **Excluded VMs** sheet with the reason and a recommended action, so nobody has to wonder whether the tool lost them.

## What it does not do

**It does not let you reorder waves.** The up and down arrows on the estimator order the maintenance-window calendar and the `.ics` export, not the wave numbering in the playbook. That is on purpose. The plan is a suggestion, customers re-cut it every time, and an analyst who needs a different order has the spreadsheet.

**It does not let you name waves.** Labels are generated: `Wave 001`, or `Wave 042 (Windows)` with OS grouping on.

**It is not a schedule.** Waves say what goes together. The Migration Window Schedule, further down the same screen, is what packs them into your maintenance windows and produces dates.

**It does not size your target cluster.** If you plan a subset with "Select VMs for this wave", Cluster Architecture, Storage Analysis and the Subscription Estimator keep describing every VM in the export, because the ones you left out still occupy hosts, datastores and VLANs until they move.

## The two settings that change the shape

**Pack ecosystems** decides whether small ecosystems share a wave. Off by default, which gives one wave per ecosystem even when that ecosystem holds a single VM. Turn it on and small ecosystems fill a wave up to the cap. Measured across 19 real exports: strict mode produced 4036 waves, 1787 of them holding one or two VMs; packed produced 1735, with 162 that small. It still never reaches across a cluster to fill a wave.

**Group waves by Operating System** puts all Windows waves first, then all Linux, each in crawl-walk-run order. Useful when different teams own different OS families, or when VirtIO and cloud-init prep run on separate tracks. Same trade as before, one step sharper: an ecosystem with mixed OS gets split across non-adjacent waves.

## How to use it

Load the export, open **Migration Time Estimator**. The screen tells you which grouping mode is active and where to change it.

Set the caps to a night your team can actually work. The defaults assume the wave is the unit of a maintenance window.

Migrating a subset first? Turn on **Select VMs for this wave** and paste the list of names, or pick a cluster. It reports what matched and what did not, because a name the customer sent that is not in this export is a question for them, not something to drop quietly.

Then download the **Wave Execution Playbook**. In the Migration Waves sheet, filter the Cluster column to your cluster and the Wave column to your night. Wave labels are zero-padded so the filter dropdown sorts naturally rather than putting Wave 100 next to Wave 10.

## When the plan looks wrong

**Waves of one or two VMs.** Pack ecosystems is off. Turn it on.

**One ecosystem spread across several waves.** Either it is over the cap for its tier, or it lives in more than one cluster. The playbook says which in the wave's Notes cell; the plan says it in the ecosystem label, next to the subwave count.

**A VM you expected is missing.** Check the Excluded VMs sheet in the playbook before checking anything else. Both documents drop the same VMs, so the sheet explains the plan too.

**Ecosystems that look nothing like your organisation.** The folder pattern does not match your vCenter. Fix it under Folder Structure, not in the spreadsheet.
