---

```yaml
name: My First GitHub Actions
```

* **`name`**: The name of this workflow.
* It will show up in the **Actions tab** in GitHub as *“My First GitHub Actions”*.
* This is just a label, has no effect on execution.

---

```yaml
on: [push]
```

* **`on`**: Defines what **triggers** the workflow.
* Here, `[push]` means: **every time you push commits to any branch**, this workflow will run.
* Other triggers could be `pull_request`, `schedule`, etc.

---

```yaml
jobs:
```

* **`jobs`**: A workflow is made of one or more **jobs**.
* Jobs are groups of steps that run on a specific machine (runner).

---

```yaml
  build:
```

* **`build`**: The name of the job (can be anything, e.g., `test`, `deploy`).
* Jobs are defined under `jobs:`.

---

```yaml
    runs-on: ubuntu-latest
```

* **`runs-on`**: Defines the operating system that the job will run on.
* `ubuntu-latest` = GitHub provides a fresh Linux VM (Ubuntu) as the runner.
* Other options: `windows-latest`, `macos-latest`, or self-hosted.

---

```yaml
    strategy:
      matrix:
        python-version: [3.8, 3.9]
```

* **`strategy.matrix`**: Lets you run the same job with **different variations** (like Python versions, Node versions, OSes, etc.).
* Here: the job will run **twice** → one with Python 3.8 and one with Python 3.9.
* `python-version` is a variable you can use later in the steps.

---

```yaml
    steps:
```

* **`steps`**: A job is made up of steps.
* Each step runs either an **action** (reusable piece of code) or a **shell command**.

---

```yaml
    - uses: actions/checkout@v3
```

* **`uses`**: Runs a prebuilt GitHub Action.
* `actions/checkout@v3` → This checks out your repo’s code onto the runner, so the job can access the source code.
* Without this, your job wouldn’t have access to the files in your repository.

---

```yaml
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}
```

* **`name`**: Human-readable name shown in GitHub UI.
* **`uses: actions/setup-python@v2`** → Prebuilt action to install Python on the runner.
* **`with:`** → Passes input options to the action.
* `python-version: ${{ matrix.python-version }}` →

  * Uses the variable from the **matrix** defined earlier (`3.8` and `3.9`).
  * So, the job will install Python 3.8 in one run and 3.9 in another run.

---

```yaml
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest
```

* **`run`**: Executes shell commands directly on the runner.
* `|` means you can write multiple lines of commands.
* Steps here:

  1. Upgrade pip → `python -m pip install --upgrade pip`.
  2. Install pytest → `pip install pytest`.
* This ensures your environment has the dependencies needed for testing.

---

```yaml
    - name: Run tests
      run: |
        cd src
        python -m pytest addition.py
```

* Another step, runs commands.
* **`cd src`** → Moves into the `src` folder (where your code lives).
* **`python -m pytest addition.py`** → Runs pytest on the file `addition.py`.
* If tests fail, the workflow fails.

---

✅ **So in plain English**:

* When you push code →
* GitHub spins up an Ubuntu VM →
* Runs the job twice (Python 3.8 & 3.9) →
* Checks out your code →
* Installs Python →
* Installs dependencies →
* Runs pytest tests on `addition.py`.

---
