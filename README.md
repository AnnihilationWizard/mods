# mods

<p align="center">
  <strong>One place for every AnnihilationWizard mod.</strong><br>
  A Claude Code plugin marketplace: add it once, then install the mods you want.
</p>

<p align="center">
  <a href="#mods"><img src="https://img.shields.io/badge/Mods-5-green.svg" alt="5 mods"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="MIT License"></a>
  <a href=".claude-plugin/marketplace.json"><img src="https://img.shields.io/badge/Type-Plugin%20marketplace-6E7681.svg" alt="Plugin marketplace"></a>
</p>

<p align="center">
  <a href="#mods">Mods</a> ·
  <a href="#install">Install</a> ·
  <a href="#update">Update</a> ·
  <a href="#adding-a-mod">Adding a mod</a> ·
  <a href="#project-structure">Project structure</a>
</p>

Each mod lives in its own repository, with its own version, changelog and issues. This repository only holds the catalog in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json), so Claude Code fetches each mod from its own repository when you install it. You can install, update or turn off each mod on its own.

## Mods

| Mod | What it does |
| --- | --- |
| [quiet-diffs](https://github.com/AnnihilationWizard/quiet-diffs) | Shows file edits as one-line summaries instead of full diffs |
| [you-should-know-relay](https://github.com/AnnihilationWizard/you-should-know-relay) | Forwards each you-should-know heads-up to Claude, so you no longer paste it in yourself |
| [chrome-close](https://github.com/AnnihilationWizard/chrome-close) | Allows one headless Chrome at a time and flags helpers left running |
| [secret-guard](https://github.com/AnnihilationWizard/secret-guard) | Adds a gitleaks pre-commit hook to each Git project and worktree you open, with pre-commit, Lefthook or Husky |
| [node-close](https://github.com/AnnihilationWizard/node-close) | Ends the Node processes a finished command left running, such as test runners and esbuild; `/node-close` finds orphaned ones |

### quiet-diffs

**Follow what Claude did, not every line it changed.** Each `Edit`, `Write` and `NotebookEdit` result shows the path and `+added −removed` instead of the whole diff. `/quiet-diffs` brings the full diff back for the session.

![Side-by-side Claude Code transcript: on the left an edit shows a full red and green diff, on the right the same edit shows one dim summary line](https://raw.githubusercontent.com/AnnihilationWizard/quiet-diffs/main/docs/images/preview.svg)

### you-should-know-relay

**Claude hears the heads-up too.** When the built-in you-should-know plugin shows a heads-up above the prompt, the mod hands the same heads-up to Claude, while it stays on your screen as before. Use it in a terminal session: as of Claude Code 2.1.302, the Code tab of the Claude desktop app shows no heads-ups, so there is nothing to forward.

![Side-by-side Claude Code session: on the left a heads-up is pasted in by hand, on the right the same heads-up is forwarded to Claude with a confirming toast](https://raw.githubusercontent.com/AnnihilationWizard/you-should-know-relay/main/docs/images/preview.svg)

### chrome-close

**One headless Chrome at a time.** Before Claude runs a Bash or PowerShell command that may start a headless Chrome, the mod checks for one still running, or for helpers a closed one left behind, and refuses the launch with the PIDs and the command to end them. Commands that only read or search, such as `grep playwright` or `Select-String`, still run. Works on macOS, Linux and Windows, and guards only commands Claude runs.

![Side-by-side Claude Code session: on the left three test runs leave headless Chrome processes holding 1.2 GB; on the right the second launch is refused with the PIDs to end, Claude kills them and the next run goes ahead](https://raw.githubusercontent.com/AnnihilationWizard/chrome-close/main/docs/images/preview.svg)

### secret-guard

**Secrets stop at the commit.** When a session starts in a Git project, or Claude enters a worktree, the mod checks for a gitleaks pre-commit hook and adds one if it is missing, with pre-commit, Lefthook, or Husky in JS/TS projects that use it. With both pre-commit and Lefthook installed, `/secret-guard-use` picks one, switches the projects it set up and can remove the other. It fixes worktrees missing their config, reminds you to commit the hook file, and never rewrites an existing config or adds dependencies unless you opt in. Needs `gitleaks` and pre-commit or Lefthook. Tested on macOS and Windows.

![Side-by-side terminal: on the left a fresh clone has no hook and a commit with a GitHub token lands in history; on the right secret-guard sets up a gitleaks hook when Claude Code starts and the same commit is stopped](https://raw.githubusercontent.com/AnnihilationWizard/secret-guard/main/docs/images/preview.svg)

### node-close

**Finished commands leave no Node behind.** After a shell command Claude runs finishes, the mod ends the Node processes it started and left running, such as a test runner, its workers and their esbuild service: `SIGTERM`, then `SIGKILL` after a second. Tools such as esbuild count too, background commands are followed until they stop, and processes started on purpose with `&`, `nohup` or a process manager are left alone. `/node-close` lists orphaned Node anywhere, with CPU use, and ends the ones you name. On macOS and Linux it ends only what this Claude Code started, so a Node another session or you start meanwhile is left alone; on Windows any new orphaned Node still counts.

![Side-by-side Claude Code session: on the left three finished test runs leave Node runners, workers and esbuild holding 940 MB; on the right each run's leftovers are closed when it finishes and a toast reports the count](https://raw.githubusercontent.com/AnnihilationWizard/node-close/main/docs/images/preview.svg)

## Install

In a Claude Code **terminal** session, add the marketplace once:

```
/plugin marketplace add AnnihilationWizard/mods
```

Then install the mods you want:

```
/plugin install quiet-diffs@annihilationwizard-mods
/plugin install you-should-know-relay@annihilationwizard-mods
/plugin install chrome-close@annihilationwizard-mods
/plugin install secret-guard@annihilationwizard-mods
/plugin install node-close@annihilationwizard-mods
```

1. Choose a scope for each mod. The **user** scope loads the mod in every session.
2. Claude Code confirms `Plugin is now active.` No restart is needed.

<details>
<summary>Add the marketplace and install in one step</summary>

```
/plugin install quiet-diffs --marketplace AnnihilationWizard/mods
```

Answer `y` to add the `github:AnnihilationWizard/mods` marketplace.

</details>

> [!TIP]
> If you already load a mod with `--plugin-dir` or `CLAUDE_CODE_PLUGIN_DIRS`, remove it from there after installing it here, or it loads twice.

## Update

Refresh the catalog, then update a mod:

```bash
claude plugin marketplace update annihilationwizard-mods
claude plugin update quiet-diffs@annihilationwizard-mods
```

Run `/reload-plugins` in an open session to pick up the new version. You can also choose **Update now** in the `/plugin` **Installed** tab.

To update automatically, open `/plugin` > **Marketplaces** and enable auto-update for `annihilationwizard-mods`. Auto-update is off by default for marketplaces other than the official ones.

## Adding a mod

1. Publish the mod in its own repository with a `.claude-plugin/plugin.json`.
2. Add an entry to `plugins` in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

   ```json
   {
     "name": "my-mod",
     "description": "What the mod does, in one line",
     "source": { "source": "url", "url": "https://github.com/AnnihilationWizard/my-mod.git" }
   }
   ```

3. Check the catalog, then push:

   ```bash
   claude plugin validate --strict .
   ```

A mod's update reaches users only when its `version` in `.claude-plugin/plugin.json` changes, so bump it with each release. Updating a mod does not need a change here.

## Project Structure

```text
.claude-plugin/   marketplace.json, the catalog of mods
README.md         this page
LICENSE           MIT License
```

The mod folders next to these files on the author's machine are separate Git repositories and are ignored by this one.

## License

[MIT License](LICENSE). Copyright © 2026 [AnnihilationWizard](https://github.com/AnnihilationWizard).
