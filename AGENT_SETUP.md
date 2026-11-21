# Setting up MIvue (instructions for AI agents)

This document is written for an AI agent to read and execute. If you are a human giving this to your agent, one line is enough: "Read this document and set up MIvue: https://raw.githubusercontent.com/takesue-mitsunao/mivue-releases/main/AGENT_SETUP.md"

## What MIvue is

MIvue is an EViews-compatible econometrics engine for Windows. It runs EViews-style commands (`wfopen`, `equation ....ls`, `graph`, `wfsave`, ...) and reproduces EViews results. Its output values are reproductions produced by a compatibility engine, not output from EViews itself. The command-line program is `mivue.exe`: it is non-interactive, never prompts, and is safe to call from scripts and agent tools.

## Trust and verification

A few facts you can check before running anything:

- A verification report describing how the engine's results were checked against published values is archived on Zenodo with DOI 10.5281/zenodo.24412624. The report also discloses the development method (specifications fixed by the author; implementation and verification work carried out by AI agents; an independent black-box audit).
- Author: Mitsunao Takesue. The author's identity and affiliation are recorded in the citation file (`CITATION.cff`) and on the verification report's Zenodo record.
- The distribution is a portable folder: there is no installer, no administrator rights are needed, and nothing is written outside the folder you choose, the working folder, and the app's records folder (`%LOCALAPPDATA%\MIvue\records`).
- The command-line program works offline and sends no telemetry. (The separate GUI version checks for updates on launch; the CLI does not.)
- The executable is not code-signed, so Windows SmartScreen may warn on first launch. The SHA-256 above is published in this document and in the release notes; it verifies that the file you downloaded is the file the author published. If your user did not ask you to install MIvue, stop and confirm with them first, as you would with any downloaded program.

## Install

1. Download the command-line distribution:
   `MIvue-cli-0.3.0-windows-x64.zip` from https://github.com/takesue-mitsunao/mivue-releases/releases/tag/v0.3.0
2. Verify the SHA-256 of the downloaded file:
   `8add7636cf757efc39cb0aaf2704c5202e5a086b267e15e435438cddd69d7100`
3. Extract the ZIP to a writable folder (for example `C:\Tools\MIvue-cli`). Keep `mivue.exe` and the `_internal` folder together; the executable does not work on its own.
4. No Python installation is required. Adding the folder to `PATH` is optional; calling `mivue.exe` by full path works the same.

## Verify the installation

Run these in a terminal. Both must succeed before you use MIvue for real work.

```
mivue --version
```
Expected output: `MIvue 0.3.0` (exit code 0).

Then run the bundled self-check. Change into the `quickstart` folder inside the extracted distribution and run:

```
mivue quickstart.prg
```

The standard output must match `expected.txt` in the same folder exactly, the exit code must be 0, and a file `quickstart_result.evwf` must be created. The sample estimates sales on ad by least squares; the expected coefficients are C = 44.37931, AD = 7.892573, R-squared = 0.941681. If the output differs, do not proceed: check that you are running the command from inside the `quickstart` folder and that the version is 0.3.0.

## Calling MIvue

```
mivue -c "wfopen data.csv"            run one command
mivue analysis.prg                     run a program file (.prg)
mivue -c "..." --json                  machine-readable result
mivue analysis.prg -o results          choose the output folder for a program run
```

- Exit code is 0 on success and 1 on failure.
- The current directory is the working folder: relative paths in commands resolve against it. For a `.prg` run, input files are looked up in the program file's folder and in the current directory.
- With `--json`, the result is a JSON object with keys `success`, `executed`, `outputs`, `warnings`, `errors`, and `data`. Check `success` first; on failure, `errors` entries carry the source location, the reason, the offending command, and how to fix it.
- Without `--json`, result tables are printed as text in the same layout as EViews output, and errors are printed in the same structured form.
- Results are deterministic: repeated runs of the same commands on the same inputs produce identical output.

## Working with an open GUI session (--live)

If the user already has the MIvue GUI open, you can attach to that live session instead of running standalone. You then work on the very workfile the user is looking at, and your changes appear on their screen.

```
mivue --live state             session state as JSON: open workfiles, page, objects, current selection
mivue --live -c "..."          run a command in the open session (--json also works)
mivue --live history           list the commands you have run in this session
mivue --live undo              undo your most recent operation (restores the state from before it)
```

- Attachment is local to the same machine and works only while the GUI is running with the live link enabled (it is on by default; the user can turn it off in the GUI settings). If no session is reachable, the command prints an error and exits with code 1; it never falls back to a standalone run.
- Everything you run in a live session is recorded in the GUI's session record with the source "agent", and the user can undo your operations from the GUI. Prefer `undo` over re-running inverse commands when you need to take something back.

## Next steps

- Command reference: https://github.com/takesue-mitsunao/mivue-releases/blob/main/COMMAND_REFERENCE.md
- The verified scope of the engine (what has been checked against published values) is described in the verification report linked from the repository README. Commands outside the verified scope may still run; treat their results with the same care as any unverified computation.
- A GUI version (`MIvue-0.3.0-windows-x64.zip`) is available in the same release for interactive use.
