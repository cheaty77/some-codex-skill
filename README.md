# some-codex-skill

A personal inbox for interesting Codex/agent skills found on GitHub.

## Add a skill from your phone

1. Open **Issues** in this repository.
2. Choose **Add a skill**.
3. Paste a GitHub directory URL such as:
   `https://github.com/owner/repo/tree/main/path/to/skill`
4. Submit the issue.
5. GitHub Actions imports that directory into `skills/` and closes the issue when successful.

## Imported folder naming

Skills are stored as:

`skills/<owner>--<repo>--<skill-folder>/`

This avoids collisions when different repositories contain skills with the same folder name.

## Notes

- The importer is intended for public GitHub repositories.
- It imports a directory, not an entire repository.
- An existing destination is never overwritten automatically.
- Each imported skill includes `_source.json` with its source repository, branch, path, URL, and source commit.
