# BioCode Lab v1.0.1

**Learn Python. Decode Biology.**

BioCode Lab is a bilingual, gamified, story-driven learning environment for undergraduate biology students. It integrates Python programming, command-line skills, biological sequence files, and introductory bioinformatics workflows in a single interactive HTML application.

**BioCode Lab by Yair Cardenas Conejo**

## Release status

This is the public-ready release of BioCode Lab: **v1.0.1**.

The core application is distributed as a **single HTML file**. The BioCode Lab logo is embedded directly inside the HTML, so no separate image or assets folder is required.

## Learning paths

BioCode Lab v1.0.1 offers two ways to move through the same 17 missions:

- **Guided Path (Story Mode)** — missions unlock sequentially. This is the recommended path for a first pass through the course because concepts are introduced in a planned progression.
- **Free Explore** — every mission is immediately available. Students can begin with Python, Terminal, FASTA, Biopython, or any topic they want to review.

Changing the learning path does not erase XP, completed challenges, Boss Challenges, badges, code, or learning evidence. Progress remains in the same local BioCode state.

In Guided Path, sequential unlocking is based on the contiguous set of completed missions from the beginning of the course. A mission completed previously in Free Explore remains completed, but it does not automatically skip the prerequisite sequence of the Guided Path.

## Course architecture

### Part I — Python Foundations

1. **Boot Sequence** — variables, strings, numeric values, `input()`, `int()`
2. **Sequence Decoder** — `len()`, indexing, slicing
3. **Genome Metrics** — nucleotide counts and GC%
4. **Restriction Hunt** — `if`, `elif`, `else`, `in`, `and`, `or`
5. **Mutation Alert** — comparisons and sequence normalization
6. **Sample Processor** — `for`, `while`, `continue`
7. **Viral Data** — lists and dictionaries
8. **Build Your Tools** — functions, parameters and `return`
9. **Unknown Virus** — integrated DNA Analyzer

### Part II — Terminal Foundations

10. **Terminal Bootcamp** — `pwd`, `ls`, `cd`, relative and absolute paths
11. **File Operations** — `mkdir`, `cp`, `mv`, `rm`, `nano`, `head`, `tail`
12. **Search & Pipes** — `grep`, `wc`, `|`, `>`, `>>`, `history`, `chmod`, Python script execution

The terminal section includes a local virtual filesystem and supports both a **Linux/macOS view** and a **Windows PowerShell view**. The goal is to teach transferable command-line concepts rather than two independent shell courses.

### Part III — Bioinformatics Toolkit

13. **Import Lab** — `import`, Python standard library, `collections.Counter`
14. **FASTA Files** — `open()`, `with`, reading/writing FASTA text, optional real FASTA upload
15. **Command Line Biology** — `sys.argv`, validation, `pathlib`, `try/except`
16. **Biopython** — `Bio.Seq.Seq`, complement, reverse complement, translation, `SeqIO`
17. **Viral CLI Analyzer** — final `sys.argv + FASTA + functions + Biopython` workflow

## Learning design

BioCode Lab uses a recurring pattern:

**Quick check → Example → Transfer → Run → Modify → Debug → Apply**

Guiding questions are placed according to what the learner has already seen, not according to a fixed quota. Questions that require Python syntax or command behavior are normally placed **after a worked example** and ask the learner to apply that pattern to a different value, sequence, filename, or command. Only a few questions appear before examples when they genuinely frame the need for the concept.

The current v1.0.1 question set contains **20 questions**: 4 contextual questions before examples and 16 application questions after examples. They focus on predicting code/command behavior and computational reasoning rather than testing biology knowledge. They do not award or remove XP and never block progress.

Worked examples intentionally use different variables, sequences, filenames, or contexts from the following challenge whenever practical. The first exercise can remain closely guided, but later exercises require a small transfer instead of copying the example verbatim.

The system includes:

- Guided Path sequential unlocking plus optional Free Explore mode;
- one non-blocking programming quick check per mission;
- transfer-oriented examples that avoid duplicating the following exercise verbatim;
- XP and levels;
- skill-based badges;
- personalized Boss Challenges;
- strategic written reflections;
- explicit red error feedback;
- rule-based tutor hints for common beginner errors;
- biological narratives and synthetic sequence cases;
- Lab Log / learning history;
- PDF learning evidence;
- social achievement cards.

## Individual mode

The complete course can be used without an account or backend.

Progress is stored locally in the browser using `localStorage`.

This makes the resource suitable for:

- self-study;
- GitHub Pages;
- Zenodo;
- workshops;
- classroom activities where no server is available.

## Classroom Lite

Students can enter a class code and alias without creating an account.

BioCode Lab stores their progress locally and can export a portable `.biocode` progress file.

The same HTML includes a **Teacher Dashboard** that can load multiple `.biocode` files locally and summarize:

- number of students;
- average XP;
- average missions completed;
- program executions;
- hints used;
- completion by mission;
- leaderboard;
- difficult challenges.

No student files are uploaded to a server by this workflow.

## Social sharing

On compatible mobile browsers, BioCode Lab uses the native Web Share interface to share achievement cards.

On desktop, the sharing panel provides:

- QR-to-phone flow when the app is hosted over HTTPS;
- image download;
- generated caption text;
- Challenge-a-Classmate cards.

The achievement card includes the BioCode Lab identity and author attribution.

## Embedded branding

The BioCode Lab logo is embedded directly in the HTML as a Base64 data URI.

This preserves the single-file distribution model:

`index.html`

No `images/`, `assets/`, or external logo file is required.

The interface also includes a footer ribbon with:

**BioCode Lab by Yair Cardenas Conejo**

together with version, educational purpose, technologies and license information.

## Runtime requirements

BioCode Lab v1.0.1 loads:

- **Pyodide** from a CDN for browser-based Python execution;
- **QRCode.js** from a CDN for QR generation;
- **Biopython** through Pyodide when the Biopython missions are used.

Internet access is currently required for these runtime components.

## Running BioCode Lab

Open:

`index.html`

in a modern web browser.

For the best experience—especially Python execution and QR-to-phone sharing—host the HTML over HTTPS, for example with GitHub Pages.

## Publishing with GitHub Pages

The repository is prepared to publish directly from its root folder:

1. Create an empty GitHub repository.
2. Push the `main` branch from this folder.
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**, then select `main` and `/ (root)`.

GitHub Pages will open `index.html` automatically. No build command, server, database, account or environment variable is required.

## Distribution

This release package is prepared for GitHub and Zenodo.

Included:

- `index.html`
- `README.md`
- `CHANGELOG.md`
- `LICENSE`
- `CITATION.cff`
- `VERSION`
- `.nojekyll`

## Citation

Citation metadata is provided in `CITATION.cff`.

GitHub can use this file to expose a **Cite this repository** option, and the metadata can also be reused when depositing the release in Zenodo.

## License

BioCode Lab is distributed under the **MIT License**.

## Author

**Yair Cardenas Conejo**

## Version

**BioCode Lab v1.0.1 — 2026-09-03**
