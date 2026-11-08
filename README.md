# ZTM X

**Processor-performance research, sustained benchmarking and coordinated fleet operations**  
**I2PS Engineering** · **Version 1.0.0** · Linux and macOS

ZTM X is a hardware-aware CPU research ecosystem developed by **I2PS Engineering**. It consists of two independently operable applications: **ZTM Special Ops X**, which characterizes a machine, measures its sustained performance, records telemetry and supervises permitted local workloads; and **ZTM Vanguard X**, which manages paired machines through a unified fleet interface and includes a local workload engine on the controller itself.

The engineering objective is specific: determine what **each physical machine can sustain** under its actual CPU topology, cache, memory, thermal, scheduling and network constraints. Short-lived peak hashrate, a guessed thread count and a simulated dashboard value are not substitutes for qualified online evidence.

![ZTM Vanguard X controller and worker fleet dashboard, populated with illustrative sample measurements](https://raw.githubusercontent.com/I2PS-Engineering/ZTM_X/main/assets/gui/ztm-vanguard-x-fleet-demo.png)

*Vanguard X interface illustration. Displayed activity is demonstrative, not a claim of field-tested fleet output.*

> **Engineering test status — 8 November 2026:** Application packages and software test evidence are available for ZTM X 1.0.0. The independent 24-hour physical-hardware trial and macOS signing/notarization have **not** been completed. The figures in interface examples must not be reported as validated machine performance.

| Application | Linux | macOS |
|:--|:--|:--|
| **ZTM Special Ops X** | Native worker application: hardware preparation, qualified benchmarking, six live chart families, report handling and authorization-gated Mining | Native macOS application: machine-level benchmarking, live reporting, research upload and authorization-gated Mining |
| **ZTM Vanguard X** | Paired-worker fleet management **plus its own independent local engine** | Paired Linux-worker management **plus its own independent local engine** |

**One ecosystem, two applications, four platform packages.** Vanguard's local Benchmark does not require a separate Special Ops X installation or window. A paired worker remains identifiable and controllable regardless of whether the operator is viewing its own application or the Vanguard dashboard.

## Contents

1. [Architecture and application responsibilities](#1-architecture-and-application-responsibilities)
2. [Physical-machine research methodology](#2-physical-machine-research-methodology)
3. [Benchmark execution and evidence qualification](#3-benchmark-execution-and-evidence-qualification)
4. [Approved mathematical research reference](#4-approved-mathematical-research-reference)
5. [ZTM Special Ops X desktop interface](#5-ztm-special-ops-x-desktop-interface)
6. [ZTM Vanguard X fleet interface](#6-ztm-vanguard-x-fleet-interface)
7. [Network pairing and control boundaries](#7-network-pairing-and-control-boundaries)
8. [Installation, runtime and platform behavior](#8-installation-runtime-and-platform-behavior)
9. [Research reports, Mining authorization and privacy](#9-research-reports-mining-authorization-and-privacy)
10. [Interpreting results and operating safely](#10-interpreting-results-and-operating-safely)
11. [Distribution, contributions and license](#11-distribution-contributions-and-license)
12. [Test packages and interface previews](#12-test-packages-and-interface-previews)

---

## 1. Architecture and application responsibilities

ZTM X separates **measurement at the machine** from **oversight across the fleet**.

```text
                     ZTM VANGUARD X
               Controller + local workload
             Fleet | Terminal | Telemetry | Reports
                              |
                 Manually approved LAN pairing
                              |
          +-------------------+-------------------+
          |                   |                   |
   SPECIAL OPS X       SPECIAL OPS X       SPECIAL OPS X
      Worker A             Worker B             Worker N
   Local metrics        Local metrics        Local metrics
   Local reports        Local reports        Local reports
```

The diagram is conceptual; it does not imply that connected machines are automatically discovered or that every platform pairing combination is shipped in 1.0.0.

**ZTM Special Ops X** owns the machine-side research lifecycle: preflight inspection, hardware preparation where supported, workload supervision, benchmark trials, sustained validation, qualified research reports and explicitly authorized personal Mining. It presents local telemetry and can operate with a desktop session or, on Linux, in a managed background configuration.

**ZTM Vanguard X** is the operator's fleet view. It remembers manually paired workers, shows their identity and observed state, presents permitted Start/Stop actions, and consolidates real measurements. **The controller is also a worker in its own right**: selecting it enables its local preparation and benchmarking controls without launching a second ZTM application. The controller contributes to fleet totals only while it is performing measured work.

Every participating machine has a stable **ZTM ID**. The human-readable worker name may be edited without changing the underlying identity. Changes in network address do not create a new machine identity. Disconnected devices are identified as unavailable rather than silently counted as actively reporting workers.

The research and control roles are deliberately distinct from the external report and authorization service. Dashboard telemetry remains a local operational concern; submission of a report is a separate, explicit action. The public documentation does not expose the server's protective authorization mechanisms.

## 2. Physical-machine research methodology

ZTM X treats processor tuning as a constrained physical experiment, not a universal preset. Two machines with the same CPU model may diverge because of memory rank and timing, channel configuration, thermal interfaces, firmware, power limits, background load or actual memory bandwidth and latency.

The experiment evaluates factors such as:

| Measurement or constraint | Engineering significance |
|:--|:--|
| **Logical and physical topology** | Allowed CPUs, physical cores, SMT siblings, cache-sharing groups and, when exposed, performance/efficiency cores |
| **Cache geometry** | Available L2/L3 capacity relative to RandomX per-worker working-set pressure |
| **Memory capacity and access** | Available RAM, page allocation, measured copy-bandwidth proxy, pointer-chase latency and operating-system memory pressure |
| **CPU placement** | Legal affinity/worker assignments and potential contention among siblings, cache domains and NUMA locality |
| **Power and thermal state** | Sustainable operating conditions, thermal saturation and clock/power limitations over time |
| **Pool and network state** | Readiness, work availability, share acceptance/rejection and interruptions that affect qualified observation |
| **Operational stability** | Process failures, stop requests, missing sensors and interruptions recorded as events rather than hidden from results |

For a useful analogy, the CPU is the engine, memory access is traction, scheduling is the gear selection, cooling is endurance and pool difficulty is the slope: raising reported pool difficulty does not make a processor calculate more RandomX hashes per second.

![Machine-specific processor performance analogy from I2PS Engineering](https://raw.githubusercontent.com/I2PS-Engineering/ZTM_SpecialOps/main/assets/figures/01-motorcycle-var-diff-whiteboard.png)

### Platform-specific preparation

**Linux** may evaluate legal CPU-affinity placements, physical/SMT grouping, cache-domain contention and supported memory preparation. Requested Huge Pages or privileged settings are distinguished from those actually applied; an unsupported or rejected operation is recorded as such.

**Intel macOS** uses the macOS-native preparation and execution path. Linux-only affinity and MSR mechanisms are not represented as macOS features. **Mac Apple Silicon** uses its supported architecture-specific automatic benchmark path and is not labeled as having Intel-only optimization or unsupported memory-page controls. Platform comparisons must preserve these distinctions.

### Controlled comparison

A measurement is useful only when the environment and the metric are identifiable. ZTM X begins with the verified XMRig automatic reference configuration, evaluates physically motivated candidates within a bounded search and repeats reference observation to detect drift. The winning configuration is derived from measured evidence, not simply the first candidate to produce a high instantaneous reading.

Memory-copy and pointer-latency probes are **indicators**, not substitutes for observed RandomX throughput. Similarly, shares provide external quality evidence but do not turn share difficulty into hashrate. Observed sustained hashrate, measured stability and pool acceptance remain distinct dimensions of the result.

## 3. Benchmark execution and evidence qualification

### From preflight to candidate selection

The benchmark workflow proceeds through **machine inspection, reference configuration, bounded candidate trials, comparison and endurance validation**. It establishes the available machine capabilities before making a performance claim. Unsupported preparation features, denied permissions or missing probes are kept visible as limitations.

The established trial framework evaluates **no more than seven full configurations**, with a target of **600 qualified online seconds per full candidate after warm-up**. Linux may also conduct shorter legal-affinity screens before a candidate proceeds to full evaluation. Candidate construction considers the available cores, cache budgets, worker count, initialization count, memory conditions and permitted placement topology.

Each qualifying trial records actual XMRig work and a reason for accepting or excluding the observation. A repeat of the reference profile helps separate changing environmental conditions from a genuine improvement. When results are too close to resolve, the result remains a practical tie or invokes additional comparison; it is not forced into a false winner.

### Qualified online time

The endurance target is **86,400 qualified online seconds (24 hours)**. This is not automatically equivalent to 24 hours of wall-clock runtime. Preparation, dataset initialization, disconnected periods, unconfirmed pool work, paused operation and shutdown are not represented as productive online time.

A benchmark that has run for 24 wall-clock hours may still be incomplete if it accumulated fewer than 86,400 qualified seconds. A stopped or interrupted run retains its research evidence and actual stop reason but cannot be marked as fully qualified through a displayed elapsed-time counter alone.

The user-facing progress indicator must distinguish **qualified progress** from preparation or other provisional elapsed time. A full research record supports later comparison of online measurement, candidate trial accounting and sustained-run evidence.

### Statistical evaluation and reporting

The measurement method emphasizes robust summaries over single peak values. Qualified trials use hashrate samples, variability estimates, drift controls and statistical comparison where sufficient evidence exists. When candidates remain close, a paired **A–B–B–A** comparison can help expose time-dependent environmental effects.

The workflow preserves the distinction among **best observed instantaneous hashrate**, **candidate trial summary**, **selected configuration**, and **qualified sustained result**. These are different facts and must not be merged into one unsupported performance claim.

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

## 5. ZTM Special Ops X desktop interface

ZTM Special Ops X presents four principal areas: **ONGOING**, **PROFILE RESULTS**, **FINAL RESULTS** and **MINING**. The workflow covers local preparation, a visible **Start Benchmark** action, active operation, progress checkpoints, Stop, report review and personal Mining where explicitly permitted. There is **no separate wallet-generation or wallet-recovery tab**.

- **ONGOING** reports the live phase, permitted controls, elapsed and qualified progress, relevant preparation state and important stop/transition events.
- **PROFILE RESULTS** shows candidate-level evidence, the current profile, event markers and synchronized technical charts.
- **FINAL RESULTS** separates a completed qualified result from an incomplete or interrupted research run, with measurable evidence and available report details.
- **MINING** presents the authorization state and, when eligible, explicit Start Mining/Stop actions with an operator-supplied **public receiving address**. An unavailable authorization is not bypassed by a completed local Benchmark.

The application displays its **1.0.0 version, build identifier and code fingerprint** through the supported title triple-click interaction. The layout adapts to screen DPI and available display area. Telemetry comes from the active supervised workload; remotely triggered jobs must not leave the local interface showing an unrelated idle state.

### Six live chart families

The chart window shares a rolling **15-minute timeline**, where supported, with event annotations at actual state changes.

| Chart | Intended visual treatment | Interpretation |
|:--|:--|:--|
| CPU temperature | Orange | Actual available temperature sensor measurements; otherwise **unavailable** |
| CPU usage | Yellow | Observed system-wide CPU utilization |
| RAM usage | Red | Actual system RAM usage; not silently substituted with an invented process-memory reading |
| Hashrate | Green | Observed online XMRig throughput; disconnected or unmeasured intervals identified distinctly |
| Pool VarDiff | Violet | The pool's reported share-difficulty behavior; not a multiplier for local hashing speed |
| Share activity | Accepted green, rejected red | Cumulative share counts and associated state changes, with no fabricated discontinuities |

Markers may denote a profile transition, affinity-screen decision, permitted runtime change, donation interval, validated winner or interruption. A trophy/winner marker identifies a selection event; **it is not, by itself, proof of 24-hour qualification**. Unknown or unsupported readings are shown as unavailable rather than interpolated as facts.

## 6. ZTM Vanguard X fleet interface

Vanguard X exposes a fleet dashboard, in-window operational tabs and the controller's native local workload. Its platform-specific title is **ZTM VANGUARD X | LINUX** or **ZTM VANGUARD X | macOS**.

### Topology-first dashboard

The controller appears at the top of the topology. With **one paired worker**, that worker appears **directly underneath the controller**, joined by a connecting line. With more workers, the central pane wraps cards responsively with **up to four workers per row**. A resizable divider separates the fleet topology from the **CUMULATIVE FLEET** pane, allowing operators to adjust the information density without hiding core actions.

A node card identifies the device by its editable display name and immutable **ZTM ID**, and reports available job status, observed hashrate and share counts. The current state is clearly marked as **Benchmark**, **Mining**, **Idle**, **Offline** or measurement unavailable; a disconnected worker is not treated as actively hashing.

### Controller operates independently

Selecting the **controller card** exposes that machine's own Prepare, **Start Benchmark**, Stop, local output and telemetry. It can execute its supported research workload and report measured values without launching Special Ops X separately. Its activity contributes to cumulative fleet statistics when it is actually reporting verified operational measurements. Its idle or offline periods do not produce simulated hashrate.

Selecting a **paired worker** changes the action and telemetry context to that worker. A Start Benchmark initiated from Vanguard is reflected in the worker's live state. Actions must preserve the same workload and authorization boundaries used by the worker's own interface.

### Integrated operational views

**Fleet Dashboard**, **Terminal**, **Telemetry** and **Reports** are tabs in the **same Vanguard window**, not detached windows. They retain the selected node and expose the permitted information relevant to that node.

- **Terminal** provides read-only supervised application output, **not** an unrestricted operating-system command shell.
- **Telemetry** provides the selected node's currently available measurements and charts, including gaps and sensor limitations.
- **Reports** presents the relevant local or received research evidence and its status without turning an incomplete trial into a qualified report.
- **CUMULATIVE FLEET** sums actually reported, eligible measurements; controller and worker Benchmark/Mining participation are differentiated so the total is not mistaken for one uniform wallet-Mining session.

No static number from an illustrative dashboard is a measured fleet result.

## 7. Network pairing and control boundaries

ZTM Vanguard X uses **Pair by address…**: the operator enters a known local-network address shown by an enabled worker. The design does **not** depend on mDNS broadcasts or automatic discovery of every machine on the LAN. Existing LAN and Wi-Fi workers can be represented in the same dashboard after their approved pairing; static IP assignment is not a prerequisite to the initial workflow.

For **Linux Special Ops X 1.0.0**, the operator enables Vanguard pairing at the worker, reads its advertised address, starts pairing from Vanguard and confirms the displayed matching code. The worker's local authorization step remains necessary; access is not granted merely because a network endpoint answered. Supported preparation operations may require an administrator confirmation.

Once paired, Vanguard remembers the worker's identity and approved connection. It offers only the workload actions and read-only operational views defined by the application; **pairing does not provide a general-purpose remote shell**. Start/Stop, current-state reporting and Mining eligibility follow the same rules regardless of whether control was initiated locally or remotely.

**Release boundary:** In 1.0.0, direct pairing is supported for **Linux Special Ops X workers**. Pairing a macOS Special Ops X worker to Vanguard X is **not included** in this testing build. On macOS, Vanguard X still provides its **own** local workload and supported fleet management of paired Linux workers.

## 8. Installation, runtime and platform behavior

### Linux — shell-based applications

The Linux packages distribute an **`install.sh` installer**, not a Debian `.deb` application package. The Special Ops X target is Ubuntu Linux x86-64 with Python 3.10 or later and the required OS/runtime dependencies. Installation and live benchmark use require network access as described in the bundled platform notes.

From the extracted **ZTM Special Ops X Linux** package:

```bash
bash install.sh --verify-only
bash install.sh --desktop
```

For a worker without a desktop session, the corresponding mode is:

```bash
bash install.sh --headless
```

The regular installer may also be run as `bash install.sh`. The verification-only mode checks the supplied package without performing installation. The installer uses the host package manager only where needed for system dependencies and recognized predecessor installations; it does **not** install ZTM X itself through a `.deb` artifact.

The installed worker can be launched from the desktop menu or with `ztm-special-ops`. Headless pairing is available through the installed `ztm-worker` command. Background operation and optional restart after reboot are managed explicitly; **installation or reboot must not silently start Benchmark or Mining**.

**ZTM Vanguard X Linux** has its own installer and native local engine. It does not require starting the Special Ops X desktop application before the controller can benchmark itself.

### macOS — separate applications and native local engines

The two macOS ZIPs provide **ZTM Special Ops X** and **ZTM Vanguard X** as independent applications, each using the supported native macOS workload path. The packaged installer accepts:

```bash
./install.sh --verify-only
./install.sh
```

Follow the individual kit's prerequisites and macOS permission prompts. **Intel macOS** and **Mac Apple Silicon** use their supported architecture-specific research behavior. An Apple Silicon automatic baseline must not be described as an Intel count/affinity optimization. Available hardware sensors and system privileges can differ between machines.

The macOS packages in this handoff are **engineering test kits**. They are **not yet signed or notarized** release distributions. Installation and physical qualification must not be represented as already completed.

### Identity, upgrades and integrity

An installation should preserve the machine's stable ZTM ID, existing research records, consent state, approved pairing data and applicable local settings. Installer integrity verification uses the included SHA-256 checksums; **a checksum alone does not establish publisher authenticity**. A recognized predecessor installation may be retired only through the installer's controlled upgrade path, without arbitrary removal of unrelated software.

Never upgrade or replace a package over an active, supervised Benchmark or Mining run. The software build and any report should remain traceable to their actual version and machine identity.

## 9. Research reports, Mining authorization and privacy

### Research evidence

A ZTM X report is a structured machine-research record. Subject to hardware capabilities and availability, it can contain:

| Evidence class | Typical contents |
|:--|:--|
| Machine context | Stable ZTM ID, platform, available CPU topology, hardware characteristics and relevant capabilities |
| Preparation | Supported setup, observed page/memory state, successful and unsuccessful prerequisites |
| Candidate research | Reference measurements, candidate profiles, eligible worker/placement decisions and actual sample statistics |
| Endurance | Qualified online seconds, measured sustained throughput, interruptions and completion state |
| Operational quality | Available accepted/rejected shares, pool status, event journal and observed workload stability |
| Environment | Available thermal, CPU and RAM measurements, with missing fields explicitly marked unavailable |

A local report is **not** an authorization token. **Personal Mining requires a current, valid I2PS authorization** and compliance with the application’s qualification conditions. A fresh ZTM X benchmark and any required research submission are separate from the final authorization decision; a result or screenshot alone does not unlock Mining.

### User control and personal Mining

Personal Mining requires explicit user action, consent and an operator-supplied **public Monero receiving address**. No wallet recovery phrase or private key is requested. Benchmark and personal Mining must not operate concurrently on the same machine. An authorization refusal, unavailable required condition or a user Stop action is not reinterpreted as a permission to continue.

A research upload occurs through the application's documented consent flow. Fleet operational telemetry is distinct from report submission. Only the applicable research fields should be sent, and information that the platform cannot measure is not fabricated. Machine IDs, reports and permitted connection credentials should be handled as potentially sensitive operational information.

The public README documents **what the operator can verify and control** without disclosing private server implementation details.

## 10. Interpreting results and operating safely

**Measured** means collected on the actual machine during an observed workload. **Illustrative** means a GUI sample or explanatory figure. **Provisional** means the evidence is not yet sufficient for a final endurance claim. **Unavailable** means the relevant sensor or measurement did not provide trustworthy data.

A technical comparison should document CPU model and topology, memory capacity/configuration, RAM-performance evidence when available, cooling and ambient conditions, operating-system state, thread configuration, online measurement window and interruptions. Comparing peak values from unequal sessions is not a reproducible engineering result.

Sustained CPU workloads increase energy consumption and heat. Operate only on machines you own or are expressly authorized to test, with adequate cooling and ventilation. ZTM X does not guarantee mining revenue, hardware improvement, reduced power consumption or that requested privileged optimizations can be applied on every machine.

An incomplete run stays incomplete. A failed sensor remains unavailable. A pool disconnect is not credited as qualified online work. Stopping a supervised workload must preserve the applicable stop reason and research evidence without granting qualification by inference.

### Present qualification boundary

**ZTM X 1.0.0 is awaiting the first complete physical 24-hour qualification cycle.** Packaged test reports and software checks establish implementation evidence, not a guaranteed sustained hashrate on a particular CPU. No result in an illustrative screenshot should be cited as the completed hardware baseline for this release.

## 11. Distribution, contributions and license

ZTM X application code, the machine-specific performance research and the proprietary optimization components are **I2PS Engineering** work. The applications are **proprietary, not open source**, and their distribution and permitted use are subject to the accompanying **I2PS Engineering noncommercial license**. Availability of this README or of a test package does not grant rights to redistribute, modify or commercially exploit proprietary source.

For expressly permitted personal Mining, the documented contribution schedule uses **1% of runtime for I2PS Engineering**, separately from the **1% XMRig contribution configured in the mining runtime**. A runtime allocation is not a guaranteed percentage of accepted shares or income. The Benchmark's disclosed research destination is different from a user's personal Mining receiving address.

The presence of interoperable third-party runtimes or dependencies does not transfer ownership of those components to I2PS Engineering or override licenses applicable to those components. Distribution packages must preserve any notices required by their respective licenses. **The authorship and project credit for ZTM X belong exclusively to I2PS Engineering.**

## 12. Test packages and interface previews

The handoff contains these **four testing packages**; listing them does not assert they have been published as a public release:

| Archive | Scope |
|:--|:--|
| `ZTM-Special-Ops-X-1.0.0-Linux.zip` | Linux worker, native engine, shell installer |
| `ZTM-Vanguard-X-1.0.0-Linux.zip` | Linux fleet controller and independent local engine |
| `ZTM-Special-Ops-X-1.0.0-macOS.zip` | macOS worker application and native workload engine |
| `ZTM-Vanguard-X-1.0.0-macOS.zip` | macOS fleet controller and independent native local engine |

The documented version identifier is **ZTM X 1.0.0**. Build identifiers and SHA-256 package digests should be distributed alongside the corresponding packages; they must refer to the exact files released, not an earlier or independently modified build.

### ZTM Special Ops X interface

![ZTM Special Ops X Benchmark screen with illustrative processor profiles and telemetry](https://raw.githubusercontent.com/I2PS-Engineering/ZTM_X/main/assets/gui/ztm-special-ops-x-benchmark-demo.png)

*Special Ops X — interface demonstration. The displayed values are example UI data, not a qualified physical-machine measurement.*

---

**I2PS Engineering**  
*Built in Morocco, Engineered in the future.*  
*Technology that empowers.*
