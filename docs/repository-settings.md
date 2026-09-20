# GitHub Repository Settings

After uploading this project to GitHub, configure the following settings.

## Repository Name

`fitflow-redesign`

## Visibility

Use the visibility required by your lecturer/course. If no restriction is given, a public repository is
easier to submit through a link.

## Main Branch Protection

GitHub UI:

1. Open the repository.
2. Go to **Settings**.
3. Open **Rules → Rulesets** (or **Branches**, depending on the current GitHub UI).
4. Create a rule/ruleset targeting `main`.
5. Enable:
   - Require a pull request before merging.
   - Block force pushes.
   - Block deletions.
6. Save the rule.

## Secret Handling

- Do not commit `.env` files.
- Do not commit API keys, passwords, Firebase private keys, or database credentials.
- Use `.env.example` only for variable names/placeholders.

## CI/CD

The repository includes `.github/workflows/repository-check.yml`, a basic GitHub Actions workflow
that verifies the required Activity 5 files and folders on every push or pull request.
