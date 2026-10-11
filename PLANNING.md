# Voice Memo Organizer — Project Plan

> Idea-layer plan. Implementation details are intentionally light.

## 1. Problem

Everyone with a phone has a pile of voice memos. Almost nobody goes back to them.

- Memos are named "New Recording 123" and are indistinguishable from each other.
- There is no way to know what a memo *is* (a song idea, a meeting debrief, a note to self) without replaying it.
- Memos that contain commitments (action items, "I should revisit this") are silently forgotten.
- Recording is easy. Everything after recording is hard. So the pile grows and the value drops to zero.

**Core insight:** voice memos fail at the moment of capture, not retrieval. The recording tool treats every memo as an identical file. The fix is not a better player. It is a system that understands what each memo is and what it wants from you.

**One-line pitch:** an inbox for voice memos that empties itself.

## 2. Who we talked to

- People who use voice notes heavily for meetings and personal notes.
- Musicians who capture ideas in voice notes.

Common thread: "I have hundreds of these. I know there's good stuff in there. I never look."

## 3. Personas

| Persona | What a memo usually is | Why they forget it |
|---|---|---|
| Musician | Melody, lyric fragment, riff, chord idea | Hundreds of fragments, no way to compare or find the good ones |
| Meeting dumper | Post-meeting debrief, brain dump | Action items buried in audio, never written down |
| User researcher | Interview recording | Not synthesized; themes and quotes never pulled out |
| Interview practice | Self-recorded answers | No reflection loop; never reviewed |
| Game reviewer | Impressions while playing | Never drafted into the actual review |

**Design principle:** personas differ in *vocabulary*, not mechanics. A musician's "come back to this" and a PM's "follow up with Sarah" are the same primitive: a pending state with a reason. Build one pipeline, let memo type drive the details.

## 4. The three layers

### Layer 1 — Capture stays dumb

One tap, talk, done. No folders, no tags, no decisions at record time. The moment a user has to pick a category before recording, they go back to the native app.

Categorization is **post-hoc**. After the memo is processed, the AI suggests a category and tags. The user can accept, change, or ignore them, and can create their own categories (a musician might want "riffs" and "lyrics", a PM might want "1:1s" and "standups"). The AI then sorts into the user's categories, not a fixed list. This is how "each person sorts in their own way" happens without ever having to sort up front.

### Layer 2 — Understanding is automatic

Every memo is enriched without user effort:

- **Title** — one line that replaces "New Recording 123".
- **Summary** — two or three sentences.
- **Key moments** — timestamped positions in the audio that matter most when revisiting. For a 45-minute recording, this is the 3–5 places worth jumping to (the hook, the decision, the quote), not a text bullet list. Each moment has a timestamp, a short label, and a one-tap seek.
- **Category** — AI-suggested from the user's own categories. Defaults to a small starter set (song idea, meeting, note to self) that the user can rename, delete, or add to.
- **Context metadata** — time, location, overlapping calendar event, people mentioned.
- **Pending items** — action items, open questions, explicit "revisit this" statements.

### Layer 3 — Triage is the UI

Two states. This is the heart of the product.

| State | Meaning | Examples |
|---|---|---|
| 🟡 **Yellow** | This memo still wants something from you | Unchecked action items. "I want to come back to this." Interview not yet synthesized. |
| 🟢 **Green** | Resolved. Searchable forever, never nags. | Action items done. Idea moved into a project or marked dead. Interview coded. |

The daily job of the app is to move yellow to green. That gives users a reason to return, which no plain memo app has.

### What "green" means per persona

| Persona | Yellow trigger | Green trigger |
|---|---|---|
| Musician | Flagged "develop later"; similar to another idea | Moved into a project, or marked as a dead end |
| Meeting dumper | Action items extracted | Items checked off or exported to a task tool |
| User researcher | Not yet coded | Themes tagged, quotes pulled |
| Interview practice | No reflection yet | Reviewed, notes written |
| Game reviewer | Not drafted | Pulled into a review draft |

## 5. Surfacing — later, not now

The long-term win over the native app is bringing forgotten memos back at a useful moment (digests, similarity links, calendar proximity, staleness nudges). None of that is in the demo. For now, the yellow list *is* the surfacing: anything pending is visible at the top, and that is enough to prove the idea.

## 6. Open questions

1. **Source of memos.** Import from Apple Voice Memos (messy) vs. record in our own app (clean, but changes a habit). *Hackathon lean: import a folder of audio, skip the recorder.*
2. **How much does the user confirm?** Auto-classification will be wrong sometimes. A light "is this right?" on the first few memos could train per-user preferences and double as onboarding.
3. **Which personas for the demo?** *Lean: musician + meeting dumper.* Same pipeline, visibly different outcomes.
4. **What is "done" for a musician?** Fuzziest green state. Ask the musicians we talked to.
5. **Privacy.** Audio is intimate. Is processing on-device, or cloud with clear consent?
6. **What happens to green memos?** Archive with search, or do they also feed long-term knowledge (e.g. "what did I say about pricing across all meetings")?

## 7. Demo narrative

Keep it bare bones. One screen, one flow.

1. Drop in a handful of unlabeled recordings.
2. Each one gets a title, suggested tags, a summary, and key moments. The list sorts into yellow and green.
3. Open a long memo. Tap a key moment and jump straight to that spot in the audio.
4. Open a meeting memo. See the action items that made it yellow.
5. Check them off. The memo turns green.

That is the whole demo.

## 8. Scope for hackathon

**In:**
- Import audio files
- Title + summary + key moments (timestamped, tap to seek)
- AI-suggested category and tags, editable after the fact
- User-defined categories
- Action item / revisit extraction → yellow/green state
- Single list view, yellow on top
- Mark items done → memo turns green

**Out (for now):**
- Full transcript view (the audio stays the source; we surface title, summary, and key moments instead)
- Digests, nudges, or any notification
- Similarity / idea linking
- In-app recorder
- Calendar / location integration
- Task tool export
- Multi-user / sharing
- Mobile app (web is fine)

## 9. Success criteria

- A new user can look at the sorted list and know what each memo is without playing it.
- For a long memo, the user can jump to the important part without scrubbing.
- Every memo with a commitment in it is yellow. Nothing with a commitment is green.
- Someone who watched the demo says: "I would dump my memos into this."
