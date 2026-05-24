🚨 **Problem Statement: **

The xFusionCorp Industries ML team enforces code quality with ruff and black on every pull request. The project at /root/code/fraud-detection/ currently fails both tools. Make it pass them.

The project at /root/code/fraud-detection/ contains a pyproject.toml and sample sources under src/.

The corrected project must meet the following requirements:

- ruff and black are both configured with a line length of 120.
- ruff lint rule selection includes E, F, W, and I, and is declared under [tool.ruff.lint] – The schema required by ruff 0.1 and later.
- Running ruff check src/ from the project directory exits with status 0.
- Running black --check src/ from the project directory exits with status 0.
- Review the existing configuration and source files, and correct everything that prevents the two commands above from exiting cleanly.

ruff, black, and mypy are already installed.

🛠️ **Solution:**
1. Update **pyproject.toml** as below:

[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 120 **#Updated length to 120**
**Added below 2 lines for lint**
[tool.ruff.lint]
select = ["E", "F","W","I"]

[tool.black]
line-length = 120 **#Updated length to 120**

2. Run the below commands:
- ruff check src/
- black --check src/

You should see **All Checks Passed ✅** as below.