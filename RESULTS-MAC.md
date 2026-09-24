# Mac benchmark results

Run date: September 24, 2026. Procedure: [BENCHMARK.md](BENCHMARK.md), at commit `205e4c7`. Run identifier: `mac-m4-2026-09-24`.

All seven tasks reached their acceptance criteria, using the macOS equivalents for the three native Windows tasks. Six tasks completed without recovery. TextEdit's initial launch inspection timed out; reacquiring the app recovered it. No task remains unresolved.

## Environment and substitutions

This was a local Apple M4 Mac, model identifier Mac16,1, with 16 GiB RAM, running macOS 26.6.2 on arm64. Chrome was already running. Other user applications remained open; this was not an isolated performance test.

Chrome version was 153.0.8010.54 (build 8010.54), Calculator 12.0 (225), and TextEdit 1.20 (415). The agent used the Codex desktop `cua_repl` API, Chrome's connected extension and accessibility actions, and macOS native accessibility actions. No page scripts, direct filesystem writes of the sample, or screenshot-coordinate fallbacks were used to complete the tasks.

B1–B4 used the original websites and exact inputs. N1 used macOS Calculator in Basic mode, the counterpart to Windows Standard mode. N2 used a new TextEdit document instead of Notepad. N3 converted that document from rich text to plain text and used the native Save As dialog to save it as `computer-use-benchmark-sample.txt`. These are functional substitutions, not identical application workflows.

## Measurements

| ID | Task | Elapsed seconds | Outcome | Evidence |
| --- | --- | ---: | --- | --- |
| B1 | Text, multiline entry, checkbox | 14.895 | Pass | Text input was `Benchmark Alpha`; Textarea contained both exact lines; Default checkbox value was 1. |
| B2 | Dropdown selection | 0.222 | Pass | Dropdown value was `Two`, and its Two option was selected. |
| B3 | Replace text and change radio | 0.239 | Pass | Text input was `Benchmark Beta`; Default radio value was 1 and Checked radio value was 0. |
| B4 | Wikipedia search | 12.732 | Pass | Entered `tmux` in the page search field and pressed Return; title was `tmux - Wikipedia`, heading was `tmux`, and URL was `https://en.wikipedia.org/wiki/Tmux`. |
| N1 | Calculator: 7 × 8 | 28.636 | Pass | Accessibility state showed last expression `7×8` and display `56`. |
| N2 | TextEdit typing and replacement | 26.750 | Pass after retry | Both initial and replacement text were observed; final editor value was exactly `Benchmark Beta: 84 items.`. |
| N3 | Native Save As | 33.586 | Pass | Save dialog closed; window title showed `computer-use-benchmark-sample.txt`; an independent filesystem read confirmed the exact 25-character text. |

The sum of task timers was 117.060 seconds. This excludes gaps between tasks, setup, result writing, environment inspection, and cleanup. It is not the total session duration.

## Timer boundaries

Each timer began immediately before the first task action and ended immediately after the final acceptance-state capture returned in the same action cell. B1, B4, N1, and N2 include tab creation or app launch and initial observation. B2 and B3 reuse the observed form state and run their actions and final observation within one cell. Reasoning between calls within a task is included; planning before starting a timer and interpretation after ending it are excluded. N3's independent filesystem verification occurred after its timer ended, matching the original run's treatment of that check.

B1 also includes automatically emitted first-use browser documentation and the time spent reading it before entering the form. This was not removed from the measured result. B2 and B3 use fast accessibility value changes and have no between-call reasoning interval. The original Windows B1–B3 timers ended at the beginning of the next action cell, after result inspection, so their boundaries differ. The very small B2/B3 values must not be presented as proof of a Mac hardware speedup. Decimal precision records the timer values, not repeatability.

## Recovery and native workflow details

The first `cua.getApp('com.apple.TextEdit')` call returned `Computer Use server error -10005: timeoutReached`. A second call succeeded and exposed TextEdit's Open dialog. Clicking New Document created the benchmark document. The N2 timer includes the failed launch observation and recovery. The timeout's cause is unknown.

TextEdit initially used rich text and automatically gave the new document an iCloud-backed Untitled path. During N3, Format → Make Plain Text converted it to text. Command-Option-Shift-S opened Save As. The filename was entered through the dialog, Command-Shift-G selected the task's local output directory, and clicking Save succeeded on the first attempt. No user document was edited. Cleanup found no remaining `Untitled.rtf` or `Untitled.txt` at the observed auto-save location.

The saved sample is included at [results/mac-m4-2026-09-24/computer-use-benchmark-sample.txt](results/mac-m4-2026-09-24/computer-use-benchmark-sample.txt). That repository copy was made only after the UI-created original was independently verified. The benchmark tabs were closed and the Calculator and TextEdit applications launched for this run were quit.

## Interpretation pending the third computer

The Mac matched the existing Windows run's functional result: seven of seven tasks completed. Its one recovered task was TextEdit startup, while the existing Windows run recovered Calculator accessibility and Save As targeting. Calculator and Save As on this Mac exposed usable accessibility controls throughout.

This is one short run per task, with different native applications, automation backends, action batching, and timer boundaries. It demonstrates that the specified simple workflows worked on this Mac; it does not establish general platform reliability or machine throughput. The third computer's results were not present when this report was prepared. A three-machine comparison should distinguish task completion and recovery behavior from the less directly comparable elapsed times.
