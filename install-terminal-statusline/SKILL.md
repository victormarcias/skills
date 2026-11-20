---
name: install-terminal-statusline
description: Install the Claude Code status line that shows project, git status, model, a context bar with a red marker at 100K tokens, and tokens used in the terminal. Writes ~/.claude/statusline.sh and registers it in ~/.claude/settings.json. Use when the user asks to install, restore or fix the terminal status line, or sees no tokens in the terminal.
model: haiku
---

# Install terminal status line

Writes the status line script and registers it in the user settings. The whole
script is in this file: nothing is copied from anywhere else.

1. **Write the script.** Create `~/.claude/statusline.sh` with exactly the
   content below and make it executable (`chmod +x`). If the file already
   exists and differs, show a short diff and ask once before overwriting. If it
   is a symlink, remove the link first and write a regular file.
2. **Register it.** In `~/.claude/settings.json` set `statusLine` to
   `{"type": "command", "command": "~/.claude/statusline.sh"}`. Merge with
   `jq` and keep every other key. If the file doesn't exist, create it with
   only that key. If `statusLine` already has that value, change nothing.
3. **Check it.** Run the script with this sample input and show the output:
   `{"workspace":{"project_dir":"/a/b/demo"},"model":{"display_name":"M"},`
   `"context_window":{"total_input_tokens":1500,"total_output_tokens":20,`
   `"context_window_size":200000}}`
   It must print
   `demo · M · ░░░░░░░░░░▼░░░░░░░░░░ ↑1.5k ↓20`, with demo in white, ↑1.5k in
   cyan, ↓20 in magenta and the ▼ in red (no git segment, since the sample
   folder is not a repo). If `jq` is missing, say so and stop.
4. **Report** in one line whether the script and the setting were created,
   updated or already in place. The status line shows up in the next session.

Inside a git repo, the line also shows the branch and the counts of staged
(`+`), modified (`~`) and untracked (`?`) files, plus `⇡`/`⇣` for commits
ahead of or behind the remote.

The bar spans the whole context window. The red ▼ marks 100K tokens, where
the quality of the answers starts to drop, and the part of the bar past it
turns red.

```bash
#!/usr/bin/env bash
# Claude Code status line: project, git status, model, context bar, tokens.
# Claude Code sends the session data as JSON on stdin.
input=$(cat)

project=$(echo "$input" |
  jq -r '(.workspace.project_dir // .cwd // "") | split("/") | (last // "")')
model=$(echo "$input" | jq -r '.model.display_name // "?"')
tokens_in=$(echo "$input" | jq -r '.context_window.total_input_tokens // 0')
tokens_out=$(echo "$input" | jq -r '.context_window.total_output_tokens // 0')
size=$(echo "$input" | jq -r '.context_window.context_window_size // 200000')
[ "$size" -gt 0 ] 2>/dev/null || size=200000

# Git segment: branch, staged (+), modified (~), untracked (?), ahead/behind.
dir=$(echo "$input" | jq -r '.workspace.current_dir // .cwd // ""')
git_seg=""
if [ -n "$dir" ] &&
  status=$(git -C "$dir" --no-optional-locks status --porcelain -b 2>/dev/null)
then
  head=$(echo "$status" | head -1)
  branch=$(echo "$head" |
    sed -e 's/^## //' -e 's/^No commits yet on //' -e 's/\.\.\..*//' \
      -e 's/ \[.*//')
  ahead=$(echo "$head" | sed -n 's/.*ahead \([0-9]*\).*/\1/p')
  behind=$(echo "$head" | sed -n 's/.*behind \([0-9]*\).*/\1/p')
  counts=$(echo "$status" | tail -n +2 | awk '
    /^\?\?/ { t++; next }
    { if (substr($0, 1, 1) != " ") s++; if (substr($0, 2, 1) != " ") m++ }
    END { printf "%d %d %d", s, m, t }')
  read -r staged modified untracked <<< "$counts"
  git_seg=" · ${green}${branch}${reset}"
  [ -n "$ahead" ] && git_seg+=" ⇡$ahead"
  [ -n "$behind" ] && git_seg+=" ⇣$behind"
  [ "$staged" -gt 0 ] && git_seg+=" ${green}+$staged${reset}"
  [ "$modified" -gt 0 ] && git_seg+=" ${yellow}~$modified${reset}"
  [ "$untracked" -gt 0 ] && git_seg+=" ${yellow}?$untracked${reset}"
fi

width=20          # cells in the bar
dumb_zone=100000  # tokens where the red arrow sits
red=$'\033[31m'
white=$'\033[97m'
green=$'\033[32m'
yellow=$'\033[33m'
cyan=$'\033[36m'
magenta=$'\033[35m'
reset=$'\033[0m'

# 1234 -> 1.2k, 1234567 -> 1.2M
fmt() {
  awk -v n="$1" 'BEGIN {
    if (n >= 1000000) printf "%.1fM", n / 1000000
    else if (n >= 1000) printf "%.1fk", n / 1000
    else printf "%d", n
  }'
}

# Filled cells up to the used tokens, red past the arrow, arrow at 100K.
marker=$(( dumb_zone * width / size ))
filled=$(( tokens_in * width / size ))
bar=""
for (( i = 0; i < width; i++ )); do
  [ "$i" -eq "$marker" ] && bar+="${red}▼${reset}"
  if [ "$i" -ge "$filled" ]; then
    bar+="░"
  elif [ "$i" -ge "$marker" ]; then
    bar+="${red}█${reset}"
  else
    bar+="█"
  fi
done

echo "${white}${project}${reset}$git_seg · $model · $bar" \
  "${cyan}↑$(fmt "$tokens_in")${reset}" \
  "${magenta}↓$(fmt "$tokens_out")${reset}"
```
