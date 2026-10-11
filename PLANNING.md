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

One tap, talk, done. No folders, no tags, no decisions at record time. The moment a user has to pick a tag before recording, they go back to the native app.

Tagging is **post-hoc**. After the memo is processed, the AI suggests one or more tags from the user's own tag list. There is one concept, the tag, not categories plus keywords. "Song idea" and "Meeting" are tags like any other. A memo can carry several (a chorus take might be both "Song idea" and "Rehearsal"). The user can add or remove tags on any memo and create new ones (a musician might want "Riffs" and "Lyrics", a PM might want "1:1s" and "Standups"). Every tag is a filter in the sidebar. This is how "each person sorts in their own way" happens without ever having to sort up front.

### Layer 2 — Understanding is automatic

Every memo is enriched without user effort:

- **Title** — one line that replaces "New Recording 123".
- **Summary** — two or three sentences.
- **Key moments** — timestamped positions in the audio that matter most when revisiting. For a 45-minute recording, this is the 3–5 places worth jumping to (the hook, the decision, the quote), not a text bullet list. Each moment has a timestamp, a short label, and a one-tap seek.
- **Tags** — one or more, AI-suggested from the user's own tag list. Starts with a small default set (Song idea, Meeting, Note to self, Interview) that the user can rename, delete, or add to. No separate keyword tags; if a word is not worth sorting by, it is not worth showing.
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

## 5. Export — the memo leaves as what you needed

Nobody wants the audio back. They want what was *in* the audio, in the shape their next tool expects. Export is the fourth step after capture, understand, and triage.

**Tags decide the export shape.** Same logic as yellow/green: one pipeline, different vocabulary. A memo with several tags gets the union of their suggestions.

| Tag | Suggested exports |
|---|---|
| Meeting | Tasks (to Reminders), meeting notes (Markdown), follow-up email |
| Song idea | MIDI, sheet music, trimmed audio stem, lyrics |
| Interview | Key quotes (CSV), quote clips, insight sheet |
| Note to self | Reminder, plain text |
| Any | Markdown, PDF, plain text, trimmed audio |

**The export sheet has three tiers:**

1. **Suggested** — two to four big buttons chosen by tags and content, each with a one-line preview of what you get.
2. **More formats** — the generic ones, always available, never in the way.
3. **"Something else?"** — a chat field. Describe what you need in plain words ("a table I can paste into Notion", "something Ableton can open"). The assistant replies with a concrete format, which appears as a new button. Formats found this way are remembered and suggested next time for that tag. The chat is scoped to shaping exports only, not to answering questions about the memo.

**Export can resolve the memo.** After a successful export, a "Mark as resolved" switch appears. It defaults on when action items were sent to a task tool, off otherwise. For musicians this gives green a concrete meaning: an idea is done when it has left for a project.

*Demo: all Wizard of Oz. Buttons show a preview and a success state. The chat matches keywords to a handful of canned formats. Nothing is written to disk.*

## 6. Search — ask, don't browse

Because every memo has a title, summary, key moments, and pending items, search can work on meaning rather than filenames. The field accepts plain questions: "what did I promise Sam", "that song idea in D minor", "what did Priya say about onboarding", "anything pending from meetings".

- Results explain how the question was understood ("commitments you made · mentioning Sam").
- Each result shows *why* it matched: the action item, the key moment, or the sentence from the summary.
- When the match is a key moment, the result offers a one-tap jump to that spot in the audio.

*Demo: Wizard of Oz. Keyword and pattern matching over the mock enrichment, with a short "listening" delay. Good enough to show the shape.*

## 7. Surfacing — later, not now

The long-term win over the native app is bringing forgotten memos back at a useful moment (digests, similarity links, calendar proximity, staleness nudges). None of that is in the demo. For now, the yellow list *is* the surfacing: anything pending is visible at the top, and that is enough to prove the idea.

## 8. Open questions

1. **Source of memos.** Record in-app so the full capture-to-triage workflow is visible, plus import for existing files. *Demo: recording is Wizard of Oz. The button and timer are real, the audio is not captured.*
2. **How much does the user confirm?** Auto-classification will be wrong sometimes. A light "is this right?" on the first few memos could train per-user preferences and double as onboarding.
3. **Which personas for the demo?** *Lean: musician + meeting dumper.* Same pipeline, visibly different outcomes.
4. **What is "done" for a musician?** Fuzziest green state. Ask the musicians we talked to.
5. **Privacy.** Audio is intimate. Is processing on-device, or cloud with clear consent?
6. **What happens to green memos?** Archive with search, or do they also feed long-term knowledge (e.g. "what did I say about pricing across all meetings")?

## 9. Demo narrative

Keep it bare bones. One screen, one flow.

1. Tap Record, talk for a few seconds, tap Stop. Drop in a few older recordings too.
2. Each one gets a title, suggested tags, a summary, and key moments. The list sorts into yellow and green.
3. Open a long memo. Tap a key moment and jump straight to that spot in the audio.
4. Open a meeting memo. See the action items that made it yellow.
5. Tap Export. Pick Tasks, or type "a table for Notion" and take the format the assistant offers. Leave "Mark as resolved" on.
6. The memo turns green.

That is the whole demo.

## 10. Scope for hackathon

**In:**
- Record button (simulated for the demo, no audio captured)
- Import audio files
- Title + summary + key moments (timestamped, tap to seek)
- AI-suggested tags, multiple per memo, add or remove on any memo
- User-defined tags, each a sidebar filter
- Action item / revisit extraction → yellow/green state
- Single list view, yellow on top
- Mark items done → memo turns green
- Export sheet: suggested formats by tag, more formats, and a chat to find a format (simulated)
- Natural-language search with "why it matched" and jump-to-moment (simulated)

**Out (for now):**
- Full transcript view (the audio stays the source; we surface title, summary, and key moments instead)
- Digests, nudges, or any notification
- Similarity / idea linking
- Real audio capture and real AI processing (both faked in the demo)
- Calendar / location integration
- Multi-user / sharing
- Mobile app (web is fine)

## 11. Success criteria

- A new user can look at the sorted list and know what each memo is without playing it.
- For a long memo, the user can jump to the important part without scrubbing.
- A user can find a memo by asking a question, without remembering when they recorded it.
- Every memo with a commitment in it is yellow. Nothing with a commitment is green.
- Someone who watched the demo says: "I would dump my memos into this."
