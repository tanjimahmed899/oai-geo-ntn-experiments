# OAI GEO NTN Experimental Evidence

Experimental evidence for a 5G NR Non-Terrestrial Network (NTN) study using OpenAirInterface (OAI) and RF Simulator, focusing on GEO satellite-delay conditions, HARQ behavior, and end-to-end transport performance.

---

## Research Objective

The objective of this experimental work is to evaluate the behavior and performance of an OAI-based 5G NR NTN system under GEO propagation delay.

The experiments currently focus on:

- GEO NTN connectivity
- HARQ ON vs HARQ OFF behavior
- End-to-end RTT
- UDP uplink and downlink performance
- TCP uplink and downlink behavior
- TCP behavior under large GEO delay
- RRC/PDU session stability during transport experiments

---

## Main Experimental Setup

- Platform: OpenAirInterface
- Radio environment: RF Simulator
- NTN scenario: GEO
- GEO propagation delay: 238.74 ms
- NR band: 254
- Numerology: 0
- UE PDU session IP: `10.0.0.3`
- EXT-DN IP: `192.168.70.135`
- UE tunnel interface: `oaitun_ue1`

---

## Experimental Scenarios Completed So Far

### 1. GEO NTN – HARQ OFF

Directory:

`raw_runs/geo_harq_off_20260910T170205Z/`

Available evidence includes:

- gNB log
- UE log
- gNB/UE configurations
- OAI commit information
- iperf3 client/server versions
- Ping connectivity measurement
- TCP UL measurement
- TCP DL measurement
- UDP UL at 1 Mbps
- UDP DL at 1 Mbps
- UDP UL at 2 Mbps
- UDP DL at 2 Mbps

---

### 2. GEO NTN – HARQ ON

Directory:

`raw_runs/geo_harq_on_20260910T181542Z/`

Available evidence includes:

- gNB and UE logs
- HARQ-ON configuration
- Connectivity measurement
- TCP UL/DL measurements
- Post-TCP health checks
- RAN status after TCP tests
- Clock monitoring
- Process-clock monitoring
- Configuration difference information
- OAI commit and iperf3 version information

---

### 3. Clean HARQ-ON Validation

Directory:

`raw_runs/geo_harq_on_clean_20260910T200307Z/`

This run was performed as a cleaner validation run after earlier debugging.

Available evidence includes:

- Clean HARQ-ON configuration
- gNB and UE logs
- Clean ping measurement
- Clean UDP UL measurement
- Clean UDP DL measurement
- TCP UL attempts
- gNB/UE logs preserved before release debugging

Observed behavior:

- UE successfully reached `NR_RRC_CONNECTED`
- PDU session was successfully established
- UE received IPv4 address `10.0.0.3`
- `oaitun_ue1` was successfully brought up
- End-to-end ICMP connectivity was successful
- UDP UL/DL operation was demonstrated
- TCP behavior remained abnormal and required further investigation

---

### 4. Controlled TCP Diagnostic Run

Directory:

`raw_runs/tcp_debug_20260911T060117Z/`

This experiment was created specifically to investigate abnormal TCP behavior.

Evidence includes:

- `gnb.log`
- `ue.log`
- `tcp_ul_baseline.log`
- `ss_tcp_ul.log`
- `tcp_ul_baseline.pcap`
- `tcp_packets.txt`
- Exact gNB and UE configurations

Important observations from the controlled TCP run:

- TCP connection establishment was successful
- TCP ACKs were observed
- TCP congestion window increased during the test
- TCP throughput was highly intermittent/bursty
- A short burst of approximately 2.10 Mbps was observed during one interval
- iperf3 failed to complete the final result exchange
- The UE tunnel remained UP after the TCP test
- Post-TCP ICMP connectivity remained operational
- No new RRC release was observed during the clean TCP failure

Therefore, the clean TCP failure was not directly caused by an RRC/PDU-session release.

The exact TCP root cause is still under investigation.

---

## Current Preliminary Findings

### Connectivity

The GEO NTN setup is operational.

The following have been successfully demonstrated:

- gNB operation
- UE operation
- RFsim-based NTN connection
- RRC connection
- PDU session establishment
- UE tunnel creation
- EXT-DN reachability

### Ping

Clean measurements showed:

- 0% packet loss
- RTT in the several-hundred-millisecond range

The high RTT is consistent with the GEO-delay experimental environment, although detailed statistical analysis is still pending.

### UDP

UDP UL and DL communication has been successfully demonstrated.

Experiments currently include:

- 1 Mbps UL
- 1 Mbps DL
- Additional HARQ-OFF 2 Mbps UL/DL measurements

### TCP

TCP is currently the main unresolved experimental issue.

Observed characteristics include:

- Successful TCP handshake
- ACK reception
- Increasing congestion window
- Large and variable RTT
- Highly bursty data delivery
- Long zero-throughput intervals
- iperf3 result-exchange failure

Further packet-level analysis is required before drawing a final conclusion.

---

## Important Debugging Finding

An earlier RRC release event was investigated separately.

During the later controlled TCP baseline experiment:

- `oaitun_ue1` remained UP
- Post-test ping remained successful
- No new gNB RRC release was detected

This indicates that the observed clean TCP failure can occur while the RRC connection and PDU session remain operational.

---

## iperf3 Version Note

During current experiments:

- Host/client iperf3: 3.16
- EXT-DN/server iperf3: 3.9

This version difference is being treated as an experimental variable.

It has not yet been proven to be the cause of the TCP anomaly.

A future controlled experiment should use matching iperf3 versions on both endpoints.

---

## Repository Structure

```text
raw_runs/
├── backup_ntn/
├── geo_harq_off_20260910T170205Z/
├── geo_harq_on_20260910T181542Z/
├── geo_harq_on_clean_20260910T200307Z/
└── tcp_debug_20260911T060117Z/
