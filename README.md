# ZTM X

**ZTM X** is the processor-performance research, benchmarking and fleet-management ecosystem developed by **I2PS Engineering**. It unites **ZTM Special Ops X**, the native machine-level measurement and supervised workload application, with **ZTM Vanguard X**, the graphical controller and independent local workload system. Both applications are designed for **Linux and macOS**.

Its research foundation is the study of *sustained, measured performance* under actual hardware constraints—not brief peak numbers, generic CPU presets or unverified optimization claims. The approved engineering mathematics, methodology, physical-machine considerations and research evidence are retained in this README.

![ZTM Vanguard X fleet dashboard showing controller, connected workers and illustrative workload measurements](assets/gui/ztm-vanguard-x-fleet-demo.png)

*ZTM Vanguard X — interface demonstration with illustrative activity and measurements, not a live fleet measurement.*

**I2PS Engineering**  
*Different machines. Different capabilities. Same destination.*

| Application | Linux | macOS |
| :--- | :--- | :--- |
| **ZTM Special Ops X** | Native processor characterization, benchmark research, telemetry, reporting and authorized Mining | Native processor characterization, benchmarking and reporting with the **original ZTM Special Ops macOS native engine** |
| **ZTM Vanguard X** | Graphical fleet controller with its own local native workload engine | Graphical fleet controller with its own local workload powered by the **original ZTM Special Ops macOS native engine** |

**Two applications, one research ecosystem.** Vanguard manages paired workers but can also benchmark and perform permitted operations locally without opening Special Ops X on the controller machine.

## Contents

1. [Two applications, one system](#1-two-applications-one-system)
2. [The engineering objective](#2-the-engineering-objective)
3. [Special Ops X: Benchmark engine](#3-special-ops-x-benchmark-engine)
4. [Approved mathematical research reference](#4-approved-mathematical-research-reference)
5. [Special Ops X interface and Mining](#5-special-ops-x-interface-and-mining)
6. [Vanguard X: fleet dashboard](#6-vanguard-x-fleet-dashboard)
7. [Secure pairing and remote control](#7-secure-pairing-and-remote-control)
8. [Background operation, installation and platform support](#8-background-operation-installation-and-platform-support)
9. [Research reports, authorization and privacy](#9-research-reports-authorization-and-privacy)
10. [Safety, contributions and licensing](#10-safety-contributions-and-licensing)

---

## 1. Two applications, one system

**ZTM Special Ops X** performs local hardware measurement, CPU topology analysis, RandomX/XMRig benchmarking, performance validation, supervised personal Mining, live telemetry and report generation. It can run with a desktop interface or as a managed background service.

**ZTM Vanguard X** pairs with multiple Special Ops X workers, exposes their job status and metrics, starts/stops permitted workloads, and provides a graphical fleet overview, read-only Terminal output and Telemetry views. It is also a **self-contained controller and local worker**: selecting its own node should offer hardware preparation, Start Benchmark, Stop and live local telemetry **without opening or installing a separate Special Ops X app**. The controller has its own permanent ZTM ID, and its actual local workload statistics contribute to fleet totals only while measured and reporting.

On Linux, Special Ops X and Vanguard X use native workload engines suitable for their respective roles. On macOS, the **original ZTM Special Ops macOS native engine** provides the machine-level research foundation and is integrated into Vanguard X for its local workload. Both platforms retain their native operating-system constraints and capabilities.

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

On Linux, where supported, the engine evaluates worker counts **and legal CPU-affinity placements**. CPU placement considers core and SMT siblings, shared caches, hybrid performance/efficiency cores, NUMA and available memory evidence. On Intel macOS, the **original ZTM Special Ops macOS native engine** uses native scheduling and supported processor optimization. Linux-specific CPU pinning and MSR behavior are not attributed to macOS. **Mac Apple Silicon** follows its supported automatic configuration rather than claiming Intel-style optimization or Huge Pages capabilities.

### Discovery, comparison and endurance

The benchmark starts from XMRig's automatic reference configuration, evaluates a bounded set of hardware-motivated challengers when supported, and collects another reference measurement to reveal drift. The established engine framework has **at most seven full configurations**, with 600 qualified online seconds per full profile, after warm-up; Linux may use shorter affinity screens before full trials.

The evaluation measures XMRig's real hashrate, operational stability, accepted/rejected shares and available system telemetry. Share acceptance is a **quality gate**, not a multiplier that can make a slow configuration falsely appear fast. The selected winner is supported by recorded evidence and a confidence/tiebreak process; a statistical tie is reported rather than disguised as certainty.

The final endurance target is **86,400 qualified online seconds (24 hours)**. Offline time, unconfirmed pool activity, preparation and shutdown are not credited as online work. A 24-hour qualified benchmark may therefore take more than 24 hours of wall-clock time. An interrupted benchmark retains its evidence but cannot fabricate a completed qualification.

### Measurements and reports

The local report records hardware identity, planner inputs, candidate profiles, affinity decisions where supported, memory/page preparation, trial measurements, winner evidence, qualified time, terminal events, stop reason and available telemetry. It is bound to the worker and machine. Missing temperature, RAM, latency, bandwidth or power data is shown as **unavailable**, never substituted with invented readings.

The six familiar live chart families are **CPU temperature, CPU usage, RAM usage, hashrate, pool VarDiff and share activity**, with event markers and a rolling measurement window. The worker GUI and Vanguard's remote views use the **real active workload state**, whether a session began on the worker or was started remotely.

## 4. Approved mathematical research reference

The original mathematical framework below remains part of the public engineering explanation. **The equations and coefficients in this section have not been recalculated or retuned.** They document hardware-aware candidate priors and the earlier reference-noise research method; the measured statistical decision is established by the applicable native engine. The methodological distinction is intentional: theoretical ranking is not a substitute for measured online evidence.

Let `L` be allowed logical CPUs, `P` physical core groups, `B` the verified XMRig automatic worker count, `n` active mining workers, `A` a legal affinity set, and `I` dataset-initialization threads. `P_p` and `P_e` are performance/efficiency-core counts when those classes are exposed. Initialization threads are not steady-state mining workers.

### Constant initializer and memory feasibility prior

$$
I=\min\big(L,\;I_{mem}\big),\qquad I_{mem}=\begin{cases}\lceil P/2\rceil & \text{if measured copy bandwidth}<3\ \text{GB/s or available RAM}<2{,}336+1{,}024\ \text{MiB}\\ P & \text{otherwise}\end{cases}
$$

### Physically motivated candidate anchors

$$
\mathcal{A}=\langle\,B{+}1,\;P,\;C_3,\;C_3{+}1,\;B{-}1,\;B{+}2,\;[P_p,\;P_p{+}\lceil P_e/2\rceil,\;P_p{+}P_e]_{\text{hybrid}},\;L,\;C_2,\;P{+}1,\;B{-}2,\;B{+}3\,\rangle
$$

Here $C_3=\sum_d\lfloor S_d/2\,\text{MiB}\rfloor$ over L3 domains and $C_2=\sum_d\lfloor S_d/256\,\text{KiB}\rfloor$ over L2 domains, each capped at $L$. The budgets reflect the RandomX working sets. The theoretical lower bound for sufficiently many physical cores is $f=\max(2,\lceil B/2\rceil)$, with the documented performance-core exception.

### Linux placement prior

$$
J(A)=2P_{L3}+0.6P_{L2}+0.35P_{L1}+1.3\,\varphi\,\frac{S(A)}{P}+E(A)+P_{mem}(A)+0.12\frac{|n-B|}{L}-0.12\frac{n}{L}+P_{L1,fallback}
$$

$P_{Lk}=\sum_d \max(0,a_d-c_d)/c_d$, where $c_d=\max(1,\lfloor C_d/R_k\rfloor)$. The original vendor-SMT factor $\varphi$ is Intel `1.0`, AMD `0.25`, unknown `0.6`; $E(A)=0.5\cdot\min(e_{sel},p_{idle})/n$ penalizes selecting an E-core while a P-core remains idle. This is a dimensionless **prior**, not a predicted hashrate.

With $n$ workers, $S$ SMT siblings, $P$ physical cores and $o=\max(0,n-P)$, the approved memory-penalty terms are:

| Term | Applied when | Original formula |
|---|---|---|
| Capacity | Available RAM $M$ measured | $M<M_{min}=2{,}336+2n+1{,}024$ MiB: $\;1.8+\min\!\big(4,\tfrac{M_{min}-M}{1024}\big)\big(0.3+\tfrac{n}{P}\big)$, otherwise zero |
| Bandwidth proxy | Copy bandwidth $W$ (GB/s) measured | $0.70\cdot\min\!\big(3,\tfrac{\max(0,\,n-c)}{c}\big)$, $\;c=\max(1,W/2)$ |
| Bandwidth × SMT | $W$ measured | $0.18\cdot\tfrac{S}{P}\cdot\min\!\big(2,\tfrac{\max(0,\,8-W)}{6}\big)$ |
| Latency proxy | Pointer-chase latency $\lambda$ (ns) measured | $0.35\cdot\min\!\big(2,\max(0,\tfrac{\lambda-95}{95})\big)\cdot\big(\tfrac{o}{P}+0.2\tfrac{n}{P}\big)$ |
| DDR speed prior | Rated/configured MT/s with confidence weight | $0.08\cdot\text{conf}\cdot\operatorname{clamp}(3200/\text{MT/s},\,0.25,\,2)\cdot\tfrac{o}{P}$ |
| Verified channels | Verifiable channel count | $0.20\cdot\tfrac{\max(0,\,n-T/4)}{\max(1,\,T/4)}$, $\;T=\text{MT/s}\times8\times\text{channels}/1000$ |
| Memory pressure | Linux PSI some avg10 $>2\%$ | $0.40\cdot\min\!\big(3,\tfrac{\text{PSI}-2}{10}\big)\cdot\tfrac{n}{P}$ |
| Huge Pages shortfall | Free Huge Pages $<2{,}336+2n$ MiB | $0.04\cdot\tfrac{n}{P}$ |

Missing inputs must not be replaced by fictitious measurements. Memory bandwidth and latency probes are proxies, not RandomX-specific measurements. Physical topology, RAM performance, Huge Pages allocation and temperature are hypotheses until the online measurements confirm their impact.

### Original reference-repeat noise formulation (research reference)

$$
g=\frac{|H_{rep}-H_{ref}|}{(H_{ref}+H_{rep})/2},\qquad \hat\sigma=g\,\frac{\sqrt{\pi}}{2},\qquad \delta=\operatorname{clamp}\!\Big(2\sqrt{\tfrac32}\,\hat\sigma,\;1\%,\;10\%\Big)
$$

The original analysis used $E|X_1-X_2|=2\sigma/\sqrt\pi$ and a $\sqrt{3/2}$ variance adjustment. Other reference-model expressions included the trial quality term $Q=\tilde H\cdot\max(0.70,\,1-\min(\mathrm{CV},0.30))$, the drift correction $\tilde H_i=H_i/(1+d\,\tau_i)$ for $d=H_{rep}/H_{ref}-1$, and the historical reference-margin decision $Q_{c^*}>\bar Q_{ref}\,(1+\delta)$. **That final margin decision is preserved as historical research; it must not be mistaken for an assertion about the current engine's complete selection implementation.**


**Additional notation retained from the approved mathematical reference:** Count feasibility spans $1..L$, where the automatic baseline is $B$ and a neighbor candidate can be $B{+}1$. Performance- and efficiency-core counts are $P_p$ and $P_e$; an eligible performance-core-only exception requires $P_p\ge2$. $C_3$ denotes the L3 anchor. A placement is $A$, its extra sibling occupation is $S(A)$, and its memory prior is $P_{mem}$. The historical warm-up rule was $\min(25\,\text{s},\,\text{duration}/5)$, and its variation statistic was $\mathrm{CV}$. The reference-noise estimate $\hat\sigma$ could produce a provisional $\delta=3\%$ or a high-noise flag at $\delta>10\%$, with a noise-margin symbol $\delta$, relative trial position $\tau_i\in[0,1]$ and $\bar Q_{ref}$ the mean of reference results. These are retained as original research symbols, **not a substitute for the native engine's decision logic**.

### Measured comparison and statistical selection

The **hardware prior** ranks physically plausible configurations for investigation; it does not select a winner by itself. The online research distinguishes observed sustained hashrate from share-pool difficulty and separates workload stability from raw speed.

For qualified comparable trials, the research uses robust summaries of 10-second hashrate samples, block-median uncertainty estimates, drift assessment and Welch–Satterthwaite comparisons. The published research distinguishes statistically resolved comparisons from practical ties and provisional outcomes. Where comparable candidates remain close, paired **A–B–B–A** observations can help distinguish a real advantage from time-dependent environmental drift. The historical research reference above is preserved with its original coefficients and notation; no published heuristic is represented as a substitute for actual measured data.

### Evidence integrity and measurement limits

Memory copy bandwidth, pointer-chase latency, configured DDR speed, cache geometry, CPU topology, Huge Pages availability and Linux memory pressure are distinct measurements or priors, not interchangeable with effective RandomX hashrate. **Unavailable** means unavailable: neither a missing sensor nor a requested feature is recorded as a measured success. Performance conclusions rely on measurements from the actual machine under the applicable workload.

## 5. Special Ops X interface and Mining

The desktop app groups the workflow into **ONGOING**, **PROFILE RESULTS**, **FINAL RESULTS**, and **MINING**. It provides preparation, authorization, **Start Benchmark**, **Stop**, profile/telemetry charts, report review and eligibility-gated Mining. There is **one Mining area**, not a separate wallet creation or wallet recovery tool.

**Personal Mining is available only with valid authorization.** A local Benchmark, a submitted report or an accepted research result does not independently authorize Mining. The application enforces authorization requirements without exposing protective implementation details in this public documentation.

When authorized, personal Mining requires a **public Monero receiving address** and explicit consent. ZTM X never requests or stores wallet recovery seed phrases or private keys; wallet resources are linked from the official [Monero site](https://www.getmonero.org/). Benchmark and personal Mining never execute concurrently on one worker.

The title supports a **triple-click** to display the application's **1.0.0 version, build code and code fingerprint**. Both apps adapt their layouts and DPI scaling to the display.

### Shared GUI charts and event annotations

The **six synchronized live charts** share a rolling **15-minute** clock in the Profile Results view:

| Chart | Display / data policy |
|---|---|
| CPU temperature | Orange; available OS temperature sensor readings only |
| CPU usage | Yellow; observed system-wide CPU usage |
| RAM usage | Red; Linux charts display system RAM, not a fabricated XMRig RSS figure |
| Hashrate | Green; pool-connected XMRig hashrate, with unmeasured periods distinguished from zero |
| Pool VarDiff | Violet; normal pool difficulty, with XMRig donation intervals clearly marked |
| Share activity | Accepted shares green, rejected shares red; cumulative curves continue through intervals with no new share rather than breaking |

Charts include actual profile-change markers and available event explanations; relevant affinity screening, transitions and donation intervals are distinguished. Missing telemetry remains **unavailable**, never silently interpolated into a measured value. The winning-profile trophy/validation indicator is an event cue, **not** independent proof of benchmark qualification.

## 6. Vanguard X: fleet dashboard

The title reflects the platform:

- **ZTM VANGUARD X | LINUX**
- **ZTM VANGUARD X | macOS**

The first screen uses a **graphical network topology**. The controller occupies the top node; **when only one worker is paired, it appears directly below the controller with a connecting line**, not displaced to the side. Additional worker cards appear beneath it in a flexible middle pane, with **up to four cards per row** and responsive wrapping. The divider for the **CUMULATIVE FLEET** pane is draggable, so the operator can trade detail-panel width for more visible workers without clipping the controls.

Each worker card shows its editable name, **immutable ZTM ID**, current status, actual hashrate and relevant accepted/rejected shares. Selecting a worker exposes its controls and its Terminal and Telemetry views. Selecting the **controller** instead exposes its own local engine, Prepare, **Start Benchmark** and Stop controls, plus its real output and metrics; no second application should need to be opened. **Fleet Dashboard, Terminal, Telemetry and Reports stay as tabs within the same Vanguard window**, retaining the selected node instead of opening detached windows. **Start Mining** is available only where authorized. Status text distinguishes **Benchmark**, **Mining**, **Idle**, **Offline** and unavailable measurements.

The cumulative panel combines the controller **when it is actually hashing** and all connected reporting workers. It presents total measured hashrate, accepted/rejected shares, Benchmark and Mining participation, Idle/Offline counts and live fleet state. Telemetry from missing or offline machines is excluded rather than silently treated as a measured zero. Benchmark and Mining activity are reported separately so totals do not imply that benchmark work is personal wallet Mining.

The **Terminal** presents read-only supervised engine output, **not** an unrestricted SSH shell. **Telemetry** provides live measurements and charts for the selected worker, with appropriately marked missing sensors and interruptions. The remote GUI mirrors the workload's actual state if the job is started from Vanguard.

## 7. Secure pairing and remote control

In Special Ops X, the operator makes the worker available for approved pairing. In Vanguard X, **Pair by address…** connects to a selected worker on the local network without automatic device broadcasts or discovery. The operator reviews the pairing request, confirms the intended machine and approves access.

Connected nodes retain their identity and a human-readable name. Vanguard exposes only permitted workload actions and read-only operational views; its Terminal is not an unrestricted remote shell. Remote Start/Stop respects the same authorization and workload-state restrictions as the local application. Security-sensitive pairing, authentication and anti-tampering mechanisms are deliberately not documented here.

## 8. Background operation, installation and platform support

### Linux

**ZTM Special Ops X** provides a native Linux benchmarking and Mining application with a desktop interface, supported background operation, supervised workload control and a shell-based installation path. **ZTM Vanguard X** provides the fleet interface and its own independently operable local workload engine.

Background availability does not imply permission to start hashing automatically. Workloads remain explicitly controlled. Installations and upgrades are intended to preserve worker identity, research reports and authorized connection settings while respecting existing workloads and unrelated software.

### macOS

**ZTM Special Ops X** brings its **original ZTM Special Ops macOS native engine** to the macOS application. **ZTM Vanguard X** integrates the same native engine for its local workload while adding fleet control, Terminal and Telemetry views.

Intel macOS and Mac Apple Silicon follow their respective supported architecture-specific paths. Mac Apple Silicon is not represented as having Intel-only optimization capabilities. Platform-specific preparation, permissions, and available hardware measurements are handled according to the host machine.

### Distribution and upgrade integrity

ZTM X consists of **two applications on two platforms**. Official installation packages, verification information, compatibility requirements and distribution instructions accompany their respective release assets when published. This README does not claim that a separate software release has already been published.

Existing machine identity and locally retained research records are preserved through supported application upgrades. Software is not replaced or removed solely because an unrelated file resembles a ZTM component.

## 9. Research reports, authorization and privacy

ZTM X reports are structured hardware-research records, not merely benchmark scores or screenshots. Depending on the hardware and platform, they may include processor topology, cache organization, memory measurements, observed workload results, affinity evidence where supported, thermal and stability telemetry, qualification status and documented interruptions.

Results are associated with the participating machine. Benchmarking and personal Mining are distinct operations. **Valid server-issued authorization is required for Mining**, irrespective of whether a Benchmark was completed locally or its report was accepted for research. Protective authorization, heartbeat and anti-tampering internals are not part of this public engineering reference.

Personal Mining uses an operator-supplied **public receiving address**. Recovery phrases and private wallet keys are not required by ZTM X. Operators should safeguard their exported reports, machine identifiers and remote-access credentials.

The research described here supports physical-host CPU and memory investigations. It does not imply that all measured data automatically feeds a separate RISC-V, RTL or FPGA research pipeline.

## 10. Safety, contributions and licensing

Continuous CPU work consumes electricity, raises temperature and can shorten hardware life if cooling is inadequate. Use it only on systems you own or are authorized to operate. ZTM X does not guarantee earnings, overclock the CPU, fabricate sensor readings, assume Huge Pages allocation or terminate unrelated processes. **Stop** ends the supervised workload and preserves the stop reason without falsely granting 24-hour qualification.

The established ZTM Special Ops contribution framework discloses **1% scheduled runtime for I2PS Engineering** during personal Mining, separately from XMRig's independent **1% upstream developer contribution**. Scheduled time is not an exact percentage of shares or earnings. Pool work during the explicitly authorized Benchmark uses the disclosed research/development receiving address.

ZTM application source and proprietary optimization components are **not open source**. Public use is subject to I2PS's noncommercial license and the terms supplied with the application. Third-party dependencies, including **XMRig (GPLv3)**, retain their own licenses and notices. No distribution of proprietary ZTM X source is implied by this repository's documentation.

---

**I2PS Engineering**  
*Built in Morocco, Engineered in the future.*

## ZTM Special Ops X GUI

![ZTM Special Ops X Benchmark interface showing sample processor measurements and telemetry](assets/gui/ztm-special-ops-x-benchmark-demo.png)

*ZTM Special Ops X — Benchmark interface demonstration with illustrative telemetry. Not a real-machine qualification report.*
