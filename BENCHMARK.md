# Computer and browser use benchmark

A small, manual smoke benchmark for an AI agent controlling Chrome and native Windows apps. Recorded results are in [RESULTS.md](RESULTS.md).

## Scope

Exercise browser form controls, web navigation, native text editing, arithmetic, and a Save As dialog. Use synthetic text and a new document. Preserve existing user documents and tabs.

This tests local UI automation. It does not test remote SSH or Tailscale access, machine throughput, or long-running autonomous workflows.

## Setup

- Windows with Chrome, Calculator, and Notepad installed.
- Browser automation connected to Chrome.
- Native Windows automation with accessibility inspection, screenshots, clicks, and keyboard input.
- An existing writable output directory for the saved text sample.
- One run per task. Execute B1-B3 in order on the same form and N2-N3 on the same new document.

## Tasks and acceptance criteria

| ID | Task | Steps | Pass condition |
| --- | --- | --- | --- |
| B1 | Text, multiline entry, checkbox | Open https://www.selenium.dev/selenium/web/web-form.html. Enter `Benchmark Alpha` in Text input. Enter `Synthetic browser benchmark.` followed by a newline and `Second line: 42.` in Textarea. Check Default checkbox. | The visible/accessibility state contains both exact text values and the checkbox is checked. |
| B2 | Dropdown | Select `Two` in Dropdown (select). | The control reports `Two` as selected. |
| B3 | Correct text and change radio selection | Replace Text input with `Benchmark Beta`. Select Default radio. | The replacement text is exact, Default radio is selected, and Checked radio is no longer selected. |
| B4 | Search and navigation | Open https://en.wikipedia.org/wiki/Main_Page. Search for `tmux` through the page search field. | The browser reaches the tmux article, with matching page title, heading, and `/wiki/Tmux` URL. |
| N1 | Calculator | Launch Calculator in Standard mode. Calculate `7 × 8`. | The display shows `56`. |
| N2 | Notepad editing | Launch Notepad and create a new document. Focus its editor. Type `Benchmark Alpha: 42 items.`. Select all and replace it with `Benchmark Beta: 84 items.`. | The editor contains the exact replacement text. |
| N3 | Native Save As | Save the N2 document as `computer-use-benchmark-sample.txt` in the output directory using the Save As dialog. | The dialog closes, the document title shows the filename, and reading the saved file confirms the expected text. |

## Execution rules

Use UI actions to perform the tasks. Accessibility inspection and screenshots may verify results; do not inject page scripts to change form state or write the Notepad sample directly through the filesystem. A filesystem read may independently verify the saved sample.

Refresh UI state after actions before selecting new targets. If accessibility targeting fails, inspect a screenshot and attempt a grounded recovery. Record errors and recoveries even when the final task passes.

Do not submit the Selenium form, log into websites, or modify Wikipedia. No personal input data is needed.

## Measurement

Record task outcome, elapsed wall-clock time, and any recovery. Time includes tool calls and agent reasoning between calls. Startup and initial page observation are included where the timer begins before launch/navigation. Setup and documentation reading before the task timer are excluded.

The original run used manual timer boundaries. For B1-B3 the elapsed value was captured at the beginning of the next action cell, after inspecting the previous result. The other timers were captured in the final UI-observation cell. N3's independent filesystem check occurred afterward. Preserve these details when comparing the recorded run; future runs should use consistent start and end boundaries.

These tasks have different lengths and dependencies. Do not interpret their timings as a controlled browser-versus-desktop speed comparison. Repeated trials and a larger task suite are needed for a reliability or performance claim.
