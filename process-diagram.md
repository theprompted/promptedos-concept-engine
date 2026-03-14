# Ad Concepts Generator — Process Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           AD CONCEPTS GENERATOR                              │
│                              Process Flow                                    │
└─────────────────────────────────────────────────────────────────────────────┘

                                    START
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 0: LOAD COPY METHODOLOGY (GATE)                                       │
│  ════════════════════════════════════                                        │
│                                                                              │
│  Read: references/copy-methodology.md                                        │
│                                                                              │
│  ⚠️  DO NOT PROCEED until this is read                                       │
│     (Contains nuance on why copy fails vs succeeds)                          │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 1: GET INPUTS                                                          │
│  ══════════════════                                                          │
│                                                                              │
│  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐       │
│  │  FUNNEL / LP     │    │ PRODUCT IMAGES   │    │  CLIENT FOLDER   │       │
│  │  ────────────    │    │ ──────────────   │    │  ─────────────   │       │
│  │  URL or HTML     │    │  Search for:     │    │  Check for       │       │
│  │  provided by     │    │  *product*.png   │    │  existing files  │       │
│  │  user            │    │  *reference*.jpg │    │                  │       │
│  │                  │    │  assets/         │    │                  │       │
│  │                  │    │                  │    │                  │       │
│  │                  │    │  If missing:     │    │                  │       │
│  │                  │    │  ASK USER        │    │                  │       │
│  └──────────────────┘    └──────────────────┘    └──────────────────┘       │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 2: CONSUMER MAPPING                                                    │
│  ════════════════════════                                                    │
│                                                                              │
│         Search: *consumer-mapping*.md or *hell-heaven*.md                    │
│                                                                              │
│              ┌─────────────┐              ┌─────────────┐                    │
│              │   EXISTS?   │──── YES ────▶│   LOAD IT   │                    │
│              └─────────────┘              └─────────────┘                    │
│                    │                                                         │
│                   NO                                                         │
│                    │                                                         │
│                    ▼                                                         │
│         ┌─────────────────────────────────────────────┐                     │
│         │  CREATE IT using 7 questions:               │                     │
│         │  ─────────────────────────────              │                     │
│         │  1. What's in their minds?                  │                     │
│         │  2. What permission to believe?             │                     │
│         │  3. What imagery catches attention?         │                     │
│         │  4. What phrases catch attention?           │                     │
│         │  5. What's been happening acutely?          │                     │
│         │  6. What's Heaven Island imagery?           │                     │
│         │  7. Symptomatic moments at critical times?  │                     │
│         │                                             │                     │
│         │  Save to: [client]/[product]-consumer-mapping.md                  │
│         └─────────────────────────────────────────────┘                     │
│                                                                              │
│  OUTPUT: Hell Island scenes, Heaven Island imagery, Copy Ammunition          │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 3: SELECT REFERENCE IMAGES (NOT FORMAT TYPES!)                         │
│  ═══════════════════════════════════════════════════                         │
│                                                                              │
│  Read: /Resources/ad-swipe-file/ad-formats.json                              │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                                                                      │    │
│  │   ❌ WRONG: "I'll use 3 native-advertorial formats"                  │    │
│  │                                                                      │    │
│  │   ✅ RIGHT: "I'll model bqhj.jpeg, bqjq.jpeg, bqsv.jpeg"             │    │
│  │                                                                      │    │
│  │   Each image IS a template with exact:                               │    │
│  │   • Circular insets                                                  │    │
│  │   • POV angles                                                       │    │
│  │   • Text card positions                                              │    │
│  │   • Masthead placements                                              │    │
│  │                                                                      │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  Selection criteria:                                                         │
│  • Different format_type values (diversity)                                  │
│  • Match vibe.would_work_for (product fit)                                   │
│  • Cover different primary_mechanic (social proof, authority, transform)     │
│                                                                              │
│  OUTPUT: 5-15 specific image IDs + filenames                                 │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 4: GENERATE PROMPTS (1 IMAGE = 1 PROMPT)                               │
│  ═════════════════════════════════════════════                               │
│                                                                              │
│  ⚠️  ENFORCEMENT GATE: For EACH prompt:                                      │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  1. Read FULL schema for that reference image                        │    │
│  │  2. Identify exact structural elements                               │    │
│  │  3. Write prompt preserving EXACT structure                          │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌─────────────────────────┐         ┌─────────────────────────┐            │
│  │  WHAT YOU ADAPT:        │         │  WHAT YOU PRESERVE:     │            │
│  │  ─────────────────      │         │  ──────────────────     │            │
│  │  • Copy/headlines       │         │  • Layout structure     │            │
│  │  • Product shown        │         │  • Visual hierarchy     │            │
│  │  • Human subjects       │         │  • Compositional elems  │            │
│  │    (demographics)       │         │  • Typography style     │            │
│  │                         │         │  • Constraints/negatives│            │
│  └─────────────────────────┘         └─────────────────────────┘            │
│                                                                              │
│  COPY REQUIREMENTS (from methodology):                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  • Name problem explicitly ("finishing in under 2 minutes")         │    │
│  │  • Specific transformation ("lasted so long I asked if he took...")  │    │
│  │  • Include mechanism ("trains the muscle that controls it")          │    │
│  │  • Standalone (works for first-time viewer)                          │    │
│  │  • Sound real ("I'm still in shock" not "crying writing this")       │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  PRODUCT VISIBILITY:                                                         │
│  ┌──────────┬──────────┬──────────┬──────────┐                              │
│  │   none   │background│prominent │   hero   │                              │
│  │  (not    │ (corner, │ (clearly │ (main    │                              │
│  │  shown)  │ partial) │ visible) │ element) │                              │
│  └──────────┴──────────┴──────────┴──────────┘                              │
│                                                                              │
│  OUTPUT: JSON prompts with copy, image specs, product placement              │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 5: OUTPUT HTML                                                         │
│  ═══════════════════                                                         │
│                                                                              │
│  Create: [client-folder]/concept-engine-[date]/concepts.html                 │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                                                                      │    │
│  │   ┌─────────────────────────────────────────────────────────┐       │    │
│  │   │  [REFERENCE IMAGE]                                       │       │    │
│  │   │                                                          │       │    │
│  │   │   ┌────────────────────────────────────────────────┐    │       │    │
│  │   │   │  PROMPT TEXT                                    │    │       │    │
│  │   │   │  ...                                            │    │       │    │
│  │   │   │                                    [COPY BTN]   │    │       │    │
│  │   │   └────────────────────────────────────────────────┘    │       │    │
│  │   └─────────────────────────────────────────────────────────┘       │    │
│  │                                                                      │    │
│  │   (repeat for each prompt)                                           │    │
│  │                                                                      │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  Features:                                                                   │
│  • PromptedOS design system (light theme, teal accents)                      │
│  • Mobile responsive                                                         │
│  • Copy buttons for each prompt                                              │
│  • Reference image displayed ABOVE each prompt                               │
│                                                                              │
│  Provide URL: http://100.109.172.85:8080/[path]                              │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
                                    DONE

                         User copies prompts to image
                         generator (FLUX, GPT-4o, etc.)
```

---

## Quick Reference: The 5 Steps

```
┌────────────────────────────────────────────────────────────────┐
│                                                                │
│   0. GATE ──▶ Load copy methodology (required)                 │
│       │                                                        │
│       ▼                                                        │
│   1. INPUT ──▶ Funnel/LP + Product images + Client folder      │
│       │                                                        │
│       ▼                                                        │
│   2. MAPPING ──▶ Consumer mapping (load or create)             │
│       │            └── 7 questions → Hell/Heaven Islands       │
│       ▼                                                        │
│   3. SELECT ──▶ Pick 5-15 SPECIFIC reference images            │
│       │            └── NOT format types, ACTUAL images         │
│       ▼                                                        │
│   4. GENERATE ──▶ 1 image = 1 prompt                           │
│       │            └── Preserve structure, adapt copy          │
│       ▼                                                        │
│   5. OUTPUT ──▶ HTML with copy buttons                         │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## Key Insight: The Funnel Filter

```
          ┌─────────────────────────────────────────┐
          │                                         │
          │           FUNNEL / LP                   │
          │         (the filter)                    │
          │                                         │
          └─────────────────────────────────────────┘
                           │
                           │  What does it
                           │  actually say?
                           ▼
    ┌──────────────────────────────────────────────────────┐
    │                                                      │
    │   ┌─────────┐   ┌─────────┐   ┌─────────┐           │
    │   │ ANGLE 1 │   │ ANGLE 2 │   │ ANGLE 3 │  ...      │
    │   └────┬────┘   └────┬────┘   └────┬────┘           │
    │        │             │             │                 │
    │        ▼             ▼             ▼                 │
    │   Supported?    Supported?    Supported?             │
    │        │             │             │                 │
    │       YES           YES           NO                 │
    │        │             │             │                 │
    │        ▼             ▼             ▼                 │
    │      ✅ USE       ✅ USE       ❌ DISCARD            │
    │                                                      │
    └──────────────────────────────────────────────────────┘
```
