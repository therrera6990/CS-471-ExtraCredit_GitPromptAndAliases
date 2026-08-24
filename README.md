# Git Prompt and Aliases

## Author
Thomas Herrera

## Overview

This project customizes my **Bash terminal prompt** to display useful Git information and creates several **Git aliases** for commonly used commands.

The customized prompt displays:

- Current working directory
- Current Git branch
- Modified or staged file indicator
- Untracked file indicator
- Remote tracking branch
- Number of commits ahead or behind the remote branch

The project also includes three Git aliases:

- **git gs** – Git status
- **git gd** – Git diff
- **git gl** – Git log with graph

---

## Environment

- **Operating System:** Linux on Onyx
- **Terminal:** Bash inside Visual Studio Code
- **Shell:** `/bin/bash`

---

## Original Terminal

Before making any modifications, my terminal used the default Onyx Bash prompt.

Example:

```text
[thomasherrera@onyx CS-471-ExtraCredit_GitPromptAndAliases]$
```

**Screenshot**

Add original terminal screenshot here.

---

## Bash Prompt Customization

The Bash prompt was customized by modifying:

```bash
~/.bashrc
```

The following code was added to the bottom of the `.bashrc` file:

```bash
# CS471 Git prompt customization

parse_git_branch() {
    git branch 2>/dev/null | sed -n '/^\*/s/^\* //p'
}

git_status_symbols() {
    local symbols=""

    if ! git diff --quiet 2>/dev/null || ! git diff --cached --quiet 2>/dev/null; then
        symbols="${symbols}*"
    fi

    if [ -n "$(git ls-files --others --exclude-standard 2>/dev/null)" ]; then
        symbols="${symbols}%"
    fi

    echo "$symbols"
}

git_remote_status() {
    local branch
    local upstream
    local behind
    local ahead

    branch=$(git symbolic-ref --short HEAD 2>/dev/null) || return
    upstream=$(git rev-parse --abbrev-ref "@{upstream}" 2>/dev/null) || return

    read behind ahead <<< "$(git rev-list --left-right --count "$upstream...HEAD" 2>/dev/null)"

    if [ "$ahead" -gt 0 ]; then
        echo -n " +$ahead"
    fi

    if [ "$behind" -gt 0 ]; then
        echo -n " -$behind"
    fi

    echo -n " $upstream"
}

PS1='\n\[\e[33m\]\w\[\e[0m\] \[\e[36m\]($(parse_git_branch) $(git_status_symbols)$(git_remote_status))\[\e[0m\]\n$ '
```

After saving the file, the configuration was reloaded using:

```bash
source ~/.bashrc
```

---

## Customized Prompt

Example customized prompt:

```text
~/CS-471-ExtraCredit_GitPromptAndAliases (main * origin/main)
$
```

where:

- `main` = current Git branch
- `*` = modified or staged files exist
- `%` = untracked files exist
- `origin/main` = remote tracking branch
- `+1` = one commit ahead of remote, if applicable
- `-1` = one commit behind remote, if applicable

The prompt also includes a blank line before the next command prompt.

**Screenshot**

Add customized terminal screenshot here.

---

## Git Aliases

Three Git aliases were created using `git config --global`.

### git gs

**Command**

```bash
git config --global alias.gs status
```

**Usage**

```bash
git gs
```

Equivalent command:

```bash
git status
```

**Output**

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  modified: README.md
```

---

### git gd

**Command**

```bash
git config --global alias.gd diff
```

**Usage**

```bash
git gd
```

Equivalent command:

```bash
git diff
```

**Output**

```text
diff --git a/README.md b/README.md
--- a/README.md
+++ b/README.md
```

---

### git gl

**Command**

```bash
git config --global alias.gl "log --oneline --decorate --graph --all"
```

**Usage**

```bash
git gl
```

Equivalent command:

```bash
git log --oneline --decorate --graph --all
```

**Output**

```text
* 1188b7d (HEAD -> main, origin/main, origin/HEAD) Initial commit
```

---

## Alias Verification

The aliases can be verified using:

```bash
git config --global --get-regexp '^alias\.'
```

**Output**

```text
alias.gs status
alias.gd diff
alias.gl log --oneline --decorate --graph --all
```

---

## Alias Summary

| Alias | Command | Purpose |
|:------|:--------|:--------|
| `git gs` | `git status` | Shows repository status |
| `git gd` | `git diff` | Shows file changes |
| `git gl` | `git log --oneline --decorate --graph --all` | Shows Git history |

---

## Screenshots

### Before Customization

Add screenshot of the original terminal here.

### After Customization

Add screenshot showing the customized Bash prompt here.

### Git Aliases

Add screenshots showing:

- `git gs`
- `git gd`
- `git gl`

---

## Resources Used

- Git documentation
- Bash documentation
- CS471 assignment README
- ChatGPT for help with Understanding what the extra credit assignment wanted me to do and troubleshooting with some of the syntax errors I was running into with .bashrc

---

## Notes

- The Bash prompt configuration is stored in `~/.bashrc`.
- The prompt automatically displays the current Git branch.
- `*` indicates modified or staged files.
- `%` indicates untracked files.
- The remote branch is displayed when one is configured.
- Git aliases reduce the amount of typing needed for common Git commands.
- `git gl` provides a compact graphical view of repository history.