# STORY_BIBLE

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

> Spoiler document. Contains the hidden truth. Content notes: institutional negligence, fatal fire, guilt, grief. No gore; no depiction of deaths.

## 1. Premise (what the player sees)

You arrive at **the Concord**, a municipal Hall of Records and Testimony. A courteous voice, **the Clerk**, processes your *intake*. You are told you are here to "correct an entry". You must pass through the building's departments to reach the **Verdict Hall**.

## 2. The hidden truth (what happened)

| Item | Fact |
|---|---|
| The Witness | A records clerk at the real Concord Hall of Records. Unnamed in-game. Internal name: **Wren Calloway**. |
| The event | A fire at **Alder Court**, a tenement block. Seven households. |
| The record | **Record 4471** — *Notice of Evacuation, Alder Court.* |
| The lie | 4471 was stamped **SERVED**. It was never delivered. |
| Why | Supervisor **Mr. Pell** told the Witness to "clear the backlog by Friday". The Witness stamped it served, believing the building was safe. |
| Aftermath | The fire occurred that weekend. Seven households. The Witness signed a statement: **"Every notice was served."** |
| The Concord in-game | An interior reconstruction — ambiguous (see §7) — in which the building *protects the record* by managing what the Witness can believe. |
| The Clerk | The *voice of the record*: courteous, procedural, protective. Revealed as the Witness's own rehearsed official voice. |

### The Clerk's core directive

> "The record must remain consistent."

The Clerk will lie, omit and reinterpret to keep the record consistent with the original false statement, but it **cannot alter the literal text of what it said**. This constraint is the fair-play backbone of the game (reinterpretation must be consistent with literal text).

## 3. Cast

| Character | Source ID | Role | Reliability profile |
|---|---|---|---|
| **The Clerk** | `SRC_CLERK_*` | Guide/antagonist. Courteous, formal, never contracts "do not" to "don't" when lying (voice tell, partial) | ~55% literally true; 25% technically true/incomplete; 20% false |
| **Previous Witness (W.)** | `SRC_MARGIN` | A prior visitor whose marginalia litters the building | Honest but *wrong about frames* (they saw a different Concord state); ~65% accurate |
| **The Dispatcher** | `SRC_DISPATCH` | Radio voice; claims to be an outside operator | Truthful in the *operator frame*; seems to contradict Clerk |
| **Echo of Mr. Pell** | `SRC_ECHO_PELL` | Supervisor remembered | Self-serving; lies to protect himself; tells consistent |
| **Echo of Mrs. Haddad** | `SRC_ECHO_HADDAD` | Alder Court neighbour | Honest but mistaken about times |
| **Echo of the Courier** | `SRC_ECHO_COURIER` | Delivered notices | Accurate on routes; omits his own shortcut |
| **The Witness (you)** | — | Silent protagonist | — |

**Echoes** are faceless, tailored figures (paper-white blank faces, slight heat shimmer) — memories, not ghosts.

## 4. The seven households (Returned Notices)

Adeyemi · Castellano · Nowak · Haddad · Ito · Brannigan · Pereira.

Seven envelopes hidden across levels bear these names. Collecting all seven unlocks the secret ending.

## 5. Act beats

| Beat | Act/Level | Content |
|---|---|---|
| Setup | L01 | Intake. The Clerk is fully truthful. Plaque: *NOTICES SERVED WITHOUT FAIL SINCE 1962.* Ticket number **4471** (unremarked). |
| First anomaly | L01 end | The wall clock hand steps backwards once. No one mentions it. |
| Growing uncertainty | L02–L04 | Omission, sound mismatch, conflicting notes. |
| Discovery | L05–L09 | The Concord has levels the Clerk denies; mirrors show the truer state. |
| Escalation | L10–L14 | The building starts quoting your habits; testimonies conflict; panels lie. |
| Major reveal | L15–L16 | Hearing Room: Record 4471 deferred. The reel reveals the unserved notice. |
| False resolution | L17 | The Clerk says "You are free to go." You walk out. Credits roll. |
| Final revelation | L18–L19 | The Vault, the Reading Room, the dictaphone with your own voice. |
| Verdict | L20 | Choose: Amend / Affirm / Expunge (+ Corroborate). |

## 6. Endings

| Ending | Condition | Outcome |
|---|---|---|
| **Amend** | Choose "Amend" and have ≥ 1 supported exhibit for 4471 | You restore the entry: *NOT SERVED.* The building opens; the Clerk falls silent, then speaks one last true statement. Quiet, hard, honest. |
| **Affirm** | Choose "Affirm" | You re-sign. The Clerk thanks you. The Intake Hall resets with subtle differences (NG+ hook). Horrifying warmth. |
| **Expunge** | Choose "Expunge" | You burn the record and the building. Ambiguous: truth and lie both destroyed. |
| **Corroborate** (secret) | All 7 Notices + Amend | Names are read aloud (Witness's own voice). No Clerk. Best-earned ending. |

## 7. Interpretations (never confirmed)

1. **Reconstruction of conscience** — the Concord is the Witness's mind.
2. **Literal tribunal** — an automated inquest; the Clerk is a system.
3. **Purgatorial audit** — the Witness is dead or dying.
4. **Meta-reading** — the *player* has been complicit by trusting a polite voice. (Supported by "Your File" in L19.)

Clues support all four. No clue is exclusive to a single reading.

## 8. The title as a statement

`STM-TITLE-001: "Everything is a lie."` — **CONTEXTUAL / DECEPTIVE.** It is the Clerk's thesis: if nothing is true, no one is responsible. The final act disproves it with true things: seven names, one stamp, one fire.

## 9. Voice and writing rules for the Clerk

- Formal, warm, procedural. Prefers "kindly", "as far as the record shows", "I assure you".
- Never swears, never threatens, never shouts.
- Every statement must parse literally for fairness (see §2 directive).
- Statements are short (≤ 18 words typical). No monologues > 40 words.
- **Tells (partial, not absolute):** "As far as the record shows" → incomplete/technically true (~75%). "I assure you" → deceptive (~60%). Formal "do not" → more likely deceptive than "don't" (~55%). "The record states" → literal quote of a document (true about the document, not the world).
- Truth-when-lie-expected beats: L03 (door C), L09 ending, L17 ("This is not the end."), L19 ("I did not lie about the names.").

## 10. Themes

Responsibility, institutional comfort, the politeness of harm, memory as editing, trust as complicity.

## 11. Acceptance criteria

- Every deception in LEVEL_DESIGN maps to an authored line consistent with the Clerk's literal-text constraint.
- No scene presents the fire graphically.
