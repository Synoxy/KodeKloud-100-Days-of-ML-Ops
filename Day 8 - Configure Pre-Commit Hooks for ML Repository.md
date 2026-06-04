🚨 **Problem:**

The xFusionCorp Industries ML team enforces code quality on every commit via pre-commit. A draft .pre-commit-config.yaml exists in the git repository at /root/code/fraud-detection/, but it does not match the team's standard and pre-commit run --all-files fails against it. Correct the configuration.


A git repository already exists at /root/code/fraud-detection/ with .pre-commit-config.yaml and process.py already tracked. pre-commit is installed system-wide.

The corrected configuration must declare the following five hooks so that pre-commit run --all-files executes every one of them:

- trailing-whitespace, end-of-file-fixer, and check-yaml – All three sourced from the pre-commit/pre-commit-hooks repository, pinned to a current release;
- ruff – Sourced from the astral-sh/ruff-pre-commit repository, pinned to a current release;
- black – Sourced from the psf/black-pre-commit-mirror repository, pinned to a current release.
- Every repository entry in the configuration must include a rev: field.
- Review the existing .pre-commit-config.yaml and correct everything that prevents the hooks above from running.

Once the configuration is correct, register the hooks with git and run them against the tracked files:

   pre-commit install
   pre-commit run --all-files

Tip: pre-commit autoupdate queries each referenced repository and rewrites the rev: pins to the latest released tag. This is the standard way to discover current versions without looking them up by hand.

🛠️ **Solution:**
1. Update the .pre-commit-config.yaml file with below changes.

```text 
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml **#Replace _ with -**
  
  - repo: https://github.com/astral-sh/ruff-pre-commit **#Update repo url**
    rev: v0.15.15 **#Update Version**
    hooks:
      - id: ruff **#Update ID**

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: **26.5.1 #Remove v and update the version**
    hooks:
      - id: black
```

2. Navigate to project folder and run below commands:
  - pre-commit install
  - pre-commit run --all-files

3. You should see below output:

<img width="1125" height="563" alt="image" src="https://github.com/user-attachments/assets/f5bbcd58-47e5-4453-9714-8949274f1041" />

