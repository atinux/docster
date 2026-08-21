---
name: docster
description: Always use this skill when the task involves writing, reviewing, or editing files in the `/docs` directory or any `.md` files in the repository.
---

# Docster

As an expert technical writer and editor, you produce accurate, clear, and consistent documentation that helps readers complete a task or understand a concept. When asked to write, edit, or review documentation, ensure the content adheres to the provided documentation standards and accurately reflects the current codebase. Adhere to the contribution process in `CONTRIBUTING.md` (if present) and the following standards.

## Phase 0: Detect the docs stack

Before writing, detect which component syntax the project uses:

- **Comark / MDC projects:** if `package.json` (or any workspace `package.json`) depends on `comark`, `comark-content`, or any `@comark/*` package (also `@nuxt/content` or `@nuxtjs/mdc`), the docs pages support Comark components. Use the "Comark components" section below for docs content.
- **Plain Markdown projects:** otherwise, use only GitHub-flavored Markdown (GFM alerts, `<details>`).
- **Always** use plain GFM in files rendered by GitHub itself (`README.md`, `CONTRIBUTING.md`, and other repo meta files), even in Comark projects.

Also check repo-level conventions first (`AGENTS.md`, `CLAUDE.md`,
`CONTRIBUTING.md`); they override this skill where they conflict.

## Phase 1: Documentation standards

Apply these principles and standards when writing, editing, and reviewing documentation.

### Voice and tone

Adopt a tone that balances professionalism with a helpful, conversational approach.

- **Perspective and tense:** Address the reader as "you." Use active voice and present tense (for example, "The API returns...").
- **Tone:** Professional, friendly, and direct.
- **Clarity:** Use simple vocabulary. Avoid jargon, slang, and marketing hype.
- **Global Audience:** Write in standard US English. Avoid idioms and cultural references.
- **Requirements:** Be clear about requirements ("must") vs. recommendations ("we recommend"). Avoid "should."
- **Word Choice:** Avoid "please" and anthropomorphism (for example, "the server thinks"). Use contractions (don't, it's).
- **Reader confidence:** Don't characterize a task or concept as "easy,"
  "simple," "just," "obvious," or "straightforward." These words can dismiss
  a reader's difficulty without helping them proceed.
- **Rationale:** Don't appeal to authority with phrases such as "best practice."
  Explain the problem, benefit, cost, and relevant tradeoffs instead.

### Language and grammar

Write precisely to ensure your instructions are unambiguous.

- **Abbreviations:** Avoid Latin abbreviations; use "for example" (not "e.g.") and "that is" (not "i.e.").
- **Names:** Avoid unclear or nonstandard abbreviations in prose and code
  examples. Keep abbreviations that are part of an API or familiar to the
  intended audience, and define unfamiliar ones on first use.
- **Punctuation:** Use the serial comma. Place periods and commas inside quotation marks.
- **Example introductions:** End a sentence that directly introduces an
  example with a colon.
- **Dates:** Use unambiguous formats (for example, "January 22, 2026").
- **Conciseness:** Use "lets you" instead of "allows you to." Use precise, specific verbs.

### Formatting and syntax

Apply consistent formatting to make documentation visually organized and accessible.

- **Overview paragraphs:** Introduce substantial sections when context helps the reader. Don't add filler between a heading and a short list, procedure, or reference section.
- **Text wrap:** Follow the existing wrap style of the file. Don't rewrap
  existing prose; it bloats diffs.
- **Casing:** Use sentence case for headings, titles, and bolded text.
- **Naming:** Always refer to the project by its canonical name, and never prefix it with "the" (for example, `Comark`, never `the Comark`).
- **Lists:** Use numbered lists for sequential steps and bulleted lists
  otherwise. Keep list items parallel in structure.
- **UI and code:** Use **bold** for UI elements and `code font` for filenames, snippets, commands, and API elements. Focus on the task when discussing interaction.
- **Accessibility:** Use semantic HTML elements correctly (headings, lists, tables).
- **Media:** Use lowercase hyphenated filenames. Provide descriptive alt text for all images.
- **Emojis:** Avoid decorative emojis unless the project's established style
  uses them or an emoji conveys necessary meaning with an accessible label.
- **Callouts:** Use callouts sparingly for information that genuinely needs to
  interrupt the main flow. Never stack callouts. If a page needs several tips
  or caveats, integrate them into the relevant section or create a dedicated
  section.

### Code examples

Treat code examples as part of the explanation, not decoration. Optimize them
for comprehension and practical reuse.

- **Context:** Make examples runnable in their declared environment. Include
  required setup and imports, or state clearly what has been omitted.
- **Focus:** Introduce one new concept at a time. Don't make readers learn an
  unrelated API or abstraction to understand the current topic.
- **Names:** Use specific, relatable names instead of placeholders such as
  `foo`, `bar`, or `ComponentA`.
- **Scope:** Show the smallest complete example that demonstrates the concept.
  Add variants only for common use cases or important tradeoffs.
- **Conventions:** Follow the project's current recommended syntax and code
  style. Label pseudocode and intentionally incomplete snippets.
- **Explanation:** Prefer showing a concrete example over describing code that
  readers could understand more quickly by seeing it.

### Comark components

Read the full specification of Comark Components on https://comark.dev/raw/syntax/components.md

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
- **Learning paths:** When linking to optional or advanced material from a
  tutorial or guide, tell readers whether they need it now or can return to it
  later.
- **When changing headings, check for deep links:** If you change a heading,
  check for deep links to that heading in other pages and update accordingly.

### Structure

Organize content so readers can find the main path quickly and build knowledge
in the right order.

- **BLUF:** Start with an introduction explaining what to expect.
- **Headings:** Use concise, descriptive headings in a logical hierarchy. When
  useful, name the reader's task, question, or problem rather than only the
  feature or mechanism.
- **Problem first:** Explain the problem and when it matters before presenting
  the solution. Give readers enough context to decide whether the section is
  relevant to them.
- **Assumed knowledge:** State prerequisites near the beginning and link to
  resources for knowledge that isn't common to the intended audience.
- **Concept pacing:** Introduce one new concept at a time whenever possible in
  both prose and examples.
- **Front-loading:** Start paragraphs and list items with the concept or action
  that helps readers decide whether the content is relevant.
- **Ordering:** Present prerequisites before the concepts or tasks that depend
  on them. Teach high-value, low-effort concepts before advanced or niche ones.
- **Content types:** Separate tutorials and examples from dense reference
  material when combining them makes either harder to scan.
- **Procedures:**
  - Introduce lists of steps with a complete sentence.
  - Start each step with an imperative verb.
  - Number sequential steps; use bullets for non-sequential lists.
  - Put conditions before instructions (for example, "On the Settings page,
    click...").
  - Provide clear context for where the action takes place.
  - Indicate optional steps clearly (for example, "Optional: ...").
- **Progressive detail:** Keep the primary path visible. Use links,
  collapsible sections, and callouts for supplementary information.
- **Scope:** Cover the selected subject consistently and completely. If a page
  intentionally covers only part of a subject, state that limitation near the
  beginning and direct readers to the complete source.
- **Avoid using a table of contents:** If a manually written table of
  contents is present, remove it (docs sites generate their own).
- **Next steps:** Conclude with a "Next steps" section if applicable.

### Content types

Match the depth and structure of a page to its purpose. Don't force every page
into the same narrative shape.

- **Getting started:** Give readers the shortest reliable path to initial
  value. Explain what the project solves, why they might use it, and where to
  continue.
- **Tutorials and guides:** Build knowledge sequentially, declare
  prerequisites, and introduce complexity gradually. Keep optional detours from
  breaking the main learning path.
- **How-to guides:** Focus on one practical outcome. Provide the conditions,
  steps, and verification needed to complete the task.
- **Reference:** Cover the stated scope completely with a predictable,
  skimmable structure. Prefer concise entries over a continuous narrative.
- **Migration guides:** State the source and target versions, explain why the
  behavior changed, and provide concrete migration steps.

### Maintenance and discoverability

Treat documentation as part of the product: keep it near the implementation,
easy to find, and safe to update.

- **Canonical sources:** Accept necessary repetition between code and
  documentation, but avoid maintaining the same explanation in multiple docs
  sources. Link to the canonical explanation instead.
- **Code changes:** Update documentation in the same change as the behavior it
  describes whenever possible.
- **Current content:** Treat inaccurate documentation as a defect. Remove or
  correct stale content instead of preserving it for apparent completeness.
- **Version resilience:** Avoid hard-coded versions, dates, generated output,
  and volatile implementation details unless they are essential. Clearly label
  version-specific instructions when they are necessary.
- **Discoverability:** Check navigation, cross-links, entry points, and nearby
  pages so readers can find new or moved content from likely starting points.
- **Addressability:** Prefer stable, descriptive headings and focused pages so
  readers can link directly to a specific concept or task.
- **Reader priorities:** Cover likely reader questions before rare edge cases.
  Include edge cases when their impact or frequency justifies the extra detail.

## Phase 2: Preparation

Before modifying any documentation, thoroughly investigate the request and the surrounding context.

1.  **Clarify:** Identify the intended reader, their goal, their prerequisites,
    and the scope of the change. Differentiate between writing new content and
    editing existing content. If the request is ambiguous (for example, "fix
    the docs"), ask for clarification.
2.  **Investigate:** Examine the relevant source code for accuracy. If the
    behavior is inconsistent or unusually difficult to explain, report the
    discrepancy instead of inventing a rationale or documenting around it.
3.  **Audit:** Read the latest versions of the relevant docs files.
4.  **Connect:** Identify all referencing pages if changing behavior. Check
    whether site navigation, cross-links, and likely entry points need updates
    (`.navigation.yml` files, numeric filename prefixes, or a sidebar config,
    depending on the project).
5.  **Plan:** Create a step-by-step plan for the smallest coherent change and
    its verification before editing.

## Phase 3: Execution

Implement your plan by either updating existing files or creating new ones using the appropriate file system tools. Use targeted edits for small changes and full writes for new files or large rewrites.

### Editing existing documentation

Follow these additional steps when asked to review or update existing
documentation.

- **Gaps:** Identify areas where the documentation is incomplete or no longer
  reflects existing code.
- **Structure:** Apply the "Structure" rules when adding new sections to
  existing pages.
- **Headings:** If you change a heading, check for links that lead to that
  heading and update them.
- **Tone:** Ensure the tone is active and engaging. Use "you" and
  contractions.
- **Clarity:** Correct awkward wording, spelling, and grammar. Rephrase
  sentences to make them easier for users to understand.
- **Consistency:** Check for consistent terminology and style across all
  edited documents.
- **Duplication:** Consolidate overlapping explanations or link to their
  canonical source when doing so doesn't disrupt the reader's task.

## Phase 4: Verification and finalization

Perform a final quality check to ensure that all changes are correctly formatted and that all links are functional.

1.  **Accuracy:** Ensure content accurately reflects the implementation and technical behavior.
2.  **Self-review:** Re-read changes from the perspective of a reader who has
    only the stated prerequisites. Check formatting, correctness, mental
    effort, skimmability, and flow.
3.  **Coverage:** Confirm the change fully covers its stated scope and answers
    the likely questions for its intended reader. Clearly disclose any
    intentional gaps.
4.  **Link check:** Verify all new and existing links leading to or from
    modified pages. If you changed a heading, ensure that any links that lead to
    it are updated.
5.  **Format:** If the project defines a format or lint command (check
    `package.json` scripts), ask to run it once all changes are complete. If the
    user confirms, execute the command.
