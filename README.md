# Humanizer DE

Humanizer DE makes AI-written German text sound like a person wrote it, without changing what it says. It is a German adaptation of [Humanizer](https://github.com/blader/humanizer) by blader, which is built on Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing). It works in Claude Code, Claude.ai, Codex, and any other agent that supports skills.

The instructions are in English. The watch lists and before/after examples are in German, because they describe German text. The rewrite is always German.

**Before:**
> Wir freuen uns sehr, euch mitteilen zu können, dass die neue Exportfunktion endlich da ist! 🚀 Monatelang hat unser Team mit Excel-Listen gekämpft — und wir wussten, dass es einen besseren Weg geben muss. Es geht nicht nur um ein neues Feature, sondern um eine ganz neue Art der Zusammenarbeit. Darüber hinaus lassen sich Berichte jetzt nahtlos als PDF teilen. Und das Beste daran? Die Funktion ist ab sofort in allen Tarifen kostenlos verfügbar. Lass das mal sacken.

**After:**
> Die neue Exportfunktion ist da. Monatelang hat unser Team mit Excel-Listen gearbeitet, jetzt lassen sich Berichte direkt als PDF teilen. Die Funktion ist ab sofort in allen Tarifen kostenlos.

Humanizer DE edits for human readers. Getting past AI detectors is not a goal.

## Why a separate German skill

The structural tells are the same in both languages. The rules that fire most often are not:

- **Dashes.** German typography uses the spaced en dash („ – “), so it is not a tell by itself. The English em dash („—“), spaced or not, is a strong one in German text.
- **English showing through.** Calques such as „am Ende des Tages“ or „einen Unterschied machen“, English quotation marks, title case in headings, split compounds („Anlagen Buchhaltung“), and English number formats.
- **German sentence habits.** Connector chains („Darüber hinaus … Zudem … Abschließend“), nominal style („die Durchführung der Prüfung erfolgt“), and German stock words.
- **Rules that do not apply.** The English hyphenation rule for compound modifiers has no German counterpart and is dropped.

For English text, use the original [Humanizer](https://github.com/blader/humanizer).

## The five strongest tells

Humanizer DE rewrites any of these on sight:

1. **Not X but Y:** „Es geht nicht nur um ein Feature, sondern um eine neue Art der Zusammenarbeit.“
2. **One-line closers:** „Genau das ist der Punkt.“ „Lass das mal sacken.“
3. **Sayings that sound deep:** „Im Kern geht es um …“
4. **A staged run-up:** „Es ist wichtig zu beachten, dass …“ „Ehrlich gesagt?“
5. **Arguing with no one:** „Versteh mich nicht falsch, aber …“

Humanizer DE checks for [28 patterns](#the-28-patterns) in all.

## Installation

Once installed, the skill answers to `/humanizer-de`.

### Claude Code

```text
/plugin marketplace add UweBear/humanizer-de
/plugin install humanizer-de@humanizer-de
```

The plugin answers to `/humanizer-de:humanizer-de`. Like the original, it needs Claude Code 2.1.142 or newer. You can also use the Skills CLI: `npx skills add UweBear/humanizer-de --global --agent claude-code`.

### Codex

```bash
npx skills add UweBear/humanizer-de --global --agent codex
```

### Claude.ai and Claude Desktop

Download this repository as a ZIP (**Code → Download ZIP**) and upload it as a skill in Settings.

### Other agents

```bash
npx skills add UweBear/humanizer-de --global --agent '*'
```

Leave off `--global` in any command above to install it only in the current project. For an agent the Skills CLI does not know, copy `SKILL.md` into its skill folder. It carries the license text.

Humanizer DE and Humanizer can be installed side by side. The description of Humanizer DE names German text, so an agent can tell them apart. To be sure, call the skill by name.

## Usage

Call the skill directly:

```
/humanizer-de

[paste your German text here]
```

Or ask in plain language:

```
Überarbeite diesen Text, damit er nicht nach KI klingt: [Text]
```

To rewrite a file, give Humanizer DE its path:

```
Humanize the German prose in docs/release-notes.md
```

### Match your voice

To make the rewrite sound more like you, include a sample:

```
/humanizer-de

Hier ist eine Probe meines Schreibstils:
[2–3 Absätze eigener Text]

Überarbeite jetzt diesen Text:
[KI-Text]
```

Humanizer DE follows the sample's rhythm, word choice, punctuation, and deliberate quirks, including dashes if you use them.

## How it works

A language model writes whatever is most likely to come next, so by default it makes the choice that fits the widest range of readers and subjects. A person chooses for one reader and one subject. Every tell Humanizer DE looks for is a form of that default choice: a sentence that signals importance instead of adding a fact, rhythm or formatting applied by rule, an ordinary fact dressed as a pivotal one, English habits carried into German, text left over from the chat, or a reply that re-explains what the reader already knows.

Humanizer DE marks every tell it finds, strongest first. It drafts a rewrite, checks the draft against the patterns and the original claims, and then writes the final version. It does not make things up. A name, number, date, quote, citation, or other factual detail must come from the source or the writer. If a text has few tells, it says so and changes little.

When you paste text, Humanizer DE shows the first rewrite, a short list of anything that still sounds artificial, and the final version. Point it at a file and it changes only the prose, leaving code, data, frontmatter, and link targets alone. It keeps the writer's conventions: *du* or *Sie*, gender-inclusive spelling, spelling variants, technical terms, and nominal style in legal and accounting text.

## The 28 patterns

The patterns are numbered by strength and frequency. The first five justify an edit on a single sighting. Patterns marked *weak alone* count only when several tells share a passage, because a careful writer may use any one of them on purpose.

### A. Staging instead of stating

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Not X but Y** | „nicht nur X, sondern auch Y“, „Das heißt nicht X. Es heißt Y.“ | State the point directly |
| 2 | **One-line closers and dramatic fragments** | „Genau das ist der Punkt.“; „Keine Kompromisse. Keine Ausreden.“ | Cut the closer that repeats; merge fragments into a specific claim |
| 3 | **Sayings that sound deep** | „Im Kern geht es um …“, „Daten sind die Währung des Vertrauens“ | Replace the saying with the specific claim |
| 4 | **Staged run-up before the point** | „Lass uns eintauchen“, „Es ist wichtig zu beachten, dass“ | Remove the run-up and state the point |
| 5 | **Arguing with no one** | „Versteh mich nicht falsch“, „Naheliegend wäre …“ | Remove the unraised objection or fake option; keep any real claim |

### B. Rhythm by rule

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 6 | **Forced triads** | „Effizienz, Transparenz und Sicherheit“ | Use the number of items the meaning needs |
| 7 | **Monotonous sentences** | „Das System … Das System … Das System …“ | Merge sentences, change the subject, or reorder |
| 8 | **Connectors at the start of every sentence** (*weak alone*) | „Darüber hinaus … Zudem … Abschließend …“ | Keep only connectors that name a real relation |
| 9 | **Dashes as the universal connector** | „Die Regelung — ohne Vorwarnung — betrifft …“ | Replace em dashes; keep spaced en dashes rare or at the writer's rate |
| 10 | **Stacked qualifiers** (*weak alone*) | „könnte unter Umständen möglicherweise“ | Keep only qualifiers the source supports |
| 11 | **Nominal style and light-verb constructions** (*weak alone*) | „Die Durchführung der Prüfung erfolgt …“ | Use the full verb, except in legal and accounting text |
| 12 | **Impersonal sentences and missing actors** (*weak alone*) | „Keine Konfigurationsdatei erforderlich.“ | Name the actor when that helps |

### C. Inflation and borrowed authority

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 13 | **Overused AI words** | „ganzheitlich … nahtlos … integraler Bestandteil … Mehrwert“ | Use plain words |
| 14 | **Inflated significance** | „markierte einen Meilenstein“, „Die Zukunft sieht vielversprechend aus“ | Keep the fact and drop the significance |
| 15 | **Vague connection or association** | „stand in Verbindung mit der Geschäftsführung“ | State the relationship the source gives |
| 16 | **Shallow trailing clauses** | „, was die Bedeutung von … unterstreicht“ | Keep only what the source supports |
| 17 | **Sales language** | „eingebettet in die malerische Landschaft“ | State what the thing is |
| 18 | **Borrowed authority** | „Experten sind sich einig“, „Studien zeigen“ | Name a real source and what it said, or remove the claim |
| 19 | **Avoiding ist, sind, and hat** | „fungiert als … verfügt über … weist … auf“ | „ist … hat“ |

### D. Formatting by rule

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 20 | **Bold as decoration** | „**Performance:** Die Performance wurde gesteigert“ | Remove the bold; turn a labeled list into prose |
| 21 | **Decorative headings** | „🚀 Startphase:“, „Die Entscheidung auf einen Blick“ | Remove emojis and arrows; name what the section holds |

### E. English showing through

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 22 | **Calques and anglicisms** (*weak alone*) | „am Ende des Tages“, „einen Unterschied machen“, „X ist der Schlüssel“ | Use the ordinary German phrase |
| 23 | **English typography and spelling** (*weak alone*) | “Anlagen Buchhaltung”, „Die Wichtigsten Punkte“, 1,250.00 € | „Anlagenbuchhaltung“, „Die wichtigsten Punkte“, 1.250,00 € |

### F. Leftovers from the chat and the draft

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 24 | **Chatbot residue** | „Gute Frage! … Ich hoffe, das hilft!“ | Remove the wrapper and keep the content |
| 25 | **Knowledge-limit disclaimers and guesses** | „Stand meines Wissens …, vermutlich …“ | State what the source shows, or remove the sentence |
| 26 | **A heading repeated in the first sentence** | „## Performance“ + „Geschwindigkeit ist wichtig.“ | Let the heading do the work |
| 27 | **Writing about the document instead of its subject** | „Die folgende Tabelle vergleicht …“, „auf Basis von … erstellt“ | Describe the subject |

### G. Writing for the wrong reader

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 28 | **Re-explaining what the reader knows** | A reply that walks the diagnosis before the decision | Lead with the decision; keep only evidence the reader needs to check it |

## Limits

The English original rests on a well-maintained Wikipedia guide. We are not aware of a German source of that quality. The German additions, especially the word list in §13, come from a t3n article, an LMU Munich checklist for assessing student papers, and observation. Treat them as signals to look closer, not as proof. Reports of tells that are missing or wrong are welcome as issues.

## Sources

- [blader/humanizer](https://github.com/blader/humanizer) is the original skill this one adapts.
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup), is the source of the original pattern list.
- [t3n: KI-Texte erkennen](https://t3n.de/news/ki-texte-erkennen-floskeln-buzzwords-gedankenstriche-chatgpt-1745388/) and the [LMU Munich AI checklist (PDF)](https://www.geschichte.uni-muenchen.de/download/ki/operationalisierungsbogen_ki.pdf) informed the German additions.

## Version history

See [CHANGELOG.md](CHANGELOG.md).

## License

MIT. See [LICENSE](LICENSE). Based on Humanizer, Copyright (c) 2025 Siqi Chen.
