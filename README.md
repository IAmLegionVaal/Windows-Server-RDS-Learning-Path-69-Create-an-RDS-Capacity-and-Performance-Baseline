# Windows Server RDS Learning Path 69 — Create an RDS Capacity and Performance Baseline

**Level:** Expert · **Module:** 69/70

## Goal
Build a repeatable capacity baseline for CPU, memory, storage, network, logon time and user density.

## Setup
1. Define representative task, knowledge and power-user workloads.
2. Record VM hardware, storage and network design.
3. Run single-user and multi-user workloads.
4. Capture CPU, available memory, disk latency/IOPS, network throughput, session count and logon duration.
5. Identify saturation points and required safety margin.
6. Calculate current and projected capacity.
7. Define alert thresholds and a repeatable retest method.

```powershell
$Counters = @(
    '\Processor(_Total)\% Processor Time'
    '\Memory\Available MBytes'
    '\PhysicalDisk(_Total)\Avg. Disk sec/Transfer'
    '\Network Interface(*)\Bytes Total/sec'
    '\Terminal Services\Active Sessions'
)

Get-Counter -Counter $Counters -SampleInterval 5 -MaxSamples 120 |
    Export-Counter -Path '.\RDS-Capacity-Baseline.blg' -FileFormat BLG -Force
```

Review the captured sample:

```powershell
Import-Counter -Path '.\RDS-Capacity-Baseline.blg' |
    Select-Object -ExpandProperty CounterSamples |
    Select-Object Path,CookedValue,Timestamp
```

## Evidence
Store environment specification, workload definition, raw counter export, logon timings, session density, capacity model, bottlenecks and alert thresholds under `evidence/`.

## Troubleshooting
A short idle sample is not a capacity test. Repeat tests at comparable times and correlate CPU, memory, storage and network instead of relying on one counter.

## Security
Use synthetic data and users. Performance captures can reveal server names and activity patterns, so sanitize public evidence.

## Rollback
Remove test workloads and restore normal collection limits after measurement.

## Next
`Windows-Server-RDS-Learning-Path-70-Capstone-Secure-Multi-Server-RDS-Deployment`
