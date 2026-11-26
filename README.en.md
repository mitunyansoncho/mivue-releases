# MIvue

Distribution page for **MIvue**, an econometric software package compatible with EViews. This repository is for distribution only and does not contain source code.

Japanese version: [README.md](README.md)

## Download

Get the latest ZIP from [Releases](https://github.com/takesue-mitsunao/mivue-releases/releases).

- `MIvue-1.1.1-windows-x64.zip` (GUI version, Windows 10/11, 64-bit)
- `MIvue-cli-1.1.1-windows-x64.zip` (command-line version, Windows 10/11, 64-bit)

For the GUI version, extract the ZIP and run `MIvue\MIvue.exe`. No installation is required.
The command-line version can be set up by an AI agent: have your agent read [AGENT_SETUP.md](AGENT_SETUP.md). Command syntax is summarized in [COMMAND_REFERENCE.md](COMMAND_REFERENCE.md) (in Japanese).

## First launch and SmartScreen

Because the executable is unsigned, Windows SmartScreen may show a warning on first launch. In that case, choose "More info" and then "Run anyway".

## Usage essentials

- Objects such as series, equations, and graphs are created inside a workfile (.evwf). The working folder from your previous session is restored automatically.
- You can work with EViews-style commands (`wfcreate` / `series` / `equation ....ls` / `graph` / `wfsave`, etc.), and you can also estimate from the estimation dialogs without using commands.
- New in 0.3.0: the command-line program can attach to an open GUI session (`mivue --live`). An agent can run commands directly in the workfile you have open; changes appear on screen, are recorded with their source, and can be undone.
- New in 0.3.1: forecast standard errors and a forecast evaluation table. Standard errors are available for both static and dynamic forecasts as a series, and graphs can show a ±2 standard-error band. The evaluation table reports RMSE, MAE, MAPE, Theil's inequality coefficient and its decomposition (bias, variance, and covariance proportions). No existing computation results have changed.
- New in 1.0.0: a Stata mode. It reads and writes `.do` and `.dta` files and covers the standard applied workflow in Stata's grammar: data import and reshaping, descriptive statistics, regression (factor variables, robust and cluster standard errors), `logit`/`probit`, instrumental variables, basic panel methods, and basic time series. In the GUI you can switch between EViews mode and Stata mode mid-session; both sides' data and objects are retained, and the most recent estimation is re-run on the other side.
- New in 1.1.0: the chat panel's AI-agent integration now completes inside the app — Claude Code, Codex, and agy can be installed and signed in from the GUI, and estimation work can be requested in plain words. The GUI was also refined (variable labels shown alongside results and editable on screen, better behaviour in narrow windows), and Stata mode gained %t format display and the `format` command. The GUI ZIP now bundles the command-line program (`MIvue\cli\mivue.exe`).
- New in 1.1.1: internal restructuring only — all circular dependencies between modules were eliminated. Estimation numerics, command grammar, storage formats, and GUI behaviour are unchanged from 1.1.0.
- If you are new to MIvue, start with the [quick manual](MANUAL.md) (in Japanese). For details, see also `README_DIST.md` included in the distribution ZIP.

## Terms of use

- Distributed free of charge to students and researchers. The full terms are in the [Terms of Use (TERMS)](TERMS.md): free for educational and research purposes; commercial use requires prior permission; redistribution is not permitted.
- EViews is a trademark of IHS Global Inc. Stata is a trademark of StataCorp LLC. This software is an independent implementation, unrelated to the providers of either product. "Compatible" means that, given the same input, it returns the same result within stated numerical tolerances (as defined in the verification report).

## Verification

- Verification was performed on version 0.1.0. Versions 0.2.0 and 0.3.0 changed no numerics, command grammar, or storage formats (as stated in each release's notes). Version 0.3.1 likewise changes no existing numerics, command grammar, or storage formats; its added forecast standard errors and evaluation table were verified by independent recomputation from the formulas in the official documentation (User's Guide, Chapter 25). Version 1.0.0 also changes no existing EViews-side numerics, grammar, or storage formats; the numerics of the added Stata mode were verified against the printed examples in Stata's official documentation. Version 1.1.0 likewise changes no existing estimation numerics or storage formats; its additions and fixes centre on GUI usability and display and on Stata format display. Version 1.1.1 is internal restructuring only and changes no existing numerics, grammar, storage formats, or GUI behaviour.
- Verified scope: linear regression and diagnostic statistics; unit root and cointegration tests; instrumental variables and GMM; the ARCH family; discrete choice and GLM; panel estimation, panel unit root and cointegration; VAR and system estimation; state space models; and file formats (reading and writing .wf1, reading .edb), among others. The evidence is of four kinds: cell-by-cell collation against published tables and printed examples, independent recomputation from the formulas in the official documentation, the full test suite, and an independent black-box audit.
- Limitations: areas without published reference values rest on independent recomputation, and collation against a real EViews installation is by spot checks. Random number sequences do not match EViews. The reference specification is centred on EViews 12. See the verification report for the full scope and limitations.

## Development

The author fixed the specifications in advance and made all design judgements and acceptance decisions; AI agents carried out the implementation and verification work. The expected values used in verification come from published tables and the formulas in the official documentation. Rulings on the six discrepancies found by the independent audit (their reproduction, correction, and re-verification) and on conflicts between reference materials were made by the author. The audit was performed as a black-box check, without access to the implementation code, by an AI that did not share context with the AI that carried out the implementation.

## Tables and sources

P-values and critical values are computed from the following published sources (the sources are also recorded in the program).

- MacKinnon (1996), MacKinnon–Haug–Michelis (1999): response surfaces for unit root and cointegration tests. Distribution is made with the consent of the author, James G. MacKinnon (free distribution to students and researchers).
- Levin–Lin–Chu (2002), Im–Pesaran–Shin (2003), Pedroni (1999, 2004), Pesaran (2007): adjustment terms and critical values for panel tests.

## Citation

If you use MIvue in your research, please cite it as follows.

English:
Takesue, M. (2026). MIvue (Version 1.1.1) [Computer software]. https://github.com/takesue-mitsunao/mivue-releases

Japanese:
武末光尚 (2026)『MIvue 1.1.1 — EViews 互換計量経済エンジン』. https://github.com/takesue-mitsunao/mivue-releases

A verification report documenting the scope, methods, and results of the verification is also available.
Takesue, M. (2026). MIvue Verification Report: Scope, Methods, and Results of Verifying an EViews-Compatible Econometric Engine. Zenodo. https://doi.org/10.5281/zenodo.24412624

Author: Mitsunao Takesue (ORCID: [0009-0001-6500-7743](https://orcid.org/0009-0001-6500-7743))

## Update check

MIvue checks this repository for the latest release at startup and via "Help → Check for updates". It never downloads or overwrites anything automatically.
