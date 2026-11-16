# Data Hub (Trusted Data Products): overview

For sponsors and the team: one document, readable without the factory. Plain language throughout, with no requirement IDs, codes or jargon. Written in Analyse beside the Application Definition and approved with it at Gate 1.

## Executive summary

**The problem.** People at BUUK, and the AI assistants they are starting to use, get data as raw tables and files. Nothing in the data says how fresh it is, where it is weak, who owns it or whether AI may use it. So AI answers are either refused because "the data isn't trustworthy enough", or given without the caveats a careful analyst would add.

**The impact.** Each dataset becomes a published product that carries its trust level, known issues, freshness, owner and licence with it, whichever way it is reached: a web page, SQL, a file download or an AI assistant. The first release proves this on one real public dataset, the Environment Agency's Historic Flood Map, plus a small made-up sites dataset. We will know it works when a scripted fifteen-minute demonstration runs from a clean start, an AI answer that combines both datasets arrives with both sets of caveats and the weaker trust level, a person without access is refused through every route, and the spatial combination of the two datasets runs in acceptable time on the real data.

| Reach | Impact | Confidence | Effort | Priority score |
|---|---|---|---|---|
| About 15 people a month (build team and demo audience now, a pilot group next) | 2, high | 50% | About 8 person-weeks | 1.9 |

Not triaged; these are estimates. Reach is the people who would use it monthly in the pilot. Impact is high because it changes whether data can be used with AI at all. Confidence is 50% because the heavy parts (scheduler, spatial performance) are untested. Effort is the first release only, built with a coding assistant.

**Who benefits.** Analysts and GIS users get data with its caveats attached. Data owners get a repeatable way to publish without hand-built pages. Stewards get one list of findings to triage. AI tool users get answers that say how far they can be trusted.

**Effort and cost.** Roughly eight person-weeks to the first release. It uses only free, open-source software on one machine in Docker, and public data that needs no key or account. No licences or cloud services are needed for the first release; Microsoft Copilot and sign-in with company accounts come in a later phase and will need licences then.

**Key risks.** The built-in scheduler is heavy to run on one machine: kept small and its versions pinned. Access could be checked differently on one route: one shared access rule, tested on every route. Answers sent to an AI assistant go to that assistant's provider: only public data, marked as allowed for AI, can be reached.

**The decision asked.** Approve this definition of the first release (the prototype) so the detailed specification can be written. This commits the factory's time only. It does not commit any BUUK data, licences, hosting or later phases.

## What it is
A small web platform, run in Docker, with a catalogue of data products. Each product has one page with five tabs: overview, data dictionary, quality, known issues and access, under a header that shows its trust level, who can see it, how fresh it is and who owns it. Behind it, a scheduled pipeline downloads the source, cleans it, checks it, profiles it and publishes it. The same product can then be queried with SQL, downloaded as a CSV or GeoPackage with a small file describing its caveats and version, or asked about through an AI assistant connected to the platform.

## How it changes people's work
- **Analysts and GIS users:** today they find a file and guess how reliable it is. With it they search the catalogue, read the trust level and known issues, and query or download data that keeps its caveats.
- **AI assistant users:** today an assistant either cannot use the data or uses it without caveats. With it the assistant finds the product, checks its trust level, and its answer carries the caveats; it is refused when a product is not allowed for AI or below the level needed.
- **Data owners:** today publishing means building a page by hand. With it they write a short description file and the platform builds the pipeline and the page; they fill only the gaps it flags and promote the product when the checks pass.
- **Stewards:** they see only exceptions: failed checks and reported issues, and sign off products for the highest trust level.

## In scope and out of scope
**In the first release:** the Historic Flood Map from download to a published product with a trust level; a made-up sites dataset restricted to one group; a spatial query across the two; sign-in with four roles and groups; the catalogue and product page; SQL, file download and AI assistant access; a reported-issue button; a demonstration script and a note of what was learned.

**Not in the first release:** company sign-in and Microsoft Copilot (a later phase); map previews, Power BI feeds, GIS web services and a REST interface (the next phase); a form-based pipeline builder; alerts and subscriptions; any BUUK internal data; any text written by AI inside the platform.

## What happens next
The detailed specification comes next: requirements for each part of the first release, how the parts fit together, and the delivery plan in thirteen small slices. The sponsor is asked to approve it before building starts. Building then goes slice by slice, each tested, and ends with the demonstration and an independent check against the specification before anything is released.
