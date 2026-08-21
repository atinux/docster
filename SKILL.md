---
name: docster
description: Always use this skill when the task involves writing, reviewing, or editing files in the `/docs` directory or any `.md` files in the repository.
---

# Docster

As an expert technical writer and editor, you produce accurate, clear, and consistent documentation. When asked to write, edit, or review documentation, you must ensure the content strictly adheres to the provided documentation standards and accurately reflects the current codebase. Adhere to the contribution process in `CONTRIBUTING.md` (if present) and the following standards.

## Phase 0: Detect the docs stack

Before writing, detect which component syntax the project uses:

- **Comark / MDC projects:** if `package.json` (or any workspace `package.json`) depends on `comark`, `comark-content`, or any `@comark/*` package (also `@nuxt/content` or `@nuxtjs/mdc`), the docs pages support Comark components. Use the "Comark components" section below for docs content.
- **Plain Markdown projects:** otherwise, use only GitHub-flavored Markdown (GFM alerts, `<details>`).
- **Always** use plain GFM in files rendered by GitHub itself (`README.md`, `CONTRIBUTING.md`, and other repo meta files), even in Comark projects.

Also check repo-level conventions first (`AGENTS.md`, `CLAUDE.md`,
`CONTRIBUTING.md`); they override this skill where they conflict.

## Phase 1: Documentation standards

Adhering to these principles and standards when writing, editing, and
reviewing.

### Voice and tone

Adopt a tone that balances professionalism with a helpful, conversational approach.

- **Perspective and tense:** Address the reader as "you." Use active voice and present tense (e.g., "The API returns...").
- **Tone:** Professional, friendly, and direct.
- **Clarity:** Use simple vocabulary. Avoid jargon, slang, and marketing hype.
- **Global Audience:** Write in standard US English. Avoid idioms and cultural references.
- **Requirements:** Be clear about requirements ("must") vs. recommendations ("we recommend"). Avoid "should."
- **Word Choice:** Avoid "please" and anthropomorphism (e.g., "the server thinks"). Use contractions (don't, it's).

### Language and grammar

Write precisely to ensure your instructions are unambiguous.

- **Abbreviations:** Avoid Latin abbreviations; use "for example" (not "e.g.") and "that is" (not "i.e.").
- **Punctuation:** Use the serial comma. Place periods and commas inside quotation marks.
- **Dates:** Use unambiguous formats (e.g., "January 22, 2026").
- **Conciseness:** Use "lets you" instead of "allows you to." Use precise, specific verbs.
- **Examples:** Use meaningful names in examples; avoid placeholders like "foo" or "bar."

### Formatting and syntax

Apply consistent formatting to make documentation visually organized and accessible.

- **Overview paragraphs:** Every heading must be followed by at least one
  introductory overview paragraph before any lists or sub-headings.
- **Text wrap:** Follow the existing wrap style of the file. Don't rewrap
  existing prose; it bloats diffs.
- **Casing:** Use sentence case for headings, titles, and bolded text.
- **Naming:** Always refer to the project by its canonical name, and never prefix it with "the" (for example, `Comark`, never `the Comark`).
- **Lists:** Use numbered lists for sequential steps and bulleted lists
  otherwise. Keep list items parallel in structure.
- **UI and code:** Use **bold** for UI elements and `code font` for filenames, snippets, commands, and API elements. Focus on the task when discussing interaction.
- **Accessibility:** Use semantic HTML elements correctly (headings, lists, tables).
- **Media:** Use lowercase hyphenated filenames. Provide descriptive alt text for all images.

### Comark components

Please read the full specification of Comark Components on https://comark.dev/raw/syntax/components.md

#### Nuxt UI detected

When Phase 0 detected a Comark project with Nuxt UI package (`@nuxt/ui`), use the prose components documented in [Nuxt UI typography](https://ui.nuxt.com/docs/typography) instead of GFM alerts or raw HTML. Match the component set already used in the project's `content/` directory before introducing a new one.

- **Callouts:** `::note`, `::tip`, `::warning`, `::caution`, or a generic
  `::callout{icon="..." to="..."}`. Close every block with `::`.

  Example:

  ```md
  ::note
  Bodies are parsed lazily on `get()`.
  ::
  ```

- **Nesting:** nested components add a colon per level (`:::card` inside
  `::card-group`).

  ```md
  ::card-group
    :::card{icon="i-lucide-rocket" title="Fast"}
    Parses on demand.
    :::
  ::
  ```

- **Grouped code:** `::code-group` with one fenced block per tab
  (```bash [pnpm]` style filenames/labels).
- **Procedures:** `::steps` around a sequence of `###` headings.
- **Tabs:** `::tabs` with `:::tabs-item{label="..."}` children.
- **Collapsible content:** `::collapsible` or `::accordion` instead of
  `<details>`.
- **Experimental features:** if a feature is clearly noted as experimental,
  add this immediately after the introductory paragraph:

  ```md
  ::warning
  This is an experimental feature currently under active development.
  ::
  ```

#### Custom components

You can also find the components used as prop in the `<Markdown>` or `<MarkdownDocument>` components to see which components are available in Markdown, have a look at them to know the available props and slots.

### GFM fallback (plain Markdown projects and GitHub meta files)

- **Details section:** Use the `<details>` tag to create a collapsible
  section for supplementary or data-heavy information.

  Example:

  <details>
  <summary>Title</summary>

  - First entry
  - Second entry

  </details>

- **Callouts:** Use GitHub-flavored markdown alerts (`NOTE`, `TIP`,
  `IMPORTANT`, `WARNING`, `CAUTION`). If the project runs Prettier over
  Markdown, place an empty line and `<!-- prettier-ignore -->` (or
  `{/* prettier-ignore */}` in `.mdx`) directly before the alert block so
  formatting is preserved.

  Example (.md):

<!-- prettier-ignore -->
> [!NOTE]
> This is an example of a multi-line note that will be preserved
> by Prettier.

- **Experimental features:** add a `> [!NOTE]` alert stating the feature is experimental, immediately after the introductory paragraph.

### Links
- **Accessibility:** Use descriptive anchor text; avoid "click here." Ensure
  the link makes sense out of context, such as when being read by a screen
  reader.
- **Use relative or root-relative links in docs:** Follow the project's
  existing link style, and verify every link resolves to a real page. Meta
  files such as `README.md` and `CONTRIBUTING.md` may use absolute URLs.
- **When changing headings, check for deep links:** If you change a heading,
  check for deep links to that heading in other pages and update accordingly.

### Structure
- **BLUF:** Start with an introduction explaining what to expect.
- **Headings:** Use hierarchical headings to support the user journey.
- **Procedures:**
  - Introduce lists of steps with a complete sentence.
  - Start each step with an imperative verb.
  - Number sequential steps; use bullets for non-sequential lists.
  - Put conditions before instructions (e.g., "On the Settings page,
    click...").
  - Provide clear context for where the action takes place.
  - Indicate optional steps clearly (e.g., "Optional: ...").
- **Elements:** Use bullet lists, tables, collapsible sections, and callouts.
- **Avoid using a table of contents:** If a manually written table of
  contents is present, remove it (docs sites generate their own).
- **Next steps:** Conclude with a "Next steps" section if applicable.

## Phase 2: Preparation

Before modifying any documentation, thoroughly investigate the request and the surrounding context.

1.  **Clarify:** Understand the core request. Differentiate between writing
    new content and editing existing content. If the request is ambiguous
    (e.g., "fix the docs"), ask for clarification.
2.  **Investigate:** Examine the relevant source code for accuracy.
3.  **Audit:** Read the latest versions of the relevant docs files.
4.  **Connect:** Identify all referencing pages if changing behavior. Check whether the site navigation needs updates (`.navigation.yml` files, numeric filename prefixes, or a sidebar config, depending on the project).
5.  **Plan:** Create a step-by-step plan before making changes.

## Phase 3: Execution

Implement your plan by either updating existing files or creating new ones using the appropriate file system tools. Use targeted edits for small changes and full writes for new files or large rewrites.

### Editing existing documentation

Follow these additional steps when asked to review or update existing
documentation.

- **Gaps:** Identify areas where the documentation is incomplete or no longer
  reflects existing code.
- **Structure:** Apply the "Structure" rules (BLUF, headings, etc.) when
  adding new sections to existing pages.
- **Headers:** If you change a header, you must check for links that lead to
  that header and update them.
- **Tone:** Ensure the tone is active and engaging. Use "you" and
  contractions.
- **Clarity:** Correct awkward wording, spelling, and grammar. Rephrase
  sentences to make them easier for users to understand.
- **Consistency:** Check for consistent terminology and style across all
  edited documents.

## Phase 4: Verification and finalization

Perform a final quality check to ensure that all changes are correctly formatted and that all links are functional.

1.  **Accuracy:** Ensure content accurately reflects the implementation and technical behavior.
2.  **Self-review:** Re-read changes for formatting, correctness, and flow.
3.  **Link check:** Verify all new and existing links leading to or from modified pages. If you changed a header, ensure that any links that lead to it are updated.
4.  **Format:** If the project defines a format or lint command (check `package.json` scripts), ask to run it once all changes are complete. If the user confirms, execute the command.
