# Benchmark results

The original Windows run is recorded below. The additional macOS run is in [RESULTS-MAC.md](RESULTS-MAC.md).

Run date: September 24, 2026. Procedure: [BENCHMARK.md](BENCHMARK.md).

All seven tasks reached their acceptance criteria. Five completed without a recovery; two native tasks required a fallback or retry. This is a single smoke-test run, not an estimate of general reliability.

## Environment

- Local Windows desktop session.
- Chrome controlled through the connected browser extension and accessibility-based browser actions.
- Calculator and Notepad controlled through the Windows computer-use `@oai/sky` API.
- Browser and application versions were not recorded.
- This run did not exercise SSH or Tailscale, despite those being configured earlier in the session.

## Measurements

| ID | Task | Elapsed seconds | Outcome | Evidence |
| --- | --- | ---: | --- | --- |
| B1 | Text, multiline entry, checkbox | 8.314 | Pass | Exact single-line and multiline values appeared in accessibility state; Default checkbox reported checked. |
| B2 | Dropdown selection | 3.593 | Pass | Dropdown reported `Two`; the corresponding option was selected. |
| B3 | Replace text and change radio | 5.252 | Pass | Text input reported `Benchmark Beta`; Default radio was selected and the previous radio was unselected. |
| B4 | Wikipedia search | 5.276 | Pass | Browser title was `tmux - Wikipedia`, heading was `tmux`, and URL was `https://en.wikipedia.org/wiki/Tmux`. |
| N1 | Calculator: 7 × 8 | 40.828 | Pass with fallback | Screenshot showed `7 × 8 =` and `56`. |
| N2 | Notepad typing and replacement | 22.254 | Pass | Accessibility document text was `Benchmark Beta: 84 items.`. |
| N3 | Save As dialog | 25.949 | Pass after retry | Window title showed `computer-use-benchmark-sample.txt`; an independent filesystem read confirmed `Benchmark Beta: 84 items.`. |

Times include agent reasoning and tool calls. They are not application response-time measurements. See the procedure's measurement section for the original timer boundaries. The decimals reproduce recorded values and do not imply repeatable millisecond precision.

## Errors and recoveries

### Calculator accessibility was unavailable

The first text-only state capture returned a null accessibility object. Attempting to read its tree raised `Cannot read properties of null (reading 'tree')`. A capture requesting both text and a screenshot still returned no accessibility data, but the screenshot was usable.

The agent switched to screenshot-grounded clicks for 7, multiplication, 8, and equals, refreshing the screenshot after each click. The result was correct. The observed failure does not establish why accessibility was missing or whether it would recur.

### Save As initially targeted the parent window incorrectly

The Save As controls appeared in the accessibility tree and setting the filename succeeded. Clicking Save by element index was rejected because the point was over the dialog's Save control rather than the expected parent window. The file did not yet exist.

The agent inspected the window inventory, activated Notepad, captured fresh screenshots, and clicked Save using the observed coordinates. The dialog closed and the saved file contained the expected text. One Save click failed; the subsequent coordinate retry succeeded.

## Assessment

Browser control completed all four tasks without observed retries. The form exposed clear semantic controls, and navigation through Wikipedia search reached the intended article.

Native Windows control also completed all three tasks. Notepad exposed useful accessibility data for editing and file entry. Calculator required visual interaction, and the modal Save As dialog required recovery from a targeting error.

In this run, the browser tasks were smoother. The native issues were recoverable, but they added observation and targeting work. The task sets differ, so the timing gap alone cannot establish an intrinsic speed advantage.

## Limits

- One trial per task, with only seven short tasks.
- B1-B3 shared a form; N2-N3 shared a document. These are not independent trials.
- No authentication, uploads, downloads, drag-and-drop, canvas interaction, or complex multi-application workflow was tested.
- No remote-session behavior, locked desktop, unattended reliability, or network-failure recovery was tested.
- No standardized model comparison, human baseline, or repeated latency measurement was performed.
- Screenshots and raw transcripts are not included in this two-file repository; the evidence above summarizes the observed tool outputs.

The useful conclusion is narrow: basic browser and native Windows tasks worked on this machine, with two native UI recovery cases worth retesting.

## Additional runs

- [September 24, 2026, 05:18:54 UTC — HP Dragonfly post-restart](runs/2026-09-24T05-18-54Z-hp-dragonfly-postrestart/README.md): all seven tasks passed; includes app versions, timestamps, recovery observations, and the native-saved sample. The original run above is preserved.
