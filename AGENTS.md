# Repository Guidelines

Guidance for coding and documentation agents working in this repository.

## Project Structure

This repository contains the Laws.Africa Developer Guide as GitBook-style
Markdown documentation.

- `README.md`: landing page for the guide.
- `SUMMARY.md`: table of contents and navigation order.
- `get-started/`: introductory concepts, quick start, webhooks, and changelog.
- `api/`: Content API reference pages.
- `ai-api/`: AI API reference pages.
- `how-to-guides/`: task-focused guides.
- `tutorial/`: multi-module tutorial content.

There is no application source code or local build configuration in this repo.
Treat the Markdown files and GitBook navigation as the source of truth.

## Editing Rules

- Keep changes focused on the requested documentation area.
- Update `SUMMARY.md` whenever adding, moving, renaming, or removing pages.
- Preserve GitBook directives such as `{% hint %}`, `{% content-ref %}`, and
  front matter blocks.
- Do not rewrite large pages for style only; avoid repo-wide formatting churn.
- Keep examples realistic and aligned with current Laws.Africa API endpoints.
- Never commit API tokens, private credentials, or user-specific values.

## Writing Style

- Audience: developers integrating Laws.Africa legislation or AI content into
  their applications.
- Use clear, direct, technical prose.
- Prefer short sections with descriptive headings.
- Use numbered lists for procedures and bullets for reference lists.
- Explain required concepts before using specialist terms such as FRBR URI,
  expression, enrichment, or Akoma Ntoso.
- Keep API examples copy-paste friendly and include placeholders such as
  `<YOUR_AUTH_TOKEN>` for secrets.
- Use UK/South African English conventions where wording differs.

## Markdown and GitBook Conventions

- Pages usually start with YAML front matter containing `description`.
- Use one `#` heading per page, matching the page title in `SUMMARY.md`.
- Use fenced code blocks with language tags such as `bash`, `javascript`,
  `json`, `python`, `html`, or `markup`.
- Existing command examples include a leading `$`; keep the style consistent
  within the page being edited.
- Use GitBook hints for important notes, warnings, and limitations:

```markdown
{% hint style="info" %}
Important supporting note.
{% endhint %}
```

- Keep relative links valid. Prefer linking to existing Markdown files rather
  than broken GitBook placeholder URLs.
- Images are referenced from `.gitbook/assets/` relative to the current page
  when assets are present.

## API Documentation Standards

- State the endpoint and supported content types explicitly.
- Document authentication requirements before authenticated examples.
- Include request examples and describe the response shape in prose or JSON.
- When documenting pagination, formats, or parameters, be precise about defaults
  and optional values.
- If API behaviour may have changed, verify against the live OpenAPI schema or
  current platform documentation before updating examples.
- Do not invent undocumented fields, parameters, or response values.

## Tutorial Standards

- Keep tutorial steps executable in order.
- Explain why each step matters before introducing larger code blocks.
- If a previous module establishes a model, variable, package, or view name, use
  the same name consistently in later pages.
- Avoid advanced alternatives unless they directly help the tutorial goal.

## Validation

There is no configured automated test suite in this repo. For documentation
changes:

1. Check changed Markdown for broken relative links.
2. Check `SUMMARY.md` if navigation changed.
3. Review code blocks for syntax, indentation, and placeholder consistency.
4. For API examples, verify endpoint paths and documented fields when possible.
5. Run `git diff --check` before finishing to catch whitespace problems.

## Commit and Pull Request Notes

- Keep commits scoped to one documentation change or related group of pages.
- Use concise commit subjects such as `Add AI API query docs` or
  `Fix quick start links`.
- In PR descriptions, summarize the docs changed, list any validation performed,
  and call out any API behaviour that was verified externally.

## When Unsure

- Prefer preserving existing structure and terminology.
- Ask before reorganizing large sections or changing navigation hierarchy.
- For API facts, verify rather than guessing.
