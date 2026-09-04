# Changelog

All notable changes to BioCode Lab are documented here.

## v1.0.1 — 2026-09-03

### Release packaging
- Renamed the public application to `index.html` so GitHub Pages opens it
  automatically.
- Kept the complete learning application in one HTML file. The optional
  terminal command PDF and its download controls were removed; the bilingual
  essential-command reference remains inside the terminal.
- Standardized the public package on pinned CDN dependencies. The interface
  and documentation now state clearly that internet access is required.
- Added social metadata, accessible names for the alias and terminal command
  fields, and GitHub Pages publishing instructions.

### Fixed
- **The commented guidance inside the editor never changed language.** The
  starter code is written in both languages, but it is copied into the editor
  only once -- the first time the student opens the challenge -- so it froze in
  whichever language was active at that moment. Someone who began in Spanish
  and switched to English kept reading Spanish comments in every exercise they
  had already opened. It is now retranslated on every language switch, but
  only where the code is still character-for-character identical to the other
  language's starter: anything the student has typed, even a single line added
  under the guidance, is left untouched.
- **Mission 1 never explained print().** Its topic line promised
  "print · variables · strings · input · int", the briefing covered variables,
  strings and input/int — and print, the one thing the very first challenge
  asks the student to do, appeared nowhere. Not in the briefing, not in a
  single one of the four quick examples. It existed only inside a commented
  example in the editor, so the student was expected to infer it. It is now
  the FIRST thing the mission explains, because it is what makes everything
  else visible, and it shows up in the examples both on its own and applied
  to a variable. Auditing every mission for concepts promised in the topic
  line but never taught turned up one more of the same kind: Mission 12
  listed history and never said what it does.
- **"Reset progress" confirmed twice and then reset nothing.** The function
  removed the saved key and called `location.reload()` — but reloading fires
  `beforeunload` and `pagehide`, and both handlers call `flushSave()`, which
  wrote the still-populated in-memory state straight back into `localStorage`.
  The page came back with every mission, badge and XP intact. A lock is now
  set *before* clearing, so once a reset is confirmed nothing can resurrect
  the progress: no pending `saveSoon`, no unload handler, no later `save()`.
  The language preference deliberately survives — that is a setting, not
  progress.
- **Finishing a mission did not unlock the next one.** The student completed
  Mission 1's Boss, saw "mission complete", and Mission 2 stayed locked —
  the kind of thing that makes someone close the app for good. It fixed itself
  on switching learning path or reloading, because both go through `paths.js`,
  which recalculates `S.unlocked`. The cause was this project's recurring
  cross-scope bug for the fourth time: `completeMission()` lives in the base
  layer and `storyUnlockedFromDone()` inside the expansion IIFE, and the call
  was written as `typeof storyUnlockedFromDone === "function" ? …() :
  S.unlocked` — so it never threw, it silently took the fallback and left
  `S.unlocked` pinned at 1 forever. The unlock is now recalculated from the
  expansion, where the function is actually visible.
- **The scope guard that exists to catch exactly that bug was blind to most of
  the base layer.** `tools/check-scopes.mjs` strips comments and strings before
  looking for impossible calls, but it did not know about regular-expression
  literals: on reaching `/[&<>"']/g` in `state.js` — the fifth line of the base
  layer — it entered a string state it never recovered from and blanked the
  fourteen files that follow. It reported 25 base functions; it now sees 119.
  With that fixed it immediately found **two** real impossible calls, not one:
  the unlock above, and `missionAccessible` inside `goNextMission`, which also
  stopped the "Next mission" button from moving past the story-unlocked point
  in Free Explore.
- **Part I of the map could not be collapsed.** Clicking it did nothing while
  the other two folded fine. The open-parts state used an empty array to mean
  *"the student has not decided anything, open the current Part automatically"*
  — so folding the only open Part emptied the list and the auto-open reopened
  it in the same render. Part I was the visible victim because it is the one
  that opens by itself at the start. An empty list is now a real decision
  ("all folded"); *no decision* is null.
- **The "Your progress" card spilled its text outside the border.** `.card`
  carries no padding of its own — each variant sets it, like `.mission` does —
  and the card had none, so the text sat flush against the edge and the
  percentage clipped. It was also a single item in the map's two-column grid,
  so on a desktop it only covered half the width.
- **The Part header count overlapped its own title on a phone.** The counter
  and chevron are absolutely positioned on the right with no space reserved
  for them, so at 360 px they printed over the heading
  ("FUNDAMENTOS DE PYTHON0 / 9").
- **The "Skip to content" link had no CSS and was visible on every screen.** It
  was added for keyboard accessibility in Phase 5A but never styled, so it sat
  as a loose blue link above the logo in all views. It is now hidden until it
  receives keyboard focus, which is the only moment it is meant to exist.
- **The bottom tab bar broke into two rows when a fourth tab was added.** Its
  grid was hard-coded to `repeat(3,1fr)`; it now sizes its columns
  automatically, so adding another destination cannot wrap it over the content.
- **The answers you had to type were in English inside a Spanish course.** 38
  required outputs were English and 4 were Spanish; within Mission 5 alone the
  student typed `Identical` and dragged `Mutacion detectada`. Prose invented by
  the course now follows the interface language, in both the expected output
  and the emergency solution — and the Spanish solutions are *derived* from the
  English ones rather than written by hand, so a future edit cannot leave them
  out of sync. Output that mirrors real tooling (`Usage:`, `File not found`,
  `EcoRI: YES`) stays English on purpose: that is what the student will meet in
  their career. `check-starters.py` now executes **both language branches** of
  every bilingual challenge, Parsons and variant against its own expected
  output — 18 branches that previously nobody verified.
- **Mission 5's position-by-position comparison demanded 44 lines of
  copy-paste.** It asked for six positions with `for`/`while` explicitly
  forbidden, which is pedagogically deliberate — it creates the need that
  Mission 6 answers — but it was the single most tedious moment in the course
  and a likely place to quit, especially on a phone. Three positions teach the
  same lesson; the exercise now says so out loud.
- **The very first two challenges rejected a correct answer over the variable
  name.** `m1c1` and `m1c2` demanded the exact literal name suggested in the
  instructions (`sample`, `dna`) via a regex, so a student who created and
  printed the right value under any other name was told they were wrong.
  Both now accept any variable name, as long as the value is actually stored
  in one (not just hard-printed) and the output is exact.
- **La reflexión ya no bloquea el avance.** Escribirla era obligatoria para
  poder seguir, y eso volvía a chocarle al mismo usuario que la pidió. Ahora
  el reto se resuelve en cuanto la salida es correcta, se haya escrito la
  reflexión o no; si se escribe, sigue guardándose como evidencia y sigue
  desbloqueando la respuesta del experto. Como consecuencia directa, el
  diagnóstico del tutor ya no queda tapado por el aviso de "escribe la
  reflexión para avanzar": aparece siempre que la salida está mal.
- **`check-starters.py` nunca había validado `missions-extra.js`.** El
  validador de CPython real solo cargaba `missions-python.js` y
  `missions-bioinfo.js`; los 13 retos de la fase anterior (que se insertan a
  mitad de una misión existente en vez de declararse en un arreglo propio)
  pasaban sin que ningún compilador de Python real los tocara. Ahora se
  cargan igual que la app los carga, y dos retos con archivo (`m14q1`,
  `m16p1`) necesitaron que el validador supiera escribir `c.files` en un
  directorio temporal antes de ejecutar.

### Added
- **A Welcome view, so the app introduces itself.** A student opening BioCode
  Lab landed straight on a map of 17 locked missions, with nothing explaining
  what this is, what they would learn, or who made it. A new *Home* tab — first
  in the bar, and the default landing for anyone who has not started yet —
  opens with "You are going to learn to program", the three Parts of the course
  with their own marks (a Python logo drawn as inline SVG, a terminal, a DNA
  helix, all embedded so they work offline), four reasons this is not a video
  course, and the author credit with the licence. Returning students still land
  on the map, which is what they expect.
- **The release stays genuinely single-file.** Pyodide and QRCode.js use
  pinned CDN versions, while the logo, interface, missions and terminal remain
  embedded in `index.html`. This distribution requires internet access, which
  is stated consistently in the app and README.
- **A safety net for the terminal missions.** Missions 10-12 were the only
  stretch of the course with no way out: 16 tasks, zero stored solutions, and
  the scaffolding that carries the student through nine missions vanishing
  exactly where the mental context switches from Python to a shell. Each task
  now has a solution in both Linux and PowerShell form, behind the same gate
  as everywhere else (3 attempts, all hints spent). The smoke-test assertion
  meant to catch this had been **skipping terminal missions on purpose**,
  which is why nobody noticed; it no longer skips them.
- **Course-level progress and a time estimate.** The map showed "3/6" inside a
  mission and nothing at all about the course, so a student started thinking
  it was an afternoon's work and met 126 exercises. The map now shows missions
  done, percent complete and remaining time — computed from the student's own
  measured pace once there is enough of it to average.
- **A pilot variant bank, so resetting progress doesn't replay the exact same
  exercises.** Four challenges (`m5c1`, `m5c2`, `m6c1`, `m6c2`) now each carry
  3 interchangeable literal sequences, picked by a seed generated once per
  install and stored in `localStorage` — not by the student's alias, which is
  what the existing Boss variant system already uses, and which means a
  student who resets and retypes their own name gets the identical Boss every
  time. `resetProgress()` wipes the seed along with everything else, so a
  fresh start now genuinely can look different. `tools/check-starters.py`
  gained a matching validation pass: all 12 (4 challenges × 3 variants)
  compile and their known solution produces the exact declared output,
  checked against real CPython on every `npm test`.
- **Bosses for Missions 1, 2 and 13 stopped being a smaller copy of a
  challenge already solved minutes earlier.** Playing Mission 1 start to
  finish, its Boss ("store the sequence, print it") was mechanically
  identical to `m1c2`; Mission 2's Boss was the same shape as `m2c3`. From
  Mission 3 onward every Boss already combines 2-3 concepts from its mission;
  these three didn't. Mission 1's Boss now also requires the repetition
  operator (print the sequence doubled); Mission 2's is a four-line report
  combining `len()`, indexing and reversal; Mission 13's adds a fifth line
  using `Counter.most_common(1)`. A new generic test type
  (`regexTokensOutput`) was added to the base validator for the first case.
  Bosses have no automated test coverage in this repo, so all three were
  hand-verified against real Python before shipping.
- **The shared achievement card said "2 badges" without saying which ones.**
  The in-app profile already listed badge names and icons; the downloadable
  PNG card only ever drew the count. It now draws the actual earned badge
  icons next to that number.
- **Two "dirty data" challenges for `translate()`.** Every sequence that ever
  reached Biopython's `translate()` in the whole course was uppercase, free of
  `N`, and a clean multiple of three — the one Biopython method most exposed to
  real, messy sequencing data had never been tested against any of that. Mission
  16 now has `ATGNCCATTGTA` (an ambiguous base becomes `X`, not a crash) and an
  11-base sequence that is one short of a full codon: `translate()` neither
  crashes nor errors on it, it silently drops the leftover base and warns
  through `stderr`, which never reaches the app's output panel — so the
  challenge asks the student to trim to a multiple of three themselves instead
  of trusting a warning they will never see. (Lowercase input was already
  covered, by `m17d1`'s GC% bug — `translate()` itself normalises case on its
  own, so there was nothing new to teach there.)
- **A tutor that diagnoses what does not crash.** The app taught the silent
  error in mission 8 and did not practise it in its own tutor: it read tracebacks
  well and answered a wrong *result* with "compare your output with the requested
  result" — which is exactly what the student is already doing when they read
  that message. Playing through the course as a beginner also turned up two rules
  that could never fire: `code.includes("int(")` is true for any code containing
  `print(`, so the lesson of mission 1 had been unreachable since it was written,
  and the slice rule recognised `dna[:2]` but not `dna[0:2]`. A new engine now
  compares the output with the expected one and names the signature of the error,
  for all 91 challenges rather than per id: a 0 that never crashed (a search that
  found nothing, almost always letter case), a result exactly 100 times smaller,
  one character missing from a slice, an answer reversed, more lines than asked
  for (the print stayed inside the loop), None (the function never returned),
  nothing printed, a number that counts too much or too little — and failing all
  that, the exact line where the two outputs separate.
- **13 new challenges, and a rhythm that stops being predictable.** Missions 2 to
  8 all had the same bar — four write, one repair, one order — and Part 3 was 22
  challenges of which 22 were writing, with the scaffolding gone exactly where
  the material gets hard. No mission now repeats another's bar, and all five
  missions of Part 3 have something that is not typing. Debugging went 9 → 16,
  Parsons 7 → 12, prediction 3 → 6. Almost every new one is the error that does
  *not* crash: the parser that counts the header as DNA (the C of "synthetic"
  counted as a cytosine), the script that always prints its own name instead of
  the requested file, Counter counting the bag instead of the letters, the elif
  chain that answers the wrong question because of its order, `b = a` that does
  not copy the list, and a field file with half the sequence in lowercase that
  reports a perfectly believable, perfectly wrong 21.4% GC.
- **`help` in the terminal, in the student's language.** The Part 2 briefing
  promises "learn to find your way and to ask for help"; typing `help` — or
  `ayuda`, which is what someone using the whole app in Spanish types — answered
  "Command not recognized". Every terminal message was in English inside an
  otherwise Spanish app, precisely the messages a stuck beginner has to read. A
  mistyped command now suggests the closest one instead of only refusing.

### Changed
- **The expected output is no longer given away.** It was shown on 92% of
  challenges from the first second which, together with the commented guidance in
  the editor, let many be solved by copying the pattern above and matching the
  result below. It now appears the moment you run — which is when it is useful
  for comparing — and invites a prediction before that, with a button to reveal
  it anyway.
- **The twin exercises are gone.** "Count the G" followed by "count the C" was
  the same exercise with one letter changed. That slot is now the zero that never
  crashes.
- **An escape hatch, with 78 verified solutions.** A stuck student had two ways
  out: abandon the mission, or paste an answer from an AI without understanding
  it. Now there is a third. The gate opens late on purpose — three attempts, all
  hints spent, the challenge still unsolved, and never in exam mode — and it does
  not fill the editor: it shows the code to be read and compared, because a
  solution you did not type teaches nothing. Every reveal is counted in the Lab
  Log, so the teacher can see *where* a group gets stuck, which a Word handout
  never told them. The 71 written solutions use nothing the student has not met
  yet at that point in the course; `npm run solutions` types each one into the
  real editor and presses Check against real Pyodide. All 78 (71 plus the 7
  Parsons) pass — the first evidence that every challenge is solvable at all.
- **The reflection questions now answer back.** Fifteen challenges asked the
  student to explain why something works, and nothing ever replied. Writing into
  a void teaches you to fill the box with anything to unlock the button. Each
  question now has an expert answer that appears *only* after the student wrote
  theirs and solved the challenge — in that order, or the question stops being a
  question. Nothing is marked right or wrong; the two sit side by side to be
  compared.

### Changed
- **Visual identity: the "Chromatogram" direction.** The palette is now built on
  the four Sanger base colours (A green, C blue, G amber, T red) that students
  will meet for the rest of their careers. Those four are reserved for *data*;
  actions use a mint that is deliberately none of them, so a button can never be
  mistaken for a nucleotide. 243 hard-coded colours were moved family by family,
  preserving the light/dark relationships that already worked, and 114 surface
  and border literals became tokens so contrast can be tuned in one place.
- **Real typography, embedded.** The CSS asked for Inter and never loaded it, so
  every heading fell back to the system font. Two faces are now embedded as
  base64 (51 KB total): Bricolage Grotesque, instanced to one weight and subset
  to Latin-1, for headings; DM Sans variable for text. The app opens from disk,
  so fonts cannot depend on a connection. 94 loose sizes between 9 and 45 px were
  snapped onto a nine-step scale.
- **Buttons and cards.** Pill buttons in the action mint; card radii now vary by
  role instead of one radius stamped on everything.

### Added
- **An interaction layer, which did not exist.** The stylesheet previously had
  zero `:hover`, zero `:focus`, zero `transition` and zero `@keyframes` rules —
  that, not the palette, was why the interface felt dead. Buttons and cards now
  respond, keyboard focus is visible for the first time, XP animates to its new
  value, and correct/incorrect answers get distinct motion. All of it respects
  `prefers-reduced-motion`.

### Fixed
- **Part headings showed the same sentence twice.** The template rendered
  `p.text.split(".")[0]` as the heading and `p.text` as the description, so with
  single-sentence descriptions both lines were identical. Each Part now has its
  own headline.
- **Mobile keyboards no longer break code.** Every field where a student types
  code, commands or sequences now carries `autocapitalize="off"`,
  `autocorrect="off"`, `autocomplete="off"` and `spellcheck="false"`.
  Previously iOS and most Android keyboards capitalised the first word of each
  line, turning `print(dna)` into `Print(dna)` and producing a `NameError` the
  student never made. Prose fields (reflections, alias, share caption) keep
  autocorrection, where it helps.
- **Infinite loops no longer freeze the tab.** Student code now runs under a
  1-second guard (`sys.settrace`, limited to the learner's own frames so
  Biopython and SeqIO stay fast). On timeout the output panel keeps whatever
  was printed and appends an explanation pointing at the unchanged loop
  variable — the exact mistake Mission 6 warns about.
- **A failed Python download is now visible.** `initPy()` used to swallow the
  error, leaving "Loading Python engine..." on screen forever. The brief now
  shows an explicit error state with a Retry button.
- **Level names displayed correctly.** `LEVELS` entries are
  `[xpThreshold, nameEN, nameES]`, but the code indexed them with `idxLang()`,
  so English showed the XP number ("0" instead of "Genome Rookie") and Spanish
  showed the English name. Affected the profile, the level-up notice, the
  achievement card and the PDF evidence.

- **The achievement card logo was broken by the asset change.**
  `achievementCanvas()` cropped the logo with fixed coordinates from the
  original 1536x1024 PNG. After the asset was replaced with a screen-sized one,
  those coordinates fell outside the image and every shared card showed a
  fragment in the corner of a white rectangle. It now fits the image using its
  natural dimensions, so a future asset change cannot break it again.

### Added
- **Reset progress.** A confirmed two-step button in the profile card, which
  suggests exporting a `.biocode` copy first. Previously there was no way to
  start over short of clearing browser data — a problem on shared lab computers.
- **Ctrl+Enter / Cmd+Enter** checks the current challenge from the editor.
- **Esc** leaves the code editor. `Tab` inserts indentation, which trapped
  keyboard and screen-reader users inside the textarea.

### Changed
- **The single HTML file went from 3.11 MB to 0.38 MB.** The logo was embedded
  twice as byte-identical 1.07 MB copies of a 1536x1024 PNG, shown at 42 px and
  220 px. It is now two right-sized assets: a square icon for the top bar (which
  previously cropped the full lockup into an unreadable square) and a 460 px
  lockup for the hero. Both sit on a deliberate light plaque instead of a bare
  white rectangle.
- **Typing no longer writes to `localStorage` on every keystroke.** Writes are
  batched and flushed on page hide, unload and tab switch.

### Internal
- **CSS consolidated.** `05-readability.css` was re-declaring ~65 selectors
  already defined in `01-core`/`02-terminal`/etc., so every size existed in two
  places. Those 62 rules were merged into their original definitions; each
  selector is now declared once. Verified pixel-identical across 20 views.
  This unblocks designing a real typographic scale.
- **The seven `patches.js` overrides were kept, deliberately.** On inspection
  they are clean "add new cases, then delegate" wrappers, not tangled code.
  Merging them would rewrite ~400 dense lines for no user benefit and real
  regression risk. Documented in `src/README.md` instead, with a rule for
  where new code belongs.
- **Screenshot regression harness.** `npm run shots` captures 20 views
  (4 widths x 5 screens) using the installed Chrome; `npm run compare` diffs two
  sets pixel by pixel.
- The application is now built from `src/` by `build.mjs` (no dependencies)
  instead of being edited as one 1,643-line file. The split was verified to
  reproduce the original byte for byte before any change was made.
- `npm test` runs a 33-check smoke test against the built HTML in a real DOM.
- `tools/timeout-guard-test.py` verifies the infinite-loop guard in CPython.

## v1.0 — 2026-08-31

### First full release
- Promoted BioCode Lab to its first full public release: **v1.0**.
- Consolidated the complete 17-mission learning pathway.
- Embedded the BioCode Lab logo directly inside the HTML.
- Reduced the displayed hero logo size for a more compact interface.
- Added visible author attribution: **BioCode Lab by Yair Cardenas Conejo**.
- Added a footer information ribbon with version, educational purpose, technologies and MIT license.
- Preserved the single-file core application model.

### Python Foundations
- Variables, strings, numeric conversion and `input()`.
- Sequence indexing and slicing.
- Nucleotide counts and GC%.
- Conditional logic.
- Sequence comparison and normalization.
- `for`, `while` and `continue`.
- Lists and dictionaries.
- Functions, parameters and `return`.
- Integrated DNA Analyzer project.

### Terminal Foundations
- Interactive virtual terminal.
- Linux/macOS and Windows PowerShell views.
- `pwd`, `ls`, `cd` and path navigation.
- `mkdir`, `cp`, `mv`, `rm`.
- Nano/notepad-style text editing.
- `head`, `tail` and PowerShell equivalents.
- `grep`, `Select-String`, `wc`, `Measure-Object`.
- Pipes and redirection.
- Command history.
- `chmod +x` and Python script execution.

### Bioinformatics Toolkit
- Python modules and `import`.
- `collections.Counter`.
- FASTA reading and writing.
- Optional local FASTA upload.
- `sys.argv`.
- `pathlib`.
- Input validation and `FileNotFoundError` handling.
- Biopython `Seq` and `SeqIO`.
- Complement, reverse complement and translation.
- Final Viral CLI Analyzer.

### Learning experience
- Audited the complete guiding-question set against the worked examples.
- Replaced abstract or low-value questions with concrete **predict-the-result / apply-the-example** questions wherever syntax or command behavior is being learned.
- Mission 3 now asks the learner to apply `.count()` directly to a new sequence (`dna.count("G")`) instead of asking about a decontextualized GC count.
- Questions now appear before examples only when they genuinely frame a concept; syntax-dependent questions are placed after the relevant examples.
- The set now contains **20 naturally placed questions**: 4 contextual questions and 16 post-example applications.
- Replaced the mechanical one-question-per-mission pattern with a **natural contextual flow**: 17 opening guiding questions plus 9 second questions only in missions where prediction or transfer adds value.
- Second questions are positioned **after the worked examples and before the exercises**, so they help students transfer the demonstrated programming pattern instead of simply copying it.
- Guiding questions remain non-blocking, carry no XP, and focus on programming/computational reasoning rather than biology knowledge.
- Added **Guided Path (Story Mode)** and **Free Explore** learning paths. Guided Path preserves sequential unlocking; Free Explore opens all 17 missions so students can start with Terminal, Python, FASTA, Biopython, or another topic.
- Added one short, non-blocking programming **Quick Check** to each mission. These questions provide computational context without testing biology content or affecting XP.
- Reworked the mission examples so they teach the programming pattern with a different variable, sequence, filename, or context instead of directly reproducing the following challenge whenever practical.
- Preserved a gradual difficulty curve: guided first attempts, small-transfer exercises, combined exercises, and personalized Boss Challenges.
- Restored the beginner-friendly `input()` explanation in Mission 1 and clarified that `input()` returns text before numeric conversion.
- Clarified Mission 9 **Build base counts** to explicitly request printing the complete `counts` dictionary.
- Clarified the final DNA Analyzer Boss by explicitly stating the required report labels, one labeled result per line, and the required use of functions for GC percentage and reverse complement.
- Fixed Function Toolkit Boss validation so Python booleans are expected as `True` / `False` instead of JavaScript `true` / `false`.
- Replaced literal dictionary-syntax checks in Viral Record and Function Toolkit with semantic validation that accepts dictionary literals or `dict(...)`.
- Base Counter now accepts either `for` or `while` and tolerates spaces around `=`, while still requiring the four requested output lines.
- Clarified that `gc` in Viral Record and Function Toolkit means GC percentage, not raw G+C count.
- Improved feedback for GC-count-vs-GC-percentage confusion and for functions that ignore their `dna` parameter.
- Reverse-complement validation now accepts either loop type while still requiring a dictionary, a loop, a returned result, and the correct reverse complement.
- Reworked the introduction to `while` in Mission 6 with a three-part mental model (start value → condition → update), an infinite-loop warning, a counter-only stepping-stone challenge, and a guided DNA `while` exercise.
- Added Mission 5 Case 5, **Inspect several positions**, where students manually compare six nucleotide positions and identify two substitutions before learning loops.
- Audited challenge validation across BioCode Lab so valid equivalent Python syntax is accepted. Validation now tolerates harmless whitespace and quote style, ignores comments, and removes unnecessary literal-solution checks in loops, functions, dictionaries, FASTA parsing, CLI handling, Biopython, and Boss challenges.
- Mission 2 now accepts equivalent valid Python expressions: `dna[0]` or `dna[0:1]` for the first nucleotide, and `dna[:3]` or `dna[0:3]` for the first codon.
- Improved Mission 1 Case 3 with a beginner-friendly explanation of `input()`, strings, integers, `int()`, and why arithmetic requires type conversion.
- Progressive mission unlocking.
- XP, levels and skill badges.
- Personalized Boss Challenges.
- Strong red wrong-answer feedback.
- Rule-based tutor hints.
- Strategic written reflections.
- Lab Log.
- Printable PDF evidence.
- Social achievement cards.
- Mobile native sharing where supported.
- Desktop QR-to-phone flow and downloadable cards.

## v0.5 — 2026-08-31
- Expanded BioCode Lab from 9 to 17 missions.
- Added Terminal Foundations.
- Added Bioinformatics Toolkit.
- Added interactive virtual terminal.
- Added FASTA, `sys.argv` and Biopython workflows.

## v0.4 — 2026-08-31
- Completed the original 9-mission Python course.
- Added functions mission and final DNA Analyzer.
- Added rule-based feedback.
- Extended personalized Boss variants.
- Improved social sharing.

## v0.3 — 2026-08-31
- Added explicit red wrong-answer states.
- Reduced required written reflections.
- Simplified Classroom Lite.
- Added local Teacher Dashboard.
- Added Lab Log.
- Added Challenge-a-Classmate.

## v0.2
- Improved Classroom Lite onboarding.
- Improved Check / Next navigation.
- Improved achievement cards.

## v0.1
- Initial BioCode Lab prototype.
- Single-HTML application.
- English/Spanish interface.
- Pyodide-based Python execution.
