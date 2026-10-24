# Contributing

Thanks for helping make this list better. The goal is a reliable map of routes that actually work, not a list of every project that mentions emulation.

## What belongs here

A project should meet all of these:

1. It lets software run on a host it was not built for, or makes a meaningful part of that route possible.
2. It has a public source, release, or official documentation page.
3. The guest → host route can be described concretely.
4. Its limitations can be stated honestly.
5. It does not exist primarily to distribute copyrighted games, firmware, BIOS files, or keys.

Projects may be experimental, archived, or commercial. Label them accurately.

## What does not belong here

- Ordinary application containers, remote desktop, and cloud streaming.
- Storefronts, launchers, and compatibility-rating databases.
- DRM removal, anti-cheat bypasses, and piracy tools.
- Projects that only redistribute ROMs, ISOs, BIOS files, firmware, keys, or game data.
- Speculative routes with no working implementation.

## Adding a project

1. Pick the right category. If a project spans categories, choose the primary mechanism.
2. Add the entry in the same format as the existing entries:

```markdown
### Project name

**Route:** Guest → Host · **Status:** Mature/Active/Experimental/Legacy/Proprietary · **Type:** short mechanism

One short paragraph explaining what it does, what it enables, and the main limitation. Mention the CPU/API translation combination when it is not obvious.

[Website](https://example.com/) · [Source](https://github.com/)
```

3. Update the **guest → host matrix** if the project adds a new route.
4. Update both `README.md` and `README.zh-CN.md`. Keep the two files synchronized.
5. Add the entry to the correct position, not just the end of the section.
6. Run a link check (see below).

## Status labels

| Label | Use when |
| --- | --- |
| **Mature** | Widely used and stable for the stated route. |
| **Active** | Maintained and improving, with uneven compatibility. |
| **Experimental** | Early-stage, limited titles/features, expected breakage. |
| **Legacy** | Discontinued, deprecated, or superseded. |
| **Proprietary** | Closed-source or commercial. |

If a project is both proprietary and experimental, choose the label that matters most to a reader and explain the rest in the paragraph.

## Writing style

- Write in English for `README.md` and Simplified Chinese for `README.zh-CN.md`.
- Keep descriptions factual and specific. Avoid marketing language.
- State the main limitation; do not hide it in a footnote.
- Prefer primary sources: official documentation, release notes, and the project repository.
- Use the project's current official name, and mention renames or forks when relevant.
- Link to the source or official site, not to a mirror, unless the mirror is the canonical source (for example, projects that moved off GitHub).
- Do not use emojis unless the repository explicitly adopts them.

## Link checks

The repository uses [lychee](https://github.com/lycheeverse/lychee) through GitHub Actions. To run it locally:

```sh
# Install lychee using the instructions at https://github.com/lycheeverse/lychee
lychee --config .lychee.toml --no-progress './**/*.md'
```

Some official sites block automated requests. If a link is correct but the checker cannot reach it, add a narrowly scoped exclusion to `.lychee.toml` with a comment explaining why. Do not exclude a whole domain just to make the check pass.

## Pull requests

- One project or one coherent documentation change per pull request.
- Explain the route and why the project belongs in the list.
- Include the date you verified the source if the project changes frequently.
- Be ready to update the entry if a maintainer asks for clarification.
- By contributing, you agree that your text is licensed under the repository's MIT license.

## Legal

Do not contribute links to copyrighted content, firmware, BIOS files, decryption keys, or circumvention tools. This list documents execution routes, not content distribution.
