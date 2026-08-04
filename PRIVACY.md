# Privacy

RVTMA is designed so that customer data never leaves the operator's machine. This document describes the data-handling model, the privacy-by-design properties that fall out of that model, and the honest caveats that come with running anything inside a web browser.

This is technical documentation, not legal advice. Whether a specific deployment of RVTMA satisfies LGPD, GDPR, or any other data-protection regime is a determination for the operator's legal and compliance teams, not for this document.

## Data-handling model

RVTools exports loaded into RVTMA are processed entirely in the operator's browser. Every parsing step, every analysis function, every chart and table is computed in the JavaScript runtime of the browser tab the operator opened. No data is sent to a server controlled by RVTMA; there is no server controlled by RVTMA.

When the operator closes the browser tab, the parsed data is gone. The same applies to a browser refresh, a crash, or any other end-of-session event. RVTMA does not write to `localStorage`, `sessionStorage`, `IndexedDB`, the file system through the browser, or any other client-side persistence mechanism for customer data. Local storage is used only for operator preferences such as theme, language, and last-selected tab.

Generated deliverables (PDF, AsciiDoc, Markdown, Excel) are produced at runtime and downloaded to the operator's machine through the browser's standard "save file" flow. Once a deliverable lands on disk, RVTMA's involvement ends; what happens to the file afterwards is governed by the operator's organization, not by the tool.

## What RVTMA does not do

- **No telemetry.** No analytics events, no error reports phoned home, no usage statistics collected, no heartbeat pings. The codebase contains zero calls to analytics or telemetry endpoints. A `grep` for `fetch(`, `XMLHttpRequest`, or `navigator.sendBeacon` will turn up only the vendor library bundles and a small number of internal calls (PDF generation in container mode talks to the local container, never to a remote service).
- **No third-party SDKs.** The bundled vendor libraries (Chart.js, SheetJS, ExcelJS, Mermaid, PatternFly) are loaded locally from `assets/`. There is no Google Analytics, no Mixpanel, no Sentry, no Datadog RUM, no LinkedIn Insight, no LaunchDarkly, no Stripe pixel, no New Relic browser agent, nothing.
- **No cross-origin requests with customer data.** The only network calls RVTMA's runtime makes are to fetch its own static assets from the same origin that served the page. Customer data is never serialized into a URL, a form post, a WebSocket frame, or any other transport.
- **No accounts, no auth, no identity.** RVTMA does not know who the operator is and does not try to find out. There is no login, no user record, no session identifier tied to a server.

The one documented exception: the **opt-in update check**. Disabled by default. When the operator clicks "Check now" in the About panel, or enables the daily check there, RVTMA makes a single anonymous GET to the public GitHub releases API (`api.github.com/repos/elastocera/releases/releases`) to compare version numbers. The request carries no payload, no identifier, and nothing from any loaded RVTools file - it is the same request a browser makes when opening that public page. The daily preference lives in localStorage as a UI setting, the check fails silently on networks that block GitHub, and unchecking the box stops it entirely.

## What the operator gets, by design

- **Predictable data residency.** Data resides where the operator is sitting. If the operator is in Sao Paulo, the data is in Sao Paulo. If the operator is on a corporate laptop in Munich, the data is in Munich. No transfer crosses any border that the operator did not already cross by sitting at their machine.
- **Minimal breach surface.** Because RVTMA persists nothing on the server side (there is no server side), there is no database to leak, no S3 bucket to misconfigure, no backup tape to lose. The only thing that can leak is whatever the operator does with the generated deliverables on their own disk.
- **Clear chain of custody.** Customer file in -> operator's browser -> deliverables out. Three steps, all on the same machine, all under the operator's direct control.
- **No vendor-lock data flow.** Because nothing is stored, there is no question of "how do we get our data out". The data was always with the operator.

## Honest caveats

A web browser is a complex piece of software. A few caveats apply that are outside RVTMA's control:

- **Browser memory may be swapped to disk** by the operating system under memory pressure. This is a property of every web application, not RVTMA specifically. On machines that handle sensitive data, full-disk encryption mitigates this risk; the operator's organization should already require it.
- **Browser extensions installed by the operator** can in principle access the data RVTMA loads, because that is how browser extensions work. RVTMA cannot detect or block this. The operator's organization should have a browser-extension policy; that policy applies here.
- **Screen capture, screen recording, and shoulder surfing** are physical-world threats that no web application can defend against. Standard operational hygiene applies during screen-shares and meetings.
- **Generated deliverables are persistent artifacts.** Once a PDF, Excel, or AsciiDoc file is saved, it is the operator's responsibility to handle that file according to their organization's data-classification policies. RVTMA has no insight into where the file goes after the download dialog closes.
- **Container mode runs a small local web service** for PDF generation, bound to `127.0.0.1` by default. The operator's data is sent to the local container, never beyond it; the container does not phone home. Operators running in shared environments (multi-user hosts, dev containers, etc.) should be aware that anything listening on localhost is reachable from other processes on the same host.

## Operator responsibilities

RVTMA is a tool, not a compliance framework. The operator (and their employer) remains responsible for:

- Establishing a lawful basis to process the customer data being analyzed (contract, legitimate interest, consent, or any other ground their jurisdiction recognizes).
- Maintaining the Data Processing Agreement or equivalent contract with the customer whose data is being analyzed.
- Handling the generated deliverables according to the customer's data-classification rules.
- Running RVTMA on a machine that meets the operator's organization's endpoint-security baseline (disk encryption, browser policy, etc.).
- Producing whatever records of processing activity their compliance regime requires. RVTMA does not generate audit logs, because RVTMA does not run anywhere it could log.

## Reporting privacy concerns

For privacy questions or concerns specific to RVTMA's behavior (as opposed to operational privacy questions tied to a specific engagement, which the operator handles): open an issue in this repository, or contact the maintainer listed in the About panel.

For broader data-protection questions, route through your organization's Data Protection Officer.
