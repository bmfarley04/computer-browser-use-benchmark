# Comparison of the three recorded benchmark runs

The Mac completed every task and needed fewer recoveries than either recorded Windows run. Its measured task total was about five seconds longer than the HP Dragonfly's, but these runs do not establish which computer is faster. The strongest observed Mac advantage was that Calculator and Save As worked through accessibility controls without a visual fallback.

This comparison covers three result sets available on September 24, 2026, through commit `2f85a25`. Braeden confirmed that the original Windows run came from his gaming PC, distinct from the HP Dragonfly. This establishes three participating computers. The gaming PC's hardware and application versions remain unrecorded.

## Outcomes and elapsed time

| Measure | Gaming PC | Mac M4, 16 GiB | HP Dragonfly G2, i7-1185G7, 16 GB |
| --- | ---: | ---: | ---: |
| Tasks passed | 7/7 | 7/7 | 7/7 |
| Tasks with a fallback or recovery | 2 | 1 | 2 |
| Browser tasks, summed seconds | 22.435 | 28.088 | 17.696 |
| Native tasks, summed seconds | 89.031 | 88.972 | 94.072 |
| All task timers, summed seconds | 111.466 | 117.060 | 111.768 |

These totals sum task timers, excluding gaps between tasks and report preparation. They are not full-session durations or application response times.

| Task | Gaming PC seconds | Mac seconds | HP seconds |
| --- | ---: | ---: | ---: |
| B1: enter text and check box | 8.314 | 14.895 | 6.928 |
| B2: select Two | 3.593 | 0.222 | 0.283 |
| B3: replace text and switch radio | 5.252 | 0.239 | 0.776 |
| B4: search Wikipedia for tmux | 5.276 | 12.732 | 9.709 |
| N1: calculate 7 × 8 | 40.828 | 28.636 | 37.699 |
| N2: replace text in a new document | 22.254 | 26.750 | 18.928 |
| N3: save through the native dialog | 25.949 | 33.586 | 37.445 |

## What actually happened

On the Mac, calculating 7 × 8 exposed readable controls and the result 56 through accessibility, the interface that lets automation identify buttons and text. Both Windows reports instead needed screenshots because Calculator's accessibility data was unavailable. The Mac's Calculator task finished in 28.636 seconds versus 37.699 seconds on the HP, but the HP also incurred avoidable output-processing overhead. The practical observation is that the Mac had a simpler control path in this run.

When saving the text file, both Windows runs rejected a Save click because the automation associated the dialog with the wrong window. A screenshot-based coordinate click recovered each run. The Mac's Save click succeeded directly through accessibility. Its 33.586-second save task also included converting TextEdit's rich text document into plain text and navigating to the destination folder. This is different work from saving a Notepad document, despite the identical final file.

The Mac's only recovery happened when the first TextEdit launch inspection timed out. A second inspection succeeded. The HP's Notepad editing task completed without recovery and took 18.928 seconds versus the Mac's 26.750 seconds. Thus the Mac avoided the Windows dialog problems but was not free of automation friction.

All three browser runs completed without observed retries. The Mac and HP both performed dropdown and radio changes in well under a second using batched accessibility actions. The original run recorded several seconds for those actions with different timer boundaries. Those differences are insufficient evidence of hardware performance differences.

The Mac and HP saved samples were independently checked during this comparison. Both contain exactly `Benchmark Beta: 84 items.` as 25 bytes and share SHA-256 `6e9abe74232fce725c7057be76db2d7ea099e80dc0f2c7ddf18b2af5d01c8884`. The original Windows run reports successful file verification but does not include its sample for this independent check.

## What the comparison supports

For these simple workflows, the Mac was fully capable and had fewer tasks needing recovery. All three runs ultimately produced correct results. The Mac's total was 5.292 seconds longer than the HP's, about 4.7%, while its native-task subtotal was 5.100 seconds shorter. These are descriptive arithmetic differences, not demonstrated speed advantages or disadvantages.

The Mac's first browser task included automatically emitted tool documentation and reading it; HP documentation setup was excluded. Agent reasoning, network navigation, action batching, observation overhead, and recovery time also contribute. Native apps and automation backends differ across operating systems, and the original Windows environment is incompletely recorded. No repeated trials, controlled background load, or standardized agent configuration were established.

The useful conclusion is that macOS handled this suite with less observed recovery work, particularly for Calculator and Save As. There is no evidence here that the M4's hardware makes automation broadly faster, nor that Windows is less reliable in general. Long unattended workflows, remote control, locked screens, and memory pressure were not tested.

## Source reports

The [original Windows results](RESULTS.md), [Mac results](RESULTS-MAC.md), and [HP Dragonfly results](runs/2026-09-24T05-18-54Z-hp-dragonfly-postrestart/README.md) describe each run. The HP's [machine-readable measurements](runs/2026-09-24T05-18-54Z-hp-dragonfly-postrestart/results.json) provide timestamps. All use the acceptance criteria in [BENCHMARK.md](BENCHMARK.md), with the Mac's documented native-app substitutions.
