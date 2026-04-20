# Venue-Specific Notes: SIGMOD, VLDB/PVLDB, ICDE, CIDR

Read this when the user asks about venue choice, formatting, or you need to match specific venue conventions.

## SIGMOD (ACM)

**Publication channel**: Proceedings of the ACM on Management of Data (PACMMOD) — SIGMOD is now a journal-conference hybrid.

**Template**: ACM `acmart` class, `\documentclass[sigconf,review]{acmart}` for submission, `sigconf` for camera-ready.

**Length**: Typically 12 pages + unlimited references for research track (check the current call — this changes year to year).

**Review**: Double-blind. Strip author info; use `\documentclass[sigconf,anonymous,review]{acmart}`.

**Style notes**:
- Section numbering: `\section`, `\subsection`, down to `\subsubsection`
- Uses `\textsc{}` for small caps (common for system names)
- Bibliography: ACM Reference Format via `\bibliographystyle{ACM-Reference-Format}`
- Figures: typically single-column `figure` or double-column `figure*`

**Paper categories**:
- Research papers
- Industrial track (different review criteria)
- Experiments, Analysis & Benchmark (EA&B) — title suffix `[Experiment]`
- Demo papers (shorter)

## VLDB / PVLDB

**Publication channel**: Proceedings of the VLDB Endowment (PVLDB), monthly rolling deadlines (1st of each month).

**Template**: Custom PVLDB style (download from the current year's site: `vldb.org/pvldb/volXX/`). Uses `\documentclass{vldb}` or similar.

**Length**: 12 pages **not including references** (references unlimited). Appendices are inside the 12-page limit.

**Review**: Single-blind — **include author names on submission**.

**Unique features**:
- **Rolling monthly deadlines** — user may time submission based on this
- **Revision round**: unique to PVLDB; if paper gets "revise" decision, author has 3 months to resubmit with explicit change list. Revisions don't count against the submission cap.
- **Cap on submissions**: no author may have >2 papers with earlier IDs under review in the same month (as of PVLDB Volume 20; check current rules).

**Special tracks** (title suffix required):
- `[Vision]` — forward-looking, argumentative
- `[Experiment]` — EA&B, reproducibility artifacts required
- `[Innovative Systems]` — systems with novel architectures

**Style notes**:
- Uses `abbrv` or custom bibliography style
- System names usually in `\textsc{}`
- Figure captions below figures, table captions above tables (ACM/IEEE convention)

## ICDE (IEEE)

**Publication channel**: IEEE International Conference on Data Engineering.

**Template**: IEEE conference template (`IEEEtran` class), `\documentclass[conference]{IEEEtran}`.

**Length**: 12 pages including references (stricter than VLDB).

**Review**: Double-blind (typical; check current year).

**Style notes**:
- IEEE bibliography style (`IEEEtran`)
- Slightly different sectioning conventions from ACM
- Figures and tables: numbered Roman numerals for tables in some years

**Categories**:
- Research papers
- Industrial and application papers
- Short papers (6 pages)
- Demo papers

## CIDR

**Publication channel**: Conference on Innovative Data Systems Research — biennial, invitation-heavy but open submissions accepted.

**Template**: Custom CIDR style — much looser than other venues.

**Length**: Typically 6–8 pages, but shorter papers encouraged.

**Review**: Typically single-blind.

**Style and content notes**:
- CIDR explicitly welcomes **visionary** and **opinion** papers
- Strong preference for systems papers with working prototypes
- Less formal evaluation required — a single compelling experiment often suffices
- Writing style is looser, more conversational, more argumentative
- Bold claims accepted if defended well

**When to target CIDR**: early-stage ideas, provocative positions, systems papers that aren't ready for full SIGMOD/VLDB evaluation, visionary research agendas.

## Quick venue picker

If the user isn't sure where to submit:

- **Deep, mature, rigorous evaluation, incremental technical contribution**: SIGMOD or PVLDB (interchangeable in practice; PVLDB's monthly deadlines favor faster turnaround).
- **Experimental study / benchmark / reproducibility paper**: PVLDB EA&B track.
- **Industry paper with production deployment**: SIGMOD Industrial or PVLDB Industrial.
- **Visionary, early, or provocative**: CIDR.
- **Strong fit for European community or want a bit less competitive reviewing**: ICDE or EDBT (not covered here but similar expectations).
- **Tight deadline constraint + want fast review**: PVLDB (monthly cycle).

## Formatting fixes to check

When revising, quickly check:

- [ ] Class file matches venue (`acmart` for SIGMOD, `vldb` for PVLDB, `IEEEtran` for ICDE)
- [ ] Review mode flag set correctly (`anonymous` for double-blind venues)
- [ ] Bibliography style matches venue
- [ ] Title suffix for special tracks (`[Vision]`, `[Experiment]`, etc.)
- [ ] Page count within limits
- [ ] No font size / spacing hacks to shrink (reviewers check)
- [ ] Author info stripped (for double-blind) or included (for single-blind)
- [ ] ACM CCS concepts and keywords (SIGMOD)
- [ ] Reference format matches the template's bibliography style

## Things that differ (quick reference)

|  | SIGMOD | PVLDB | ICDE | CIDR |
|---|---|---|---|---|
| Class | `acmart` | `vldb` | `IEEEtran` | custom |
| Review | double-blind | **single-blind** | double-blind | single-blind |
| Length | 12p + refs | 12p + refs | 12p incl. refs | 6–8p |
| Deadline | annual | monthly | annual | biennial |
| Revise round | no | **yes (3 months)** | no | no |
| EA&B track | yes | **yes (reproducibility required)** | experimental track | — |
| Vision papers | — | `[Vision]` track | — | **core strength** |
