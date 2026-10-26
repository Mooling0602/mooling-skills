---
name: ddgs
description: Search the internet for real-time information with the ddgs CLI — latest versions, news, docs, current events, and any other lookup that needs live web results. Use when a question cannot be answered from local files or prior knowledge, or when the user says to search / check online.
metadata:
  source: https://github.com/Mooling0602/mooling-skills/tree/main/skills/ddgs
---

# ddgs

Use `ddgs` CLI tool to search the internet. The executable is located in `~/.local/bin` and installed with command `uv tool install <package>`. If could not execute `ddgs`, try to find it and reinstall with uv when it is missing.

## Usage

Use `ddgs text -q <content>` to search for internet informations.

For example, if you want to search what's the latest version of game Minecraft, just execute:

```sh
ddgs text -q "Minecraft latest version"
```

Then you'll get the output of the search result.

For more usages, execute `ddgs --help` to get more details.
