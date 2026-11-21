# MIvue

Official distribution page for **MIvue**, an econometric software package compatible with EViews. This repository is for distribution only and does not contain source code.

Japanese version: [README.md](README.md)

## Download

Get the latest ZIP from [Releases](https://github.com/mitunyansoncho/mivue-releases/releases).

- `MIvue-0.2.0-windows-x64.zip` (GUI version, Windows 10/11, 64-bit)
- `MIvue-cli-0.2.0-windows-x64.zip` (command-line version, Windows 10/11, 64-bit)

For the GUI version, extract the ZIP and run `MIvue\MIvue.exe`. No installation is required.
The command-line version can be set up by an AI agent: have your agent read [AGENT_SETUP.md](AGENT_SETUP.md). Command syntax is summarized in [COMMAND_REFERENCE.md](COMMAND_REFERENCE.md) (in Japanese).

## First launch and SmartScreen

Because the executable is unsigned, Windows SmartScreen may show a warning on first launch. In that case, choose "More info" and then "Run anyway".

## Usage essentials

- Objects such as series, equations, and graphs are created inside a workfile (.evwf). The working folder from your previous session is restored automatically.
- You can work with EViews-style commands (`wfcreate` / `series` / `equation ....ls` / `graph` / `wfsave`, etc.), and you can also estimate from the estimation dialogs without using commands.
- If you are new to MIvue, start with the [quick manual](MANUAL.md) (in Japanese). For details, see also `README_DIST.md` included in the distribution ZIP.

## Terms of use

- Distributed free of charge to students and researchers.
- EViews is a trademark of IHS Global Inc. This software is an independent implementation, unrelated to the provider of EViews; "compatible" refers to identical functionality and computation results.

## Tables and sources

P-values and critical values are computed from the following published sources (the sources are also recorded in the program).

- MacKinnon (1996), MacKinnon–Haug–Michelis (1999): response surfaces for unit root and cointegration tests. Distribution is made with the consent of the author, James G. MacKinnon (free distribution to students and researchers).
- Levin–Lin–Chu (2002), Im–Pesaran–Shin (2003), Pedroni (1999, 2004), Pesaran (2007): adjustment terms and critical values for panel tests.

## Citation

If you use MIvue in your research, please cite it as follows.

English:
Takesue, M. (2026). MIvue (Version 0.2.0) [Computer software]. https://github.com/mitunyansoncho/mivue-releases

Japanese:
武末光尚 (2026)『MIvue 0.2.0 — EViews 互換計量経済エンジン』. https://github.com/mitunyansoncho/mivue-releases

A verification report documenting the scope, methods, and results of the verification is also available.
Takesue, M. (2026). MIvue Verification Report: Scope, Methods, and Results of Verifying an EViews-Compatible Econometric Engine. Zenodo. https://doi.org/10.5281/zenodo.24412624

## Update check

MIvue checks this repository for the latest release at startup and via "Help → Check for updates". It never downloads or overwrites anything automatically.
