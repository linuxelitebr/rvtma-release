# Changelog

All notable changes to this project will be documented in this file.

---

## [2.10.3] - 2026-11-02 - Front Door

The last of the text printing over other text, and a front door for the project.

### Added

- A public home for the docs, the OpenShift manifest, the changelog and the issues: [rvtma-release](https://github.com/linuxelitebr/rvtma-release), refreshed on every release, with a sample export to try it on. Picking this tool up for the first time? Start there.

### Fixed

- **Long service tags printed over Model in the Server Allocation Plan.** All three host tables. 132 overlapping words on 3 exports, now 0. A normal 7-character tag stays on one line and copies clean. A 20-character serial takes 3 lines in that narrow column: legible, not pretty.
- **Department, ecosystem and size printed over each other in the Migration Time Estimate VM list.** 28 overlapping words on 1 export, now 0.
- **The Ecosystems line in the Migration Plan ran off the page.** 49 words past the margin, by up to 47pt, and one of them reached the edge of the page. Now 0.

Same catch as 2.10.2: the break points are zero-width spaces, so a wrapped name copied out of these PDFs, or out of their Markdown or AsciiDoc source, brings invisible characters along. Only words over 10 characters get them.

### Validation

One PR at a time, same drill: the test suite (1761 tests at release), a mutation pass showing each new test fails without its fix, CI, and main against the previous release on real RVTools exports.

Before the release, the affected documents ran over 24 exports in en, pt-BR and es, 207 PDFs, with no overlapping words left in any of them. Then every screen and document ran on the published 2.10.2 and on this release over 5 exports in the same three languages: 300 screens, 174 PDFs rendered by the release image's toolchain and 411 text documents per side. 0 errors, 0 dialogs, 0 invalid PDFs, and every difference traced back to what these fixes were allowed to change or to noise in the harness.

Known and still open:

- An export parsed within the first second after page load loses its dvPort rows, so the network YAML lists fewer VLANs. Same bug 2.10.2 reported. No human uploads that fast.
- 18 words past the margin in the Deployment Plan host list, by up to 40pt. Wrapping would put invisible characters in the FQDNs people copy out of that list, so they stay.
- 180 words past the margin in the Network Remediation Guide are 4-digit page numbers in the table of contents, by up to 5.5pt. Nobody loses a digit. 1 more in the Storage Analysis report.

---

## [2.10.2] - 2026-11-02 - Inside the Lines

A patch for what the last two releases left on the page. PDF tables assumed names have spaces in them, and real exports do not always agree: a name with nothing to break on printed straight over the next column. The rest is a slider, a label and a number format, all small, all wrong.

### Fixed

- **Long names ran through the next column in three PDFs.** Datastores and hosts in the Server Allocation Plan, datacenter, department and ecosystem in the Migration Plan wave table, VMs and clusters in the Storage Analysis report. They wrap now. Over 24 real exports in three languages, overlapping words went from 594 to 132 in the Allocation Plan, 777 to 0 in the Migration Plan and 52 to 0 in Storage Analysis, and words past the margin in the Allocation Plan went from 14,634 to 0. The Allocation Plan also lost 473 pages. The catch: the break points are zero-width spaces, so a name copied out of these PDFs, or out of their Markdown or AsciiDoc source, brings invisible characters along. If a name is headed for a command, copy it from the screen.
- **The `ifcfg` `printf` in the Network Remediation Guide ran off the page.** 2.10.0 fixed the backslashes in that command and claimed it printed correctly for the first time. It did, as far as the edge of the page: 187 characters in a box that holds 103, so the end of the command was simply not on paper. Now one value per line, same output in sh, bash and dash.
- **Storage report numbers followed the browser's language, not the report's.** An English report from a pt-BR browser wrote `1.234` for `1,234`, in a document about capacity. The Storage Analysis and Storage Architecture reports now format numbers in the report language.
- **One OS support tier, two names in the OS Analysis report.** The same VMs were Not Listed in one table and Unsupported in the other. Both say Not Listed now, in all three languages.
- **The parallel migrations slider stopped at 10** while the deadline check could ask for 20. It reaches 20, and a test ties the two together.

### Changed

- The changelog in the public [rvtma-release](https://github.com/linuxelitebr/rvtma-release) repo now updates on every release. It stopped at 2.5.0 back when it depended on someone remembering.

### Validation

Every fix was validated one PR at a time: the test suite (1739 tests at release), a mutation pass showing each new test fails without its fix, and main against the previous release on real RVTools exports. GitHub Actions is running again, so these went through CI as well.

Before the release, the whole app ran headless on 2.10.1 and on main over 24 exports in en, pt-BR and es: 1,440 screens, 810 PDFs rendered by the release image's toolchain and 2,289 text documents. 0 errors, 0 dialogs, 0 invalid PDFs, and every difference traced back to what its PR was allowed to change or to noise in the harness. Swapping the image under an open browser profile and reloading left 0 of 110 local files stale.

Two things are known and still open. 132 overlapping words remain in the Allocation Plan on 3 exports and 28 in the Migration Time Estimate on 1, none of them new. And the sweep turned up an older bug: an export parsed within the first second after page load loses its dvPort rows, so the network YAML lists fewer VLANs. No human uploads that fast. A test harness does, ask me how I know.

---

## [2.10.1] - 2026-10-30 - Counted, Not Guessed

A patch for OS Analysis, found while validating 2.10.0. The report printed `undefined` where it could not place an OS version, the charts and the summary cards quietly left those VMs out, and a Windows Server 2003 box came out one version check away from certified. Every VM is now counted, and the ones nobody could identify are counted as exactly that. None of this is new in 2.10.0; 2.9.1 does the same.

### Fixed

- **`undefined` in the OS Analysis report.** The Support Tier, Migration Notes and Support columns printed it for any OS without a version, such as "Windows Server" with no year or "Ubuntu Linux (64-bit)". They now read Certified\* (Unverified) or Commercial\* (Unverified), with "Confirm the exact version before migration" as the note, in en, pt-BR and es. Plain Certified would have been the one-line fix, and it would have told a customer that a Windows box nobody could identify is certified.
- **The OS Analysis charts and summary cards lost the unverified VMs.** Migration Readiness, Risk Level and the Support Tier radar counted five of the seven tiers, so their bars came up short, and the tier cards added up to less than Total VMs, printed two rows down on the same screen. Each now has its own purple bucket (Needs Version Check in the charts, Unverified in the cards), kept out of Certified and Commercial and drawn only when it has VMs.
- **Legacy Windows was filed as almost certified.** Windows Server 2003, Windows 2000, XP, NT, Vista and 8.x lost their version on the way through the OS analyzer and landed in Certified\* (Unverified), weight 95 in the readiness score. They are now named and Not Listed. On the exports we validate against that is 438 VMs, readiness scores drop by up to 6 points, and the unsupported guest OS risk card picks up the XP, NT, Vista and 8.x VMs it was missing.
- **The OS age chart put XP, NT, Me and Vista in "5-10 years".** They have a word where the version number goes, so they found no release year and took the chart's can't-tell default. They now carry their release year and land in Legacy.

### Validation

GitHub Actions is still blocked by billing, so every fix was validated locally, one PR at a time: the test suite in en_US and pt_BR (1703 tests at release), a mutation pass showing each new test fails without its fix, and main against branch on 23 real RVTools exports.

Before the release, the whole app ran headless on 2.10.0 and on main over all 23 exports, with hooks installed before page load to record errors, `alert`/`confirm`/`prompt`, console warnings and toasts. Every screen, every document button, and the PDFs rendered by the release image, in en, pt-BR and es. 0 errors and 0 dialogs on either side, the same warnings and toasts on both. Of 112 kinds of output, 105 came out identical, the migration plans, the Wave Execution Playbook and every other XLSX included. The other seven are the OS Analysis report and screen, the Risk Summary (PDF, Markdown and screen) and the unsupported guest OS entry in the risk register, which is what this release changes. The age chart fix landed after that run and went through the same app check on the four exports it touches.

---

## [2.10.0] - 2026-10-29 - Fit to Print

Most of this release is about the PDF, the part of RVTMA that actually leaves the building. It now carries the issuer's brand, speaks the report language down to the page footer, draws its figures for paper instead of photographing the screen, and stops quietly rewriting backslashes on the way to pandoc. The Risk Summary is translated card by card, and the NAD output points at where the NADs actually live.

### Fixed

- **The PDF endpoint rewrote backslashes.** It saved each report with `echo`, and `/bin/sh` in the image is dash, whose `echo` interprets escapes: `\n` became a newline (the "ewpage" that once made us pull the Subscription Estimator page breaks), `\t` a tab, `\v` a control character that failed the whole PDF, and `\c` cut the report short right there. Now written byte for byte with `printf`. The example `printf` command in the Network Remediation Guide prints correctly for the first time.
- **The architecture figure in the Cluster Architecture Deployment Plan.** Hosted cluster names were truncated exactly where the suffix that tells them apart lives, connectors crossed cards past four hosted clusters, and an estate with many sites got a blank page followed by a figure running over the footer. Now one figure per site, an indented tree whose lines never cross a card, full names in mono, and one square per host so cluster sizes compare at a glance.
- **The Storage Connectivity figure.** Fifteen arrays and thirty clusters joined by curves was a ball of yarn. Now one card per array, the clusters it serves as pills with their fair share of PVCs, and a pressure bar against the config max the architect typed on the Storage Architecture screen. The report used to know only the per-vendor estimate, and now says so when that is all it has.
- **The Datacenter Infrastructure Report charts.** They were screenshots of the dashboard canvases, so a wide monitor printed postage-stamp pies with grey 11px legends. Redrawn for paper at a fixed size, one chart per page, the host models bar at full width and at its real proportions.
- **The workload type boxes in Cluster Architecture decide what gets built.** Unticking a type used to change nothing. A type that cannot get hosts of its own is greyed out with the reason on hover, and the boxes are wide enough for their DETECTED tag.
- **A big PDF waits its turn** behind another render instead of timing out at 60 seconds.
- **Browsers mixed a new `index.html` with an older release's cached CSS and JS**, which put every brand in the masthead at once. Assets are cache-busted per release, and the masthead shows exactly one brand even with stale CSS.
- **NADs.** Each workload group gets every NAD its VMs use, shared port groups included. The NAD report verifies them where they are actually created. Export values on the NAD results screen are escaped.
- **Smaller ones.** The migration estimator no longer prints NaN with no VMs selected. The folder config examples name real VMs instead of "Unknown VM". The PDF settings modal no longer resets the settings it does not show. The Data Source Quality doughnut prints at a size you can read. The Spanish Network Remediation Guide is 50 pages shorter after one over-long placeholder. Accents are back in the pt-BR and es report strings.

### Added

- **PDF brand packs.** Red Hat (default), Linux Elite and IBM Consulting, picked in PDF settings, applied to the cover, the page backgrounds and the masthead logo. The analyst named in PDF settings goes on the Server Allocation Plan cover.
- **Translated page chrome** for pt-BR and es: cover subtitle, header, page numbers, footer and the table of contents title.
- **The Risk Summary in pt-BR and es, card by card**, counts, variants and items included.
- **A written answer to "why is this still in English?"**: which documents and terms change language, which deliberately do not, in the docs and in Settings.

### Changed

- The OS Analysis modal and the Ctrl+Shift+, settings modal are retired. The OS Analysis screen and the sidebar settings panel are the one place for each.
- Dependency bumps: vite, vitest, js-yaml, brace-expansion, source-map-js.

### Validation

GitHub Actions was blocked by billing for this whole cycle, so every change here was validated locally, one PR at a time: the test suite in en_US and pt_BR (1641 tests at release), a mutation pass showing each new test fails without its fix, main against branch on real RVTools exports, and PDFs rendered by the release image's own pandoc, lualatex and rsvg-convert, in three languages and both orientations wherever layout changed.

---

## [2.9.1] - 2026-08-31 - One Story, Two Documents

Significant work on migration wave planning. The two documents that describe a migration now agree with each other, the plan says which cluster split a wave, and a matrix of every setting combination stands between them and the next disagreement.

The XLSX and the Migration Plan each cut waves with their own code. Same rules, two implementations, nothing checking they still matched. They stopped matching in 2.8.0 and nobody noticed, including the release that shipped it.

### Fixed

- **The Migration Plan and the Wave Execution Playbook describe the same waves again.** The cluster boundary added in 2.8.0 went into the Excel exporter's emitters; the plan has its own parallel implementation and was never touched. An ecosystem living in two clusters came out as two waves in the playbook and one in the plan. It matters because the two go to different readers: the cutover team works from the playbook, the sponsor reads the plan PDF, and neither can see the other to notice. Applied in all three emission paths, including packing, which now fills a wave from several small ecosystems within one cluster rather than reaching across to do it. Estates where every ecosystem sits in one cluster are unaffected; the plan changes shape for everyone else, in the direction 2.8.0 already took the playbook.

### Added

- **The plan names the cluster when the cluster is what split the wave.** Fixing the boundary without this left an ecosystem in three clusters showing as three waves with identical Datacenter, Department, Ecosystem and Complexity, and nothing to tell the rows apart. That is the state the playbook was in before 2.8.0 gave it a Cluster column. The plan carries it in the ecosystem label, beside Multi-NIC and the subwave count, because that table already runs wide in portrait. Only when the ecosystem actually spans more than one cluster: in a single-cluster estate the qualifier would distinguish nothing from nothing.
- **A page on how waves are cut.** `docs/migration-waves.md`. What decides a wave, what is excluded and why, what the feature deliberately does not do, the two settings that change its shape, and the four things people actually hit. The "does not do" section is the point: wave ordering and wave naming have both come back from the field as bugs, and neither is one.

### Validation

The durable part of this release is a test that runs **both generators** over the same input and compares the VM-to-wave maps they publish. Wave numbering and labels may differ; membership may not. Thirty-two combinations of pack ecosystems, OS grouping, powered-off, guest-not-running and two cap sets, against an estate built to exercise all of them at once.

Two false greens had to be cleared before that number meant anything. The first run picked the two smallest exports on hand, both with a single ecosystem in a single cluster, so the boundary never applied and the matrix passed on nothing. The second had three of its four toggle dimensions inert: the exclusion filters read powerstate, template and guest state out of the workbook rather than off the VM object, so setting those fields on the object changed no outcome at all. The test now builds a real vInfo sheet, and the harness reports whether each dimension actually produced distinct results.

Against nine real RVTools exports, chosen because every one has ecosystems spanning clusters: **288 combinations, no disagreements**. The largest estate exercised was 9476 VMs across 328 ecosystems and 37 clusters.

Reverting the fix fails 24 of the 53 tests in that file, which is the check that matters. A matrix that stays green over a known regression is worse than no matrix.

---

## [2.9.0] - 2026-08-31 - Say What You Mean

The whole release came out of one afternoon of a colleague using the tool and saying what he found. Two things: a guide arriving in English when the report language was set to Portuguese, and wave ordering that did not behave the way the screen implied. Both turned out to be true, and neither was the thing anyone would have picked off a roadmap.

The pattern underneath them is the same. The tool knew the answer and the interface said something else: a Settings toggle promising to translate everything, a control labelled "migration order" that ordered a calendar, a mode that solves the exact problem someone was reporting and that nothing on screen mentioned.

### Added

- **Five more reports follow the Report Language setting.** Subscription Estimator, Storage Architecture, the NAD Implementation Guide, the Risk Summary and the Network Remediation Guide, 233 keys per language and 699 strings in all. Commands, YAML, SKU product names, Red Hat documentation titles, MAC addresses, port groups and anything else out of the export stay English in every language, which a test enforces on the two command-heavy guides by extracting every fenced block per language and comparing them. An `nmcli` line nobody can paste is worse than an English one. The Network Remediation Guide had shipped English-only by an explicit decision, on the grounds that it was the migration engineer's bench tool; the field said the PDF is being handed to the customer-side operator who actually types the commands, so the premise was wrong and the comment now records that.
- **The estimator says which wave grouping mode is on.** The pack-ecosystems toggle decides whether small ecosystems share a wave or each gets one of its own, and it lived in Settings with nothing on the estimator pointing at it. A plan full of one-VM waves read like the tool's opinion rather than a setting. Measured across 19 real exports, strict mode produces 4036 waves of which 1787 hold one or two VMs; packed produces 1735 of which 162 do. The strip names the active mode and points at Settings rather than repeating the toggle, so the value keeps one home.

### Fixed

- **The Migration Plan lost a number in Portuguese and Spanish.** The warning about VMs too large for a single migration window carries the duration of the biggest one. Both translations had dropped the placeholder, so those readers got the sentence without it. Present since 2.5.0. The duration also appears in the list directly below, so nothing was actually unreadable, but a translation should not diverge from its source unnoticed.
- **Four reports would have served the previous language after a switch.** Each generator caches its render for the PDF path, and changing the language clears those. Risk Summary, Subscription Estimator, Storage Architecture and the Network Remediation Guide were never on that list. Harmless while all four were English-only, a stale-text bug the moment any of them got translated, and indistinguishable from a missing translation when it happened.
- **The wave reorder arrows say what they order.** They order the maintenance-window calendar and the `.ics` that comes out of it, and never touched the wave numbering in the Wave Execution Playbook or the plan tables. The control was labelled "Waves (migration order)" and the section said it packs waves "honoring wave order", both true of the calendar and both reading as a promise about the whole plan. Someone reordered, regenerated the XLSX and reported a bug; the code was right and the label was not. The label, the section text and the plan's own schedule section now scope themselves, the last one in all three languages, because a reader holding the PDF cannot see the estimator screen.
- **The singular case of the risk narrative did not agree with itself.** "1 item require attention before Wave 1" came from one string pluralising two independent counts. Split into clauses, which the other two languages needed anyway since neither pluralises by appending an s.

### Changed

- **Settings no longer promises to translate everything.** The hint read "Language used in exported reports and PDFs" with no carve-out, which is how someone set pt-BR, downloaded a guide and got English. It now names what stays English and why: the Wave Execution Playbook, and any generated YAML or commands, are read at a console rather than by a sponsor.

### Validation

Two static guards went in before any string moved, and both failed on their first run, which is how the two bugs above were found rather than shipped. One checks key parity across the three languages including the placeholder set per key, because a missing key silently falls back to English and only warns to a console nobody reads during a PDF export. The other checks that every cached report is cleared when the language changes.

Every report got a golden-master snapshot committed before its refactor, in its own commit, so the diff proves the English did not move. Across all five, exactly one English string changed, and it was the grammar defect above.

Run against the 19 real RVTools exports on hand: 8 reports in 3 languages each, **456 renders, no failures**, no unresolved keys, no unfilled placeholders, and an identical number sequence between English and each translation every time. The largest estate exercised was 9476 VMs across 305 hosts. The two reports this release did not touch were included on purpose, to catch collateral damage rather than confirm the happy path. The two translated guides were also run through pandoc and lualatex in all three languages.

### Internal

- **A tool for calibrating the migration time model.** `tools/mtv-conversion-stats.py` reads MTV Migration and Plan CRs and measures whether conversion overhead scales with disk size. It does not: correlation -0.02 against disk size and -0.01 against disk count across 465 real conversions, which closed the long-standing request for a disk-scaled overhead term and confirmed the flat one already in the tool. Bench tooling; the image does not carry it.

---

## [2.8.0] - 2026-08-26 - One Cluster at a Time

Another one straight from the field. Someone planning a wave of 43 machines found it reaching across several clusters, and spent the afternoon taking the spreadsheet apart by hand to fix it.

Waves group by ecosystem, which keeps an application together and is the right instinct. The problem is that an ecosystem does not stop at a cluster boundary and a migration does: live migration will not cross clusters, hosts and datastores belong to one, and the cutover team works a cluster at a time. A wave touching three clusters is a night touching three clusters.

### Changed

- **A migration wave no longer spans clusters.** Ecosystem is still the grouping. Cluster is now a boundary inside it, applied before the VM cap divides what is left, so a subwave reads "2 of 3 within this cluster" rather than across the estate. An ecosystem living in three clusters produces three wave sequences instead of one wave that reaches into all three. On the estate this was tested against, 132 ecosystems were spread across more than one cluster, so the shape of the plan changes for anyone in that position. Both emitters were updated: the default one and the packed mode behind the pack-ecosystems setting, which packs small ecosystems together to avoid undersized waves. Packing still does that, it just no longer reaches across a cluster to fill a wave. Fixing only the default would have left a setting that silently undid the boundary.

### Added

- **A Cluster column in the Migration Waves sheet, and a filter on it.** An ecosystem split across three clusters yields three waves, and without the column the analyst sees near-identical rows with no explanation. The sheet had a frozen header but no autofilter, despite a comment in the code explaining that wave labels are zero-padded so the autofilter dropdown sorts naturally. Now it has one, so the person running the cutover can pull their own night's rows instead of reading all of them. Each wave also carries the reason in its Notes cell.

### Validation

Run against a 7343-VM estate spread over 42 clusters. Every VM name in every wave was checked back against its real cluster: 6414 names, no wave carrying a VM from another cluster, and the sheet totals for VMs, data and hours all still agree with the rows above them, which is what proves the shifted column indices are right. Both emission modes, with OS grouping intact.

---

## [2.7.0] - 2026-08-25 - The Short List

A customer asks for a pilot. Move these 122 machines first, the other 1678 follow later. The list arrives as a column pasted out of a spreadsheet, because that is how customers send things, and until now the answer was to pick 122 rows out of 1800 by hand, or to go edit the RVTools export before loading it and lose the audit trail along with the risk register. The Migration Time Estimator has a toggle for that now.

### Added

- **Select VMs for this wave (#404).** A toggle in the Migration Time Estimator that plans a migration for a chosen subset instead of the whole estate. Paste the list of VM names, or pick a cluster, and the time estimate, the waves, the schedule, the `.ics` and the Wave Execution Playbook all describe that selection, with the playbook's host, datastore and VMkernel sheets carrying only what those machines touch. Pasting reports what matched and, more usefully, what did not: a name the customer sent that is not in this export is a question for them, not something to drop in silence. Matching ignores case and stray whitespace, which is where a copied spreadsheet column usually goes wrong. Turning the toggle off returns to the whole estate but keeps the picks, so a misclick does not cost the work; only the explicit clear discards them. Nothing is persisted, deliberately: a selection is a list of customer VM names, and those do not belong on disk. The panel says so.
- **The scope boundary, on screen (#405).** "This plan covers 122 of 1800 VMs" reads like the whole tool narrowed, and it did not. Cluster Architecture, Storage Analysis and the Subscription Estimator keep describing every VM in the export, because the ones left out still occupy hosts, datastores and VLANs until they move. Left unsaid, someone plans a pilot and quotes a target sized for 122 machines. It now sits in a caution box rather than in the same grey as the housekeeping note next to it.

### Security

- **nanoid bumped to 3.3.18 (#406).** Cleared an advisory where a custom generator can loop forever on a zero size. Transitive and dev-only (vitest to vite to postcss), so it never shipped in the image, but Dependabot had been failing to open the PR since 14 August and the alert was sitting open. Three lines of lockfile.

---

## [2.6.2] - 2026-08-18

The Infrastructure Summary says how many of the VMs it counted are templates. The Estimator learned that in 2.6.1, but the report from the field was about the Virtual Machines card, which is a different screen reading the same sheet. Both say it now, in the same words.

### Added

- **The Infrastructure Summary names the templates behind its VM count.** The Virtual Machines card counts vInfo rows, and vInfo lists templates next to running VMs, while the Wave Execution Playbook drops them before it builds waves. The subtitle under the card now names the part that will not ride a wave. The number itself does not move: it stays honest about what the export holds. On an estate carrying exactly one template the gap between the two totals reads as an off-by-one, which is how this arrived.

### Changed

- **Two user-facing strings stop using a spaced hyphen as punctuation.** The templates risk card and the Database VMs note break those sentences with a period instead.

---

## [2.6.1] - 2026-08-14

A field report about a VM count being off by one turned into something bigger than the report. The pre-flight publisher read vInfo column names literally, and a good share of RVTools exports prefix every column, so on those the risk cards counted zero and retracted themselves. The analyst saw an empty Risk Summary with no way to tell that apart from a healthy estate. Five of the eight exports on hand were in exactly that state, one of them sitting on 682 VMs with a stopped guest and 84 templates while the screen showed nothing at all. The off-by-one that started it is here too: the Migration Time Estimator now names the templates hiding inside its own total.

### Added

- **The Estimator names the templates behind its total (#398).** "Selected VMs" is the raw vInfo count, and vInfo lists templates next to running VMs, while the Wave Execution Playbook drops them before it builds waves. Two totals that disagreed, with nothing on screen to say why. The total stays honest about what the export holds, and a line under it names the part that will not ride a wave. On an estate carrying exactly one template that gap reads as an off-by-one, which is how it reached us.

### Fixed

- **The pre-flight risk cards resolve vInfo columns through the mapper (#397).** Some exports ship every column prefixed: `vInfoTemplate` for `Template`, `vInfoPowerstate` for `Powerstate`, `vInfoOS` for the guest OS. The publisher matched the literal names, so every card fed by them counted zero and retracted. Templates, stopped guests, suspended VMs, unsupported OS: all quietly absent, on five of the eight exports checked. RVToolsColumnMapper has carried both spellings all along, which is the entire reason it exists. The exports that already worked come out identical. The old-hardware card stays quiet on prefixed exports because they genuinely do not carry an HW version column, which is a different problem for a different day.

### Changed

- **The update check asks for 100 releases instead of 30 (#396).** The releases repo is shared with the other elastocera projects, so the rvtma tags sit behind however many releases the siblings cut in between. Nothing had fallen off the end yet, but the margin was 22 sibling releases before the newest rvtma tag left the page and the check started calling a stale version current.

### Security

- **js-yaml bumped to 4.3.1 (#395).** Transitive dev dependency. It does not ship in the image.

---

## [2.6.0] - 2026-08-04 - Table Stakes

The database VMs get their own planning surface. RVTMA already knew which VMs looked like databases and counted them in aggregate, then left them scattered across the workload views where a DBA could not do much with them. This release pulls them into a dedicated Database VMs view, one row per VM with the engine named, so the sizing and licensing conversation starts from a list instead of a filter you rebuild each time. MongoDB joins Oracle and SQL Server as a recognised engine, the SQL name matcher stops missing instances that glue the word into the hostname, and the Support Data Manager finally wears the Red Hat styling the rest of the dashboard already had. The rest is the docs catching up to the code.

### Added

- **The Database VMs planning view (#385).** The workload detector already tagged database tiers, but only in aggregate, so a database estate read as a number on a chart and nothing you could hand to a DBA. This adds a dedicated view over the VMs it detects as databases: one row per VM, the engine (Oracle, SQL Server, MongoDB) named, so the sizing and licensing conversation starts from a list instead of a filter rebuilt by hand.
- **MongoDB as a recognised database engine (#387).** Detection knew Oracle and SQL Server, so a Mongo fleet came through as generic Linux. MongoDB is the third engine now, and it lands in the Database VMs view with the rest.

### Changed

- **Broader SQL instance-name detection (#386).** The matcher keyed on "sql" standing on its own and missed the names that glue or prefix it, an instance run together with the rest of the hostname or a `sql2019` style suffix. It catches the `\bsql` and `sql\d` shapes now, so fewer SQL Server boxes hide in the general population.
- **The Support Data Manager wears the Red Hat / PatternFly look, and its manual moved into `docs/` (#389).** The screen that edits the OS support data was still on the old styling while the rest of the dashboard moved on. It matches now, and the how-to that lived inline is in `docs/` with the other guides.
- **ARCHITECTURE.md counts and dashboard sections brought back in line with the code (#388).** The file counts and section lists had drifted from what the app actually ships.
- **README: a Database VMs section, and the stale v2.0 markers dropped (#390).** The README still described the feature set at a version we had left behind and said nothing about the new view.

### Fixed

- **The Support Data Manager's tier header rendered invisible (#389).** A latent styling bug drew a section header in the support-data editor in a colour you could not see against its background. It surfaced while restyling the screen and got fixed in the same pass.

---

## [2.5.0] - 2026-07-27 - Boxes and Calendars

Three headline additions and a long tail of report polish. The Migration Time Estimator can pack VMs into maintenance windows instead of assuming a flat eight-hour day, exports the schedule as an .ics you drop into a calendar, and folds the window plan into the Migration Plan PDF. The report PDFs learned to draw WARNING / NOTE / IMPORTANT as an icon and a box instead of a bare paragraph, on both the AsciiDoc and Markdown paths. And there is a Generate PDF screen now: hand it an .adoc or .md you edited by hand and it returns the styled PDF, no RVTools export required. The rest is the usual round of making the reports read the way they should have to begin with.

### Added

- **The Migration Window Schedule (#350, #351, #352, #353, #354, #356, #358).** The classic wave model assumed eight hours a day, five days a week, and left the analyst to reconcile that with a real change calendar. This adds a second modality: define maintenance windows (days plus time ranges) and the estimator packs the migration waves into them, prefilled Monday to Friday, 9 to 18, so it is useful before anyone touches a setting. Waves group and reorder, the VMs that do not fit a single window roll up with the concurrency assumption stated out loud instead of buried, and the whole thing exports as an .ics for import into a calendar. It rides into the Migration Plan PDF as its own section (P3b). Classic waves are untouched; this is an option, not a replacement.
- **Generate PDF from an edited report (#369).** Analysts download a report as .adoc or .md, fix a number or reword a line before it reaches the customer, and until now had no way back to a PDF short of re-running the analysis. A new Settings screen takes the edited file and renders it through the same pipeline the built-in report PDFs use. Format comes from the extension, no RVTools export needed to reach it.
- **Admonitions render as an icon and a box in the PDFs (#360, #361, #362, #363, #364, #365).** WARNING / NOTE / IMPORTANT / CAUTION / TIP printed as a bare paragraph that read like body text. They now carry an icon and a coloured box, via a pandoc Lua filter bundled into the image (built-in Lua, no external filter, in keeping with the image's design) plus awesomebox, which eisvogel ships the environments for but never loads. The text sits centred against the icon. Enabled on both the AsciiDoc and Markdown paths and put to use in the Risk Summary disclaimer and blocker note, the Server Allocation "Important Note", and the storage LUN fan-out caveat.

### Changed

- **The Migration Guide's per-VM steps are lists, not crammed command boxes (#372, #374, #375).** The Detailed Per-VM Process stuffed its prep and validation steps into command boxes, so HTML collapsed the newlines and the whole thing rendered on one unreadable line. Those steps are an ordered and a bulleted list now. Along the way, PatternFly's reset had quietly stripped the bullets off every list in the guide, so the semantic lists read as plain prose; the discs are back, scoped so the checklists keep their checkboxes and the styling shared with Cluster Conversion stays put.
- **The report attribution footer is translated (#355).** The "Generated by RVTMA ..." line was English on every report regardless of the selected language.
- **The storage capacity donut legend sits centred against the chart (#367),** instead of hugging the top when the legend is shorter than the ring.
- **Wider columns in the Wave Execution Playbook (#368).** Several columns across the Executive Summary, Migration Waves, Migration Detail, Excluded VMs, VM Details and Multi-NIC tabs were narrow enough to clip their contents.
- **The standalone CSS ships minified (#371).** The build minified HTML and our JS but swept css/*.css into the image untouched, and the minification gate only ever looked at JS. A clean-css pass runs over the stylesheets now (patternfly included), and the gate flags unminified CSS.

### Fixed

- **The AsciiDoc PDFs got their footer separator rule back (#366).** DocBook has no thematic-break element, so asciidoctor turned the report's `'''` into a processing instruction that pandoc dropped, and the rule above the attribution footer vanished on the adoc path while surviving on markdown. A Lua filter reinserts it.
- **Bold survives an underscore in a cluster name (#357, #359).** breakableIdent, which sprinkles zero-width spaces into long identifiers so LaTeX can wrap them, dropped one right against a Markdown emphasis character, so a name like `cluster_a` lost its bold. Fixed at the root rather than with the per-report band-aid the first pass reached for.
- **The Migration Guide's StorageClass YAML stopped truncating (#373).** A generated `oc apply -f - <<EOF` in a bare command box made the browser parse the heredoc marker as a tag and swallow the manifest. Wrapped in a preformatted block with the marker escaped, and the interpolated datastore values escaped too, since the guide is built with innerHTML.

### Security

- **The customer-string pre-commit hook stopped hanging (#370).** The guard scans the whole tree against a local banlist that has grown to 258 patterns, and pointing BSD grep at a 3.2 MB minified source map with an alternation that size took long enough that the honest response was `--no-verify`, which turns the guard off exactly when it matters. The vendored source maps and the Chart.js full source now sit outside the scan (our own code is still checked), and the whole-tree scan is back from a hang to about eleven seconds.

---

## [2.4.0] - 2026-07-19 - End of the World Cup

Cluster Architecture and the storage LUN model, in that order of size. Workload types now come out of the export instead of a fixed list, the calculated architecture draws itself into the Deployment Plan PDF, and the sizing underneath got taken apart after a pre-release review found it was eating whole workloads on the tidiest estates. A customer with two servers gets a plan now, which is a sentence that should have been true a while ago. Then the storage LUN view got rebuilt: it leads with the measured fan-out, sizes against min(what a node can address, what the array can actually present), stops counting local RAID controllers as SAN, and shows per-storage pooled pressure so a shared array at 120% cannot hide behind three clusters that each read a comfortable 40%.

### Added

- **Workload types are detected, not assumed (#253).** A fallback chain reads the vCluster Platform column first, then guest OS, then folder / resource pool / VM naming, then custom-attribute columns, and stops at whatever answers. The old behaviour was a hardcoded list of six. Types found by heuristics arrive unticked with a `detected` tag (#255, #257): a guess never enters the architecture unless someone agrees with it.
- **How this works, in a modal (#253).** The lightbulb beside Available Workload Types explains what the export needs to look like, with an example. Detection is only obvious to the person who wrote it.
- **Dedicated SVG icons per workload type (#256).** Emoji rendered differently on every machine a TAM might present from.
- **The calculated architecture, in the PDF (#268).** The Deployment Plan carries a vector figure of the target architecture: top-down, Red Hat palette, real text, about 9 KB. It is drawn from the architecture object rather than screenshotting the page, so the same estate produces the same figure on any monitor. Reading the DOM was tried first and the output made the case against it.
- **Single-node and two-node topologies (#272, #273).** OpenShift runs on one host and on two with an arbiter; RVTMA required six and skipped anything smaller. A small site now says what it gets before the calculation, and the two-node notice is blunt about the arbiter not being optional.
- **The plan says when it is not what you asked for (#277, #278).** A site that cannot carry HCP gets the standalone cluster it can carry, and the result explains why rather than swapping the topology behind the analyst's back.
- **LUN fan-out leads the storage reports (#329).** VMware consolidates many VMs onto a handful of datastore LUNs; per-PVC Direct SAN turns each virtual disk into its own LUN. Storage Analysis and Storage Architecture now open with that fan-out (LUNs today to projected PVCs) and the hottest per-host density, instead of a utilization percent resting on an HBA-count proxy.
- **Per-storage pooled pressure (#340, #342).** A new view, in the report and on the Storage Architecture screen, sums the projected PVCs of every cluster sharing an array (fair-shared across the arrays each cluster spans) against the array config max. It catches the shared array at 120% that a per-cluster gauge never sees, because each of its three clusters reads 40%. On screen it reacts live to the Max LUNs you set per array.
- **A researched per-vendor config-max table (#346).** The default array ceilings were a five-entry stub with Dell PowerMax stuck at 4096, which made a real nine-cluster array read 233% when it is nearer 15%. The defaults now come from vendor config-maximums docs (Hitachi VSP and IBM DS8000 65,280, PowerMax 64,000, Huawei Dorado 16,384, IBM SVC/FlashSystem 15,864, HPE 3PAR 32,768, Dell Unity 1,500), each with its source in a comment. The architect still overrides with the real number.

### Changed

- **The hosting cluster stopped eating workloads (#276, #277).** It took the first N rows of the vHost sheet and reserved room for exactly one hosted cluster no matter how many workloads existed. On a twelve-host estate organised into four workload clusters, that meant two of them silently ceased to exist. It now takes the hosts nobody claims (or the ones a large workload can spare) and sizes itself around every workload, not one.
- **HCP is recommended from 12 hosts, not 7 (#278).** A hosting cluster carries the control planes of everything under it. The old threshold recommended HCP to a whole band of estates where the field answer is standalone.
- **The control plane costs 4 vCPU per node, and the taint costs nothing (#274).** `PreferNoSchedule` steers scheduling; it does not remove capacity. The reservation sits beside the total rather than being subtracted out of view.
- **Reset Configuration restores the detected defaults (#261),** rather than ticking everything including the guesses.
- **Calculate Architecture is the screen's primary action (#262),** View Diagram moved to the page header with the PDFs it belongs beside (#263), and Check System Status moved to About (#264). The diagnostics row is gone: it re-stated the page it sat on.
- **The sample export is a realistic estate (#254),** 12 hosts and 100 VMs across two datacentres instead of 536 hosts of nothing in particular.
- **The LUN utilization denominator is min(node ceiling, array config max) (#337).** Ports x 2048 had the wrong shape: an array's config max is fixed per array, not per cabled port. Capacity is now the smaller of what an OpenShift node can address (nodes x 2048 with LUN boost) and what the array can present. A two-node cluster on a giant Hitachi is node-bound; a many-node cluster on a small array is array-bound.
- **Platforms Used counts a lone Windows box (#341).** The Deployment Plan derived platforms from a winner-take-all per-cluster vote, so one Windows VM among two hundred Linux vanished from the summary. It now unions that vote with the OS each VM actually reports, and the box someone forgot to plan for shows up. Host allocation is untouched.

### Fixed

- **Workload type names are escaped before reaching innerHTML (#266).** For an unrecognised Platform value the type IS the customer's raw cell, and there is no CSP. The checkbox label a hundred lines below had been escaping the same value all along.
- **Results and header buttons survive navigation (#267).** Calculating, leaving the tab and coming back twice used to lose View Diagram and both PDF buttons permanently, with the architecture still valid. Coming back once was worse: the button was there and rendered its diagram into a hidden div.
- **`hasCustomPlatformColumn` was false for everyone (#269).** An analyst who had shipped a Platform column was told, three seconds after calculating, to add the column they already had.
- **A five-host cluster reports its two workers (#272).** It used to say "compact-3-node, 3 control planes, 0 workers" while carrying five hosts.
- **The size dropdowns stop offering hosts that do not exist (#275).** "6 hosts - Ideal HA" was on the menu for a customer with two.
- **A dongle VM is a two-step migration, not a dead end (#279).** Land it anywhere, then live-migrate it onto the host holding its device. The card now says so, and says the constraint that follows: live migration does not cross cluster boundaries, and the architecture calc does not know these devices exist.
- **The DETECTED tag fits its box (#257),** and the architecture warns when a detected workload never made it into the plan (#258).
- **Local RAID/HBA controllers are not SAN arrays (#339).** Dell PERC, IBM ServeRAID, Broadcom MegaRAID, Lenovo RAID adapters and BOSS boot devices show up in vMultiPath with `naa.` IDs exactly like a SAN LUN, so the array detection listed them as storage and let their vendor seed the config-max lookup. They back CoreOS boot disk only. Filtered by the disk's SCSI INQUIRY vendor/model, since IBM is both DS8000 SAN and ServeRAID local. On the real exports this dropped 768 of 854 detected "arrays".
- **The Server Allocation Plan drops the utilization target on 1-2 node estates (#345).** It reported "Target Utilization: 25%" and promised headroom on a two-node cluster that uses both hosts and cannot take a percentage of itself. The Deployment Plan got the same cleanup, plus a phantom empty DMZ and a Phase 3 that rendered 0/0/0 waves when there were no hosted clusters (#332).
- **The Vendors column reads A, B (#343).** SCSI INQUIRY vendor and model are space-padded fixed-width fields, so the joined column printed "HITACHI  , Lenovo" with the padding sitting before each comma. Trimmed.
- **Subscription Estimator: page breaks and host hardware (#344, #347).** The report tried to page-break with a raw `\newpage` that pandoc rendered as a literal "ewpage"; removed. The Hardware column printed the raw SMBIOS model, so a Lenovo host read "Lenovo Lenovo ThinkSystem SN550 -[7X16CTO1WW]-"; the duplicate vendor and the bracketed machine-type are stripped now.
- **The Physical Network VM Data Interface reassigns the bonds (#348).** With 6 NICs, changing it did nothing: the select had no change handler, and the topology hardcoded bond1 to VM traffic and bond2 to live migration regardless. It routes VM data to the bond you pick and live migration to the other now.
- **Every physical-network bridge carries an origin badge (#336),** green for the management bridge the cluster already owns, blue for the ones the migration adds. Bonded strategies used to drop the badge, having baked the origin into the purpose text instead of the flag the renderer read.
- **Report polish (#331, #333, #334, #335).** Storage Architecture cards stopped advertising a click that did nothing; gauge titles, the vendor column header, and subscription page breaks got tidied; and the network guide finished shedding its emoji (the first pass missed the wave and alert markers).

### Removed

- **Refresh Workload Types, Get Physical NICs, Show Statistics (#259, #260, #264).** Detection runs automatically now, the NIC button was a debug helper, and Show Statistics repeated the panel above it. Commented rather than deleted; people get attached to buttons.

### Security

- **The anonymizer's lists were denylists, and RVTools does not use the headers they named.** `^cluster$` does not match "Cluster name"; `^license key$` does not match "Assigned License(s)". A list of what to clean fails open. Real headers added, and the per-customer patterns now live outside the repo in a gitignored file the script refuses to run without.
- **The guards were never armed.** `check-customer-strings.sh` exited 0 when `.banned-strings.txt` was absent, and nobody had one, so the pre-commit hook printed `Passed` on a repo it had never scanned. It fails closed now, and refuses tracked spreadsheets outright: `.xlsx` is a zip, so `grep -I` skips it and a workbook full of customer names sails through looking clean.
- **The version gate could not see the version in the customer's PDF.** `reportAttribution()` reads it from the About block, which the build's matcher did not check. Bumping the three canonical sources would have shipped reports stamped with the previous release. It checks both now.

---

## [2.3.8] - 2026-07-03

Load-screen and Migration Time Estimator polish, a batch of fixes caught in pre-release testing, and one new Storage Analysis view. The change with teeth is still the estimator moving its recompute behind a Recalculate button, which on a large estate is the difference between a responsive tab and a frozen one. On top of that: the per-cluster report no longer zeroes out on a prefixed-format export, the Migration Plan PDF stops failing silently on big plans, and Storage Analysis gains a VM-count-by-disk-size-tier donut. Pull the container and the movement is all on screen.

### Added

- **Partial-export warning (#226).** RVTMA flags an RVTools export that looks partial: far fewer VMs than the host and cluster counts imply, or essential columns (VM name, power state, cluster, host) missing or mostly blank. The "my colleague has 100 VMs but RVTMA shows 18" case is almost always a filtered export, and the tool now says so instead of quietly analysing the 18.
- **Smooth section reveal (#241).** Switching sections fades the content in over about a quarter second instead of snapping to it. Respects the OS reduce-motion setting.
- **VM count by disk size tier (#250).** Storage Analysis groups VMs by total allocated storage into the tiers the online assessment tool uses (0-100 GiB up to 20+ TiB) and draws a doughnut with a centre total and a per-tier legend. The same breakdown rides into the Storage Analysis report.

### Changed

- **Migration Time Estimator recomputes on a button, not on every slider drag (#231, #232, #233, #234, #235).** Nudging network efficiency, latency, overhead, or parallelism used to recompute and re-render the whole panel on every tick, and on a large estate that full re-render is what locked the tab for close to a minute. Now you set the sliders and press Recalculate. The button sits beside the projected completion date, goes red when the shown estimate is stale, and disables itself while it works. The sliders picked up one-line help subtitles on the way through. The calc itself is untouched, pinned by a golden-master test (#228) so it stays that way.
- **Upload progress bar tracks the parse (#237).** The bar advances per sheet as the workbook parses instead of sitting at the start and jumping to done. On a big multi-tab export you can watch it move.
- **Tighter load screen (#238, #239, #242).** The progress bar and the "Processing files..." status moved to the top of the panel, the upload blurb went from seven lines to three, and the dropzone padding came down. The load screen stops popping a scrollbar over content with nothing to scroll, and the progress lands where you can see it.

### Fixed

- **Report tables survive pipe characters (#227, #236).** A port group, NAD, or cluster name with a pipe in it used to break the table it landed in (network remediation guide, NAD report), because a raw pipe is a column separator in those AsciiDoc and markdown tables. Those cells are escaped now.
- **Storage report buttons stop flashing on load (#240).** The eager render that builds every section on load dropped the Storage Architecture report buttons into the shared page-header slot, where they flashed for the half-second before the app moved to the Infrastructure Summary. The slot is cleared after the eager render.
- **Per-cluster report counted 0 hosts and 0 VMs on prefixed exports (#244).** The Source Cluster Profile read the cluster and host columns directly, which only knows the standard RVTools names. A prefixed-format export (vHostCluster) resolved to nothing, so the report showed "Hosts: 0, VMs: 0" for a cluster that had both. It resolves through RVToolsColumnMapper now, like the rest of the app.
- **Decorative emojis out of generated scripts (#245).** The network validation script and the PDF-generation wrapper embedded magnifier, check, cross, and warning glyphs in their echo lines. These are scripts a customer runs; the glyphs are gone, replaced with plain ASCII markers.
- **Tiny OS slices stop rendering white (#246).** In the OS Categories pie the sub-1% slices were thinner than the 2px white border between segments, so they painted white. The border dropped to 0.
- **RDM command examples render as code blocks (#247).** The commands under "Strategies for Raw Device Mappings" collapsed onto one line for lack of a `<pre>` wrapper; wrapped now, with the heredoc marker escaped so it prints.
- **Migration Plan PDF stops failing silently on large plans (#249).** A hundreds-of-VM plan tripped nginx's body-size cap or render timeout before a PDF came back, and the client masked every failure as "no PDF support". Raised the ceilings (body 10m to 100m, timeout 600s to 1800s, pod memory 2Gi to 4Gi) and the client now surfaces the real status (413 too large, 504 timeout).

### Removed

- **The hollow "Migration Guide (MD)" download (#248).** It generated a skeleton: one real infra-summary line and then [placeholder] sections, so it shipped a mostly-empty document. The real guidance is the on-screen Migration Guide and the reports that carry data (Datacenter, Storage Architecture, Subscription, Risk).

---

## [2.3.7] - 2026-06-26

The dashboard release. 2.3.6 was all pipeline and no UI; 2.3.7 is the pile of features that built up behind it. If you only pull the container, the headline is the new stuff on screen, not the plumbing.

### Added

- **Hardware profiles per cluster (#201, #202).** The Infrastructure Overview groups each cluster's hosts by physical spec (vendor, model, sockets, cores per socket, memory) and shows how many distinct profiles a cluster has, so you can tell at a glance whether the hardware you are about to reuse as OpenShift nodes is consistent or a mixed bag. Mixed clusters get flagged. The same breakdown rides into the Datacenter Report.
- **CUDN NAD export (#193).** ClusterUserDefinedNetwork joins the three existing NAD forms (OVS bridge vmnet, OVS localnet, Linux bridge), so the modern OVN-Kubernetes path is a download instead of a hand-edit.
- **Size-aware standalone default (#197).** Small environments (largest datacenter at or under 6 migrable hosts) default to standalone clusters instead of Hosted Control Planes, which matches how most of the small estates actually get built. Larger environments still default to HCP. The toggle survives a re-render either way.
- **Standalone topology diagram (#196).** Standalone mode draws its topology now instead of leaving the canvas blank.
- **Optional report attribution (#210).** A one-line "generated by RVTMA, adjust to the engagement" note on the last page of every generated document report. On by default, toggled from the Settings screen; off makes the reports byte-identical to before.
- **Opt-in update check (#190).** The About panel can check GitHub for a newer release and put a halo on the version badge when one exists. Opt-in, nothing leaves the browser unless you ask for it.
- **Storage Architecture report (#219).** The Storage Architecture screen downloads now (MD and PDF), like the rest. The report carries the per-cluster capacity table (HBAs, LUNs in VMware vs projected PVCs), the shared storage arrays it detects by NAA prefix, the Direct SAN scaling advisory with the backup-vendor caveat, and the LVM note - the picture that used to live only on screen.

### Changed

- **Cluster diagram restyled (#200).** Bespoke styling with a dark-mode canvas that does not blind you, and the Bootstrap tell is gone.
- **Masthead GitHub button points at the container package (#199).** The icon up top now takes you to the published image on GitHub Packages.
- **Cluster report repositioned as a Source Cluster Profile (#218).** The per-cluster download stopped pretending to be a WIP "migration plan" and became the honest source-side picture, pointing to the Migration Time Estimator for the wave plan and Cluster Architecture for the target topology. Dropped the placeholder "node preparation" commands (hard-coded fake node names; node prep is target-side anyway) and a stray code-block formatting bug.

### Fixed

- **Cluster count excludes standalone hosts (#194).** A datacenter with standalone hosts was inflating the cluster count by one. The standalone bucket is not a cluster.
- **PDF center footer truncates at a word boundary (#191).** No more words cut in half.
- **Cluster cards stopped pretending to be clickable (#212).** The Cluster Architecture cards borrow the Blast Radius card styling, which carries a pointer cursor and hover because those cards actually select something on click. The architecture cards never wired one, so the cursor promised an interaction that never happened. Affordance dropped; card detail still opens via the per-card toggles.
- **Datacenter analytics charts render crisp in the PDF (#215).** They were captured at on-screen resolution (~335 px) and looked jagged once scaled to print; now captured at ~3x so they stay small on the page and sharp.
- **VMkernel network alerts render correctly (#217).** The alerts built HTML strings that the risk-card escape below then printed literally (the `<strong>` tags showing as text). Restructured to escaped label/detail items.
- **Report attribution footer set apart from the body (#216).** A thematic break before the line so it no longer crowds the last paragraph of the report.

- **Risk-card detail escaped by default (#209).** Risk-card item detail was rendered as raw HTML; it now escapes by default, with an explicit opt-in for the two cards that ship trusted markup. Closes a latent injection path.

### Performance

- **vInfo lookups indexed by name (#208).** The migration-time Excel and report builders ran a linear scan per VM, quadratic across an export. They now build a name-to-row index once. On a ~9,500-VM export that is the difference between an export and a coffee break. Output is byte-identical, proven by lookup equivalence across the corpus.

---

## [2.3.6] - 2026-06-11

Image and release-pipeline hardening. No dashboard changes - if you only use the web UI, nothing moves. If you pull the container, this is the release to announce: same features as 2.3.5, cleaner image, and a pipeline that proves what it ships.

### Security

- **Base image CVEs patched (#185).** The pandoc/extra base shipped stale openssl (CVE-2026-45447) and libtiff (CVE-2025-9900); the image now upgrades base packages at build time. The bundled pandoc-include tool carried a vulnerable lxml (CVE-2026-41066) and was never invoked by our pandoc calls - removed, along with a leftover uv cache.
- **Trivy gate in the release build (#185).** Fixable HIGH/CRITICAL findings now block the push - a vulnerable image cannot reach the registry. Every release also ships a CycloneDX SBOM.

### Changed

- **Release builds moved local (#179, #181, #182, #184).** The image is built, smoke-tested, scanned and pushed from the maintainer's machine via build-multi-arch.sh; the git tag is a pure version marker and the GitHub workflow remains as a manual fallback. The registry copy is verified after every push (both architectures listed, latest matching digests, pulled image smoke-tested). Only the version and latest tags land in GHCR - the per-arch scaffold tags are gone.
- **PDF render smoke in the release gate (#186).** The image validation now POSTs a document down both render paths (markdown and asciidoc) and demands real PDF bytes back, covering the CGI -> pandoc -> lualatex -> eisvogel -> fonts chain that a base-package upgrade can silently break.

---

## [2.3.5] - 2026-06-10

Multi-NIC release. MTV reliably reconfigures one NIC and leaves the rest for someone to sort out at 2 AM; RVTMA now covers that whole workflow - triage on screen, planning data in the XLSX, console commands for the cutover. Plus per-cluster reporting and two report fixes. Test suite grew from 443 to 557 along the way.

### Added

- **Multi-NIC Source Inventory** XLSX tab (#167, #168). One row per NIC for every 2+ NIC VM: MAC, port group, VLAN, MTU, adapter, IPv4 when the export carries guest IPs, switch, expected NAD name, red flags. Reads both RVTools column dialects.
- **Multi-NIC Simplification Candidates** XLSX tab (#168). Flags same-portgroup NIC groups with a confidence grade: independent guest IPs mean intentional multi-homing (do not consolidate), a shared IP means a near-certain leftover P2V bond, no IP data stays verify-in-guest. Flags for review, never prescribes.
- **Network Remediation Guide**, MD + PDF (#170). The engineer's bench document for console-only network reconciliation after migrating a multi-NIC VM: console reference per OS flavor (only the flavors present in the dataset) plus per-VM network facts. Windows templates marked experimental pending validation on a live migrated VM.
- **Risk Summary card** for same-portgroup NIC groups (#171). Warning-level, splits likely bonds from multi-homed VMs, counts every VM regardless of wave scope.
- **Per-Cluster Breakdown** XLSX tab (#173). VM count, powered-off and template counts, vCPUs, memory, provisioned GB and RDM presence per source ESXi cluster, with a one-line pointer in the Migration Plan (EN/PT-BR/ES). From an external RFE; Excel pivot tables cover the group-by-pattern half.

### Fixed

- **Excluded RDM hidden when the plan has zero in-plan RDM (#166).** A fleet of powered-off RDM VMs rendered as "no RDM detected" in both the PDF and the XLSX. Both now surface the excluded breakdown.
- **Literal i18n placeholders in the Migration Plan (#174).** "Parallel Time ({0}x) (1x)" and "{0} 10 VMs AND {1} 60 minutes" had been shipping since the April i18n migration. Fixed in all three languages and fenced twice: a full-plan render test and a static sweep over every t() call site in the app (#177).

### Security

- **vitest 3.2.4 to 4.1.8 (#165, Dependabot).** Clears a critical advisory in the Vitest UI server plus a transitive DoS, both dev-only. The suite runs faster under v4.

---

## [2.3.4] - 2026-05-31

RDM accounting release. The Migration Plan PDF was under-counting VMs with RDM versus the on-screen Risk Summary because the report only iterated the wave-eligible scope. Fixed in the adoc (#163), and now mirrored in the Wave Execution Playbook XLSX so both reports tell the same story. Also picked up a transitive security patch.

### Fixed

- **RDM count parity between the Migration Plan PDF and the dashboard Risk Summary (#163).** `_generateRDMSection` was reading `selectedVMsData` only, which the v2.0 wave generator strips of powered-off, template, and guest-not-responding VMs by default. Symptom: dashboard showed 32 VMs with RDM, PDF showed 26, customer asked why. The fix passes the three excluded lists into the section so it can emit a `NOTE:` callout above the RDM tables surfacing the gap (count by exclusion reason, plus a pointer to the Excluded VMs sheet in the XLSX). The "26 of 879 selected VMs" count itself is unchanged - it is the correct number for what the migration plan actually moves; the callout is purely additive transparency, and the no-exclusion path keeps its v2.x output byte-for-byte. Same `NOTE` text translated to PT-BR and ES.

### Added

- **RDM Analysis sheet enrichments in the Wave Execution Playbook XLSX.** Two parts:
  - **OS family split** in the RDM SUMMARY section. New rows for "Windows VMs" and "Linux / Other VMs" with VM count and RDM disk total. The conversion path diverges hard between MSCS / SQL FCI on Windows and multipath / Oracle RAC on Linux, so the planner needs the totals up front instead of having to filter the per-VM table manually.
  - **RDM IN EXCLUDED VMs section** when at least one excluded VM carries RDM. Columns: Exclusion Reason, VMs, RDM Disks, Windows, Linux/Other, Recommended Action. The Recommended Action gives bucket-level guidance (`Decommission candidate or cold migrate` for powered-off, `Rebuild on target as DataVolume` for templates, `Remediate VMware Tools first` for guest-not-responding). Mirrors the new adoc NOTE so the XLSX and PDF totals reconcile. Section is omitted entirely when there is nothing to surface, so RVTools exports without excluded RDM render byte-for-byte the same as v2.3.3.

### Changed

- **Shared exclusion logic between adoc and XLSX.** Extracted `MigrationHierarchicalReport._splitVMsForWaves(vms)` and `MigrationHierarchicalReport._buildExcludedRDMBuckets(...)` so the Migration Plan generator and the Excel exporter compute exclusions and RDM aggregates from a single source. Both helpers are unit-tested. Behavior of `generateMigrationPlan` is preserved (the 9 existing RDM section tests pass unchanged); the extraction is purely mechanical.
- **Dev server `scripts/dev-server.py`** anchors its working directory to the repo root before serving, so launching it from a stale worktree or an IDE that picks the wrong CWD no longer 404s on `index.html`.

### Security

- **`ws` dependency bumped from 8.20.0 to 8.21.0 (#162, Dependabot).** Patches a remote memory exhaustion DoS in the WebSocket library. RVTMA does not ship a backend and `ws` arrives only transitively through `jsdom` used by the vitest suite, so the runtime risk to dashboard users was already nil. Bump keeps the security tab clean.

---

## [2.3.3] - 2026-05-07

### Fixed

- **Subscription Estimator PDF table widths.** The 4 wide tables in the report (SKU Comparison, Per-Cluster Breakdown, CPU Model Distribution, Per-Host Inventory) were rendering with cramped text columns and bloated numeric columns. Pandoc pipe tables size columns by the relative dash count in the separator line; the previous uniform `|---|---|---|` form gave a Yes/No boolean the same space as a long SKU name. Switched to proportional dashes so SKU names, hostnames, hardware strings, and CPU model strings get the lion's share of the row, and numeric / Yes-No columns collapse to narrow.

---

## [2.3.2] - 2026-05-07

Infrastructure-only release. Repository history was reset to a single snapshot ("Mirror Sync") to retire stale state from the elastocera transfer; no functional or behavioral changes from v2.3.1. The `build.sh` and `build-multi-arch.sh` contributor scripts now default to `ghcr.io/elastocera` instead of the personal Quay namespace, catching up with the workflow change shipped in v2.3.1. Image rebuilt at `ghcr.io/elastocera/rvtma:2.3.2`.

---

## [2.3.1] - 2026-05-06

**OSS readiness release.** Move to the `elastocera` GitHub org, license flips to Apache 2.0, container images now ship from `ghcr.io/elastocera/rvtma` instead of a personal Quay namespace, and a sweep of the docs and code so visible URLs match the new home. No user-facing app changes; if you only run the dashboard, a `podman pull` is enough.

### Changed - License

- Relicensed under **Apache License 2.0**. Replaces the PolyForm Noncommercial 1.0.0 used since v1.1.50, which was source-available but not OSI-approved and blocked legitimate OSS use. Same license as the rest of the elastocera projects (`ovtools`, `safeshift-doc-gen`).
- The PolyForm "Required Notice" header in `index.html` came out with the license switch.
- About dialog License row, Copyright link, GitHub buttons, and the masthead GitHub icon all point at `elastocera/rvtma` now. Anyone clicking through the UI lands on the right repo.

### Changed - Repository home

- Repo transferred from `linuxelitebr/rvtma` to `elastocera/rvtma`. GitHub keeps the redirect from the old URL indefinitely, so existing clones, bookmarks, and historical CHANGELOG PR links keep resolving.
- `package.json` `repository`, `homepage`, and `bugs` URLs updated.
- README, QUICKSTART, blog post, scripts, and the OpenShift Deployment manifest swapped to the new URL.

### Changed - Container image registry

- Image build pipeline pushes to **`ghcr.io/elastocera/rvtma`** (org-owned GHCR) instead of `quay.io/andrerocha_redhat/rvtma` (personal Quay namespace). Public, no authentication required to pull. The `quay.io/andrerocha_redhat/rvtma:*` tags stay pullable as a read-only archive for users who pinned them.
- Auth via `GITHUB_TOKEN` with `packages: write`. No more per-repo Quay secret to maintain.
- `workflow_dispatch` trigger added so a one-off rebuild does not require bumping a tag.

### Changed - Documentation

- README `## Running` restructured as `## Quick Start`: published container image first, OpenShift second, direct `index.html` last with a clear caveat that PDF generation does not work in that mode.
- Surgical removal of "battle-tested", "comprehensive", "seamlessly" and similar marketing-y adjectives across `README.md`, `docs/blog-post.md`, `docs/presentation-guide.md`, `docs/rvtma-advantages.md`, `docs/rvtma-features.md`, and `docs/rvtma-features-bullets.md`. Decorative Unicode arrows replaced with ASCII `->`. Em-dash in `docs/ARCHITECTURE.md` H1 swapped for ASCII hyphen.
- `RELEASE.md` runbook refreshed: GHCR links, `GITHUB_TOKEN` instead of Quay secrets, plus a new section on `workflow_dispatch` for manual hotfix rebuilds.
- Code comments and log messages in `js/` were deliberately not touched. Voice fixes there are backlog: each file gets cleaned when someone touches it for another reason.

### Added

- `THIRD_PARTY_LICENSES.md`: tracks the bundled libs (Chart.js, SheetJS, ExcelJS, Mermaid, PatternFly) and their license terms with upstream links.
- `scripts/check-customer-strings.sh` + `.banned-strings.txt.example`: per-developer guardrail that scans tracked files against a local banlist and exits non-zero on match. Pre-commit hook installs with a one-line symlink. The banlist itself is gitignored and lives only on each contributor's machine.
- `CONTRIBUTING.md` gained a **Customer data** section explaining the rule and how to install the hook.

### Removed

- `docs/v2.0-roadmap.md`. The doc was a v2.0 planning artifact; everything it described shipped between v2.0.0 and v2.0.4. Backlog moves to GitHub Issues.
- `screens/rvtma-screens.gif` (and its generator script). The GIF was captured against the pre-v2.0 UI and no longer represented what the app looks like.
- README `## To Do` section. The 5 items were all shipped.

### Fixed

- `.github/workflows/ci.yml`: action SHAs pinned and `permissions: contents: read` declared. CI was startup_failing on every push since the org transfer because elastocera's Actions policy requires SHA-pinned actions. `build.yml` was already pinned during the GHCR migration; this brings `ci.yml` in line.
- Replaced a customer identifier that had slipped into JSDoc comments with the neutral placeholder `siteA` in `js/nad-yaml-generator-multi.js`. 4 occurrences in module-level documentation, no code paths affected.

---

## [2.3.0] - 2026-05-05

**Subscription Estimator release.** New Architecture-section screen that surfaces the physical inventory and subscription sizing a customer needs to evaluate Red Hat OpenShift on the migrated infrastructure. Anchored end-to-end against the [official Red Hat OpenShift self-managed subscription guide](https://www.redhat.com/en/resources/self-managed-openshift-subscription-guide); every assumption is printed inline so the procurement reviewer can audit the math against the source.

### Added - Subscription Estimator screen (Architecture > Subscription Estimator)

- **Physical inventory panel.** Reads the RVTools vHost tab through `RVToolsColumnMapper` so legacy unprefixed and modern vHost*-prefixed exports both resolve. KPI tiles (hosts / sockets / cores / memory) with the auto-scaling memory format from the storage panels (sub-GB → MB, 1024+ GB → TB one decimal). Per-cluster, per-CPU-model, and per-host tables underneath.

- **Two subscription scenarios side by side.**
  - **1:1 Replatform (worst case)** - every source VMware host becomes an OCP worker.
  - **OCP Right-sized (efficient)** - reads the topology computed by the Cluster Architecture screen and counts only the chargeable workers (worker-role rule confirmed against a real `oc get nodes` output: dedicated control planes are exempt; compact clusters charge every node because their CP nodes also carry the worker role).

  Each card shows a right-sizing delta when both scenarios are available so sales has a single number for the procurement-savings conversation.

- **Dual commercial model** per the official subscription guide:
  - **Bare-metal node** - 1 subscription per physical server, regardless of sockets or cores.
  - **Core-pair** - 1 subscription per 2 physical cores (or 4 vCPUs).

  Each scenario card surfaces both totals and flags whichever is cheaper for the fleet with a `[BEST FIT]` badge. The customer's procurement signs the contract that lands the smaller total.

- **SKU comparison** for the four OpenShift editions a virtualization migration project typically evaluates - **OpenShift Virtualization Engine**, **OpenShift Container Platform with Virtualization**, **OpenShift Platform Plus**, and **OpenShift Container Platform** (containers-only baseline). Engine carries a `BARE-METAL ONLY` pill since per the subscription guide the core-pair model is not applicable to that SKU; other SKUs accept both models. With-virtualization SKUs render first since OCP Virtualization is the project's primary use case.

- **Three sales-track charts** - cores per cluster (top 10), chargeable vs exempt cores donut, CPU model distribution (top 8). Categorical 10-color palette so adjacent bars are visually distinct (the donut keeps semantic colors: orange chargeable / green exempt). Charts row uses `auto-fit minmax(280px, 420px)` so it gracefully wraps from 3 to 2 to 1 columns on narrower screens instead of overflowing horizontally.

- **Collapsible "How this is calculated" explainer** under the banner. Defaults closed; opens to the four rules (two commercial models, worker-role rule, SKU restriction, 1:1-vs-right-sized) plus a final paragraph clarifying the recurring sizing-vs-subscription confusion (overcommit ratios are a sizing decision upstream of subscription math).

- **Yin-Yang sidebar icon** - radially symmetric and conceptually fits the dual-model framing of the screen (two complementary halves, customer signs the side that fits).

### Added - Subscription Estimator exports

- **Markdown report.** Full report with chart images embedded as base64 PNGs (50% page width; tighter than the OS Report's 60% to leave room for the tall scenario tables underneath). Includes executive summary, both scenarios, right-sizing delta, all three charts, SKU comparison table, per-cluster, per-CPU-model, full per-host inventory, and a disclaimer paragraph that cites the Red Hat subscription guide URL.

- **PDF.** Routes the cached MD through the existing `_generatePDF` server-side pandoc + lualatex pipeline (same path OS Report uses). The PDF button triggers MD generation with `skipDownload=true` if the cache is cold.

- **Excel workbook (two sheets).** *Summary* (totals, both scenarios with both commercial models flagged, right-sizing delta, SKU comparison block, per-cluster breakdown, disclaimer with the Red Hat guide URL) and *Per-Host* (one row per ESXi host with the full hardware columns). Frozen header rows on both sheets; Arial throughout to match the Migration Time Estimator workbook.

Three page-header buttons (`Subscription Report (MD)`, `Subscription Report (PDF)`, `Subscription (Excel)`) wired through navigation.js initSection so they appear when the user navigates to the screen and disappear when they leave.

### Test coverage

- 405 / 405 tests pass (was 309 before this release; +96 new).
- New file `tests/subscription-estimator.test.js` (74 tests) pinning the data layer, math layer, scenario builders, MD report generation, and Excel workbook structure.
- Both legacy unprefixed and modern vHost*-prefixed RVTools shapes pinned through the same regression-coverage discipline applied to the v2.2.1 storage and network fixes.
- New compatibility tests cover bare-metal-node + core-pair semantics, recommended-model tiebreaker, SKU `availableModels` restriction (Engine bare-metal-only), the per-SKU `_subsForSku` helper, and the chart-anchor / report-shape invariants.

---

## [2.2.1] - 2026-05-04

**RVTools 4.x compatibility + OS Report PDF chart restoration.** Two unrelated regressions surfaced on a real customer file. No new features, no schema changes; both fixes are additive (legacy unprefixed RVTools shapes still resolve through the same path they always did).

### Fixed - RVTools 4.x prefixed-column compatibility

Modern RVTools exports carry every column with its tab prefix (`vSC_VMKPortGroup`, `vMultiPathCluster`, `vInfoVMName`, `vDatastoreClusterName`, etc). Several callers had bypassed `RVToolsColumnMapper` or were using local `_getValue` helpers with candidate lists that only knew about the legacy unprefixed shape plus `mp*`/`vm*` placeholders that no real RVTools build emits. Symptoms on prefixed exports:

- **Network Migration alerts all labeled `unknown:`** because `analysis.js` was reading `vmk['Port Group']` raw, every row hashed into the same `unknown` bucket. Switched to `RVToolsColumnMapper.getValue(vmk, 'vmkPortGroup', 'unknown')`.
- **Storage Architecture rendered as a single "Standalone Hosts" card with 0 hosts / 0 LUNs / vendor unknown.** The vMultiPath lookup chain `['Cluster', 'mpCluster']` matched neither the legacy nor the modern format. Added `vMultiPath*` (the actual RVTools prefix) as a third candidate to every multipath lookup in `js/storage-analysis.js` and `js/storage-architecture.js`.
- **Per-cluster cards showed `0 VMs` and `0 PVCs`** even after the multipath fix, because the vInfo VM-to-cluster join used `['VM', 'Name', 'vmName']` / `['Cluster', 'vmCluster']`. Added `vInfoVMName` and `vInfoCluster` as fallback candidates.
- **`datastoreCluster` column was empty in MTE Excel and Cluster Architecture** - real exports use `vDatastoreClusterName`. Added it to `datastoreCluster.alternatives` in the column mapper.
- **`vClusterHAEnabled` never resolved on older RVTools** that emit the bare `vClusterEnabled` (the HA flag's older name, before the explicit-naming refactor). Added it to alternatives.

All 5 fixes are additive: legacy unprefixed candidates remain first in every list, so existing user files resolve through the same path they always did. Mixed-format paranoid tests pin that legacy candidates still win when both shapes appear in a single row, so derivative tools that emit the legacy shape cannot accidentally re-route through the new candidate.

### Fixed - OS Report PDF strategic charts ("Chart not available")

The 5 strategic charts in the OS Report PDF (Migration Readiness Timeline, Risk Level Distribution, OS Vendor Diversity, OS Age Distribution, Support Tier Distribution Analysis) were rendering as the literal `*Chart not available*` placeholder. The 4 original charts on the same page worked.

Root cause: the v2.1.0 dark/light theme contrast pass refactored the OS Analysis modal HTML from inline-style `border-radius` cards to the `.os-chart-card` CSS class. The strategic-charts extension finds its insertion anchor by walking up from `#modalDataQualityChart` with `.closest(...)`. The selector was still the legacy `[style*="border-radius: 8px"]` which only matches inline styles - on the modern markup it returned null, the strategic `<canvas>` elements never made it into the DOM, the 5 `render*Chart` calls returned early at `if (!ctx) return`, and `captureChartImages` produced an object missing the 5 strategic keys. Selector now falls back from `.os-chart-card` (modern) to the legacy inline-style anchor for any older bundle / cached HTML / derivative tool that still ships the inline-style markup.

### Test surface

- `tests/storage-architecture.test.js` (new, 9 tests): vMultiPath legacy + prefixed paths, dead path skip, shared array detection via NAA prefix, fallback `Standalone Hosts`, mixed-format paranoid (legacy wins), prefixed VM-to-cluster join projecting PVCs and VMs.
- `tests/analysis-network-mapping.test.js` (new, 6 tests): legacy `Port Group`, `vSC_VMKPortGroup`, fallback `unknown` only when no shape resolves, MTU aggregation, mixed-format wins-legacy.
- `tests/os-analysis-strategic-anchor.test.js` (new, 6 tests): modern `.os-chart-card`, legacy inline-style, mixed-markup paranoid (modern wins, closer to canvas), graceful nulls for missing canvas / unrecognized wrapper / null container.
- `tests/rvtools-column-mapper.test.js` (extended, +9 tests): both shapes for `vmkPortGroup`, `datastoreCluster`, `vClusterHAEnabled`; mixed-format earlier-candidate-wins; `vClusterHAEnabled` does not collide with `vClusterDRSEnabled`.

`js/analysis.js` exposes `Analysis._test = { analyzeNetworkMapping, classifyVMKNetwork }` so the per-section analyzer can be driven without mocking every other slice of `allData`. Same convention as `StorageAnalysis._test`. Production code MUST NOT read from the test namespace.

309/309 tests pass (was 278). Zero lint errors.

---

## [2.2.0] - 2026-04-29

**PDF deliverable quality release.** Focused on making the customer-facing PDF reports look professional and behave correctly. One new user-facing setting (Portrait/Landscape toggle) and a sweep of silent download-button bugs that escaped the v2.1.0 testing. No analysis-logic changes - the time/wave/risk math, cluster sizing, and Excel exports compute identically.

### Added

- **Global Portrait/Landscape page-orientation toggle** in PDF Settings. Persists in localStorage, applies to every PDF the app generates. Use case: VM Details has 18 columns and Server Allocation Plan host inventory tables breathe much better in landscape; narrative reports read better in portrait.
- **Datacenter Overview chart animations** on first navigation to the screen. Pies grow + rotate in, bars rise from baseline, hover-offset on slices. Chart creation deferred so the animation actually runs in front of the user's eyes.

### Changed - PDF typography

- Cover page font switched from Source Sans Pro to Red Hat Text + Red Hat Mono. Brand-aligned across body and code blocks. DejaVu fallback chain so any glyph the Red Hat fonts don't ship (≤ ▲ ❒ math + decorative chars) draws via DejaVu instead of being silently skipped.
- Cover title in Red Hat Display Black, subtitle in Bold (Black weight), author + date stay regular Display 14pt - restores the three-tier visual hierarchy. Wrapped-title line spacing fixed: `\baselineskip` was being silently reset to font-natural ~38pt by `\textsf`'s implicit `\selectfont`; landed via counter-gated `\global\setlength\baselineskip{44pt}` injected through a redefined `\textsf`.
- Storage Connectivity & LUN Capacity table headers shortened across all 3 languages: `FC HBAs` → `HBAs`, `LUNs in use (VMware)` → `vSphere LUNs`, `LUNs to be (OCP)` → `OCP PVCs` (the OCP equivalent of a LUN is a PVC - now semantically correct, not just shorter), `Utilization to be` → `Utilization`. Cluster column widened from ~8% to ~21% so identifiers fit. The previous pt-BR / es i18n had `LUNs to be (OCP)` hardcoded in English; now properly localized.
- "Time & Cost Estimate" renamed to "Migration Time Estimate" across all surfaces (button labels, tooltips, filenames, report titles in en/pt-BR/es). The tool computes time only - no license / vendor / operational pricing inputs. The "Cost" framing was setting wrong expectations.
- Risk Summary report title `# RVTMA Risk Summary` → `# Risk Summary`. RVTMA is internal branding - has no business in a customer-facing deliverable.

### Fixed - download buttons

- **Systemic download-button sweep.** A grep across `js/` turned up 9 sites with the same pattern: a function gated on `if (skipDownload) return;` where the click `Event` from EventBinder was landing as `skipDownload` (truthy), short-circuiting before the download fired. Tightened all 9 to `=== true`. Buttons fixed:
  - Datacenter Report (MD)
  - Risk Report (MD)
  - Cluster Architecture Deployment Plan (MD)
  - Architecture Document (ADoc)
  - Storage Analysis Report (MD)
  - Migration Time Estimation (ADoc)
  - Hierarchical Migration Plan (ADoc)
  - OS Analysis Report (MD)
  - Cluster Config (per-cluster MD) - was safe via `data-args` path, tightened proactively
- Two closure-private functions exposed across the IIFE in `js/cluster-architecture-ui.js`: `generateClusterDNSRecords` (used by Server Allocation Plan ADoc download) and `getRawDataFromTab` (used by `host-physical-networks-report.js`). Both threw `ReferenceError` cross-module because closure scope doesn't escape; added `global.X = X` to the existing export block.
- `PDFSettings` and `ModalManager` exposed on `window` so EventBinder's `data-action="X.method"` resolver finds them. Top-level `const X = (() => {...})()` in non-module `<script>` tags creates a lexical binding but does NOT become a property of `window` - pre-existing bug that surfaced when the PDF Settings Save button was first clicked.

### Fixed - PDF tables and layouts

- **Long identifiers no longer overflow narrow columns.** Cluster names, VM names, hostnames, FQDNs - unbreakable strings that LaTeX couldn't wrap, so they overflowed and visually overlapped adjacent cells. New shared helper `breakableIdent()` in `js/common-helpers.js` inserts U+200B (zero-width space) between every character. Lualatex honors ZWSP as a break opportunity natively - names that fit render unchanged (ZWSP is invisible), names that don't fit wrap at any letter. Applied to: Storage Connectivity & LUN Capacity, Migration Time Estimator's Detailed VM List, Hierarchical Migration Plan's Critical/High NIC tables + RDM table, Architecture Document's Physical Infrastructure Details.
- **Column proportion inversions corrected.** Multiple tables had `cols=` specs where Ecosystem (typically a short label) got more width than VM Name / Cluster (typically long unbreakable identifiers). Reverted to explicit percentages with sensible widths across Detailed VM List, Critical/High NIC tables, RDM VM table, Migration Waves Overview, and Server Allocation Plan host inventory.
- Migration Waves Overview Wave column widened to fit OS-grouped labels like `Wave 001 (Windows)` without wrapping mid-word.
- OS Analysis Report charts no longer break pages awkwardly. Nine embedded chart PNGs gained pandoc `{width=60%}` attribute - GFM markdown viewers ignore it, only the PDF output is affected.
- Excel column widths tuned: USB Passthrough Device Type doubled (18 → 36) for USB class strings; VM Details Ecosystem and OS bumped (30 → 36) for long labels; Snapshots Risk Snapshot Name bumped (36 → 50) for backup-tool composites.

### Fixed - misc

- Datacenter Report PDF charts slightly oversized - added `{width=70%}` pandoc attribute.
- Datacenter Overview chart colors and visual hierarchy: `generateColorPalette()` now accepts `startIndex` so the two adjacent pies don't both start blue when there's a single datacenter; Host Models bar chart uses the palette per bar instead of a hardcoded blue.
- Storage Architecture eager render so the Storage Analysis Report picks up user-edited port values regardless of nav order. Previously, users who downloaded the report without first visiting Storage Architecture got fallback values (`hbaCount` auto-detect + 2048 LUNs/port literal) silently.
- 14 orphaned i18n keys in `hierarchicalMigration` synced with the code that uses them (3 languages, 42 entries). The keys *existed* with the right translations under stale identifiers from a refactor that didn't sync; missing-key warnings disappear from the browser console.

### Removed

- **Risk Analysis (WIP) feature on Migration Time Estimator.** 716 lines (`js/migration-time-risk-analysis.js` + button/handler + `<script>` tag + ARCHITECTURE.md mermaid node). The "WIP" label had been in production UI since the original commit, the report self-disclaimed its own unreliability, and zero `rvtmaRiskRegistry` integration meant deleting it didn't subtract anything from the Risk Summary aggregator. Salvage candidates were either superseded by other reports or static playbook content not worth keeping as code.

### Chore / cosmetic

- Emoji ✅ swapped to ✔ in success alerts. Some PDF font fallback chains were rendering U+2705 as a tofu box; U+2714 (HEAVY CHECK MARK) keeps the visual semantic and resolves cleanly via the DejaVu fallback.

---

## [2.1.0] - 2026-04-28

**Open-source readiness release.** 97 PRs ([#31] - [#124]) over 3 days that turned the codebase into a properly modular, tested, accessible, source-available project. Behavior is mostly preserved - this is structure, naming, and contributor-experience work, plus one real download-filename fix and a NAD pipeline rewrite. No new analysis features in this release; the v2.0.4 reports, XLSX export, and cluster sizing all behave identically.

### Fixed

- **`downloadFile()` shadow eliminated for real this time.** The v2.0.4 entry below claimed the inline `downloadFile()` in `index.html` had been unified with the canonical wrapper in `js/cluster-architecture-ui.js`, but the inline declaration was never actually removed - hoisting + script load order meant the bare version (no datacenter prefix, no i18n suffix) kept winning at runtime, and every downloaded report came out with a generic filename. Removed the inline declaration so `window.downloadFile` resolves to the rich version that v1.1.15 originally added. Verified via runtime probe: with `getReportFilenamePrefix()` returning `dc01_vcenter-prod_` and language set to `pt-BR`, the NAD implementation guide now downloads as `dc01_vcenter-prod_nad-implementation-guide-2026-04-28-PT-BR.md` (was `nad-implementation-guide-2026-04-28.md`). All exports affected: NAD migration report (`.adoc`), NAD implementation guide (`.md`), per-NAD YAMLs, cluster reports (`.md`), allocation plan (`.yaml`), deployment plan (`.md`), server inventory (`.csv`), architecture document (`.adoc`). Filenames now consistently group by `{datacenter}_{vcenter}_` prefix in the Downloads folder, which is the v1.1.15 design intent. ([#124])
- `build.sh extract_version()` regex updated to match the post-Phase-4 masthead badge shape (`<span class="rvtma-version-badge">vX.Y.Z</span>`); was still grepping for the old `<small>vX.Y.Z</small>` pattern that hadn't existed since the layout extraction. Also added cross-checks so a drift between the masthead badge, the About panel, and `package.json` fails loud instead of silently picking one.
- File:// load console errors silenced ([#36]) - guard on os-analysis fetch + remove orphan @font-face for RedHatMono in patternfly.css.

### Changed - NAD pipeline rewrite

- **Generator rewritten with 3 canonical templates** (`js/nad-yaml-generator-multi.js`) per Andre's working production examples: OVS Bridge VMnet, OVS Bridge Localnet (per-VLAN bridge mapping), and Linux Bridge. MTU 9000 hardcoded in OVS templates (OVN-K subtracts ~58 bytes for Geneve overhead even on localnet topologies); MTU intentionally omitted in Linux Bridge. NNCP linkage explicit per template. ([#93])
- **New canonical taxonomy** for NAD names: `[<site>-]<NUM>[-<descriptor>]-nic<n>` (e.g. `siteA-101-web-nic1`). Drops the old `vlan` prefix on the ID and the `workload` and `vm` primitives. The `-nic1` suffix applies universally for consistency. Computed by `buildCanonicalNadName({vlan, portGroup, site, nicNum})`. ([#93])
- **Per-row 3-button UX** in `displayNADResults` replaces the old single Copy/YAML pair. Each NAD row now offers explicit Linux Bridge / OVS Localnet / OVS VMnet downloads via `downloadSingleNADWithType` so operators can pick the right type per NAD instead of being locked into one variant. Useful for extinct-VLAN scenarios in migrations. ([#94])
- **Bulk per-cluster downloads** (`downloadNADsWithType`) and the Migration Report's embedded YAML now share the same template path - no more divergence between the per-row download and the in-report sample. ([#94])

### Changed - structure (the big one)

The project went from a 9,369-line `index.html` + 5,775-line `css/styles.css` monolith pair into 66 focused JavaScript modules and 19 CSS modules. **No code or behavior changes during the extraction** - functions and rules were copied verbatim and wrapped in IIFEs / kept in their original cascade order. Smoke-tested against the full UI flow with real RVTools data after every batch.

- **CSS modularization (Phase 4 - 17 PRs, [#95] - [#111])**: `css/styles.css` (5,775 lines) decomposed into 19 modules:
  - `css/base.css` - design tokens (`--rh-*`, `--rvtma-*`) + dark-theme overrides + body baseline
  - `css/layout.css` - PatternFly 6 overrides + page chrome (masthead / sidebar with mobile responsive / breadcrumb / page-header / content-panel / h1 / subtitle / footer)
  - `css/utilities.css` - `u-*` family + flex helpers + loader-spinner + section-header + global utilities (`.rvtma-pulse-critical`)
  - `css/components/{buttons,cards,floating-buttons,forms,tables}.css` - 5 reusable component sheets
  - `css/modules/{blast-radius,cluster-architecture,nad,os-analysis,physical-network,physical-network-modal,risk-summary,storage-analysis,storage-architecture,upload}.css` - 11 feature-scoped sheets
  - `css/styles.css` deleted entirely at the end of the wave (PR [#111]). The cascade order was preserved by loading the new files in the same positions where their rules previously lived inline.
- **JS extraction (Phase 5 - 30+ PRs)**: `index.html` dropped from 9,369 to 666 lines (-92.9%). Both inline `<script>` blocks were drained:
  - The NAD pipeline (~1,800 lines) was split into 5 dedicated modules: `nad-yaml-generator-multi.js`, `nad-analysis-core.js`, `nad-results-renderer.js`, `nad-report-generator.js` (.adoc + .md builders), `nad-ui-handlers.js` (modal/sidebar/floating + execute bridges + Ctrl+Shift+N + Escape).
  - App-wide state declarations (`allData`, `analysisResults`) moved to `js/app-state.js` (loaded as the second `<script>` in `<head>`, before any consumer).
  - Floating-buttons unified controller moved to `js/floating-buttons-controller.js`.
  - Plus 20+ other extracts: file upload, navigation, theme/sidebar, results display, common helpers, network configs, datacenter analysis, migration guide, cluster conversion, etc.
- **`docs/ARCHITECTURE.md` rewritten** to reflect the post-Phase-4/5 module map ([#119]): all file names corrected to kebab-case, new groups for Upload+Init / Network+NAD / Support+Render, removed dead nodes (`network-strategy-helpers`, `pdf-wrapper-modal` - both moved to `/lixo`), added `app-state` + `event-binder` + `error-handler` + `data-validator` to Core, new "CSS Architecture" section listing all 19 modules + cascade order.

### Changed - code quality

- **Module encapsulation (Phase 2 - 8 PRs, [#33]-[#34], [#60]-[#67])**: every mega-module now uses the IIFE pattern with explicit `global.X = X` exports. Reference modules: `modal-manager`, `cluster-architecture`, `cluster-architecture-ui`, `blast-radius`, `os-analysis-modal`, `storage-analysis`, `error-handler`, `data-validator`. Internal helpers stay private to the IIFE - reduced `window.*` leakage substantially.
- **Event delegation (Phase 2.3 - 13 PRs, [#46]-[#58])**: every inline `onclick="..."` in `index.html` and `js/` migrated to `data-action="..."` + `data-args="[...]"` attributes resolved by `js/event-binder.js`. Floating buttons, NAD downloads, navigation, cluster-architecture-ui (15 onclicks), blast-radius (28), migration-time-interface (21), and 17 misc onclicks across smaller modules. New `EventBinder` features: `data-args` JSON support, method-style auto-resolve, auto-preventDefault for `href="#"` nav links.
- **Single source of truth**: `clusterArchitectureInstance` / `architectureUI` ([#67]), `loadedTabs` ([#76]), `networkConfig` ([#78]) all promoted to canonical `window.*` references; their stub re-assignments in other modules removed. ([#68] dropped 8 belt-and-suspenders re-exports.)
- **Dead code elimination across the wave** ([#71], [#72], [#79], [#81], [#86], [#88], [#92], plus drops during many extracts): roughly 1,500 lines of zero-caller / shadowed / deprecated code removed. Notable kills: legacy NAD Debug chain (~420 lines, 9 functions), generateAllValidationTests + 12 sibling functions in the validation/automation chain (~480 lines), 5 orphan NAD/PDF functions, the duplicate `function downloadFile()` in `migration-guide-downloads`, `downloadStorageConfigs` + `generateAllStorageConfigs` + `generateAllVMTemplates` (zero callers), 4 shadowed/orphan host helpers. All confirmed via grep before removal.
- **3 orphan modules moved to `/lixo`** (kept around per Andre's request, not shipped in the build): `blast-radius.legacy.js`, `pdf-wrapper-modal.js`, `network-strategy-helpers.js`. ([#44], [#45])

### Added

- **Tests** (Phase 3 - 4 PRs, [#32], [#35], [#40], [#42]): 23 → 210 vitest tests. Covers `rvtools-column-mapper`, `vdisk-rdm-processing`, `cluster-architecture`, `migration-deadline-calculator`, `migration-os-classifier`, `error-handler`, `data-validator`, `event-binder`, `migration-folder-config`, `storage-analysis`. New helper `tests/helpers/load-script.js` evaluates a vanilla `js/*.js` file in jsdom global scope as if it were a `<script>` tag (hoists top-level functions / vars / classes onto `window`).
- **Accessibility (Phase 6 - 3 PRs, [#37]-[#39])**:
  - `aria-current` dynamic on nav, `aria-hidden` on 18 decorative SVGs in nav + 3 floating buttons, skip-to-main-content link styled via existing tokens ([#37])
  - `ModalManager` focus trap + `role="dialog"` + `aria-modal` + `aria-labelledby` + restore focus on close - centralized so every modal inherits ([#38])
  - `role` / `aria-live` / `aria-atomic` on all 3 `showAlert` variants + `#analysisAlerts` container - danger toasts use `role="alert"` + `aria-live="assertive"`, others use `role="status"` + `aria-live="polite"` ([#39])
- **Error handling (Phase 7 - PR [#35])**: `js/error-handler.js` - global `window.onerror` + `onunhandledrejection` capture + user-facing `showAlert` toast + clipboard fallback. `js/data-validator.js` - pure RVTools workbook shape validation, runs on upload, emits actionable error messages.
- **OSS contributor experience**:
  - `CONTRIBUTING.md` ([#41]) - dev setup, IIFE module pattern, a11y baseline, vitest helper docs, code style (kebab-case, English comments, no em-dash), how to add tests
  - `.github/ISSUE_TEMPLATE/` ([#123]) - form-based bug report + feature request templates with RVTMA-specific fields (RVTools file shape, run mode dropdown, scope selector). Plus a `config.yml` routing blank issues to Discussions and security reports to `SECURITY.md`.
  - `.github/PULL_REQUEST_TEMPLATE.md` ([#123]) - summary, type checkboxes, test plan, contributor checklist mirroring CONTRIBUTING.md.
  - GitHub Actions CI workflow (`ci.yml`) - lint + tests on every PR and push to main.
- **Branding-clean migration**: a customer-named folder template id was renamed to `standard-3-level`, with a transparent localStorage migration for anyone already carrying the old one ([#42]).

### Naming + housekeeping

- All `.md` files at repo root use either standard ALL_CAPS (`CHANGELOG.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `LICENSE`, `README.md`, `RELEASE.md`, `SECURITY.md`) or kebab-case (`support-data-manager-interface.md`, `QUICKSTART.md`).
  - `changelog.md` → `CHANGELOG.md` (uppercase convention - GitHub release tooling expects all-caps) ([#121])
  - `excel_export.md` → `docs/excel-export.md` (kebab-case + moved into `docs/` to match the README intro that lists it as a docs/ file) ([#122])
- All `.js` files in `js/` are kebab-case. Snake_case is reserved for shell scripts and Python (per CONTRIBUTING.md "Code Style").
- `package.json` polish ([#43]): drop deprecated ESLint `--ext` flag, declare `engines.node`, add `bugs` + `homepage` URLs.

### Removed

- `css/styles.css` (5,775 lines) - decomposed in Phase 4, see Changed > structure above.
- The inline `function downloadFile(...)` in `index.html` - see Fixed above.
- Both inline `<script>` blocks in `index.html` reduced to HTML comments pointing at the modules they were extracted to. Block 1 had `allData` + `analysisResults` declarations (now in `js/app-state.js`); block 2 had the floating-buttons IIFE (now in `js/floating-buttons-controller.js`), the Ctrl+Shift+O shortcut (now in `js/os-analysis-modal.js`), the entire NAD pipeline (5 dedicated modules), and the bare `downloadFile()` (deleted - the rich version in `cluster-architecture-ui.js` wins now).

### Deferred to v2.2 (not in this release)

- Phase 8 function/variable naming audit. File names are kebab-case across the board; function/variable name consistency (e.g. `openXxxModal` / `generateXxxContent` / `calculateXxx` / `formatXxx` patterns) was deemed lower-ROI and higher-blast-radius for now.
- PDF font issue: pandoc + eisvogel pipeline ignores the `monofont: DejaVu Sans Mono` directive and bundles Source Sans Pro / Source Code Pro instead of Red Hat Text / Red Hat Mono. Fix path is documented in the project memory (Option A: explicit `mainfont` + `monofont` in metadata.yaml + .ttf accessible to lualatex inside the container).
- Support Data Manager UX: needs a redesign before users can customize OS support tiers safely (open question: how to surface "this list was customized" so audit/handover doesn't lose track).

---

## [2.0.4] - 2026-04-25

Polish release: Risk Summary enrichment, filename consistency, dark-mode pass across multiple screens, plus targeted UX fixes.

### Risk Summary

- **9 new pre-migration risk publishers** (`js/pre-migration-risks.js`) feed the registry once after RVTools load - hardware passthrough (USB/GPU/Serial), active snapshots, suspended VMs, old VM HW version (< vmx-11), guest-not-running, unsupported OS, near-full datastores, templates count, multi-NIC count. The Risk Summary panel now shows the full picture on first visit, regardless of which other screens were opened. Validated against 5025 real RVTools VMs.
- **Report enriched**: intro section ("About this report" / "How to read it" / "What this report is NOT"), executive narrative that adapts to severity counts, and a Reference Documentation section with 8 verified Red Hat / upstream URLs (each link fetched live at commit time).
- **Risk Report (PDF)** button added next to the existing MD button - same pandoc + LaTeX pipeline as the other reports.

### Filename + download consistency

- `index.html`'s naive `downloadFile()` unified with the canonical wrapper in `cluster-architecture-ui.js`: every download now applies the `{datacenter}_{vcenter}_` prefix from `getReportFilenamePrefix()` plus the `ReportI18n.langSuffix()` automatically.
- Datacenter Report routed through the wrapper too (was bypassing the prefix).
- `downloadAllNADsDebug` collision filename fixed (was `networkattachmentdefinitions.yaml` for all 3 type variants - now `all-nads-DATE.yaml`).
- Timestamps added where missing: cluster report, migration guide, network configuration YAML, NAD migration report, NAD implementation guide.

### Network Migration screen polish

- Dropped hardcoded `VLANs: 100-199` placeholder from the Summary callout (was a static placeholder unrelated to the user's actual VLAN data).
- Reframed the 2-NIC bonding strategy block from a fake "🥇/🥈/🥉 ranking" into 3 equivalent options with explicit selection criteria (the tool does not auto-recommend - it emits NNCP YAML for whatever the user picks in the dropdown).
- Added **LACP / 802.3ad** as the first option with prerequisites (4+ NICs + cooperative switch) - canonical enterprise choice when the network team can configure the switch.
- Removed conversational "Your original idea..." copy that read as a residue of a design-time exchange.

### Dark mode pass

- **Cluster Architecture**: alerts switched from left-stripe (Bootstrap-ish) to full-border + halo (`alert-glow` pattern); Precision Tip alert wrong severity color + dark-mode contrast fixed (was rendering as danger paint on info content); workload-type checkbox tint kept user's neutral slate; node-pill outline restored on workload-accent cards (latent template-literal bug was emitting `[object Object]` as the accent class - now uses `accent.color`); accents toned down via `color-mix(<accent> 65%, text-primary)` so cards read as tinted foregrounds instead of saturated highlights.
- **Storage Architecture**: PVC count + per-cluster card pills now visible (was inheriting slate from `--rvtma-c-blue`). Decoupled token usage so the workload checkbox neutral + the Storage vibrant cyan no longer fight for the same token. Utilization bars in the Storage Type Breakdown table now show the colored fill correctly (HTML/CSS structure mismatch fixed).
- **Blast Radius**: Pattern A (c-cp), Pattern C (c-cluster), and Pattern D `.d-warning` block all read in dark; `.d-warning` swapped from left-stripe to full-border + halo. `--c-blue` decoupled inside `.blast-module` from the global `--rvtma-c-blue` (which the user tuned to subtle slate for elsewhere).
- **Migration Time Estimator**: Time Scenarios table (Network Configuration banner, header row, currently-selected row, footer) and the "Migration Plan Report Hack" lightbulb popup all replaced hardcoded light colors with theme tokens. The "Virtual Machines by Datacenter" header now reuses the `.datacenter-header` gradient pattern from Datacenter Overview.

### Smaller fixes

- `cluster-architecture-ui.js`: dropped a bogus `window.downloadStorageAnalysisAsPDF = ...` re-export that threw a `ReferenceError` on script load.
- DevTools "source map error" warnings silenced by adding `chart.umd.min.js.map` (Chart.js v4.5.0) and `exceljs.min.js.map` (ExcelJS v4.4.0) to `assets/js/`.
- NAD body buttons crash fixed: `downloadNADMigrationReport` was throwing when a hosted cluster had zero NADs (`hc.nads[0]` undefined).
- Folder Labels button: dropped the `.sh` script generation - keeps only the `.txt` command list. Button renamed accordingly. Also rebranded `Network Configuration (MD)` → `(YAML)` since the file was always YAML.

---

## [2.0.3] - 2026-04-24

Export button label consistency pass.

Audited every export button across the app and unified the label vocabulary so customers and consultants get the same naming convention everywhere. Pattern: `<Report Name> (FORMAT)` - no leading verb (icon already says download/generate), format suffix in canonical CamelCase (MD / PDF / ADoc / CSV / YAML).

17 buttons renamed across Risk Summary, Datacenter Overview, OS Distribution, Storage Analysis, Network Migration, Migration Guide, Cluster Architecture, Cluster Conversion, Migration Time Estimator, and the NAD body. "Generate PDF" generic labels replaced with `<Report Name> (PDF)` so users see the deliverable name everywhere it appears (button → tooltip → filename → cover page).

Highlights:
- Datacenter / OS / Storage / Cluster Conversion: every "Generate PDF" → `<Report Name> (PDF)`
- Network Migration "Download Network Configuration" → `Network Configuration (MD)`
- Migration Guide "Download Complete Guide" → `Migration Guide (MD)`
- NAD body "Migration Report (ADOC)" → `NAD Migration Report (ADoc)` to disambiguate from the Migration Plan
- MTE "Generate Folder Labels" → `Folder Labels (TXT)` (later corrected from YAML in v2.0.4)

---

## [2.0.2] - 2026-04-24

Cleanup pass on educational content scattered across screens.

The Cluster Architecture and Cluster Conversion screens carried Phase 1-N narratives that overlapped with the data-driven Migration Plan adoc / Wave Execution Playbook xlsx and, in one case, were inventing precision the tool did not have.

- **Cluster Architecture**: section "Migration Plan Generator" renamed to "Architecture Deployment Plan". The phases here describe how to STAND UP the OpenShift architecture (provision N hosting clusters, configure M worker nodes), not how to migrate VMs. Added a "What this is / What this is NOT" callout pointing users to Migration Time Estimator for the actual VM wave plan.
- **Cluster Conversion**: removed the "Recommended Migration Approach" block that emitted Phase 1-4 buckets with hardcoded percentages (20% dev/test, 30% web, 30% middleware, 20% critical) - there was no actual VM classification behind those percentages. Replaced with a callout pointing to Migration Time Estimator for the real, data-driven wave plan. Kept "Specific Considerations for the Cluster" (NICs, hosts, datastores, VMkernels) and "OpenShift Node Preparation Commands".
- Also fixed a hardcoded "VLANs: 100-199" placeholder in the Network Migration Summary banner that was unrelated to the user's actual VLAN data.

Net effect: zero overlap on phase narratives between the 3 educational screens. Each screen owns its scope cleanly. Customer-facing reports stop contradicting each other on what "Phase 1" means.

---

## [2.0.1] - 2026-04-24

Minor release focused on operational clarity: OS-grouped wave plans, report renames for consistent vocabulary, and customer-facing polish on the OS Distribution Analysis screen.

### Headline changes

- **OS-grouped migration waves (optional)**: new toggle "Group migration waves by Operating System" in Settings. When ON, the wave generator emits ALL Windows waves first (simple -> medium -> complex), then ALL Linux, then Other. Wave label gets a `(Windows)` / `(Linux)` / `(Other)` suffix and the Migration Waves sheet gains an **OS Group** column. Same logic mirrored in the Migration Plan adoc/PDF so both outputs produce identical wave numbers. Useful when separate teams own each OS or when tooling preparation (VirtIO drivers, cloud-init) is staged in distinct cutover windows. Trade-off: ecosystems with mixed OS get split across non-adjacent waves.
- **Always-on OS breakdown** in Executive Summary: 4 new lines under KEY METRICS show the OS family distribution of wave-eligible VMs (regardless of the grouping toggle).
- **Customer-facing polish on OS Distribution Analysis**: emoji prefixes (`✅ 🤝 👥 ⚙️ ✗`) replaced with enterprise PatternFly-style typography (uppercase + letter-spacing + semi-bold + pill border + tooltip on `*` qualifier). Chart titles get `chartPie` / `chartBar` SVG icons. Chart animation slowed from 300ms to 1000ms - bars/wedges build up visibly instead of popping in.

### Report renames + filenames

Vocabulary consistency across UI buttons, deliverable titles, and download filenames:

| Old | New |
|---|---|
| Time Estimation | **Time & Cost Estimate** |
| Hierarchical Plan / Hierarchical Migration Plan | **Migration Plan** |
| Excel Report | **Wave Execution Playbook** |
| `migration-estimation-by-datacenter-DATE.adoc` | `time-and-cost-estimate-DATE.adoc` |
| `migration-plan-hierarchical-DATE.adoc` | `migration-plan-DATE.adoc` |
| `migration-plan-DATE.xlsx` | `wave-execution-playbook-DATE.xlsx` |
| Total Data Volume | **Total Data to Migrate** |

i18n titles updated in EN, pt-BR, ES.

### New module

`js/migration-os-classifier.js` - single-source-of-truth classifier with Tools -> .vmx config -> cataloged OS fallback chain. Used by both the XLSX exporter and the Migration Plan adoc generator. 3-bucket simplification (Windows / Linux / Other) instead of the 30+ normalized labels os-analysis.js carries - this hot path runs once per VM during wave generation so a focused helper keeps it lean.

### Smaller fixes and polish

- **Excluded VMs** sheet: per-VM "Action" message for Guest-not-running VMs cross-references the vTools tab to give a specific remediation hint (install Tools, start service, upgrade, or inspect guest console).
- **Settings panel**: Save button moved to bottom (covers all sections above; the Interface toggle saves itself on click).
- **Executive Summary section captions** (KEY METRICS, OS BREAKDOWN, TIME ESTIMATES, CONFIGURATION, RISK ASSESSMENT) now style correctly via captured row references. The previous hardcoded `[5, 14, 21, 29]` row array drifted whenever rows were added or removed elsewhere on the sheet, causing the bold/grey caption fill to land on random data rows.
- **Datacenter Overview cluster cards**: Migration Priority rendered as colored pill chips (red / orange / green / grey N/A) instead of plain text.
- **Cluster Conversion**: 3 emoji titles (Network Configuration, Storage Configuration, Migration Strategy) replaced with `network` / `database` / `target` SVG icons.
- **Hierarchical Datacenter selector**: residual emojis on labels replaced with `building` / `cluster` SVG icons.
- **Physical Network LTR topology**: "Physical network topology" title was being clipped because `.cluster` had `overflow-x: auto` which forces `overflow-y` to behave as auto too (CSS spec) - the absolute-positioned `.cluster-tag` at `top: -10px` ended up outside the visible area. Switched `.cluster` to `overflow: visible` (horizontal scroll falls back to `.pn-pane__body`'s own overflow).
- **Blast Radius modal**: new `xxl` (1400px) modal size preset; `.blast-module` `max-width` bumped to match. Pattern A / Comparison Matrix cards stop getting squeezed at the edges on large screens.
- **Blast Radius prefill bug fix**: "Storage Pool Failure (ODF)" card always reported `0 TB at risk` because the column lookup used the `provisioned` mapper key, whose fallback chain has no `Provisioned MiB` (the modern RVTools 4.x column name). Cascade through `provisionedMiB -> provisioned -> inUseMiB -> inUse` so totals work on every export shape we have seen.
- **Physical Network pane**: min/max height bumped (350/500 -> 420/640; LTR variant 600 -> 760) to fit denser bond/VLAN configurations without inner scroll.

### Validation

- 4/4 deno smoke tests on `MigrationOSClassifier` against 5025 real RVTools VMs (cache invalidation, fallback chain, bucketize partition, classify counts).
- 7/7 structural python tests on the wave assembly pipeline (wave count, sequential numbering, OS purity, OS execution order, crawl-walk-run preservation, VM conservation between modes).
- End-to-end inspection of a customer XLSX: 132 waves with grouping ON, 4510/4510 OS Group cells consistent with wave label suffix, OS breakdown 2887 Win / 1585 Lin / 5 Other.

### Roadmap entry queued

`docs/v2.0-roadmap.md` gained a "Deferred to next release" section: smarter Blast Radius prefill (folder / resource pool / tag classification instead of hardcoded 60/30/10).

---

## [2.0.0] - 2026-04-22

Major release. The XLSX migration plan export expands from 8 sheets to **20 sheets**, transforming the deliverable from "VM list with extras" into a near-final wave-execution playbook the technical team and PM can act on with minimal adaptation.

### Headline changes

- **Wave-scoped XLSX**: every new sheet shows only what is relevant to the migration wave being executed (the selected VMs), not full-environment encyclopedia. Hosts in scope, datastores in scope, clusters in scope, VLANs in scope.
- **Wave split on 3+ NICs**: the user-stated planning rule that VMs with 3 or more NICs ride a separate wave (manual bridge / NAD review per VM) is now enforced in the Migration Waves sheet. Each department produces one or two waves depending on its multi-NIC footprint, with a Notes column explaining why.
- **Wave grouping by ecosystem (the application/system that owns the VMs)**: the existing Hierarchical Migration Plan PDF/MD report groups by Datacenter > Department > Ecosystem and treats each ecosystem as an atomic migration unit (the report says "Each ecosystem should be migrated as a complete unit to maintain application dependencies"). The XLSX now follows the same model - one wave per ecosystem, with the Ecosystem column promoted as the primary grouping. Ecosystems that exceed the per-complexity cap split into numbered subwaves; the ecosystem name still anchors every subwave so the cutover team coordinates with the application owner from a single stable label. Real example from a 2000-VM environment: 118 ecosystems become 197 waves (most ecosystems = 1 wave; large ecosystems like `ECOSYSTEM-INFRA-APPLICATIONS-ALARMS` with 538 VMs split into ~14 subwaves).
- **Wave size caps + complexity-aware chunking** (per ecosystem): each ecosystem's VMs are classified as simple / medium / complex (based on NICs, RDMs, snapshot chains, USB/GPU/Serial passthrough) and chunked into subwaves under a per-tier cap. Defaults: simple=40, medium=25, complex=10. All three caps are user-tunable in the Settings panel. The Migration Waves sheet carries a Complexity column and the Notes column explains every split.
- **Hardware passthrough flagging**: USB, GPU, and Serial passthrough VMs surface in dedicated sheets. The user's current client has these and they cannot be discovered ad-hoc on D-day.
- **Risk Register**: single sheet aggregating every blocker per VM (NICs, RDMs, snapshots, USB/GPU/Serial passthrough, missing tools, old hardware version) ranked by severity. The cutover team's single pane of glass.

### New parsers

`index.html` parses 4 additional RVTools tabs into `allData`:
- `vSnapshot` -> `allData.snapshots`
- `vUSB` -> `allData.usbDevices`
- `vGPU` / `vSCVMotion` (catches both naming conventions) -> `allData.gpuDevices`
- `vSerial` -> `allData.serialDevices`

All four added to the Loaded Data panel so users see them appear after upload.

### New sheets in `migration-time-excel-export.js`

Tier A - foundational inventory (data already parsed, scoped to wave):
- ESXi Hosts Inventory (source-side hosts running selected VMs)
- Datastores Inventory (source-side datastores holding selected VMs)
- Cluster Inventory (source-side clusters with selected VMs)
- VMkernel Networks (mgmt / vMotion / vSAN / FT VLANs per in-scope host)
- VLANs / Networks Map (port groups used by the wave + VLAN ID + suggested NAD type)
- VM Storage Disks (per-disk inventory: thin/thick, RDM, datastore, capacity)

Tier B - migration blockers (require new parsers):
- Snapshots Risk (one row per snapshot - long chains visible)
- USB Passthrough (manual cold migration + reattach planning)
- GPU Passthrough (target node planning - GPU Operator + DRA / vGPU profile)
- Serial Passthrough (rare - network-serial proxy or stay on legacy)

Wave-execution synthesis:
- Risk Register (CRITICAL / HIGH / MEDIUM ranking aggregating all blocker sources)

### VM Details enriched

The existing VM Details sheet gains 4 new columns: Power State, VMware Tools, HW Version, Annotation. All operational fields the cutover team checks anyway - surfacing them in the planning XLSX prevents D-day surprises.

### Implementation notes

- Shared helpers introduced: `_buildWaveScope(selectedVMsData)` builds Set<host>, Set<cluster>, Set<datastore> for O(1) scope membership; `_getVMField(vmName, mapperKey, ...rawColumns)` generalizes vInfo lookups; `_applyInventoryStyles(ws, headerRowNum, dataRows)` standardizes the new sheets' visual treatment.
- Each new sheet renders a friendly empty state when its source data is absent. The output always carries 20 sheets - no ghost content, no surprise failures.
- Wave split logic lives in `_splitDeptByNICs(dept)` which partitions a department's VMs into standard (1-2 NICs) and multi-NIC (3+ NICs) buckets. The wave loop emits one or two rows per department based on which buckets have content.

### Validation fixes (post-implementation pass)

After the initial 7-step build, validated the export against two real RVTools files (7343-VM environment + 674-VM environment). Six column-mapping bugs surfaced, all fixed:

- **Snapshots Risk** - real columns are `Size MiB (total)` (not `Size MB`), `Date / time` is an Excel date serial. Now: MiB->GB conversion, date serial->ISO timestamp, sheet shows `Filename`, `Cluster`, `Host`. Validates against a 30-snapshot chain and a 196 GB chain on real production VMs.
- **USB Passthrough** - real columns are `Device Node`, `Device Type`, `Bus number`, `Unit number`, `Annotation` (no Vendor/Product). Sheet now shows what matters: license dongles ("WIBU-Systems CodeMeter-Stick"), virtual USB host paths, vCenter annotations.
- **VM Details Tools** - Tools status lives in a dedicated `vTools` tab (modern RVTools 4.x), not as a vInfo column. New `vTools` parser + `_getVMTools()` helper that normalizes statuses (`toolsOk` -> "OK", `toolsOld` -> "Old (upgrade available)", etc).
- **VM Details HW Version** - real column is `HW version` (lowercase v). `_getVMField` now builds a lowercase index per row so all casing variants resolve.
- **VMkernel Networks** - case-insensitive raw column lookup (`Mac` vs `MAC`, `Subnet mask` vs `Subnet Mask`). Stripped exports degrade gracefully.
- **Datastore scope** - real vInfo has no `Datastore` column. `_buildWaveScope` now peels datastore names from `Path`, `Log directory`, `Snapshot directory`, `Suspend directory`, plus enriches via vDisk. Verified: 0 -> 6 datastores in scope on the test cluster.
- **Worksheet name fix** - "VLANs / Networks Map" tripped Excel's worksheet validator (forbidden `/`). Renamed to "VLANs and Networks Map".

### Roadmap doc

`docs/v2.0-roadmap.md` documents the audit, scope decisions, and implementation order. Comes back as a reference for the v2.1 follow-up (vTag, vRP, vAnnotation, HA/DRS affinity rules, dark mode refinement, snapshots surfaced in the Risk Summary screen).

---

## [1.1.50] - 2026-04-22

Closes the 1.1.x line. Final UI emoji cleanup before the dark-mode refinement pass that will lead into v2.0.

### Changed
- **Cluster Conversion dropdown**: stripped 🏢 from optgroup labels and 🔹 from option text. `<optgroup label="...">` and `<option>` text are plain-text only - SVG would render as raw markup, and the OS-shipped emojis rendered as misshapen glyphs on systems missing the codepoints. Visual hierarchy of optgroup vs option already conveys datacenter vs cluster grouping.

---

## [1.1.49] - 2026-04-22

Title broadened to include Kubernetes (RVTMA's blueprints work for any KubeVirt-based virtualization, not exclusively OpenShift Virtualization). Plus a sweep of the four UI emojis the user spotted in Cluster Conversion and Migration Time Estimator.

### Changed
- **Title**: "OpenShift Virtualization Migration Blueprint Platform" -> **"OpenShift & Kubernetes Virtualization Migration Blueprint Platform"**. Acknowledges that the architectural artifacts apply to upstream KubeVirt deployments, not only the Red Hat OCP-V product.
- **Cluster Conversion**:
  - 🎯 Select Cluster for Analysis -> target SVG
  - 🏢 cluster-name phase header (post-selection) -> building SVG, matching the Datacenter Overview pattern
  - 🖧 Detailed Configuration of ESXi Hosts step-title -> server SVG. The `🖧` (three networked computers, U+1F5A7) glyph rendered as a black rectangle on systems missing the codepoint - replacing with the `server` icon makes it readable everywhere.
- **Migration Time Estimator**:
  - 🏢 datacenter heading inside the per-datacenter VM list -> building SVG, again matching Datacenter Overview.

### NOT touched
- 🖧 / 📊 / 💾 emojis inside `echo "..."` shell command examples in the migration guide. These are content the user copies and runs in their terminal, not UI chrome. SVG would not survive the copy-paste round-trip.

---

## [1.1.48] - 2026-04-22

Product naming + masthead layout overhaul. The "Toolkit" framing understated what RVTMA had become - a planning platform that also delivers ready-to-use technical artifacts (NAD YAMLs, Ansible playbooks, MTV plans, server allocation docs). Plus a couple of late-arriving emoji cleanups.

### Changed
- **Title** in the masthead: "OpenShift Virtualization Migration Toolkit" -> **"OpenShift Virtualization Migration Blueprint Platform"**. The "Blueprint" carries the dual nature: strategic design (architecture, sizing, risks, timelines) AND the executable artifacts engineers consume downstream.
- **Masthead layout** switched from flex to a 3-column grid (`1fr auto 1fr`). The title now lives in the dead-center column with `justify-self: center`; the logo cluster pins to the left flank, the utils cluster (version badge, GitHub, theme toggle) pins to the right. The 1fr flanks absorb extra space symmetrically, so the title stays visually centered regardless of how wide either side grows. Below 900px viewport width the title hides (the Red Hat brand on the left already identifies the product).
- **Cluster Conversion** callouts at the top of the cluster-selection screen: 📊 Infrastructure / 🎯 Interface emojis -> chartBar / target SVGs.
- **Datacenter Overview** Infrastructure Analytics charts header (`assets/js/datacenter-analytics-charts.js`): 📊 -> chartBar SVG. This file lives outside the main `js/` tree so it slipped past the earlier cleanup scan.

### Notes
- Title length goes from ~37 chars to ~52 chars - still fits comfortably on viewports >= 1100px in a single line; the responsive media query handles narrower screens.
- Dependencies icon (Cluster Architecture migration plan) had its `pin` glyph swapped for `link` in a follow-up commit to v1.1.47 - thumbtack drawing was unreadable, chain-link is the canonical "depends on" metaphor.

---

## [1.1.47] - 2026-04-22

Cluster Architecture emoji cleanup, Phases 2-4 (headers, diagram chrome, system status reports). Combined with v1.1.46's Phase 1 alerts, the entire Cluster Architecture surface is now SVG-iconed except for the workload type maps (intentionally kept since they bleed into mermaid diagrams and downloaded .md/.adoc reports).

### Added
- **6 new icons** in `js/lib/icons.js`: `target` (🎯, double circle + center dot), `pin` (📌, thumbtack), `cube` (📦, isometric cube), `map` (🗺️, folded map), `bug` (🐛, oval body + 6 legs).

### Changed
- **Phase 2 - section headers/labels** in `cluster-architecture.js` and `cluster-architecture-ui.js`:
  - 🏗️ Calculated Architecture -> building
  - 📊 Data Limitation Notice -> warning
  - 🌐 Show Network Details -> globe
  - 📋 Migration Plan Generator -> clipboard
  - 📌 Dependencies -> pin
  - 🎯 Workload Types Found / Available Workload Types -> target
  - 📈 Method / 📊 Utilization Details / Actual Utilization -> chart / chartBar
  - 📦 Target Hosts -> cube, ✅ Actual Hosts -> check, 🌐 Sites Processed -> globe
  - ❌ Initialization Error -> xmark, 🔄 Refresh Page -> refresh
  - VM Name Analysis ⚠️ inline glyph -> warning
  - displayArchitectureWarnings(): 🔴/⚠️ severity icons -> xmark/warning, ⚙️ control plane note -> gear
- **Phase 3 - cluster diagram floating panel** (`cluster-diagram.js`):
  - 🗺️ Cluster Architecture Diagram -> map
  - 💾 Download SVG -> download, ✖️ Close -> xmark
  - 📊 Architecture Summary -> chartBar
  - Diagram Legend dots (🔵🟢🟠) -> inline SVG circles with explicit fill colors (`#1976d2` / `#388e3c` / `#f57c00`) matching the actual mermaid box colors. currentColor would have flattened them all to one shade.
- **Phase 4 - system status reports** (cluster-architecture-ui.js):
  - 🔍 SYSTEM STATUS REPORT / 🔍 vCluster Integration Status -> search
  - ✅/❌ checklist glyphs across both reports -> check/xmark
  - ⚠️ Limited Platform Diversity / ✅ Optimal Setup / ❌ No Platform Mappings labels -> warning/check/xmark
  - 📊 WORKLOAD TYPES STATISTICS -> chartBar
  - 🐛 Debug Information -> bug
- **Stripped** No Data Loaded ⚠️ glyph (textContent target - SVG would render as raw markup).

### NOT touched (intentional, by design)
- Workload type icon maps (🐧🪟🔒🗄️☕🔶🎩💼📦) in 3 files - shared with Mermaid diagrams (renders inline in SVG output) and downloaded `.md` / `.adoc` reports (text format, no DOM)
- Mermaid template strings with embedded emojis (🏢💻⚙️📦🔲) - same reason
- Markdown checkbox literals (☐) in `download-architecture-document.js` and the migration-plan generator - pandoc renders these as actual checkbox glyphs in the PDF output
- Two commented-out `📥 Download OpenShift Configs` button blocks (legacy, marked "DEIXAR COMENTADO" / "DUPLICATED IN THE SYSTEM")

### Notes
- Combined v1.1.46+v1.1.47 migrated **49 emoji occurrences** across 4 files. Syntax check clean. `showAlert`/`innerHTML` paths verified - all SVG strings render via `innerHTML`, never `textContent`.

---

## [1.1.46] - 2026-04-22

Cluster Architecture emoji cleanup, Phase 1 of 4. Conservative scope: only the `showAlert()` toast notifications across the 3 critical-path files (cluster-architecture-ui.js, cluster-diagram.js, download-architecture-document.js). Headers, status reports, workload icons, and diagram color legend stay for follow-up phases.

### Added
- **3 alert-class icons** in `js/lib/icons.js`: `warning` (triangle + exclamation), `lightbulb`, `info` (circle + i). Sized 14px to render inline next to alert text.

### Changed
- **32 `showAlert()` calls migrated**: ⚠️ -> warning, ❌ -> xmark, ✅ -> check, 💡 -> lightbulb, 🔄 -> refresh, ℹ️ -> info. All template-literal interpolations preserved; behavior unchanged.
- **`dataSourceInfo` strings** (cluster-architecture-ui.js L301/L304): emoji prefixes (⚠️🔄) stripped. The HTML line that consumed them is currently commented out, but the strings themselves are now neutral text ready for future use.

### NOT touched (deliberate)
- Workload type icon maps (🐧🪟🔒🗄️☕🔶🎩💼📦) - shared with Mermaid diagrams and downloaded `.md`/`.adoc` reports where SVG would not render
- Markdown checkbox literals (☐) in document generators
- Mermaid template strings with embedded emojis
- Diagram floating-panel color legend (🔵🟢🟠) - matches actual diagram box colors
- Section headers, status reports, "Show Network Details" button - Phase 2/3/4

### Notes
- Critical path verified: no `showAlert()` calls retain emoji prefixes. Syntax check passed on all 4 modified JS files.

---

## [1.1.45] - 2026-04-22

First pass on removing the remaining unicode emojis from the UI - starting with the Load RVTools "Loaded Data" panel since it's the very first screen every user sees after upload.

### Added
- **Hardware + status icons** in `js/lib/icons.js`: `monitor`, `server`, `cpu`, `memory`, `search`, `check`, `xmark`, `circleEmpty`. Same vector / currentColor approach as the rest of the library, sized 14px to match inline status text.

### Changed
- **`updateFileStatus()` in index.html**: the 9 emoji prefixes (🖥️🌐📱🏢💾⚙️📊🔲🧠) on the tab list and the 4 status glyphs (✅❌⚪🔍) all swapped for `rvtmaIcon()` SVGs. `tabStatus` entries now carry an `icon` field separately from `name`. Picks up dark mode colors automatically and renders consistently across OS (no more Apple-vs-Noto emoji rendering drift).

### Notes
- Pure visual cleanup, no behavior change. Same loaded/required/optional logic, same grid layout, same CSS classes.

---

## [1.1.44] - 2026-04-21

Three small polish items: NAD download buttons join the page-header pattern, Cluster Architecture loses the last three emoji buttons, and the "PDF Settings" menu item is rebranded "Settings" since it owns more than just the PDF section now.

### Added
- **`adoc` and `csv` icons** in `js/lib/icons.js`: same text-outside layout as `pdf`/`md` (v1.1.40), `csv` mirrors PDF's font-size=11, `adoc` drops to font-size=9 since "ADoc" is 4 chars and would otherwise overflow the 24px viewBox.

### Changed
- **NAD page-header actions**: the three "Download All NADs" buttons (OVS VMNet, OVS Localnet, Linux Bridge) move from the body to the page-header actions slot. `displayNADResults()` populates them after analysis renders; `initSection('nad')` restores them when the user navigates back via the cached `window.currentNADAnalysis`.
- **Cluster Architecture body buttons** swapped from emojis to `rvtmaIcon` SVGs: Deployment Plan (`md`), Server Allocation Plan (`adoc`), Detailed Inventory (`csv`). The `(MD)`/`(ADoc)`/`(CSV)` suffixes drop from labels - the icon now carries the format hint.
- **Sidebar menu rename**: "PDF Settings" -> "Settings". The section ID, breadcrumb mapping, and route handler all keep `pdf-settings` for compatibility.

### Notes
- Same dynamic-state contract as Cluster Architecture / Migration Time Estimator: empty before generation, populated on render, restored on nav back.

---

## [1.1.43] - 2026-04-22

Two follow-ups: a build script fix so local builds pick up the current version, and the missing half of the v1.1.42 Cluster Architecture migration.

### Fixed
- **`build-multi-arch.sh` version detection**: the original `extract_version()` regex looked for `<small>v1.1.42</small>` but the masthead markup moved to `<span class="rvtma-version-badge">` releases ago. Script now reads from `package.json` (canonical source) with HTML fallback that handles both old and new markup. Local builds work again without GitHub CI quota.
- **Cluster Architecture body PDFs** still appearing after v1.1.42: the previous release removed the buttons from the fallback `generateBasicMigrationPlan()` in `cluster-architecture-ui.js` but missed the active path on `ClusterArchitectureUI.generateMigrationPlan()` in `cluster-architecture.js`. Now both paths are clean - the 2 PDF buttons live only in the page-header actions slot.

### Notes
- Validated: zero PDF buttons in the body after Calculate, both still present in the header.
- Build script tested with both detection paths (package.json present vs missing).

---

## [1.1.42] - 2026-04-21

Cluster Architecture joins the page-header actions pattern with its 2 PDF buttons. After Calculate Architecture runs, "Deployment Plan" and "Server Allocation Plan" PDF generators move to the top of the screen next to the breadcrumb. The MD/ADoc downloads stay in the body (they're niche technical formats next to executive deliverables).

### Changed
- **Cluster Architecture** (`cluster-architecture` section): the 2 inline PDF buttons at the bottom of the migration plan card moved to the page-header actions slot:
  - `Generate PDF (Deployment Plan)` -> **`Deployment Plan`** (with pdf icon)
  - `Generate PDF (Server Allocation Plan)` -> **`Server Allocation Plan`** (with pdf icon)
- **Body kept** for the MD + ADoc downloads (`Deployment Plan (MD)`, `Server Allocation Plan (ADoc)`) - they remain in the migration plan card row alongside the in-context narrative.

### Added
- **`displayArchitectureResults()` setPageActions hook**: populates the header buttons after each successful calculation.
- **`resetArchitectureConfig()` clearPageActions hook**: clears the header when the user resets, since the previous calculation result no longer applies.
- **`initSection('cluster-architecture')` case**: re-populates the header when the user navigates away and back while results are still showing. Uses `architectureResults.style.display === 'block'` as the signal (more reliable than chasing `instance.architecture` state, which can be null in odd edge cases).

### Validated (4 state paths)
1. Enter section, no calculation yet -> header empty.
2. Calculate Architecture -> 2 PDF buttons appear with correct labels and pdf icons.
3. Navigate away and back -> buttons restore for the still-displayed results.
4. Reset Configuration -> buttons clear (results are hidden, so the buttons would download stale data).

Zero console errors.

### Notes
- Backward compatible. Same handlers (`downloadMigrationPlanAsPDF`, `downloadArchitectureDocumentAsPDF`) wired to the new buttons.
- Cluster Architecture is the LAST screen with a viable header-actions migration. NAD has download buttons but they're per-workload-type and segmenting them in a header would clutter; keeping in body for now. Other screens have action buttons (Calculate, Refresh, Reset) that are not page-level deliverables and stay in body by design.

---

## [1.1.41] - 2026-04-21

Migration Time Estimator joins the auto-run pattern from NAD (v1.1.35). The "Start Migration Analysis" button on the landing was the same kind of friction: data already loaded by the time the user reaches the screen, analysis fast enough that deferring it behind a click added no value. Now auto-runs.

### Changed
- **`generateMigrationEstimatorHTML()`** simplified - the data status panel ("vInfo: N VMs found", "vNIC: ...", "vDisk (RDM): ...") and the "Start Migration Analysis" button were removed. The function now returns just the empty container that the estimator fills.
- **`initMigrationEstimatorPanel()`** auto-calls `initMigrationEstimatorWithData()` immediately after rendering the container, so opening the section goes straight to results.

### Notes
- The "RVTools Data Loaded" panel was a sanity check (does the file have the tabs we need?) that became redundant once the main interface shows the actual VM list. If a tab is missing, the estimator surfaces it inline in its own error path.
- `startMigrationEstimatorFromModal()` and `startMigrationEstimatorFromPanel()` are kept for any other callers that may still invoke them; they're harmless but no longer reachable from the main flow.
- Backward compatible. Same handlers, same results, fewer clicks.

---

## [1.1.40] - 2026-04-21

Typography refinement on the `pdf` and `md` icons after user testing. The v1.1.39 cut used `font-size=13/14` and `font-weight=900` (extra-bold). User iterated locally and found that a lighter combination reads cleaner at the 14px header size.

### Changed
- **`pdf` icon**: text now `font-size=11`, `font-weight=600`, `x=2`. Letters are visually thinner and the document outline is no longer dwarfed.
- **`md` icon**: text now `font-size=11`, `font-weight=600`, `x=7` (centers the 2 letters within the icon bounds).

### Notes
- Pure typography tweak. Same icon shape, same caller API, no other changes.

---

## [1.1.39] - 2026-04-21

Redesign of `pdf` and `md` icons. The previous design (v1.1.36/v1.1.38) put the format letters INSIDE the document silhouette - at the 14px size used in page headers, the lettering blurred and the icons looked nearly identical. New design moves the document silhouette to the upper-right (smaller) and puts the format letters bottom-left at large size. Inspired by user-supplied reference images.

### Changed
- **`pdf` icon**: document silhouette now occupies the upper-right corner only (~10x10 of the 24x24 viewBox). "PDF" letters at font-size 13, font-weight 900, occupy the bottom-left half of the icon.
- **`md` icon**: same recipe, "MD" at font-size 14 (one bigger than PDF since 2 letters vs 3 fit cleaner).

### Result
At every size from 14px upward, the format reads at a glance. The PDF/MD distinction is unambiguous even in the compact page-header buttons. No code changes needed in callers - same icon names, same `rvtmaIcon('pdf')` / `rvtmaIcon('md')` API.

### Notes
- ADoc icon remains pending; 4 letters need a different layout (probably stacked or smaller font). Will tackle when needed.

---

## [1.1.38] - 2026-04-21

Companion release to v1.1.36/v1.1.37: a dedicated `md` icon for Markdown downloads. Together with the `pdf` icon, MD/PDF buttons across the app now read at a glance via icon shape, no need to scan the label for the format suffix.

### Added
- **`md` icon** in `js/lib/icons.js`. Same document silhouette as `document`/`pdf`, with "MD" lettering inside. Two letters fit cleaner than three, so font-size is 8 (vs PDF's 7) for slightly better legibility at 14px. ADoc (4 letters) remains a separate, harder problem to be tackled later.

### Changed
- **6 buttons explicitly labeled `(MD)`** swapped from `chartBar` (or `document`) to the new `md` icon:
  - Cluster Conversion `Cluster Report (MD)`
  - Datacenter Overview `Datacenter Report (MD)` (header + on-load fallback)
  - Storage Analysis `Storage Analysis Report (MD)`
  - OS Distribution `Download OS Report (MD)`
  - Cluster Architecture body `Deployment Plan (MD)`
- **2 buttons that download Markdown but don't say `(MD)`** also swapped from generic `download` to `md` for consistency:
  - Risk Summary `Download report` (downloads `.md`)
  - Migration Guide `Download Complete Guide` (downloads `.md`)

### Notes
- Pure visual change. Same handlers, labels, behavior. Icon now communicates the file format.
- `download` icon is preserved on buttons that produce non-Markdown artifacts: Validation Test Suite (zip), Network Configuration (.yaml), NAD per-format downloads (.yaml), NAD debug downloads.
- Result: in any screen with both MD and PDF buttons (Datacenter Overview, Storage Analysis, OS Distribution, Cluster Conversion), the user sees `[MD] Label` and `[PDF] Label` with visually distinct icons - the format reads at a glance.

---

## [1.1.37] - 2026-04-21

Polish pass: every "Generate PDF" button across the app now uses the dedicated `pdf` icon introduced in v1.1.36 instead of the generic `document` icon. Visual differentiation between MD/ADoc downloads (generic document) and PDF outputs (document with "PDF" inside) is now consistent everywhere.

### Changed
- **Page-header `Generate PDF` buttons** (5 occurrences) swapped from `rvtmaIcon('document')` to `rvtmaIcon('pdf')`:
  - Datacenter Overview (header + on-load fallback path)
  - Storage Analysis
  - OS Distribution Analysis
  - Cluster Conversion (dynamic header)
- **Body `Generate PDF` buttons** (2 occurrences) in Cluster Architecture also swapped:
  - Generate PDF (Deployment Plan)
  - Generate PDF (Server Allocation Plan)

### Notes
- Pure visual change. Same handlers, same labels, same behavior. Only the icon differs.
- Remaining `document` icon uses are correct and stay: `Download OS Report (MD)`, `Migration Report (ADOC)`, `Generate Full Report` (debug panel) - all genuinely generic document downloads, not PDF.
- MD-specific icon will be a follow-up release (`md` text inside the document silhouette, same recipe). ADoc icon is trickier because the 4 letters fit poorly at small sizes - separate discussion when we get there.

---

## [1.1.36] - 2026-04-21

Migration Time Estimator joins the page-header actions pattern with a smaller, more-focused subset. Also introduces a dedicated PDF icon so buttons can drop the literal "(PDF)" suffix and still read clearly.

### Added
- **`pdf` icon** in `js/lib/icons.js`. Same document silhouette as `document`, with a crisp "PDF" text element inside. Legible from 16px+; at 14px the lettering becomes a visual hint while the overall shape still says "document". Uses `fill="currentColor"` and `stroke="none"` on the text so the SVG-level stroke does not thicken the letters. Now any `Generate PDF` button in the app can use `rvtmaIcon('pdf')` for visual differentiation from the generic `document` icon.
- **`window.MigrationTimeEstimator._syncPageActions()`** helper - used by `initSection('migration-estimator')` to re-apply the header buttons when the user navigates away and back while VMs are still selected (the estimator panel skips re-render in that case, so `showInterface` does not fire on its own).

### Changed
- **Migration Time Estimator**: three "executive deliverable" buttons moved to the page-header actions slot:
  - `PDF (Time Estimation)` → **`Time Estimation`** (with new pdf icon)
  - `PDF (Hierarchical Plan)` → **`Hierarchical Plan`** (with new pdf icon)
  - `Generate Excel Report` → **`Excel Report`** (chartBar icon)
- **Body kept** for the niche technical outputs that are NOT the primary deliverables: Migration Time Estimation (ADoc), Hierarchical Migration Plan (ADoc), Generate Folder Labels, Risk Analysis (WIP). ADoc pipelines and operational actions belong in the section body, not in the executive-deliverables header row.
- **Labels tightened** by dropping the literal "PDF" from the two PDF buttons - the new icon carries the format signal, label focuses on the content name.

### Validated (3 state paths, matches v1.1.34 contract)
1. Enter section, nothing selected → header empty.
2. Select VMs → 3 buttons appear.
3. Navigate away and back while VMs still selected → buttons restore.

### Notes
- Backward compatible. Same handlers (`downloadMigrationTimeEstimationAsPDF`, `downloadHierarchicalMigrationPlanAsPDF`, `window.MigrationTimeEstimator.downloadExcelReport`) wired to the new buttons.
- Existing PDF buttons on other screens (Datacenter Overview, Storage Analysis, OS Analysis, Cluster Conversion) still use the generic `document` icon. Updating those to the new `pdf` icon is a natural follow-up if the visual differentiation proves useful - low risk, can go in a future polish release.

---

## [1.1.35] - 2026-04-21

Removed friction in Network Attachment Definitions. The landing card with the "Analyze & Generate NADs" button existed without a strong reason - RVTools data is already loaded by the time the user reaches this screen, and the generation is fast enough that deferring it behind a click added no value. Now auto-runs on section nav, matching the UX of the other analysis screens (Datacenter Overview, Cluster Architecture, Storage Analysis, etc).

### Changed
- **NAD section landing** (`#nad`) no longer shows the "Generate NADs" info card with a button. The container starts empty; `executeNADGenerationModal('nadPanelContent')` fills it with its own loading state + results.
- **`initSection('nad')` case** added: auto-runs the generation when the user enters the section, gated on `allData.vms.length` so it only fires when RVTools is loaded. Re-entry is safe - the generation function re-renders the container each time.

### Notes
- Backward compatible. `executeNADGenerationModal()` is unchanged; we just call it automatically instead of via a user click.
- The "Try Again" error button that the function renders when data is incomplete is preserved - if the first auto-run fails, the user still has a recovery path.
- Download buttons (per workload-type NADs: OVS VMNet / OVS Localnet / Linux Bridge, plus Migration Report + Implementation Guide) remain in the body for now. Moving them to the page-header actions slot with the same dynamic-state contract used in v1.1.34 (Cluster Conversion) is a natural follow-up.

---

## [1.1.34] - 2026-04-21

Cluster Conversion joins the page-header actions pattern, with one twist: the buttons are dynamic - they only appear once the user picks a cluster in the dropdown and update as the selection changes.

### Changed
- **Cluster Conversion** (`cluster-conversion` section): the two inline buttons at the bottom of the per-cluster detail view (Cluster Analysis Report MD + Generate PDF) moved to the page-header actions slot. Body now renders the detail view without those footer buttons.
- **Button labels tightened**: `Cluster Analysis Report - <clusterName> (MD)` → `Cluster Report (MD)`. The cluster name already appears in the detail view header above; putting it in the button label was verbose and would overflow on narrow viewports. The cluster name is preserved in the button `title=` tooltip ("Cluster: cluster-prod-08 - Download Markdown report").

### Added
- **Dynamic header sync** in `updateClusterAnalysis()`: on every cluster selection change, the function either calls `setPageActions(...)` with the two buttons (when a cluster is selected) or `clearPageActions()` (when the user picks the placeholder).
- **`initSection('cluster-conversion')` case**: re-populates the header when the user navigates away and back to the section while a cluster is still selected in the dropdown. Calls `updateClusterAnalysis()` if `clusterSelector.value` is non-empty; otherwise lets the default `clearPageActions()` at the top of `initSection` stand.

### Validated (5 state paths)
1. First visit, no cluster selected → header empty.
2. User selects a cluster → 2 buttons appear with the correct tooltip.
3. User changes to another cluster → buttons and tooltip update in place.
4. User navigates away and back → buttons restore for the still-selected cluster.
5. User selects the placeholder ("- -") → buttons clear.

All five paths tested against a production dataset (cluster-prod-01/02/03). Zero console errors.

### Notes
- Backward compatible: `downloadClusterConfig(clusterName)` and `downloadClusterConfigAsPDF(clusterName)` unchanged - the header buttons pass the cluster name the same way the old inline buttons did.
- NAD Generator has a similar "actions appear after generation" pattern and will be reviewed next with the same contract. Migration Time Estimator has too many buttons to fit in the header cleanly and will require individual triage.

---

## [1.1.33] - 2026-04-21

Propagated the page-header actions pattern from v1.1.32 to five more screens. Report-download buttons moved from the section footer to the top-right next to the breadcrumb. Analytical/interactive buttons stayed in the body.

### Changed
- **Risk Summary**: `Download report` moved to the page-header actions slot. The `Refresh` button stays next to the severity chips because it is a contextual control for "re-read what you see", not a page-level action.
- **Storage Analysis** (`storage-conversion` section): both `Storage Analysis Report (MD)` and `Generate PDF` moved to the header. The body now starts directly with the Storage Analysis heading and summary cards.
- **OS Distribution Analysis** (`os-analysis` section): `Download OS Report (MD)` and `Generate PDF` moved to the header. The PDF button gate on `osAnalysisModalInstance` was removed because the instance is created asynchronously inside `initOSAnalysisInPanel` - handlers reference it by name and work once the user actually clicks.
- **Network Migration** (`network-conversion` section): `Download Complete Network Configuration` moved to the header, text tightened to `Download Network Configuration` to fit compactly beside the breadcrumb.
- **Migration Guide** (`migration-guide` section): `Download Complete Guide` moved to the header.

### Added
- **`initSection()` dispatch cases** for `storage-conversion`, `network-conversion`, and `migration-guide`. Previously these sections were populated on file load and had no hook to re-apply page-header actions on section nav. Now each case calls `setPageActions(...)` so the buttons are re-populated every time the user navigates back to the section.

### Notes
- Backward compatible. Same handlers (`downloadStorageAnalysisReport`, `downloadNetworkConfigs`, `downloadCompleteGuide`, etc.) continue to work - only the button location changed.
- Validated against a production dataset: all 5 screens render their buttons correctly next to the breadcrumb; existing Datacenter Overview pilot from v1.1.32 still works; no console errors.
- Screens not yet migrated (Cluster Architecture, Migration Time Estimator, NAD Generator, etc) have more complex button sets and deserve individual triage - will be reviewed case by case.

---

## [1.1.32] - 2026-04-21

Page-header actions pattern. Report-download buttons move from the footer of the screen to the top-right, next to the breadcrumb. User feedback: "the breadcrumb row is mostly empty, and the download buttons hide below a long scroll - swap them". Agreed. First screen to adopt: Datacenter Overview.

### Added
- **`.rvtma-page-header`** CSS component: flex row with breadcrumb on the left and a per-section actions slot on the right. Wraps to two lines on narrow viewports (mobile).
- **`.rvtma-page-header__btn`** compact button style. Sized to sit comfortably next to the breadcrumb baseline (padding 5px 12px, font 0.82rem) - smaller than `.btn-secondary` so it reads as a page action rather than a primary form action.
- **`setPageActions(html)` / `clearPageActions()`** globals. Sections that want header actions populate the slot from their init code. `initSection()` clears the slot on every navigation so actions never leak across pages.

### Changed
- **Datacenter Overview**: the two download buttons (Datacenter Report MD + Generate PDF) moved from the bottom of the page to the header-actions slot. Now visible without scrolling past the datacenter tree and analytics charts. The footer area is now clean; only the note "Migration Strategy Recommendation" remains.

### Notes
- Pilot scope: Datacenter Overview only. The CSS + JS infrastructure is reusable, so later releases can move appropriate report-download buttons from Cluster Architecture, Storage Analysis, OS Analysis, Risk Summary, etc, into the same slot one at a time. Each screen will be reviewed individually - not every button should go to the header (action buttons like "Calculate Architecture" or "Refresh Workload Types" belong in the section body, not the header).
- Breadcrumb styling was slightly refactored: the padding, border, and background moved from `.rvtma-breadcrumb.pf-v6-c-breadcrumb` to the new `.rvtma-page-header` wrapper. Visually identical to pre-v1.1.32 for sections with no page actions.
- Validated in desktop (1440px - buttons right of breadcrumb), narrow desktop (900px - same row), and mobile (375px - buttons wrap to line below). No console errors.

---

## [1.1.31] - 2026-04-21

Two finishes on the Direct SAN scaling advisory - both came from user review of the generated report: "what does 'at-risk' actually mean per line?" and "this says 'use NFS' but what about the backup tool compatibility?". Both valid; both applied to the on-screen RiskCard and the PDF report for consistency.

### Added
- **"Why flagged" column** in the At-risk clusters table (MD report) and **"Flagged: X"** inline annotation on each item of the Storage Architecture RiskCard. The annotation shows which specific threshold fired - one or more of `≥1000 PVCs total`, `≥500 PVCs total`, `≥500 PVCs/host`, `≥200 PVCs/host`. Two clusters at the same 'warning' severity can be there for different reasons, and the remediation conversation differs (density vs aggregate count).
- **Backup compatibility caveat** paragraph in the advisory body (UI card summary + MD report). Names the common backup solutions for OpenShift Virtualization VMs (Veritas NetBackup, Huawei OceanProtect, Kasten K10, Red Hat OADP/Velero, Trilio) in a neutral tone and puts the onus on the architect to verify the proposed storage path against the backup vendor's supported matrix **before** locking the decision. Without this caveat the advisory could nudge customers into storage choices their backup tools do not address.

### Notes
- Pure content addition. Thresholds, severity logic, and list of at-risk clusters unchanged.
- Validated against a production dataset (2 clusters flagged): both appear with "Flagged: ≥500 PVCs total (watch)" annotation, advisory card shows the backup caveat, MD table has the new "Why flagged" column.

---

## [1.1.30] - 2026-04-21

Polish pass on the Storage Analysis report. User feedback: narrow centered tables with mostly numeric key-value data were wasting vertical space in the PDF. Also, the LUN explosion advisory (v1.1.16) only lived on-screen; the printed document delivered to customers had no trace of it.

### Changed
- **Summary section** converted from a 9-row table to **three bullet groups**: Capacity (datastores, total, used, free) / Virtual Disks (total, estimated PVCs) / RDM (disks, VMs). Reads as an executive summary instead of a cramped table.
- **Virtual Disk Profile section** converted from a 9-row table to **three bullet groups**: Inventory (total, thin, thick, total capacity) / Size Range (avg, median, max) / VM distribution (avg disks/VM, max on single VM).
- **Disk Size Distribution table** now has a third column **% of total** so the narrow 2-column layout has useful width and the reader sees shape at a glance (e.g. "34.6% of disks are 50-100GB").
- **VMs with Multiple Disks table** enriched from 2 columns (VM, Count) to 5: **VM / Disk Count / Total GB / Maximum Disk Size / RDM**. Each row now reads as a migration decision, not just trivia. "oracle-db-01 with 12 disks, 4.2TB total, 1.5TB largest, 3 RDMs" tells the planner to reserve a wider re-presentation window and consider storage replication - data that was previously absent from the printed report.

### Added
- **Direct SAN scaling risk section** ported from the on-screen RiskCard (v1.1.16) into the PDF report. Same thresholds (>= 500 PVCs total OR >= 200 PVCs/host = warning; >= 1000 OR >= 500/host = critical), same educational content (NFS from array / SDS / ODF preference), plus an "At-risk clusters" table with VMs, PVCs, Hosts, LUNs/host, Severity. Appears right before the Storage Connectivity & LUN Capacity section so the reader sees the anti-pattern warning before reading the capacity math.
- **`vmDiskStats`** internal aggregate (count, totalGB, maxGB per VM) to feed the enriched Multiple Disks table.

### Notes
- Pure presentation + content addition in the markdown report. Analyzer logic untouched. On-screen UI unchanged.
- Validated against a production dataset (48 hosts): Summary/Profile now render as clean bullet groups, Distribution shows "50-100GB | 494 | 34.6%", Multiple Disks shows "vm-prod-app-008 | 6 | 6709 | 4608 | -" with the largest disk clearly visible, Direct SAN advisory appears with the educational text.

---

## [1.1.29] - 2026-04-20

UX cleanup of the v1.1.28 additions. The new explanatory text under the slider and checkbox was forcing line wraps and bloating the config panel. Moved everything to mouseover tooltips; tightened the override flow.

### Changed
- **Slider explanation moved to mouseover** (`title="..."`) on the Target Utilization range input. The two-line `<p class="u-detail">` paragraph is gone. Hovering shows the migrable / standalone explanation.
- **Checkbox explanation moved to mouseover** on the checkbox label container. The "When checked / when unchecked" detail no longer takes up vertical space.
- **Checkbox label tightened** from "Honor selection above (override auto-sizing)" to **"Override auto-sizing"** - shorter, fits cleaner under the dropdown.
- **Hosted Cluster Size dropdown defaults to "Auto (derived from utilization)"** as a new first option. The dropdown is **disabled by default**. Ticking the override checkbox enables it. Picking Auto inside the (now-enabled) dropdown also routes to auto-derive, so the two paths to "use auto" stay consistent.
- **Reset architecture config** also resets to Auto + disables dropdown + unticks the override checkbox.

### Notes
- Backward compatible. Existing customers who didn't tick the override checkbox in v1.1.28 see the same numbers in v1.1.29 - just a cleaner config panel.
- The actual algorithm change shipped in v1.1.28 - this release is pure UX polish.

---

## [1.1.28] - 2026-04-20

Opt-in checkbox to honor the Hosted Cluster Size dropdown literally. Closes a long-standing disconnect noted at the end of v1.1.27 ("dropdown choice is ignored - algorithm always uses calculateDynamicHostedClusterSize"). Default OFF preserves current behavior.

### Added
- **Checkbox `Honor Hosted Cluster Size selection above`** under the Hosted Cluster Size dropdown in Cluster Architecture. Default unchecked. When checked:
  - `calculateOptimalArchitecture` reads `config.hostedClusterSize` literally instead of auto-deriving from Target Utilization. So picking "All Servers (N)" sets dynamicHostedSize to N - one fat cluster per workload, absorbing all available hosts.
  - Phase 1 `phase1ClusterSize` skips the `fairSharePerWorkload` cap. Each workload's Phase 1 cluster is allowed to absorb everything available (up to the dropdown choice). Without this, Phase 1 would still slice by leftover-budget heuristic and Phase 2 would create the bonus splits.

### Changed
- Hint under the new checkbox: "When checked, the dropdown defines max cluster size literally (e.g. 'All Servers' = one fat cluster per workload). When unchecked, size is auto-derived from Target Utilization."
- `boostApplied` summary label now reads `none (user>=cap)` when the user's slider exceeds the boost cap (cosmetic - the actual cap fix shipped in v1.1.27).

### Notes
- Backward compatible. Existing reports stable - the checkbox is opt-in.
- Reports stable: existing customers' generated reports continue to use the auto-derived behavior.

---

## [1.1.27] - 2026-04-20

Two long-standing bugs in the Cluster Architecture algorithm. Both surfaced when a user reported "I picked 100% slider and Min Hosting 3, why am I only using half the servers?". Diagnosis: a boost cap was rebaixing the slider, and standalone hosts were being counted as candidates even though they cannot be auto-classified.

### Fixed
- **Boost cap was lowering high slider values** (`calculateWeightedUtilization`). The cap was designed as a UPWARD boost for small sites (a 25% slider on a 50-host site became 50% so the analysis was not absurd), but the formula `Math.min(baseUtil * 1.5, 0.5)` returned `0.5` even when `baseUtil` was `1.0` - so 100% slider became 50% on a 30-59 host site. Wrapped each branch with `Math.max(baseUtil, ...)`, so the cap can only LIFT a low slider value, never LOWER a high one. `boostApplied` label now reads `none (user>=cap)` when the user's pick already exceeds the cap.
- **Standalone hosts were polluting the migrable pool**. Hosts with empty `Cluster` field (standalone ESXi nodes - common in small sites, sandboxes, ESXi free) were entering the architecture algorithm as candidates, but Phase 1 cannot place them (no workload bucket). They counted toward the denominator of "actual utilization" without ever being placed, producing misleading "94% used" readings even when every viable host was placed. Now filtered out at the entry of `analyzeHostsForArchitecture` into a separate `standaloneHosts` list. The migrable pool (`hosts - standalone`) is what feeds `getSites`, `groupHostsBySite`, `categorizeWorkloadsFromVCluster`, `resourceSummary`, and the `hostsToUse` calculation. `enhancedHostInfo` and `datastoreMapping` still cover all hosts so other screens that look up by host name still find standalone hosts with their datastore relationships.

### Changed
- **`actualUtilization` formula** now divides by `migrableHostsCount`, not `totalHosts`. So slider 100% on a 48-host dataset (6 standalone) reports 100% (42/42), not 87% (42/48). Honest reading.
- **Slider hint** under "Total host allocation for architecture" now mentions migrable scope and standalone exclusion: "% of migrable hosts (hosts enrolled in a vCenter cluster) released by client for the project. Standalone hosts - if any - are excluded from this percentage and from the migration plan, since the tool cannot decide which workload bucket they belong to."
- **Success alert after Calculate** shows three numbers when applicable: "Using X of Y migrable hosts (Z% actual)", "Total hosts in dataset: T", "K standalone host(s) excluded".

### Validated
- Same scenario from the user report (production dataset, 48 hosts, 6 standalone, slider 100%, Hosted Size = All Servers, Min Hosting 3):
  - Pre-fix: 24/48 = 50% (cap rebaixed slider).
  - After cap fix: 45/48 = 94% (standalone polluted denominator).
  - After standalone exclusion: 42/42 = 100% (honest, every viable host placed).
- Workload-dedicated rule maintained: each hosted cluster contains only hosts of its workload type. No cross-bucket leak in 3 generated clusters.

### Notes
- A separate disconnect surfaced during the sanity test (NOT in this release): `Hosted Cluster Size` dropdown choice is ignored by the algorithm - it always uses `calculateDynamicHostedClusterSize(targetUtilization)` instead of `config.hostedClusterSize`. So picking "All Servers" today still splits Windows into multiple clusters via Phase 2 bonus. Will be addressed in a follow-up release.

---

## [1.1.26] - 2026-04-20

Pure cleanup release. Surfaced two pieces of dead code while investigating the Cluster Architecture algorithm. Removing them so future edits land on the live code path.

### Removed
- **`generateConfigurationSection()` block-comment in `cluster-architecture.js`** (lines 2261-2318, ~58 lines). The entire method had been wrapped in `/* ... */` for several releases. Yesterday's v1.1.20 edit ("3 servers per datacenter" added to a `<select>`) actually landed in this dead block - a no-op the user could not see. Now removed completely with a short comment pointing to the live source (`generateDynamicConfigurationSection` in `cluster-architecture-ui.js`).
- **Static `<option>` children of `hostedClusterSize` select** in `cluster-architecture-ui.js`. The `<select>` element was being populated at runtime by `js/dynamic-hosted-cluster-size.js` (range: 2-50 servers/workload + "All Servers (N total)"), which calls `select.innerHTML = ...` on init and wipes any static children. The static `<option value="12/15/18">` we kept there were dead - users never saw them. Element kept empty so the dynamic module can still find it by id and populate.

### Notes
- Pure cleanup. No behavior change. Calculate Architecture produces the same numbers before and after this release.
- Validated against `RVTools_vcenter-prod_2026-04-17.xlsx` (48 hosts, production dataset): dropdown populates with 17 options (2-50 + "All Servers (48)"), Calculate runs cleanly, output matches the pre-cleanup result.
- Sets the stage for the algorithm fix to come (cap respecting slider 100%, standalone exclusion from migrable pool).

---

## [1.1.25] - 2026-04-20

User feedback after v1.1.22: the donuts in the Storage Analysis PDF were still rendering too large at 40%.

### Changed
- **Donut chart width** in Storage Analysis report dropped from `40%` to `25%` (Pandoc image attribute syntax). At 25% the chart reads as a compact glyph next to the cluster label, leaving the page mostly to the table data above.

---

## [1.1.24] - 2026-04-20

Follow-up to v1.1.23: now that the Standalone Hosts bucket has a sane name, surface it properly to the user and explain why those hosts are not in the migration plan. The goal is transparency - the tool saw them, here is why we are skipping them, and here is what you can do with the hardware later.

### Added
- **Standalone chip + accent border** on the Standalone Hosts card in Datacenter Overview. Card uses the blue accent (`--rvtma-c-blue`) so the eye groups it with "informational, not part of migration" rather than the regular cluster cards.
- **Info callout** above the cluster grid when at least one cluster in the datacenter is the Standalone Hosts bucket: "Standalone hosts detected (N). These hosts are not enrolled in any vCenter cluster. Not included in the migration plan, which is built around vCenter-cluster boundaries. The underlying hardware can be repurposed as additional OpenShift worker nodes after the migration."
- **`Migration Priority: N/A (not migrated)`** on the Standalone Hosts card (instead of the usual Low/Medium/High) so the operator does not expect it to participate in waves.
- **Info RiskCard `datacenter-standalone-hosts`** published to the Risk Registry from `analyzeDatacenterHierarchy`. Surfaces in Risk Summary with the same explanation, broken down per datacenter when there are multiple. So the operator sees the standalone story even without opening Datacenter Overview.

### Fixed
- **Risk registry was being cleared too late**. `generateAlerts()` was calling `rvtmaRiskRegistry.clear()` AFTER `analyzeAllData()` ran, which wiped any analysis-time publishes (like the new `datacenter-standalone-hosts`). Moved the `clear()` to `handleFiles` right before `analyzeAllData()`. Now publishes during analyze survive.

### Notes
- All three surfaces only appear when at least one host in the dataset has empty Cluster. Customers with all hosts properly enrolled won't see any of this.

---

## [1.1.23] - 2026-04-20

User reported a curious xlsx where one cluster in a production datacenter rendered as "Unknown Cluster" (6 hosts). Investigation showed those are valid standalone ESXi hosts not enrolled in any vCenter cluster - the field was simply empty. The "Unknown" label was misleading: it suggested a parsing failure when in fact the data was clean.

### Changed
- **Fallback label `Unknown Cluster` -> `Standalone Hosts`** across the codebase. Applied in 7 places:
  - `index.html` (4 in `analyzeDatacenterHierarchy`, 1 elsewhere)
  - `js/storage-architecture.js` (multipath cluster fallback)
  - `js/storage-analysis.js` (multipath cluster fallback)

### Why
- `Unknown` reads as "data quality problem"; `Standalone Hosts` is a recognized ESXi pattern (free hosts, sandboxes, hosts moved out of cluster). The new label is descriptive and honest.
- No behavior change. Hosts with empty `Cluster` still get bucketed together. Migration logic untouched.

### Notes
- The standalone bucket only appears when at least one host in a datacenter has an empty Cluster field. Users with all hosts properly enrolled won't see it.
- Validated against `RVTools_vcenter-prod_2026-04-17.xlsx` (production datacenter, 48 hosts, 4 buckets: 3 real clusters + 6 standalone).

---

## [1.1.22] - 2026-04-20

Storage Analysis report polish: the per-cluster LUN capacity donut images were rendering at full canvas width in the PDF, eating most of the page for what is just a 2-slice ring. Now they're scaled to 40% via Pandoc's image attribute syntax.

### Changed
- **`![cluster LUN capacity](...)` images** in the Storage Analysis report now carry `{ width=40% }` so the donuts render at roughly half page width (or less depending on layout). Pandoc honors this attribute natively when going from markdown to PDF.

### Notes
- On-screen UI is unchanged - the chart canvases still render at their original 200x200 size in the Storage Architecture panel. Only the report PDF embedding is affected.

---

## [1.1.21] - 2026-04-20

Quality-of-life improvement on the Cluster Architecture Diagram. Customer reports with many hosted clusters of the same type (e.g., 6x `HOSTED-WINDOWS-SITEA-N`) were rendering a wall of identical boxes, making the diagram hard to read.

### Changed
- **Hosted clusters now collapse by workloadType** inside each hosting cluster in the Mermaid diagram. When two or more hosted clusters share the same workloadType under the same hosting cluster, they render as a single node with a `xN` badge and aggregated totals:
  - `linux x3 | 84 VMs total | 66 CPU total`
  - `windows x2 | 75 VMs total | 60 CPU total`
- Single hosted clusters still render exactly as before (full cluster name + individual VMs/CPU). No change for small deployments.

### Notes
- Pure rendering change in `js/cluster-diagram.js` (`buildMermaidDiagram`). Architecture data is not modified - the detail toggles below the diagram still list each individual cluster for drill-down.

---

## [1.1.20] - 2026-04-20

Small but important defaults change for Cluster Architecture. The tool used to be biased toward large-cluster HA assumptions that don't fit all environments (especially small sites, remote branches, and labs that still need realistic analysis).

### Changed
- **Min Hosting Cluster Size** default dropped from **5** to **3** hosts. The "3 hosts - Minimal" option is now the selected one, matching the etcd quorum minimum and letting small-site customers get useful sizing results out of the box. Balanced/Ideal HA options (4/5/6) are still one click away.
- **Hosting Cluster Size** now offers **3 servers per datacenter** as the smallest option (in addition to the existing 6/8/10). Default stays at 6 since most customers still target that, but the option exists for small-site plans.

### Notes
- Pure default/option change. No validation or calculation logic modified. The dynamic hosting size calculator (`calculateDynamicHostingSize`) already handled 3-host floors; just the defaults were stale.

---

## [1.1.19] - 2026-04-20

Systematic sweep after the v1.1.18 PDF-button fix revealed more places with the same regression pattern (hardcoded colors left over from when buttons were red by default, plus emojis that survived earlier waves).

### Fixed
- **Copy YAML button** (`generate-physical-network-content-modal.js`) had the same symptom as the PDF buttons: success flash green (`#28a745`) then reverted to hardcoded `#3b4048` dark grey instead of the theme's `btn-secondary` look. Fixed by clearing inline styles on revert. Also swapped the `✔ Copied!` text for an inline checkmark SVG.
- **`⏳ Processing...` placeholders** in the NAD sidebar + floating panel replaced with the `.pdf-spinner` CSS spinner (which now uses `currentColor`, so visible on any theme).

### Changed
- **Floating buttons** (OS Distribution, PDF Script, Physical Network on the bottom-right of every screen) swapped from emoji labels (`📊 📄 🛜`) to inline SVGs matching the rvtma-icon vocabulary. Since they live in static HTML and cannot use `rvtmaIcon()` template literals, the SVGs are inlined directly.
- **Wave 3 long tail** of `<h4>`/`<h5>` section titles migrated from emoji to `rvtmaIcon()`:
  - Validation Tests (Infrastructure, Connectivity, Storage, VM, Performance)
  - Automation Scripts (Pre-Migration, Batch, Post-Migration, Initial Setup)
  - Cluster Architecture UI (Debug Information, Architecture Summary, Migration Plan Overview)
  - Physical Network modal (Network Topology, Configuration YAML, Copy button)
  - Per-cluster analysis (Recommended Migration Approach, Specific Considerations)
  - NAD debug panel (Hosted Clusters, Quick Results, Done!, Migration Planning Notes)
  - Dynamic Hosted Cluster Size label (`🎯` → layers SVG)
  - Per-cluster Network Settings h5

### Notes
- Some informational emojis kept intentionally as they carry meaning through color-coding (migration wave colored dots `🟢🟡🟠🔴`, per-row severity glyphs in Network Alerts items, checkmark/warning in VMs-with-VLAN stat boxes). These are content, not decoration.

---

## [1.1.18] - 2026-04-20

Three-way fix on the PDF generate buttons. User reported the buttons were landing in a weird state after PDF generation, and the progress feedback had disappeared. Root cause was leftover styling from an older version where PDF buttons were dark red by default.

### Fixed
- **PDF button stays in `rest` look after generation** (was getting forced to `#c62828` dark red on both success and error paths in `_generatePDF`'s restore callback). Cleared the inline background so the button returns to whatever the theme CSS (`btn-secondary`) defines, matching the pre-click look.
- **Spinner visible on light button** (was white-on-white after v1.1.14 removed the default red bg). Switched to `currentColor` so it inherits the button text color - works on idle/secondary, green-ready, and red-failed states alike.
- **Progress bar visible on light button** (was 70% white - invisible on the new light button). Switched to `var(--rvtma-c-blue)` at 0.8 opacity so the bar reads on any button state.

### Notes
- Pure regression fix. No behavior changes beyond the button visuals returning to the state they were designed for.

---

## [1.1.17] - 2026-04-20

Risk Summary panel: consolidates every risk surfaced across the app into one screen, so the operator does not have to tour 5 different panels to assemble a go/no-go picture. Name deliberately avoided "Migration Readiness" because the tool only sees the technical slice extracted from RVTools - infra capacity, team skills, vendor engagement, and operational windows remain outside its scope, and the panel copy says so.

### Added
- **`js/lib/risk-registry.js`** - tiny publish/getAll/retract/counts/clear registry. Keyed by risk id so idempotent - re-rendering a source screen replaces the registry entry instead of duplicating it. Sorts by severity (critical -> high -> warning -> info) then by source.
- **`publishRiskCard(opts)`** helper in `js/lib/risk-card.js` - wraps `renderRiskCard` and publishes to the registry in one call. Keeps the per-screen call site tight (one function, not two) and guarantees the registry entry matches what the user sees on screen.
- **`js/risk-summary.js`** - the Risk Summary panel module with:
  - severity counter chips at the top (critical / high / warning / info)
  - risks grouped into tiered sections with colored dividers
  - each card appended with a "Go to source" button that jumps to the screen where the risk was detected
  - markdown download of the current registry snapshot (routes through `getReportFilenamePrefix()` so it respects the project-wide `datacenter_vcenter_` naming)
  - empty state differentiates "load data first" from "nothing flagged" and reminds the user that a clean Risk Summary does NOT mean the migration is risk-free
- **Sidebar entry** under "Overview", right after "Datacenter Overview", with a shield icon.
- **Breadcrumb mapping + initSection hook** so `showSection('risk-summary')` renders correctly.

### Changed
- Five existing risk surfaces (`dashboard-incomplete-data`, `dashboard-high-risks`, `network-vmkernel-alerts`, `migration-rdm-detected`, `storage-lun-explosion`) switched from `renderRiskCard` to `publishRiskCard`. Also added `retract()` calls on the "no longer applicable" branches so resolved risks disappear from the Summary on re-analysis. The registry is `clear()`'d when a fresh RVTools file is loaded (hook in `generateAlerts`) to prevent mixing data from different exports.

### Notes
- Pure aggregation. No new analysis logic - the Risk Summary is a LENS over existing detections. Adding a new risk surface anywhere in the app will automatically appear in the Summary without touching `js/risk-summary.js`.

---

## [1.1.16] - 2026-04-20

Educational guardrail in Storage Architecture. After a reviewer flagged that "per-PVC Direct SAN does not scale like VMware VMFS" (LUN limits, multipath overhead, udev storms, boot-time regressions), the screen now calls out the anti-pattern before the plan ships to production.

### Added
- **Direct SAN scaling risk advisory** in Storage Architecture. A RiskCard appears at the top of the panel whenever at least one cluster is projected to map >= 500 PVCs in total OR >= 200 PVCs per host (warning tier). Critical tier triggers at >= 1000 PVCs total or >= 500 PVCs per host. Items list each at-risk cluster with VMs -> PVCs / hosts = LUNs-per-host math, severity-tagged. Summary educates toward NFS (array-served) or SDS (ODF/Ceph) as the preferred path for VM-dense workloads - clarifies that the container image layer stays local on the node either way, so the shared storage only needs to handle VM disks + RWX app volumes, not layered filesystems.

### Notes
- Pure educational signal. The analyzer logic is untouched - this is just a new surface reading existing numbers (`projectedPVCs`, `hostCount`, `vmsInCluster` per cluster).
- Thresholds are intentionally conservative so the advisory does not cry wolf for small labs (SMALL-TEST type clusters with 180 PVCs and 4 hosts). Tune later if needed.

---

## [1.1.15] - 2026-04-20

Small but practical fix for users juggling multiple migration projects: the filename prefix order is flipped so files group naturally by project in the Downloads folder.

### Changed
- **Filename prefix order**: `vcenter_datacenter_` → `datacenter_vcenter_`. All generated files (MD, ADoc, YAML, XLSX, PDF, SVG, SH, TXT) now sort alphabetically by datacenter, which is usually the project-level identifier. The user report: "fica difícil localizar os arquivos quando lido com múltiplos projetos." Fix at the source, one regex, everything downstream inherits.

### Fixed
- **OS Analysis modal download**: the modal had its own `downloadFile()` method that bypassed `getReportFilenamePrefix()`, so `os-analysis-report-${date}.md` was the only download missing the project prefix. Now routed through the helper like every other module.

### Notes
- No visual changes. Existing downloads keep working. Only the filename layout on disk changes going forward.

---

## [1.1.14] - 2026-04-19

Risk alerts migrated to a unified vocabulary. Until v1.1.13 every screen rolled its own `<div class="alert alert-warning alert-glow">` with emoji prefixes and inline `<ul>` blobs. v1.1.14 introduces `js/lib/risk-card.js` and ports the four most visible risk surfaces to it. Same data, consistent voice.

### Added
- **`js/lib/risk-card.js`** - new `renderRiskCard({severity, title, count, summary, items, action})` helper. Four severities (`critical`, `high`, `warning`, `info`) mapped to Blast Radius color tokens (`--rvtma-c-red/amber/blue`). Critical inherits the existing `.rvtma-pulse-critical` animation. Items render as bullet rows with `{label, detail}` pairs (or plain HTML strings for legacy formats).
- **`.rvtma-risk-card` CSS** - card with 4px left stripe colored by severity, rounded 8px corners, severity-tinted chip badge, themed icon (shield/alert/info), tema-aware via semantic tokens (works in both light and dark mode).

### Changed
- **Incomplete Data alert** (Dashboard, after RVTools load): was a plain warning div, now a warning card with each missing tab as an item row.
- **High Risks Identified alert** (Dashboard, after RVTools load): was a `alert-danger alert-glow rvtma-pulse-critical` div with inline `<ul>`, now a critical card. The summary keeps the count, items list category + issue + solution per row.
- **Network Alerts** (Network Architecture screen): was a single warning div with `<br>`-joined alerts. Now a card that auto-promotes to `critical` if any alert contains the red dot glyph (duplicate IPs etc), warning otherwise. Per-row severity glyphs preserved so users still see which alert is which.
- **RDM Detected** (Migration Time screen): was a warning div with summary text only. Now a warning card that **lists the affected VMs** (top 8 + overflow chip) with their disk count - operator sees WHO needs special handling, not just a count.

### Notes
- The two surfaces that already speak the Blast Radius vocabulary - Cluster Architecture no-HA pulse (v1.1.8) and Storage Architecture overcommit pulse (v1.1.10) - were left as-is. They are card-level decorations, not standalone alert blocks.
- "Risk Level: Low/Medium" plain text in Migration Plan and a future aggregated Risk Summary panel are deferred to v1.1.15+. They deserve their own design pass.
- Pure presentation migration. No risk detection logic changed.

### Fixed
- **`.cards-grid.cols-2`** rule was missing from CSS, so Pattern B (HyperShift) Hosted CP 1 / Hosted CP 2 cards were stacking vertically instead of side-by-side. Always-broken bug, now correctly two columns at desktop and collapsing to one column on mobile.

### Changed (additional v1.1.14 cleanup)
- **`.blast-module` horizontal padding** trimmed from 20px to 8px so the Comparison Matrix has more room without breaking the patterns visualization (cards have their own padding + grid gap).
- **Wave 3 emoji-to-SVG** for the screens that still had emojis on action buttons:
  - **NAD screen**: Start Analysis (sidebar), 3 download-by-type buttons (OVS VMNet, OVS Localnet, Linux Bridge), Migration Report (ADOC), Implementation Guide.
  - **OS Distribution Analysis modal**: Download OS Report (MD), Generate PDF, plus the two `<h4>` chart headers (Data Sources Breakdown, Data Source Quality).
  - **Migration Time Estimator** post-calculation buttons (where there was no icon at all): Migration Time Estimation (ADoc), Hierarchical Migration Plan (ADoc), Generate Folder Labels, Generate Excel Report, Risk Analysis (WIP), PDF (Time Estimation), PDF (Hierarchical Plan).
  - **About panel**: View on GitHub (now with GitHub mark) and Changelog (now with document icon).

---

## [1.1.13] - 2026-04-17

Wave 2 of the emoji-to-SVG sweep. Action buttons across the app now use `rvtmaIcon()` so they adapt to text color, stay consistent across OSes, and look at home next to the PF6-flavored chrome.

### Changed
- **Cluster Architecture action buttons** migrated to `rvtmaIcon`: Refresh Workload Types, Calculate Architecture, Get Physical NICs, Reset Configuration, Show Statistics, Check System Status, View Diagram, Check vCluster Status, Reset, Download Basic Report, Deployment Plan (MD), Server Allocation Plan (ADoc), and both Generate PDF variants.
- **Cluster report buttons** in the per-cluster analysis pane (index.html): Cluster Analysis Report (MD) and Generate PDF. `OpenShift Node Preparation Commands` h4 header also moved to SVG.
- **NAD Generator** buttons: Refresh NADs (network-nad-generator.js), Download All NADs header + per-workload VMNet/Localnet/Linux sub-buttons, debug-panel Download / View / Download All NADs YAML / Generate Full Report, and floating panel Generate button (static HTML - inline SVG).
- **Storage Analysis**: Storage Analysis Report (MD) and Generate PDF buttons.
- **Migration helpers**: VM Folder Labels modal header and Generate Commands button (migration-time-folder-labels.js), Reset to Default button (migration-folder-config.js).
- **Validation / Automation panels** (index.html): Download Complete Test Suite and Download All Scripts buttons.
- **Datacenter report** buttons: Datacenter Report (MD) and Generate PDF.

### Added
- **Cluster Architecture - hierarquia visual nos toggles de detalhe**. Após "Calculate Architecture", os ~14 `<details>` que listavam hosting + hosted clusters viviam empilhados sem separacao. Agora:
  - Group headers uppercase: "HOSTING CLUSTER" e "HOSTED CLUSTERS" com chip de contagem
  - Stripe colorida de 3px a esquerda em cada toggle, mapeada por workload family (Oracle vermelho, SQL roxo, SAP ambar, Weblogic laranja, DMZ rosa, Windows ciano, Linux/RHEL/Ubuntu teal, fallback azul)
  - Icone leading no toggle hosted via `rvtmaIcon` (database, building, gear, layers, globe), tema-aware
  - Novo helper `_archWorkloadAccent(label)` faz o mapeamento por substring case-insensitive (HOSTED-WINDOWS-SITEA-3 cai no mesmo bucket que win-old)

### Notes
- Wave 2 focuses on the ~25 most visible action buttons. A long tail of `<h4>` section titles inside generated modal and pre-blocks (VM Tests, Performance Tests, Pre/Batch/Post Migration, etc.) still carries emojis and will be addressed incrementally. They are inside narrow panes where the current emoji rendering is acceptable.
- No functional or layout changes. Pure presentation migration.

---

## [1.1.12] - 2026-04-19

Polish release after the three big component migrations. Fixes one architectural inaccuracy in Physical Network, adds missing options, opens the door to incremental emoji-to-SVG modernization.

### Added
- **OVS Balance SLB / 2-NIC layout corrected end-to-end**. The 2-NIC OVS Balance SLB topology is now modeled accurately: OVS bond inside `br-phy`, `br-ex` (existing) sources from `br-phy` via patch ports, no separate `br-vmdata` (VMs use NADs with VLAN tags on `br-ex`). Three contextual notes explain the architecture and the use case (switches that cannot be reconfigured for LACP). YAML pane shows the canonical `br-phy` snippet plus links to `linuxelitebr/openshift-balance-slb` for the full agent-based / HCP setup.
- **LACP / 802.3ad** added to the Bonding Strategy dropdown for 4+ NICs. Disabled with a hint label for 2-NIC configurations. Generates kernel bond in `802.3ad` mode with appropriate options (`lacp_rate: fast`, `xmit_hash_policy: layer3+4`).
- **LTR (left-to-right) layout** for the OVS Balance SLB diagram. The 4-card linear chain reads naturally as a flow rather than a tall stack. New `direction: 'horizontal'` option in `drawConnectorEdges`. Scoped to this one topology; other diagrams stay vertical.
- **Bonding Strategy dropdown grouped** into three optgroups (No bonding, Linux kernel bond, OVS-managed) so the operator picks based on topology family, not by alphabetical accident.
- **Diagram pane width 2:1**. The Network Topology pane now gets 2/3 of the horizontal space (was 1/2) since it became the headliner after v1.1.11. YAML pane stays usable at 1/3.
- **`js/lib/icons.js`** - centralized SVG icon library with `rvtmaIcon(name, opts)` helper. ~19 icons covering common UI roles. First batch of section headers migrated from emoji to currentColor-aware SVG.

### Fixed
- **Bond cards** in Physical Network used class `c-purple` which is not in the `.blast-module` CSS scope (only `c-storage`, `c-mgmt`, `c-hub` map to purple). Bond icons were rendering in fallback grey. Switched to `c-mgmt`.
- **LTR pane clipping** mostly fixed: `align-items: flex-start` on the body (was `center`, which clips when content overflows), padding-top bumped to 32px, max-height bumped to 600px. A residual top-edge clipping bug remains under investigation, tracked in the v1.1.8 backlog memory.

### Removed
- Legacy `generateHostingClusterDetails()` and `generateHostedClusterDetails()` from `ClusterArchitectureUI`. Unused since the v1.1.9 visual migration. Net: -82 lines.

### Renamed
- "OVS Load Balancing" -> "OVS Balance SLB (no switch reconfig)". The new label is unambiguous about what the OVS bond mode actually is and when to pick it.

---

## [1.1.11] - 2026-04-19

Third major component to adopt the v1.1.9 vocabulary: Physical Network. The 9 hardcoded topology layouts (3 strategies x 3 NIC counts) collapse into one data-driven renderer.

### Added
- **Physical Network v2 topology renderer**: NICs -> bonds -> bridges as 3 layers of cards with animated dashed SVG connectors. Same primitives as Blast Radius / Cluster Architecture / Storage Architecture.
- **Topology spec builder** (`_pnBuildTopologySpec`) describes each configuration declaratively: `{ nics, bonds, bridges, notes }`. Adding a 4th bonding strategy or an 8-NIC variant becomes a data change, not a copy-paste-and-tweak.
- **Mode-specific notes**: shared-bond warning (2 NICs bonded), HA failover ack (active-backup), load balancing ack (balance-xor), OVS SLB ack, mixed-mode note (linux-bridge). Rendered as `.alert-glow` chips below the topology.

### Changed
- All 12 production combinations validated end-to-end (no-bonding/separate-bridges/active-backup/balance-xor/linux-bridge/ovs-balance x 2/4/6 NICs).
- Emoji arrows (down arrows everywhere) and `net-box-*` hardcoded colors replaced by SVG dashed connectors + theme-aware `--rvtma-c-*` accents.

### Removed
- Legacy hardcoded topology generators: `generateNoBindingTopologyModal`, `generateSeparateBridgesTopologyModal`, `generateBondedTopologyModal` (and their window exports). Net delta: 400 lines removed, 250 lines added. Same behavior, less maintenance surface.

---

## [1.1.10] - 2026-04-19

This release elevates "Storage Connectivity & LUN Capacity" from a section inside Storage Analysis into its own dedicated screen under Architecture. Different audience (capacity planners, not migration mappers), different purpose (risk + capacity vs current state inventory), and the visual transformation that comes with the v1.1.9 vocabulary now applies to the entire screen without table-style islands.

### Added
- **New screen: Storage Architecture** under sidebar > Architecture (between Cluster Architecture and Cluster Conversion). Capacity planning, shared array detection via NAA prefix, and per-cluster LUN sizing.
- **Pattern C inverted visualization**: each storage array renders as a central card with consuming clusters fanning out below, connected by animated dashed lines (the Blast Radius `b-pattern-wrap` vocabulary). Vendor + NAA prefix in the array head, projected PVCs in monospace blue, host pills (h1..hN with FQDN tooltips) inside each consumer card.
- **Over-capacity pulse**: arrays exceeding 100% utilization (totalPVCs > ports x LUNs/port) gain `.rvtma-pulse-critical` automatically. Reduce LUNs/port or array ports to see the array light up red - immediate visual feedback for capacity planning what-if exercises.
- **Per-cluster (private storage) section** for clusters that don't appear in any shared array - rendered as standalone cards with embedded capacity calculator + optional doughnut chart in `<details>`.

### Changed
- **Storage Analysis screen** no longer renders the Storage Connectivity section inline. A small note redirects to the new Storage Architecture screen.
- **`window.updateStorageCapacity`** wrapped to also re-evaluate over-capacity pulse on every input change.

### Architecture
- Analysis logic + renderer + capacity calculator extracted into new `js/storage-architecture.js` (self-contained module).
- Wires into existing infrastructure: sidebar nav entry, breadcrumb mapping, `initSection` handler, content panel.
- CSS additions scoped to `.storage-arch-viz` (storage-arch-global-config, storage-arch-array-config, storage-arch-array-info, storage-arch-cluster-stats, storage-arch-stat-row, storage-arch-private-grid, storage-arch-chart-wrap).

### Deferred to v1.1.11
- Deduplicate the analysis logic still living in storage-analysis.js (used by the report generator's data cache). Requires careful surgery to avoid breaking downloadStorageAnalysisReport.

---

## [1.1.9] - 2026-04-18

This release extracts the Blast Radius v2 visual vocabulary into reusable RVTMA primitives and applies them to the first major component beyond Blast Radius itself: Cluster Architecture. Foundation for further migrations of Storage, Network, and other interactive views.

### Added
- **Global accent palette tokens** in `:root` - `--rvtma-c-blue/green/orange/purple/pink/teal/cyan/amber/red`. Each color documented with its semantic role (control plane, prod, storage, hosted CP, worker, infra, standalone, critical). Available to any component, not just Blast Radius.
- **`.rvtma-pulse-critical` global utility class** - the red halo + pulse animation born in Blast Radius `.card.blast`, exposed as a standalone utility any element can apply. Calm but unmistakable; shadow-only animation (no transform = no layout shift). Bumped intensity for clearer perception.
- **`js/lib/connectors.js` shared utility** - `drawConnectorsBetween(svgId, sourceId, targetIds)` and `drawConnectorEdges(svgId, edges)` extracted from Blast Radius and exposed on `window` for reuse. Enables any architectural diagram to draw animated dashed lines between HTML elements.
- **Cluster Architecture v2** - the table-style hosting/hosted visualization replaced with a proper architectural diagram:
  - Hosting cluster as top card with hosts shown as numbered node-pills (h1, h2...) carrying the full FQDN in the title attribute
  - Hosted clusters in a grid below, color-coded by workload (Linux=green, Windows=blue, SQL=purple, Weblogic=pink, Oracle=teal, DMZ=amber)
  - Animated dashed connector lines from hosting to each hosted card
  - Workload-specific monochromatic SVG icons (replacing emojis) that pick up the accent color via currentColor
  - vCluster vs Inferred badge in each hosted card header
  - Heavy details (host hardware tables, datastore lists) preserved in `<details>` accordions below the diagram
- **Standalone clusters** (when standalone-only mode active) get the same vocabulary - one card per cluster with hosts as pills, no connectors.
- **HA risk auto-detection** - clusters with fewer than 3 hosts (cannot achieve etcd quorum) automatically pulse + display a red "NO HA · N hosts" badge. Universal across hosting, hosted, and standalone cards.
- **Risk alerts pulse** - the "High Risks Identified" banner in Infrastructure Summary now adopts `.rvtma-pulse-critical`. RDM-detected xlsx and other critical risks visually demand attention without requiring the user to read.

### Changed
- **Workload checkbox checked-state** rewired to use `--rvtma-c-green` tokens with `color-mix` and a subtle outer ring. The previous dark-mode tint (`#0f2215`) was nearly invisible; now clearly visible in both themes without being loud.
- **Blast Radius `.card.blast`** updated to use the new global `--rvtma-c-red` token (was hardcoded `#ef4444`). DRY win after the foundation extraction.

### Foundation work
The Blast Radius v2 vocabulary is no longer Blast-Radius-specific. The same primitives (cards, node-pills, connectors, blast pulse, severity dots, monochromatic SVG icons) are now shared infrastructure that future component migrations (Storage Architecture, Physical Network, NAD results) can adopt without reimplementation. Cluster Architecture v2 is the first proof of value.

---

## [1.1.8] - 2026-04-18

### Added
- **Blast Radius v2** - full visual refresh of the cluster pattern comparison module. SVG diagrams replaced with HTML cards positioned by CSS grid; thin animated dashed connectors drawn as SVG overlays at runtime; severity now communicated via card `.blast` class (red glow + pulsing) instead of overlay rectangles. Each pool card carries node-pills (`cp1`, `w1`, `w2`, `s1`...) preserving the architecture-blueprint mental model. Pattern B reorganized into 3 explicit layers (mgmt CP -> hosted CPs -> hosted clusters) so subordination is visually obvious. Custom SVG icon (concentric circles + corner dashes) replaces the emoji.
- **Blast Radius standalone** at `examples/RVTMA-Blast-Radius-Component.html` - self-contained single-file embed for blogs, docs, Hugo shortcodes, Cloudflare Pages. Mirrors the integrated module 1:1; light/dark with own toggle + localStorage persistence.
- **About screen** (sidebar > Settings) with version, license, author, and repo/changelog links. Replaces the author credit in the footer.
- **Version badge** in the masthead (`v1.1.8`).
- **Dark mode** across the app with light as default; preference persists in localStorage. Full coverage for Datacenter Overview, Storage Analysis, Network Migration, NAD, Cluster Architecture, Blast Radius, Cluster Conversion, Physical Network, OS Analysis, Migration Guide.
- **PatternFly 6 visual vocabulary** - OCP Console-inspired chrome with semantic CSS tokens (`--rvtma-bg-*`, `--rvtma-text-*`, `--rvtma-border`, `--rvtma-shadow`) replacing hardcoded colors.
- **Unified callout/alert system** - `.alert-glow` (full outline + halo), `.alert-strong` (darker surface), `.rvtma-callout` variants (green/blue/orange/red), `.step-card` phases, and `.rvtma-note`. Replaces 5+ ad-hoc Bootstrap-era classes.
- **Standardized button system** - `btn-primary/secondary/tertiary/danger` applied across every screen.
- **Per-department accordion** on Migration Time Estimator VM list to tame long inventories.
- **`<details>` accordions** for long generated YAML blocks on Network Migration.

### Changed
- Sidebar + masthead adopt OCP Console density and dark chrome in both themes.
- Screen-by-screen callout consolidation: Datacenter, Storage Analysis, Network Migration, NAD, Cluster Architecture, Cluster Conversion, Physical Network, Migration Guide, Migration Estimator, Blast Radius - colored left stripes replaced by the new vocabulary.
- Emoji audit: only informational glyphs kept.
- Cluster Conversion: removed the per-host NNCP dump (already delivered via Physical Network + Cluster Architecture downloads).
- Blast Radius: darker surface on `.pattern-header`/`.calc-header`; `.calc-intro` and `.calc-result-card` severity variants use the glow pattern.

### Fixed
- **Blast Radius B hcp click** - clicking either Hosted CP previously lit up both cards (heritage from the legacy SVG region). Now each hosted CP has its own click target; impact text names which CP failed and which is unaffected, reinforcing HyperShift's per-cluster isolation guarantee.
- **Blast Radius B mgmt connectors** - lines from mgmt CP to hosted CPs were rendered but visually obscured by the cluster's opaque background (z-index conflict). Split into two SVG overlays (one nested inside the cluster for mgmt -> hcps, one at the wrap level for hcp -> workers).
- **RDM risk alert** - `analyzeRisks()` now scans the vDisk tab directly, so xlsx with RDM VMs surfaces in the High Risks banner with disk/VM counts.
- **Random VLAN IDs** - `generateAllNetworkConfigs` no longer invents VLANs via `Math.random()` when neither the RVTools `vlanId` nor the port group name provides one; untagged is emitted instead. Same cleanup in `generateDistributedSwitchGuide`.
- **Broken ResourceQuota block** on Cluster Conversion removed (NaN truncated the YAML; the heuristic itself was conceptually wrong).
- **Migration Report (ADOC)** button: portGroupSummary now carries the required fields and the handler is guarded by try/catch.
- **Infrastructure Summary stat cards** visible on white content (use `--rvtma-bg-subtle`).
- `--rvtma-bg-subtle` now distinct from `--rvtma-bg-card` in dark mode.
- NNCP YAML uses VLANs detected in RVTools instead of a static 100-199 range, with dvPort/vPort coverage.

### License
- Relicensed under **PolyForm Noncommercial 1.0.0**. Source-available for noncommercial use; commercial use by third parties requires a separate license. Replaces the prior GPLv3 / Apache-2.0 mixup in the repo.

---

## [1.1.7] - 2026-04-10

### Added
- **Report internationalization (i18n)** - exported reports and PDFs now support 3 languages: English, Portuguese (pt-BR), and Spanish (es). UI stays in English.
- **Report Language selector** in PDF Settings modal - persists in localStorage, passed to CGI for PDF date localization.
- **`js/report-i18n.js` module** - 880+ translation keys across 9 namespaces with `t()` global function, parameter substitution, and English fallback.
- **Locale-aware PDF dates** - CGI script generates dates in the selected language (e.g., "10 de abril de 2026" for pt-BR) without system locale dependency.
- **Language suffix in filenames** - exported files include language code when not English (e.g., `report-PT-BR.pdf`, `report-ES.adoc`).
- **RDM report split by OS** - Hierarchical Migration Plan now separates RDM VMs into Windows and Linux/Other groups with OS column.
- **OS field in migration data** - `processMigrationData()` now extracts Guest OS from RVTools vInfo (with multi-source fallback) for use in reports.

### Changed
- All 7 report-generating files migrated to use `t()` instead of hardcoded English strings: `storage-analysis.js`, `os-analysis-modal.js`, `migration-time-reports.js`, `migration-time-hierarchical-report.js`, `cluster-architecture-ui.js`, `download-architecture-document.js`, and inline scripts in `index.html`.
- PDF Settings modal now includes language selector between Analyst Name and Client Name fields.
- `generate-pdf-cgi.sh` reads `lang` query parameter and sets pandoc `lang:` metadata for correct hyphenation.
- `formatTimeWithUnits()` in migration reports now uses localized time unit labels.
- `getReadinessMessage()` and `generateRecommendations()` in OS Analysis now use `t()` calls.
- Report cache is invalidated when language changes, ensuring PDF generation always uses the selected language.
- PDF generation no longer triggers a .md/.adoc download as side effect (`skipDownload` parameter added to all report generators).

---

## [1.1.6] - 2026-04-09

### Added
- **Server-side PDF generation** - pandoc + lualatex + eisvogel template running natively inside the container. Supports both AsciiDoc and Markdown. No nested containers needed.
- **Generate PDF buttons** across all views: Datacenter Overview, Cluster Architecture, Cluster Conversion, Migration Time Estimator, OS Analysis, and Storage Analysis.
- **PDF Settings** modal - configure analyst name, client name, and client logo. Settings persist in browser localStorage. Logo uploaded per session.
- **PDF progress bar** - visual indicator on buttons showing elapsed time during PDF generation (300s timeout).
- **Storage Analysis** tab replacing the former Storage Conversion (WIP). Includes datastore inventory, vDisk profile, RDM detection, storage class mapping, and PVC estimation. Downloadable report (MD + PDF).
- **Cluster Conversion disclaimer** explaining why VMware-to-OpenShift conversion is not 1:1 (storage CSI, backup OADP, HA/DRS, networking, monitoring).
- **Blast Radius improvements** - per-pool granularity in Pattern A, ACM Hub clickable in Patterns C/E, HyperShift 7 mandatory gates, interactive calculator with RVTools data pre-fill.
- **Reusable ModalManager** - all 5 modals migrated from legacy static HTML to dynamic creation.
- **Automation (Ansible)** tab replacing Testing & Validation and Automation Scripts (WIP).
- **OpenShift deployment** - single-file manifest (`deploy/openshift/rvtma.yaml`) with Deployment, Service, Route (300s timeout). Container runs as non-root (port 8080, restricted SCC).
- **GitHub Actions CI** for multi-arch container builds (amd64 + arm64) on version tags.
- **Open source foundation** - package.json, CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md, QUICKSTART.md, RELEASE.md, ESLint, Prettier, unit test infrastructure, architecture diagrams.

### Changed
- Renamed document buttons: "Server Allocation Plan", "Deployment Plan", "Migration Time Estimation", "Hierarchical Migration Plan", "Cluster Analysis Report".
- Renamed 10 JS files from snake_case to kebab-case.
- Blast Radius tab moved after Cluster Architecture in navigation.
- Container listens on port 8080 (OpenShift compatible, non-root).
- PDF cover page date now always in English format.

### Fixed
- PDF generation on OpenShift (writable TEXMFVAR/TEXMFCONFIG for lualatex font cache).
- PDF title page and TOC rendering (AsciiDoc attributes were inside comment block).
- Removed python3 dependency from CGI URL-decode (pure sed).
- Removed dead code buttons in NAD legacy UI.
- Added aria-label to all floating buttons.

---

## [1.1.5] - 2026-03-20

### Added
- Interactive **Blast Radius Analysis** with 5 architecture patterns (A-E).
- **VMkernel Analysis table** with MTU, VLAN, IP, and DHCP detection.
- Real **VLAN ID extraction** from VMkernel data.
- Support for **unlimited port groups**.

### Changed
- Improved network interface selection and Physical Network modal.
- Renamed tab: Network Conversion to Network Migration.

### Fixed
- NNCP topology issues across all bonding strategies.
- Runtime crash from function name collision.
- Error isolation in generateResults().
- Internationalization: translated 140+ strings to English.

---

<!-- PR reference links (auto-generated section) -->
[#31]: https://github.com/linuxelitebr/rvtma/pull/31
[#32]: https://github.com/linuxelitebr/rvtma/pull/32
[#33]: https://github.com/linuxelitebr/rvtma/pull/33
[#34]: https://github.com/linuxelitebr/rvtma/pull/34
[#35]: https://github.com/linuxelitebr/rvtma/pull/35
[#36]: https://github.com/linuxelitebr/rvtma/pull/36
[#37]: https://github.com/linuxelitebr/rvtma/pull/37
[#38]: https://github.com/linuxelitebr/rvtma/pull/38
[#39]: https://github.com/linuxelitebr/rvtma/pull/39
[#40]: https://github.com/linuxelitebr/rvtma/pull/40
[#41]: https://github.com/linuxelitebr/rvtma/pull/41
[#42]: https://github.com/linuxelitebr/rvtma/pull/42
[#43]: https://github.com/linuxelitebr/rvtma/pull/43
[#44]: https://github.com/linuxelitebr/rvtma/pull/44
[#45]: https://github.com/linuxelitebr/rvtma/pull/45
[#46]: https://github.com/linuxelitebr/rvtma/pull/46
[#58]: https://github.com/linuxelitebr/rvtma/pull/58
[#60]: https://github.com/linuxelitebr/rvtma/pull/60
[#67]: https://github.com/linuxelitebr/rvtma/pull/67
[#68]: https://github.com/linuxelitebr/rvtma/pull/68
[#71]: https://github.com/linuxelitebr/rvtma/pull/71
[#72]: https://github.com/linuxelitebr/rvtma/pull/72
[#76]: https://github.com/linuxelitebr/rvtma/pull/76
[#78]: https://github.com/linuxelitebr/rvtma/pull/78
[#79]: https://github.com/linuxelitebr/rvtma/pull/79
[#81]: https://github.com/linuxelitebr/rvtma/pull/81
[#86]: https://github.com/linuxelitebr/rvtma/pull/86
[#88]: https://github.com/linuxelitebr/rvtma/pull/88
[#92]: https://github.com/linuxelitebr/rvtma/pull/92
[#93]: https://github.com/linuxelitebr/rvtma/pull/93
[#94]: https://github.com/linuxelitebr/rvtma/pull/94
[#95]: https://github.com/linuxelitebr/rvtma/pull/95
[#111]: https://github.com/linuxelitebr/rvtma/pull/111
[#119]: https://github.com/linuxelitebr/rvtma/pull/119
[#121]: https://github.com/linuxelitebr/rvtma/pull/121
[#122]: https://github.com/linuxelitebr/rvtma/pull/122
[#123]: https://github.com/linuxelitebr/rvtma/pull/123
[#124]: https://github.com/linuxelitebr/rvtma/pull/124
