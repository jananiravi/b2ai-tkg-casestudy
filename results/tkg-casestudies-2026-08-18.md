# Talent KG case studies: initial results and next steps

**Analysis run on 2026-08-18; technical implementation completed on 2026-08-19.**

This report summarizes the first execution of the three case studies in this repository.
It records what the Talent KG can already do and what still needs review or additional
data. Candidate names are search results for evaluation, not assignments or endorsements.

| Case study | What is now working | What remains |
| --- | --- | --- |
| [1. Niche mentorship](../analysis/niche_mentorship.Rmd) | Three synthetic personas were run, and Bridge2AI-only mentor shortlists were produced. | Reviewers assess the candidates; career-stage data still needs to be joined from onboarding data. |
| [2. Precision interdisciplinary teaming](../analysis/precision_interdisc_teaming.Rmd) | A topic-first search can exclude people the researcher already works with. | Select a real grant/persona and validation set; define the award-history rules for the program-officer view. |
| [3. Cross-disciplinary niches](../analysis/map_crossdisc_niches.Rmd) | The static Talent KG layout was queried offline and used to generate candidate coverage gaps. | Validate the regions against papers, alternative map settings, and the survey-side expertise analysis. |

## Case study 1 — finding a mentor for a specific skill gap

### Question

Can the Talent KG help a researcher find a Bridge2AI mentor who has a skill missing from
the researcher's current environment?

### What we ran

Three synthetic personas represented different needs:

| Persona | Current area | Skill sought |
| --- | --- | --- |
| A | Wet-lab proteomics and imaging | AI-ready data packaging, metadata and provenance |
| B | Clinical and critical-care data | Privacy, consent, ELSI and multi-site data governance |
| C | Voice AI | Translating technical work into clinical protocols and cross-institutional grants |

Each persona was represented by plausible paper titles describing both current expertise
and the skill sought. Those titles were embedded and searched against the Talent KG author
index. The search was run both globally and with results restricted to Bridge2AI members.

### What we learned

The graph contains 77,534 indexed authors but only 341 Bridge2AI members. As expected, the
global top 10 contained very few members:

| Persona | Bridge2AI members in the global top 10 | Examples from the member-only shortlist |
| --- | ---: | --- |
| A | 1 | Emma Lundberg, Henning Hermjakob, Nathan Sheffield |
| B | 1 | Jeffrey Klann, Lucila Ohno-Machado, Bradley Malin |
| C | 2 | Sijia Liu, Steven Bedrick, Anaïs Rameau |

This does not mean semantic search failed. It means global author discovery and internal
mentorship are different tasks and need different scopes. A Bridge2AI-only search mode was
therefore added for the mentorship workflow.

The wording of the skill gap also matters. Concrete examples such as “data governance for
multi-site clinical AI” move the search toward the missing expertise. Vague phrases such as
“building collaborations” have much less effect.

### Next step

@monicacecilia, @techvik and @jananiravi can now review the three 10-person shortlists for
topical fit, seniority, cross-boundary value and actionability. Publication history is only
a proxy for career stage, so the planned `career_stage_simple` and `institution_type` join
from onboarding data is still needed.

## Case study 2 — finding a new collaborator with the missing expertise

### Question

Can the Talent KG help a researcher find someone with the missing expertise—someone they
have not worked with before?

### What is now working

For a researcher already represented in the graph, the search can exclude the researcher
and existing direct collaborators. A separate topic-first mode ranks the remaining people
by their relevance to the requested skill without favoring candidates merely because they
are nearby in the wider co-authorship graph.

This provides the shared search mechanism for both proposed perspectives:

| Perspective | Intended use |
| --- | --- |
| Junior investigator | Find a new co-investigator who fills a specific expertise gap. |
| Program officer | Find relevant researchers for a funded topic and understand the reach of an award portfolio. |

### Next step

The first validation still needs one real grant/persona, an expected output and a known or
reviewer-approved comparison set. The program-officer view also needs an explicit decision
about which NIH/NSF/DOD award data to use, how recent an award must be, and how funding
history should affect ranking. The current search is topic-aware; it is not yet award-aware.

The institution-type × DGP and career-stage × DGP patterns from `b2ai_survey` can help
choose a meaningful structural gap for this validation.

## Case study 3 — finding adjacent areas with limited Bridge2AI presence

### Question

Can the Talent KG map point to active research communities near Bridge2AI expertise where
the consortium currently has little direct representation?

### What we ran

The layout file is a static export and can be queried without the live application. For an
initial scan, the map was divided into a 40 × 40 grid. We kept cells with at least 25
authors and no Bridge2AI member, ranked them by the number of authors publishing since
2020, and recorded the nearest Bridge2AI members as possible entry points.

| Map statistic | Count |
| --- | ---: |
| Populated cells with at least 25 authors | 577 |
| Cells containing at least one Bridge2AI member | 199 |
| Cells without a member in that exact cell | 378 (66%) |

The 378 cells are candidate regions for review, not 378 established disciplines that
Bridge2AI “does not cover.” Grid boundaries and automatic keyword summaries are imperfect.

Five interpretable examples from the first review set are:

| Candidate area | Recently active authors / all authors | Nearby Bridge2AI member |
| --- | ---: | --- |
| Glaucoma and retinal imaging | 261 / 311 | Xiaoqian Jiang |
| Macular degeneration and retinopathy | 239 / 291 | Megan Collins |
| Sepsis and critical-care outcomes | 254 / 279 | Alistair Johnson |
| Pediatric acute care and sepsis | 236 / 249 | Jennifer Muszynski |
| Cardiovascular risk and coronary disease | 213 / 243 | Karol Watson |

### Next step

Before calling any region a strategic gap, reviewers should read representative papers,
verify affiliations, and check whether the same areas remain visible under nearby grid
resolutions or a density-based method. The results should also be compared with the
TF-IDF/LDA expertise-gap analysis in `b2ai_survey` issue #3. A final framing decision is
still needed: should the first validation serve an individual researcher or program
leadership?

## Current status

The technical exploration is complete enough for review. The next phase is domain
validation rather than more unconstrained searching:

- [Issue #1](https://github.com/jananiravi/b2ai-tkg-casestudy/issues/1): reviewer scoring and career-stage data
- [Issue #2](https://github.com/jananiravi/b2ai-tkg-casestudy/issues/2): real grant/persona, validation set and award-aware PO requirements
- [Issue #3](https://github.com/jananiravi/b2ai-tkg-casestudy/issues/3): paper-level validation, map stability and survey cross-check
