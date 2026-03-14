# Ad Copy Methodology

This document contains the full nuanced understanding of what makes ad copy work vs fail, extracted from iterative refinement sessions. Read this completely before writing any ad copy.

---

## The Core Problem

**Copy fails when it's written for a reader who already has context. That reader doesn't exist.**

The person seeing your ad:
- Is scrolling past hundreds of ads per day
- Has never heard of this product
- Is not studying you like an exam
- Will not do the work of inferring what you mean
- Needs to be STOPPED first, then sold

---

## The Evolution: How Bad Copy Becomes Good Copy

### Round 1: The Original (Terrible)

```
"My husband finally told me what he's been struggling with for 10 years. I ordered him this device without telling him. Now he's asking why I didn't do it sooner. Best $300 I've ever spent."
```

**Why this fails:**
- "What he's been struggling with" — What problem? The reader has to guess.
- "Why I didn't do it sooner" — Why would he ask that? No context for what changed.
- Zero information about the actual problem or the transformation.
- Written for someone who already knows what this product does. They don't.

**Feedback:** "There's so little context here like what problem was he struggling with... why is he asking why she didn't do it sooner... it makes no sense to me. It has zero context at all into the problem that our consumer is dealing with or the promise that they care about when it comes to Heaven Island."

---

### Round 2: First Rewrite (Still Bad)

```
"For years he'd roll over and say 'I'm tired' and I knew what that really meant. He thought I didn't notice. I noticed everything. Ordered this trainer without telling him. Week 3 he initiated for the first time in I don't know how long. He doesn't know I'm crying writing this."
```

**What was attempted:** Added "I'm tired" as the avoidance, added "he initiated" as the transformation.

**Why this STILL fails:**

1. **Still doesn't name the actual problem.** "I'm tired" is avoidance behavior, but there's no moment where the reader knows FOR CERTAIN this is about PE. There's no "finishing too fast" or "it being over before it started."

2. **Written for someone who already knows what this product is for.** But the ad needs to TELL them.

3. **"I knew what that really meant"** — This is polite evasion. The reader has to do the work of figuring out what "that" means. They won't do the work. They'll scroll past.

4. **"He initiated"** — Initiated what? Sex? A conversation? The reader has to infer. You're making them work.

5. **"He doesn't know I'm crying writing this"** — This is a copywriter trying to create emotion. It doesn't sound like how a real person tweets. It sounds crafted. Which breaks trust.

6. **"For years he'd roll over..."** — That's a slow build. It's narrative. It's what you write when someone's already reading. It's not a pattern interrupt.

**The thread through all of this:** Writing for a reader who's already paying attention and already has context. That reader doesn't exist.

---

### Round 3: The Fix (Good)

```
"My husband has finished in under two minutes for 12 years. I stopped saying anything because what's the point. He started using this device that trains the muscle that controls it. Week 3 he lasted so long I asked if he took something. He said no. It's just the muscle finally working. I'm still in shock."
```

**What changed and WHY:**

| Before | After | Why It's Better |
|--------|-------|-----------------|
| "I'm tired" (vague avoidance) | "finished in under two minutes for 12 years" | EXPLICIT. SPECIFIC. The right person sees this and stops scrolling. |
| "I knew what that really meant" | Cut entirely | No inference needed. Don't make them work. |
| "I noticed everything" (tasteful) | "I stopped saying anything because what's the point" | Names the silence directly. The emotional texture of Hell Island. |
| No mechanism | "trains the muscle that controls it" | Now there's a reason to believe. An "oh that makes sense" moment. |
| "He initiated" (vague) | "he lasted so long I asked if he took something" | SPECIFIC. VISUAL. You can see this scene. |
| "crying writing this" (copywriter emotion) | "I'm still in shock" | Simple. Believable. Sounds like a real person. |

**First line names the problem. Mechanism is explained. Transformation is concrete. Sounds like a real person, not a marketer.**

---

## The Five Failures (Diagnose Your Copy)

### Failure 1: Assuming Context

**Bad:** "You've been treating the wrong problem"
**Why it's bad:** Assumes they know which problem. It's mid-conversation copy.

**Good:** "Finishing in under 2 minutes? It's not in your head. It's a muscle you've never trained."
**Why it works:** First line = the problem, explicit. Second line = the reframe with mechanism. The person scrolling at 2am who finishes too fast sees "finishing in under 2 minutes" and STOPS. That's the dog whistle.

### Failure 2: Being Tasteful Instead of Specific

**Bad:** "I knew what that really meant"
**Why it's bad:** Polite evasion. Reader has to figure out what "that" means. They won't. They'll scroll.

**Good:** "He'd finish in under a minute and pretend to fall asleep"
**Why it works:** Does the work FOR them. Names the thing. No inference required.

### Failure 3: Vague Transformations

**Bad:** "She notices" / "He initiated"
**Why it's bad:** Notices what? Initiated what? Reader has to infer.

**Good:** "Lasted so long I asked if he took something"
**Why it works:** SPECIFIC. VISUAL. You can see this scene happening.

### Failure 4: No Mechanism

**Bad:** "I ordered it and it worked"
**Why it's bad:** That's a claim, not a story that earns belief. No reason to believe.

**Good:** "This device trains the muscle that controls it" / "It's a muscle, not mental"
**Why it works:** The "oh that makes sense" moment. Now there's a reason it would work.

### Failure 5: Sounding Like Copy

**Bad:** "He doesn't know I'm crying writing this"
**Why it's bad:** Copywriter trying to create emotion. Sounds crafted. Breaks trust.

**Good:** "I'm still in shock"
**Why it works:** Simple. Believable. How a real person actually talks.

---

## Additional Failure: Labels in Prompts

When generating image prompts, don't include writer's notes:

**Bad:**
```json
"body_copy": {
  "content": [
    {"type":"reframe","text":"You see, PE is not a mental problem..."},
    {"type":"validation","text":"That's where the shame comes from..."}
  ]
}
```

**Why it's bad:** `"type":"reframe"` is a WRITER'S NOTE. The image model sees "type: reframe" and has no idea what to do with it.

**Also bad:**
```json
"body_copy": {
  "intro": "Most men over 45 try everything...",
  "authority": "Dr. Yaron Levy explains why...",
  "testimonial": "\"I've seen men regain full control...\""
}
```

**Why it's bad:** "intro", "authority", "testimonial" are LABELS telling the model what role each paragraph plays. The model just needs the TEXT.

**Good:** Just the text with position information. No labels. No writer's notes.

---

## The Headline Test

A headline that works for someone with NO context:

**Bad:** "You've been treating the wrong problem"
- Assumes they know which problem
- Commentary on their approach, not a pattern interrupt

**Good:** "FINISHING IN UNDER 2 MINUTES? IT'S NOT IN YOUR HEAD. IT'S A MUSCLE YOU'VE NEVER TRAINED."
- Line 1: Names the problem explicitly (dog whistle for the right person)
- Line 2: The reframe with the mechanism

The person scrolling at 2am who finishes too fast sees "finishing in under 2 minutes" and stops. That's the dog whistle. Then you give them the answer.

---

## Failed Attempts Must Be Named

Good copy validates their experience by naming what they've already tried:

**Bad:** No mention of what didn't work

**Good:** "Pills: Made it harder, still finished fast. Creams: Numbed both of us. Breathing tricks: Lasted 3 days. Thinking about baseball: Never worked."

**Why it works:**
- Validates their experience ("yes, I tried that too")
- Builds credibility ("they understand the problem")
- Creates contrast for why this solution is different

---

## The Mental Model

**Before writing any copy, ask:**

1. If someone who has NEVER heard of this product sees this, will they know exactly what problem it solves in the first line?

2. Is the transformation specific enough that they can visualize the scene? ("lasted so long I asked if he took something" vs "she noticed")

3. Is there a mechanism — a reason WHY it works that creates an "oh that makes sense" moment?

4. Does it sound like a real person wrote it, or does it sound like a copywriter trying to create emotion?

5. Have I named failed attempts to validate their experience?

6. Am I making them do ANY work to understand what I mean? If so, they won't. They'll scroll.

---

## Copy Checklist (Before Finalizing)

- [ ] First line names the problem EXPLICITLY (not "what he struggles with" but "finishing in under 2 minutes")
- [ ] Zero assumptions about prior context
- [ ] Transformation is SPECIFIC and VISUAL (not "she notices" but "lasted so long I asked if he took something")
- [ ] Mechanism is explained (why it works)
- [ ] Sounds like a real person, not a marketer
- [ ] Failed attempts are named (validates their experience)
- [ ] No labels or writer's notes in prompt (no "type:", "intro:", "authority:")
- [ ] No polite evasion — name the thing directly
- [ ] No slow narrative builds — first line must stop the scroll
