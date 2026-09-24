# HP Dragonfly post-restart run

All seven tasks in [BENCHMARK.md](../../BENCHMARK.md) passed in one new sequential run on September 24, 2026, starting at 05:18:54 UTC (01:18:54 America/New_York). This run used procedure commit `205e4c7dffc37f41bcd3839486d3915bb54cfd18`. It is separate from the original results.

## Environment

- HP Elite Dragonfly G2; Intel Core i7-1185G7; 16 GB installed RAM, approximately 15.25 GB usable; Intel Iris Xe; 1920 × 1080 display.
- Windows 11 Pro, version 10.0.26100, build 26100. Last boot: 01:06:23 local time that morning.
- Chrome 153.0.8010.55, connected through the browser extension in the existing extension-equipped profile. Chrome launch/profile selection and automation documentation were completed before timing.
- Calculator 11.2605.9.0; Notepad 11.2605.34.0. Native control used the Computer Use plugin's `@oai/sky` API.
- Foreground local desktop session with Codex and Chrome open. No CPU/RAM sampler ran during the timed tasks. Background activity was not controlled; this is not an idle-machine or throughput measurement.

## Measurements

| ID | Task | Seconds | Outcome | Acceptance evidence |
| --- | --- | ---: | --- | --- |
| B1 | Text, multiline entry, checkbox | 6.928 | Pass | Text input `Benchmark Alpha`; textarea `Synthetic browser benchmark.` followed by newline and `Second line: 42.`; Default checkbox value 1. |
| B2 | Dropdown | 0.283 | Pass | Dropdown value `Two`, with option Two selected. |
| B3 | Replace text and switch radio | 0.776 | Pass | Text input `Benchmark Beta`; Default radio 1; Checked radio 0. |
| B4 | Wikipedia search | 9.709 | Pass | Searched `tmux` through Main Page search field; title `tmux - Wikipedia`, heading `tmux`, URL `https://en.wikipedia.org/wiki/Tmux`. |
| N1 | Calculator 7 × 8 | 37.699 | Pass with visual fallback | Standard mode screenshot showed `7 × 8 =` and result `56`. |
| N2 | Notepad replacement | 18.928 | Pass | New blank document focused on launch; typed Alpha text, Ctrl+A selected it, replacement document text exactly `Benchmark Beta: 84 items.`. |
| N3 | Native Save As | 37.445 | Pass after retry | Save dialog closed; title `computer-use-benchmark-sample.txt - Notepad`; tab Unmodified; independent file read matched the expected text. |

Machine-readable timestamps and observations are in [results.json](results.json). The [saved sample](computer-use-benchmark-sample.txt) is a byte-for-byte copy of the file created by Notepad through Save As, not a filesystem-generated substitute. Its SHA-256 is `6E9ABE74232FCE725C7057BE76DB2D7EA099E80DC0F2C7DDF18B2AF5D01C8884` (25 bytes).

## Timing and execution

Each timer started immediately before the first task action and stopped in the cell returning its final successful UI observation. Timings include intervening agent reasoning, tool calls, observations, and recoveries. Final human-readable assessment of the returned observation happened after stopping the timer. N3's independent filesystem verification also happened afterward. Setup, documentation, report writing, and gaps between tasks are excluded. Millisecond digits preserve the recorded timers; they do not imply repeatable millisecond precision.

B1 and B4 include new-tab creation, navigation, and initial page observation. N1 and N2 include application launch and window discovery. B1-B3 share one form; N2-N3 share one newly launched blank document. One trial per task was performed in the prescribed B1, B2, B3, B4, N1, N2, N3 order.

Browser actions used semantic accessibility controls. B1 batched the two text values and checkbox click, then refreshed state; B2 set the native dropdown value; B3 batched replacement and radio selection, then refreshed. B4 filled the page search field and pressed Enter. No page scripts changed form state. No form submission, login, or Wikipedia modification occurred.

Native actions used one input action followed by refreshed observation. Calculator used keyboard 7, multiply, 8, Enter with screenshots after each action. Notepad used focused typing and Ctrl+A, then Ctrl+Shift+S to open Save As. An existing writable output directory was prepared outside the timers; the filename was entered through the dialog.

## Errors and recoveries

**Calculator:** All five state captures returned null accessibility data. Screenshots remained usable, so keyboard input proceeded with visual verification. No null-tree dereference or failed input occurred. The final screenshot showed the correct expression and result. Missing accessibility recurred after the restart; its cause is not established.

**Save As:** The first observation after Ctrl+Shift+S returned null accessibility. A fresh window inventory listed the Notepad parent but no separate Save As window. A combined screenshot/accessibility capture then exposed the owned dialog. Setting its filename by accessibility index succeeded. Clicking Save by index was rejected because the point was over the dialog's Save control rather than the expected Notepad parent window. After another fresh screenshot, a grounded click at (592, 442) within the dialog screenshot succeeded. There was one rejected Save click and one successful coordinate retry. The timer includes this recovery and a progress update between calls.

**Observation overhead:** The initial Calculator state was accidentally printed with its screenshot data URL, producing a large truncated textual output in addition to the image. Later captures printed only accessibility data. That avoidable agent-side overhead remains in N1's time; no screenshot payload is included in this report.

The evidence here summarizes live tool observations; raw screenshots and full transcripts are not committed. File contents and hashes can be checked independently from the included sample.

## Interpretation

All four browser tasks and Notepad editing completed without retries. Calculator required visual verification; the owned Save As dialog required targeting recovery. These are recoverable native automation limitations still present after restart.

The original run used different timer boundaries for B1-B3 and different Calculator interaction choices. This run batches short deterministic browser actions, explaining why its dropdown and radio timings can be much shorter without implying an equivalent machine speedup. The runs are not a controlled before/after performance comparison. One trial of seven unequal tasks cannot establish general reliability, intrinsic browser-versus-native speed, or the effect of debloating/restarting. Locked-desktop, lid-closed, remote-session, and sustained unattended agent behavior remain untested by this suite.
