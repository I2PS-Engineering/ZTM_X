# ZTM X

**ZTM X** is the unified fleet benchmarking and Mining ecosystem from **I2PS Engineering**. It brings together **ZTM Special Ops X**, the machine-level RandomX performance engine that measures processor, memory, cache and sustained workload behavior, and **ZTM Vanguard X**, the graphical controller that securely connects workers, starts authorized workloads, displays their live Terminal and Telemetry, and combines verified fleet statistics. Both applications target Linux and macOS. The current **1.0.0 packages are engineering test candidates, not public releases**; the full qualifying macOS engine remains unfinished. Mining can unlock only after the server accepts a complete qualified Benchmark report **and** the client verifies a signed acceptance receipt bound to that worker and report.

![ZTM Vanguard X fleet dashboard showing eight workers, controller activity and 14.98 kH/s cumulative hashrate](assets/gui/ztm-vanguard-x-fleet-demo.png)

*ZTM Vanguard X GUI — demonstration with simulated worker activity and measurements. Hashrate, shares and temperatures shown here are illustrative, not live fleet measurements.*

**I2PS Engineering**  
*Different machines. Different capabilities. Same destination.*

| Application | Linux 1.0.0 engineering candidate | macOS 1.0.0 engineering candidate |
| :--- | :--- | :--- |
| **ZTM Special Ops X** | Native engine + direct shell installer; field qualification pending | Full qualifying native engine and standalone app **not yet delivered** |
| **ZTM Vanguard X** | Own embedded Linux engine; local checks passed, fleet test pending | RC2 native XMRig **provisional baseline only**; full optimizer and qualified-report path pending |

**ZTM X consists of these two applications, not a third standalone application.** The target macOS distribution is Developer ID signed and notarized through Apple's Direct Distribution workflow; the supplied November 5 Mac RC2 is **not** a signed/notarized finished 1.0.0 app.

## Current development status

**Engineering baseline: 5 November 2026.** This section distinguishes verified package behavior from target features still awaiting real-machine validation.

| Area | Verified in the non-destructive Stage 1 audit | Still required |
| :--- | :--- | :--- |
| Linux Special Ops X | ZIP integrity, direct `install.sh --verify-only`, shell syntax | Actual installation, identity preservation, pairing, headless reboot, 24-hour Benchmark and receipt acceptance |
| Linux Vanguard X | Bundles 44 native engine modules byte-identical to Linux Special Ops X; 12 focused tests passed | Real local Prepare/Start Benchmark/Stop, controller totals, remote GUI sync and fleet acceptance |
| macOS Vanguard X RC2 | Consent-gated provisional XMRig baseline path; synthetic checks passed | Native Mac field run, local connection stability, complete optimizer and 24-hour qualification |
| macOS Special Ops X | No complete 1.0.0 native engine or signed app in supplied artifacts | Implement or recover authoritative native build; then test, sign and notarize |
| Validation service | Signed receipt remains the required authorization boundary | Verify live API 1.0.0 acceptance/allowlist and worker/report receipt verification |

**Audit scope:** 125/125 master file hashes and all three application ZIP hashes matched; 84/84 current Python files parsed. The full Linux suite was **blocked at collection** because the audit interpreter lacked `asyncssh`; that is not a demonstrated product failure. No physical-machine performance, live API acceptance, signing or notarization was established by these offline checks.

**Known issue ZTMX-001 (unfixed in these candidates):** Vanguard's local-controller client may fail to reconnect when an owner-owned Unix socket entry remains after the engine exits. A failed launch can also suppress a subsequent relaunch attempt in the same client instance. The condition was reproduced offline; whether it explains the earlier Mac field disconnect remains unconfirmed. The planned correction requires bounded recovery, owner/permission checks, duplicate-launch protection and Linux/macOS regression testing.

Engineering builds and screenshots must not be presented as completed 24-hour qualifications or public releases. **Mining remains locked** on missing, provisional, rejected or unverified reports.

## Contents

- [Current development status](#current-development-status)

1. [Two applications, one system](#1-two-applications-one-system)
2. [The engineering objective](#2-the-engineering-objective)
3. [Special Ops X: Benchmark engine](#3-special-ops-x-benchmark-engine)
4. [Mathematical research reference](#4-mathematical-research-reference)
5. [Special Ops X interface and Mining](#5-special-ops-x-interface-and-mining)
6. [Vanguard X: fleet dashboard](#6-vanguard-x-fleet-dashboard)
7. [Secure pairing and remote control](#7-secure-pairing-and-remote-control)
8. [Headless operation, installation and upgrades](#8-headless-operation-installation-and-upgrades)
9. [Reports, authorization and privacy](#9-reports-authorization-and-privacy)
10. [Safety, contributions and licensing](#10-safety-contributions-and-licensing)

---

## 1. Two applications, one system

**ZTM Special Ops X** performs local hardware measurement, CPU topology analysis, RandomX/XMRig benchmarking, performance validation, supervised personal Mining, live telemetry and report generation. It can run with a desktop interface or as a managed background service.

**ZTM Vanguard X** pairs with multiple Special Ops X workers, exposes their job status and metrics, starts/stops permitted workloads, and provides a graphical fleet overview, read-only Terminal output and Telemetry views. It is also a **self-contained controller and local worker**: selecting its own node should offer hardware preparation, Start Benchmark, Stop and live local telemetry **without opening or installing a separate Special Ops X app**. The controller has its own permanent ZTM ID, and its actual local workload statistics contribute to fleet totals only while measured and reporting.

The current **Linux Vanguard candidate** embeds the same 44 compiled native engine modules shipped with Linux Special Ops X; byte-for-byte parity is verified, while physical-machine functional parity remains to be tested. **macOS Vanguard RC2** embeds a consent-gated real-XMRig baseline adapter instead of the full Special Ops X optimizer. RC2 results remain **provisional** and cannot unlock Mining.

Workers keep their own immutable **ZTM IDs**. Their human-readable display names can be changed independently.

## 2. The engineering objective

A bicycle, a motocross bike and a sport motorcycle do not take on the same hill with identical gearing, cooling and traction. CPUs differ just as materially in physical cores, SMT, cache, memory, Huge Pages, scheduling, thermal headroom and sustained throughput. ZTM X studies **what the actual machine can sustain**, not its brief peak.

- **Engine** → CPU architecture and active hashing workers.
- **Preparation** → RandomX dataset initialization, page allocation and supported memory/MSR setup.
- **Grip** → thread placement, cache-sharing and memory efficiency.
- **Road conditions** → thermal behavior, power limits and competing system activity.
- **Endurance** → measured sustained online hashrate.
- **Slope** → pool share difficulty (VarDiff), which affects share-submission frequency; merely raising VarDiff does not create more valid hashes or increase block-discovery probability.

![Machine-specific performance: approved ZTM Special Ops motorcycle analogy](https://raw.githubusercontent.com/I2PS-Engineering/ZTM_SpecialOps/main/assets/figures/01-motorcycle-var-diff-whiteboard.png)

The expanded [ZTM Special Ops engineering reference](https://github.com/I2PS-Engineering/ZTM_SpecialOps#readme) preserves the original mathematical definitions, example hardware measurements and diagrams. The methods below carry this research forward into the **Special Ops X** engine without treating historical machines as live fleet data.

## 3. Special Ops X: Benchmark engine

### Preparation and processor characterization

Special Ops X detects the CPU and OS capabilities available to the actual process, examines CPU/thread topology and memory conditions, prepares supported XMRig features, and records what the machine **really** achieved. It does not silently treat a requested setting as an applied one.

On Linux, where supported, the engine evaluates worker counts **and legal CPU-affinity placements**. CPU placement considers core and SMT siblings, shared caches, hybrid performance/efficiency cores, NUMA and available memory evidence. The **intended full macOS engine** uses native scheduling; Linux-only CPU pinning and MSR claims are not transferred to macOS. **Mac Apple Silicon** follows its supported automatic configuration rather than claiming Intel-style optimization or Huge Pages capability. The delivered macOS Vanguard RC2 is only a baseline adapter: it must not be described as implementing the full candidate search or qualified-report engine below.

### Discovery, comparison and endurance

The benchmark starts from XMRig's automatic reference configuration, evaluates a bounded set of hardware-motivated challengers when supported, and collects another reference measurement to reveal drift. The established engine framework has **at most seven full configurations**, with 600 qualified online seconds per full profile, after warm-up; Linux may use shorter affinity screens before full trials.

The evaluation measures XMRig's real hashrate, operational stability, accepted/rejected shares and available system telemetry. Share acceptance is a **quality gate**, not a multiplier that can make a slow configuration falsely appear fast. The selected winner is supported by recorded evidence and a confidence/tiebreak process; a statistical tie is reported rather than disguised as certainty.

The final endurance target is **86,400 qualified online seconds (24 hours)**. Offline time, unconfirmed pool activity, preparation and shutdown are not credited as online work. A 24-hour qualified benchmark may therefore take more than 24 hours of wall-clock time. An interrupted benchmark retains its evidence but cannot fabricate a completed qualification.

### Measurements and reports

The local report records hardware identity, planner inputs, candidate profiles, affinity decisions where supported, memory/page preparation, trial measurements, winner evidence, qualified time, terminal events, stop reason and available telemetry. It is bound to the worker and machine. Missing temperature, RAM, latency, bandwidth or power data is shown as **unavailable**, never substituted with invented readings.

The six familiar live chart families are **CPU temperature, CPU usage, RAM usage, hashrate, pool VarDiff and share activity**, with event markers and a rolling measurement window. The worker GUI and Vanguard's remote views use the **real active workload state**, whether a session began on the worker or was started remotely.

## 4. Mathematical research reference

ZTM X retains the established distinction between a **hardware prior** (which candidates merit measurement) and the **online evidence** (which configuration actually performs best). The following approved mathematics describes the research foundation; it does not replace the implemented, version-specific statistical decision logic.

Let:

- \(L\) = allowed logical CPU IDs; \(P\) = distinct physical-core groups.
- \(B\) = active workers in the observed XMRig automatic reference.
- \(n\) = active hashing workers in a candidate; \(A\) = a legal affinity set with \(|A|=n\).
- \(I\) = dataset-initialization thread count, **not** the active mining-worker count.
- \(P_p,P_e\) = performance/efficiency-core counts where reported by hardware.

### Constant dataset initialization

\[
I=\min(L,I_{mem}),\quad
I_{mem}=
\begin{cases}
\lceil P/2\rceil & \text{if measured bandwidth}<3\ \text{GB/s, or RAM}<3360\ \text{MiB}\\
P & \text{otherwise}
\end{cases}
\]

Initializers construct the RandomX dataset; the steady-state hash rate is measured **after READY**, so changing \(I\) is not evidence of an increased number of hashing workers.

### Hardware-aware count anchors

\[
\mathcal A=\langle B+1,\ P,\ C_3,\ C_3+1,\ B-1,\ B+2,\ 
[P_p,\ P_p+\lceil P_e/2\rceil,\ P_p+P_e]_{hybrid},\
L,\ C_2,\ P+1,\ B-2,\ B+3\rangle
\]

Here \(C_3\) and \(C_2\) are cache-capacity anchors derived from L3/L2 sharing domains and RandomX per-worker working sets. Legal, distinct candidates are screened and bounded; trying the maximum number of threads is **not** automatically optimal.

### Linux placement prior

\[
J(A)=2P_{L3}+0.6P_{L2}+0.35P_{L1}
+1.3\varphi\,{S(A)\over P}+E(A)+P_{mem}(A)
+0.12{|n-B|\over L}-0.12{n\over L}+P_{L1,fallback}
\]

This dimensionless engineering heuristic accounts for shared-cache occupancy, SMT sibling contention, hybrid-core placement and measured memory constraints. The historically approved \(\varphi\) factors are **Intel 1.0; AMD 0.25; unknown 0.6**. Neither these coefficients nor a memory-bandwidth proxy should be represented as measured RandomX hashrate. Candidate ranking is followed by actual online testing.

### Statistical decision

Candidate speed uses robust summaries of measured 10-second hashrate readings, instability checks, reference drift and uncertainty. The measured engine-selection framework uses block medians and Welch–Satterthwaite comparisons; a clear winner must exceed the confidence threshold, a practical **0.5%** difference and relevant reference residual. Close contenders may undergo **A–B–B–A** paired tests; results distinguish statistically resolved, practical ties and provisional/unresolved outcomes.

The complete, originally approved mathematical equations and parameter tables—including memory-capacity, bandwidth, latency, PSI and Huge Pages penalties; the original repeated-reference noise model; and a measured i7 example—remain available in the [Special Ops mathematical research section](https://github.com/I2PS-Engineering/ZTM_SpecialOps#12-approved-mathematical-research-reference). That historical reference is distinguished from the current implementation rather than copied into an inaccurate version claim.

## 5. Special Ops X interface and Mining

The desktop app groups the workflow into **ONGOING**, **PROFILE RESULTS**, **FINAL RESULTS**, and **MINING**. It provides preparation, authorization, **Start Benchmark**, **Stop**, profile/telemetry charts, report review and eligibility-gated Mining. There is **one Mining area**, not a separate wallet creation or wallet recovery tool.

**Mining remains locked until a valid Benchmark report reaches the server and a verified, signed acceptance receipt is returned.** Merely completing a local 24-hour run, possessing a local report, or clicking a Vanguard control cannot unlock Mining. A network outage, failed upload, invalid signature, mismatched identity, rejection or unverifiable receipt leaves it locked.

When authorized, personal Mining requires a **public Monero receiving address** and explicit consent. ZTM X never requests or stores wallet recovery seed phrases or private keys; wallet resources are linked from the official [Monero site](https://www.getmonero.org/). Benchmark and personal Mining never execute concurrently on one worker.

The title supports a **triple-click** to display the application's **1.0.0 version, build code and code fingerprint**. Both apps adapt their layouts and DPI scaling to the display.

## 6. Vanguard X: fleet dashboard

The title reflects the platform:

- **ZTM VANGUARD X | LINUX**
- **ZTM VANGUARD X | macOS**

The first screen uses a **graphical network topology**. The controller occupies the top node; **when only one worker is paired, it appears directly below the controller with a connecting line**, not displaced to the side. Additional worker cards appear beneath it in a flexible middle pane, with **up to four cards per row** and responsive wrapping. The divider for the **CUMULATIVE FLEET** pane is draggable, so the operator can trade detail-panel width for more visible workers without clipping the controls.

Each worker card shows its editable name, **immutable ZTM ID**, current status, actual hashrate and relevant accepted/rejected shares. Selecting a worker exposes its controls and its Terminal and Telemetry views. Selecting the **controller** instead exposes its own local engine, Prepare, **Start Benchmark** and Stop controls, plus its real output and metrics; no second application should need to be opened. **Fleet Dashboard, Terminal, Telemetry and Reports stay as tabs within the same Vanguard window**, retaining the selected node instead of opening detached windows. **Start Mining** is available only where authorized. Status text distinguishes **Benchmark**, **Mining**, **Idle**, **Offline** and unavailable measurements.

The cumulative panel combines the controller **when it is actually hashing** and all connected reporting workers. It presents total measured hashrate, accepted/rejected shares, Benchmark and Mining participation, Idle/Offline counts and live fleet state. Telemetry from missing or offline machines is excluded rather than silently treated as a measured zero. Benchmark and Mining activity are reported separately so totals do not imply that benchmark work is personal wallet Mining.

The **Terminal** presents read-only supervised engine output, **not** an unrestricted SSH shell. **Telemetry** provides live measurements and charts for the selected worker, with appropriately marked missing sensors and interruptions. The remote GUI mirrors the workload's actual state if the job is started from Vanguard.

## 7. Secure pairing and remote control

1. In Special Ops X, enable Vanguard pairing and open **Pair a Vanguard**.
2. In Vanguard X, choose **Pair by address…** and enter the worker's LAN/WLAN address. Pairing is deliberate; workers are not automatically discovered or broadcast.
3. Verify the matching **six-digit pairing code** shown on both endpoints.
4. On the worker, select **Codes match — Accept**. The worker requests the **administrator password at pairing acceptance** to authorize its restricted service helper. No second routine privilege dialog is needed for authorized normal remote controls.
5. The worker's SSH host key is pinned. A later unexpected host-key change blocks access until reviewed and the trust relationship is explicitly re-established.

The authenticated control channel uses the restricted worker endpoint (default **SSH port 42022**). User credentials and remote-control privileges are not exposed in the public README. Administrator passwords must not be stored in plaintext. Remote Start/Stop obeys the same consent, one-workload and Mining-receipt gates as the local GUI.

## 8. Headless operation, installation and upgrades

### Linux

The current **Special Ops X 1.0.0 Ubuntu x86-64 candidate** ships as a ZIP with a direct **`install.sh` and precompiled payload—not a `.deb` package**. After extracting the application ZIP into its own directory, use:

```bash
cd ZTM-Special-Ops-X-1.0.0-Linux
bash install.sh --verify-only  # validate without installing
bash install.sh                # approved desktop installation
```

The installer also accepts `--headless` or `--desktop` for the applicable installation mode. It requires a supported Ubuntu environment and declared runtime dependencies; never replace an active Benchmark or unrelated mining process. The recent dependency correction uses **`polkitd` and `pkexec`**, not the unavailable transitional `policykit-1` package. Package verification passed; **installation on the affected field worker is still pending**.

**Background + Auto-start** is designed to let the Special Ops X endpoint return after reboot without an interactive desktop session. Closing the GUI does not terminate the authorized worker service. **Worker startup is not automatic Benchmark or Mining startup**: after reboot, the endpoint should be available **Idle**, with no automatic hashing. Persistence and identity preservation remain field-test gates.

Linux Vanguard X has its own installer and a bundled native controller engine. It can operate its own authorized local Benchmark without opening Special Ops X, and is designed to control separately paired Linux and macOS workers through the restricted endpoint; full fleet operation remains subject to field testing.

### macOS

The target is **standalone macOS Intel and Mac Apple Silicon** builds of *both* applications, signed and notarized for Apple's **Direct Distribution** workflow. This is the **release objective**, not the status of the supplied 1.0.0 engineering ZIPs.

The available **Vanguard X macOS 1.0.0 RC2** supplies a local architecture-selected, checksum-pinned XMRig **provisional baseline** adapter with explicit user consent. It is intended to Prepare/Start/Stop its own local supervised Benchmark and show actual Terminal/Telemetry in the same window, but those paths are **not field-verified on a Mac**. It does **not** provide the full native optimization engine, completed 86,400-second qualified-report pathway or Mining authorization. The current handoff includes **no complete macOS Special Ops X 1.0.0 app/Xcode project or signed/notarized Mac release**. Linux-specific affinity, MSR and Huge Pages claims are not transplanted to macOS.

### Upgrade and migration

The **installer** for each application detects recognized prior ZTM installations and retires superseded application binaries, versioned runtimes, services or launchers in a controlled order **after validating the replacement**. Worker IDs, reports, pairing identities, pinned host keys and essential configuration are preserved. **Vanguard legacy cleanup happens at installation only**, never at application startup. An unexpected executable or unrelated file is not removed merely because its name resembles ZTM.

## 9. Reports, authorization and privacy

ZTM reports are structured processor research evidence, not merely a score or screenshot. Their hardware/topology, cache, memory, sustained hashrate, affinity, thermal and reliability measurements support the I2PS Dual-ISA Lab's physical-host research. This does not imply that the separate RISC-V/RTL/FPGA research pipeline automatically ingests these reports.

Special Ops X submits the authorized report to the validation service. Eligibility remains fail-closed until the worker validates the signed server response for the same identity and report. Vanguard's view of an accepted job **does not replace** server-issued Mining authorization. The live server's acceptance of **1.0.0** reports and corresponding signed-receipt path have **not yet been verified** in this engineering baseline; no client may infer eligibility from local completion or an RC2 provisional report.

Pairing stores persistent identities and trusted host-key information; network reporting involves the worker ID and hardware/benchmark metadata needed for validation. Personal Mining communicates with the selected pool and the public receiving address. Operators should protect exported reports, machine identifiers and remote-control credentials.

## 10. Safety, contributions and licensing

Continuous CPU work consumes electricity, raises temperature and can shorten hardware life if cooling is inadequate. Use it only on systems you own or are authorized to operate. ZTM X does not guarantee earnings, overclock the CPU, fabricate sensor readings, assume Huge Pages allocation or terminate unrelated processes. **Stop** ends the supervised workload and preserves the stop reason without falsely granting 24-hour qualification.

The established ZTM Special Ops contribution framework discloses **1% scheduled runtime for I2PS Engineering** during personal Mining, separately from XMRig's independent **1% upstream developer contribution**. Scheduled time is not an exact percentage of shares or earnings. Pool work during the explicitly authorized Benchmark uses the disclosed research/development receiving address.

ZTM application source and proprietary optimization components are **not open source**. Public use is subject to I2PS's noncommercial license and the terms supplied with the application. Third-party dependencies, including **XMRig (GPLv3)**, retain their own licenses and notices. No distribution of proprietary ZTM X source is implied by this repository's documentation.

---

**I2PS Engineering**  
*Built in Morocco, Engineered in the future.*

## ZTM Special Ops X GUI

![ZTM Special Ops X Benchmark GUI showing an active processor Benchmark, simulated hashrate and live Terminal](assets/gui/ztm-special-ops-x-benchmark-demo.png)

*ZTM Special Ops X GUI — active Benchmark demonstration with simulated hashrate, profile progress, share counters and Terminal activity. Not a live fleet measurement.*
