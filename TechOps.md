## **SECTION A – Command Reference**

*(Grouped by category: System Monitoring, Networking, Performance, Storage, Time Sync, Automation)*

---

### **A.1 System & Process Monitoring**

---

#### **1. `ps`**

* **What it does**: Lists processes and their details.
* **Why for Tech Ops**: Identify stuck, high CPU, or zombie processes in trading apps.
* **Syntax**:

```bash
ps aux --sort=-%cpu | head
```

* **Sample Output**:

```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
trader    2435 95.0  0.2 112500  8840 ?        R    10:12   0:05 ./feedhandler
```

* **Key fields**:

  * `%CPU` – CPU usage
  * `%MEM` – memory usage
  * STAT – R (running), S (sleep), D (disk wait), Z (zombie)
* **Tip**: Sort by memory: `ps aux --sort=-%mem`

---

#### **2. `top`**

* **What**: Interactive process monitor.
* **Why**: Live troubleshooting during incidents.
* **Run**:

```bash
top
```

* **Sample**:

```
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 2435 trader    20   0  112500   8840   2304 R  95.0  0.2   0:05.17 feedhandler
```

* **Interpret**:

  * PR/NI = priority/nice
  * RES = actual RAM used
  * Use `P` to sort by CPU, `M` for memory.

---

#### **3. `htop`**

* **What**: Enhanced `top` with colors & easier navigation.
* **Why**: Quickly spot overloaded cores.
* **Run**:

```bash
htop
```

* **Sample**: CPU usage bars per core; you can F6 to sort by custom metrics.
* **Tip**: Press F2 → "Available Meters" → add "CPU affinity" to see if processes are pinned correctly.

---

#### **4. `lsof`**

* **What**: Lists open files/sockets.
* **Why**: See what network ports or files a process uses.
* **Run**:

```bash
lsof -p 2435 | grep UDP
```

* **Sample**:

```
feedhandler 2435 trader  12u  IPv4 123456 0t0  UDP 239.192.0.1:5000
```

* **Interpret**: Confirms feedhandler is joined to correct multicast.

---

#### **5. `taskset`**

* **What**: Sets CPU affinity.
* **Why**: Keep trading processes on isolated cores.
* **Run**:

```bash
taskset -cp 2,3 2435
```

* **Output**:

```
pid 2435's current affinity list: 2,3
```

---

#### **6. `numactl`**

* **What**: Controls NUMA CPU/memory binding.
* **Why**: Avoid cross-node latency.
* **Run**:

```bash
numactl --hardware
```

* **Sample**:

```
available: 2 nodes (0-1)
node 0 cpus: 0 1 2 3
node 1 cpus: 4 5 6 7
```

---

#### **7. `pidstat`**

* **What**: CPU usage per PID over time.
* **Why**: Spot spikes causing jitter.
* **Run**:

```bash
pidstat -u 1
```

* **Sample**:

```
11:22:35 AM   UID       PID    %usr %system  %guest   %wait  %CPU  Command
11:22:36 AM  1001      2435   45.00   10.00    0.00    0.00  55.00 feedhandler
```

---

#### **8. `vmstat`**

* **What**: Memory, process, I/O summary.
* **Why**: Quick high-level health check.
* **Run**:

```bash
vmstat 1
```

* **Sample**:

```
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 400000  20000 800000    0    0     1     2  200  400 20 10 70  0  0
```

---

#### **9. `free`**

* **What**: Shows memory usage.
* **Run**:

```bash
free -h
```

* **Sample**:

```
              total        used        free      shared  buff/cache   available
Mem:           15Gi       3.0Gi        9.0Gi       200Mi       3.0Gi       11Gi
```

---

#### **10. `sar`**

* **What**: Historical CPU/memory/network usage.
* **Why**: See if issue matches a pattern in the past.
* **Run**:

```bash
sar -u 1 5
```

---

### **11. `iotop`**

* **What**: Shows real-time I/O usage per process.
* **Why for Tech Ops**: Identify processes causing disk bottlenecks during trading hours.
* **Example**:

```bash
sudo iotop -oPa
```

* **Sample Output**:

```
Total DISK READ :      5.00 M/s | Total DISK WRITE :     10.00 M/s
PID  PRIO  USER     DISK READ  DISK WRITE  COMMAND
2435 be/4 trader       0.00 B/s   5.00 M/s feedhandler
```

* **Interpretation**:

  * If a process writes heavily to disk during trading, may cause latency spikes.

---

### **12. `dstat`**

* **What**: Versatile real-time resource monitor.
* **Why**: Quickly view CPU, disk, net, and paging stats together.
* **Example**:

```bash
dstat -cdngy
```

* **Sample Output**:

```
----total-cpu-usage---- -dsk/total- -net/total- ---paging-- ---system--
usr sys idl wai stl| read  writ| recv  send|  in   out | int   csw
 15   5  80   0   0|  1k   50k|  10k  20k|   0     0 | 300   400
```

* **Interpretation**:

  * Easy to correlate spikes between CPU, I/O, and network.

---

### **13. `kill`**

* **What**: Sends a signal to a process (e.g., terminate).
* **Why**: End hung processes cleanly or forcefully.
* **Example**:

```bash
kill -TERM 2435
kill -9 2435   # force kill
```

* **Interpretation**:

  * Use `-TERM` first for graceful shutdown; `-9` only if unresponsive.

---

### **14. `nice` & `renice`**

* **What**: Sets CPU scheduling priority.
* **Why**: Lower priority for non-critical jobs during trading hours.
* **Example**:

```bash
nice -n 10 some_script.sh
renice -n -5 -p 2435
```

* **Interpretation**:

  * Negative value = higher priority, positive = lower.

---

### **15. `uptime`**

* **What**: Shows system uptime, load averages.
* **Why**: Detect unexpected reboots or sustained load.
* **Example**:

```bash
uptime
```

* **Output**:

```
10:35:14 up 3 days,  4:12,  2 users,  load average: 0.34, 0.40, 0.45
```

* **Interpretation**:

  * Load averages > CPU cores count = possible overload.

---

## **A.2 – Networking Commands**

---

### **16. `ip addr` / `ip a`**

* **What**: Lists network interfaces and IP addresses.
* **Why**: Confirm interface status, IP assignments.
* **Example**:

```bash
ip addr show eth0
```

* **Output**:

```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 10.1.1.10/24 brd 10.1.1.255 scope global eth0
```

* **Interpretation**:

  * Check MTU, link status, assigned IPs.

---

### **17. `ip link`**

* **What**: Show or configure interfaces.
* **Why**: Verify operational state, enable multicast.
* **Example**:

```bash
sudo ip link set dev eth0 multicast on
```

* **Output**: No output (success).
* **Interpretation**:

  * Required for multicast feed joins.

---

### **18. `ip route`**

* **What**: Shows routing table.
* **Why**: Ensure packets follow expected low-latency paths.
* **Example**:

```bash
ip route show
```

* **Sample**:

```
default via 10.1.1.1 dev eth0
10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.10
```

* **Interpretation**:

  * Verify static routes for exchange connections.

---

### **19. `ip neigh`**

* **What**: Shows ARP table (IP→MAC mapping).
* **Why**: Troubleshoot L2 connectivity.
* **Example**:

```bash
ip neigh
```

* **Sample**:

```
10.1.1.1 dev eth0 lladdr 00:25:90:12:34:56 REACHABLE
```

---

### **20. `ss`**

* **What**: Displays socket statistics.
* **Why**: Find listening services, established connections.
* **Example**:

```bash
ss -u -a 'sport = :5000'
```

* **Sample**:

```
UNCONN 0 0 239.192.0.1:5000 *:*
```

* **Interpretation**:

  * Confirms multicast listener.

---

### **21. `ethtool`**

* **What**: NIC configuration and stats.
* **Why**: Check link speed, offloads, packet drops.
* **Examples**:

```bash
ethtool eth0
ethtool -S eth0 | grep drop
```

* **Sample Output**:

```
rx_dropped: 10
```

* **Interpretation**:

  * Non-zero drops during trading hours = investigate.

---

### **22. `tcpdump`**

* **What**: Packet capture tool.
* **Why**: Debug market data feed, latency.
* **Example**:

```bash
sudo tcpdump -ni eth0 'udp and multicast and port 5000'
```

* **Sample**:

```
10:12:34.567890 IP 239.192.0.1.5000 > 10.1.1.10.35000: UDP, length 512
```

* **Interpretation**:

  * Confirm feed arrival and packet rate.

---

### **23. `mtr`**

* **What**: Continuous traceroute + ping.
* **Why**: Find network hops causing packet loss/latency.
* **Example**:

```bash
mtr -rw 10.1.1.1
```

---

### **24. `traceroute`**

* **What**: Shows hop-by-hop path to a destination.
* **Why**: Identify routing changes or bottlenecks.
* **Example**:

```bash
traceroute 192.168.1.1
```

---

### **25. `dig`**

* **What**: DNS query tool.
* **Why**: Verify low-latency DNS responses.
* **Example**:

```bash
dig A example.com +trace
```

---

### **26. `iptables` / `nft`**

* **What**: Firewall rules inspection.
* **Why**: Ensure multicast/market-data ports not blocked.
* **Example**:

```bash
sudo iptables -L -nv
sudo nft list ruleset
```

---

### **27. `tc`**

* **What**: Traffic control.
* **Why**: Shape/impair traffic for testing.
* **Example**:

```bash
sudo tc qdisc add dev eth0 root netem loss 0.1% delay 200us 20us
sudo tc qdisc del dev eth0 root
```

---

## **A.3 – Performance & Profiling Tools**

---

### **28. `perf`**

* **What**: Kernel-level performance profiler.
* **Why for Tech Ops**: Detect CPU bottlenecks, cache misses, branch mispredictions that can cause microsecond latency hits.
* **Example**:

```bash
sudo perf stat -p 2435 sleep 10
```

* **Sample Output**:

```
 Performance counter stats for process id '2435':

       1,234,567      cycles
         123,456      instructions              # 0.10  insn per cycle
           4,567      cache-misses
```

* **Interpretation**:

  * IPC (instructions per cycle) low → possible cache thrash or stalls.
  * High cache misses → investigate data locality.

---

### **29. `pidstat` (CPU/mem/I/O per process)**

* **What**: Fine-grained per-process metrics.
* **Example**:

```bash
pidstat -u -p 2435 1
```

* **Output**:

```
11:22:35 AM   UID       PID    %usr %system  %CPU  Command
11:22:36 AM  1001      2435   45.00   10.00  55.00 feedhandler
```

* **Interpretation**:

  * CPU breakdown between user space and kernel space.

---

### **30. `sar` (sysstat)**

* **What**: Historical performance data (CPU, mem, net, etc.).
* **Why**: Correlate issues to historical load.
* **Example**:

```bash
sar -n DEV 1 5
```

* **Output**:

```
IFACE   rxpck/s   txpck/s
eth0      1500      1400
```

---

### **31. `vmstat`**

* **What**: Shows memory, processes, paging, and CPU usage.
* **Example**:

```bash
vmstat 1
```

* **Interpretation**:

  * High `wa` (I/O wait) can cause latency stalls.

---

### **32. `uptime`**

* **What**: Shows system uptime and load average.
* **Example**:

```bash
uptime
```

* **Interpretation**:

  * Load average should be less than number of CPU cores.

---

### **33. `mpstat`**

* **What**: CPU usage per core.
* **Why**: Spot uneven core usage or interrupt saturation.
* **Example**:

```bash
mpstat -P ALL 1
```

---

## **A.4 – Storage & I/O Tools**

---

### **34. `iostat`**

* **What**: Disk I/O performance per device.
* **Why**: Identify slow storage impacting log writes or market capture.
* **Example**:

```bash
iostat -xz 1
```

* **Output**:

```
Device:         r/s     w/s  rkB/s  wkB/s  %util
nvme0n1       100.0   50.0  2000   500    80.0
```

* **Interpretation**:

  * `%util` near 100% → disk saturated.

---

### **35. `df`**

* **What**: Disk space usage.
* **Example**:

```bash
df -h
```

* **Output**:

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme0n1p1  100G   50G   45G  53% /
```

---

### **36. `du`**

* **What**: Directory size usage.
* **Example**:

```bash
du -sh /var/log/*
```

* **Interpretation**:

  * Find large log files causing storage pressure.

---

### **37. `blktrace`**

* **What**: Block-level I/O tracing.
* **Why**: Debug latency in storage stack.
* **Example**:

```bash
sudo blktrace -d /dev/nvme0n1 -o - | blkparse -
```

---

## **A.5 – Time Synchronization**

---

### **38. `ptp4l`**

* **What**: Precision Time Protocol daemon for hardware timestamp sync.
* **Why**: Trading requires sub-microsecond clock sync.
* **Example**:

```bash
sudo ptp4l -i eth0 -m
```

* **Output**:

```
ptp4l[1234.567]: master offset  12 s2 freq  -15 path delay   500
```

* **Interpretation**:

  * Offset = time difference in nanoseconds or microseconds.

---

### **39. `phc2sys`**

* **What**: Syncs PHC (NIC hardware clock) to system clock.
* **Example**:

```bash
sudo phc2sys -s eth0 -w
```

---

### **40. `chronyc`**

* **What**: Monitor chrony NTP/PTP status.
* **Example**:

```bash
chronyc tracking
```

* **Output**:

```
Reference ID    : PTP
Stratum         : 1
Last offset     : 0.000002 seconds
```

---

### **41. `pmc`**

* **What**: Query PTP grandmaster.
* **Example**:

```bash
pmc -u -b 0 "GET TIME_STATUS_NP"
```

---

## **A.6 – Automation & Scripting Tools**

---

### **42. `bash` strict mode**

* **What**: Prevents silent script errors.
* **Example**:

```bash
#!/usr/bin/env bash
set -Eeuo pipefail
IFS=$'\n\t'
```

---

### **43. `ansible`**

* **What**: Infrastructure automation tool.
* **Example**:

```bash
ansible -i hosts all -m ping
```

* **Output**:

```
gw01 | SUCCESS => {"changed": false, "ping": "pong"}
```

---

### **44. `aws` CLI**

* **What**: AWS automation & control.
* **Example**:

```bash
aws ec2 describe-instances --region us-east-1
```

---

### **45. `jq`**

* **What**: JSON processor for CLI.
* **Example**:

```bash
curl -s http://localhost:9090/api/v1/targets | jq '.data.activeTargets[] | .labels'
```

---

### **46. `grep`**

* **What**: Text search.
* **Example**:

```bash
grep -i error /var/log/syslog
```

---

### **47. `awk`**

* **What**: Field-based text processing.
* **Example**:

```bash
awk '{print $1, $2}' file.txt
```

---

### **48. `sed`**

* **What**: Stream editor.
* **Example**:

```bash
sed 's/error/ALERT/g' file.txt
```

---

## **A.7 – Security & Access**

---

### **49. `sudo -l`**

* **What**: Lists allowed sudo commands for current user.
* **Example**:

```bash
sudo -l
```

---

### **50. `last`**

* **What**: Shows login history.
* **Example**:

```bash
last -n 5
```

---

### **51. `sha256sum`**

* **What**: Hash a file to verify integrity.
* **Example**:

```bash
sha256sum /path/to/file
```

---

# **SECTION B – 30 Technical Q\&A**

---

## **B.1 – Linux / Systems**

---

### **Q1: Explain how to troubleshoot high CPU usage on a Linux trading server.**

**Concept**
High CPU usage in Tech Ops can indicate a runaway process, poor CPU affinity, or abnormal network packet handling. In a low-latency trading context, even a **10% increase in CPU usage** on the wrong core can cause missed market data.

**Step-by-Step**

1. **Check top offenders**

```bash
ps aux --sort=-%cpu | head
```

Look for trading processes (`feedhandler`, `order_gateway`) spiking CPU.

2. **Live monitor**

```bash
top  # or htop
```

See if the load is concentrated on one core.

3. **Per-core usage**

```bash
mpstat -P ALL 1
```

Identify imbalances.

4. **Affinity check**

```bash
taskset -cp <PID>
```

Ensure critical processes are pinned to isolated cores.

5. **Kernel/system CPU usage**

```bash
pidstat -u 1
```

If `%system` is high, suspect interrupt handling.

6. **Interrupt sources**

```bash
cat /proc/interrupts
```

High counts for NIC IRQs may mean packet storms.

**Example**:
In a Jump-like feedhandler, high `%system` due to excessive NIC interrupts → solved by IRQ affinity tuning to dedicate 2 CPU cores to the NIC.

**CoderPad Python Snippet – CPU Spike Monitor**

```python
import psutil, time

while True:
    for p in psutil.process_iter(['pid', 'name', 'cpu_percent']):
        if p.info['cpu_percent'] > 50:
            print(f"High CPU: {p.info}")
    time.sleep(1)
```

---

### **Q2: How do you investigate high memory usage and potential leaks?**

**Concept**
Memory leaks in feedhandlers or order gateways can slowly increase latency and cause crashes.

**Step-by-Step**

1. **Current usage**

```bash
free -h
```

Check `available` memory.

2. **Per-process**

```bash
ps aux --sort=-%mem | head
```

3. **Detailed per-PID**

```bash
pmap -x <PID>
```

Shows mapped memory regions.

4. **Track growth**

```bash
watch -n 1 "ps -p <PID> -o pid,cmd,%mem,rss"
```

5. **Page faults**

```bash
pidstat -r 1
```

High major faults → swap/disk delays.

**Example**
Found a memory leak in a Python market feed parser due to unbounded message queue.

**CoderPad Snippet – Memory Usage Alert**

```python
import psutil, time

pid = 2435
p = psutil.Process(pid)
while True:
    mem = p.memory_info().rss / (1024**2)
    if mem > 500:
        print(f"ALERT: {pid} using {mem:.1f} MB")
    time.sleep(5)
```

---

### **Q3: What is the role of `/proc` in troubleshooting?**

**Concept**
`/proc` is a virtual filesystem exposing kernel and process internals — critical for fast incident debugging.

**Step-by-Step**

* `/proc/cpuinfo` – CPU details (model, cores, MHz)
* `/proc/meminfo` – RAM usage breakdown
* `/proc/net/dev` – per-interface RX/TX stats
* `/proc/<PID>/fd` – open file descriptors for a process
* `/proc/<PID>/status` – thread counts, memory usage

**Example Command**

```bash
cat /proc/net/dev
```

Output:

```
eth0:  100000 0 0 0 0 0 0 0  95000 0 0 0 0 0 0 0
```

* First column after interface = bytes received, second set = bytes sent.

---

### **Q4: How do you check if a process is bound to the right CPU cores?**

**Step-by-Step**

```bash
taskset -cp <PID>
```

Output:

```
pid 2435's current affinity list: 2,3
```

If not aligned, set it:

```bash
taskset -cp 2,3 <PID>
```

Also check if IRQs for the NIC are on same cores:

```bash
grep eth0 /proc/interrupts
```

---

## **B.2 – Networking**

---

### **Q5: How would you troubleshoot packet loss on a multicast market data feed?**

**Concept**
Packet loss means incomplete market data — dangerous in trading.

**Step-by-Step**

1. **Check NIC drop counters**

```bash
ethtool -S eth0 | grep drop
```

2. **Capture packets**

```bash
tcpdump -ni eth0 'udp and multicast and port 5000'
```

3. **Verify group membership**

```bash
ip maddr show dev eth0
```

4. **Switch/IGMP**

* Ensure IGMP snooping enabled
* Verify PIM config on routers

**CoderPad Snippet – Gap Detector**

```python
import socket, struct

G, PORT = "239.192.0.1", 5000
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind(('', PORT))
mreq = struct.pack("4sl", socket.inet_aton(G), socket.INADDR_ANY)
s.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)
last = None
while True:
    data, _ = s.recvfrom(1500)
    seq = int.from_bytes(data[:4], 'big')
    if last and seq != last + 1:
        print(f"GAP: expected {last+1}, got {seq}")
    last = seq
```

---

### **Q6: How do you measure jitter on a network link?**

**Concept**
Jitter = variability in packet latency.

**Step-by-Step**

1. Use `ping` with timestamps.
2. For one-way measurement: enable NIC hardware timestamping + PTP sync.
3. Capture packet timestamps with `tcpdump -tt` and compute deltas.

**Example**:

```bash
tcpdump -i eth0 -tt udp port 5000 > capture.txt
```

Parse in Python to get time differences.

---

### **Q7: How do you check if jumbo frames are enabled end-to-end?**

**Step-by-Step**

```bash
ip link show eth0
```

Look for `mtu 9000`.
Test:

```bash
ping -M do -s 8972 <peer>
```

If it fails, some hop doesn’t support it.

---

### **Q8: How do you inspect live socket connections for a trading process?**

```bash
ss -p -n state established '( sport = :5000 )'
```

* Shows process, protocol, and endpoints.

---

### **Q9: How do you verify if a multicast feed is actually being received by the application?**

**Concept**
Joining a multicast group doesn’t mean your app is consuming the data — the feed might be dropped before it reaches user space.

**Step-by-Step**

1. **Check kernel-level group membership**:

```bash
ip maddr show dev eth0
```

Output:

```
inet 239.192.0.1
```

2. **Capture packets at NIC**:

```bash
tcpdump -ni eth0 'udp and host 239.192.0.1 and port 5000'
```

3. **Trace packets to process**:

```bash
lsof -nPiUDP | grep 5000
```

4. **Confirm app processing**:

* Enable debug counters in the app (messages/sec, sequence gaps).

**Why important for Jump Tech Ops**
Market data gaps may appear due to kernel buffer overflow — not just network loss.

---

### **Q10: How do you find the source of high network latency on a path to an exchange?**

**Concept**
In low-latency trading, latency increases must be explained in seconds.

**Step-by-Step**

1. **Run MTR** to see per-hop latency:

```bash
mtr -rw <exchange-ip>
```

2. **Check link speed**:

```bash
ethtool eth0 | grep Speed
```

3. **Look for queueing**:

```bash
tc -s qdisc show dev eth0
```

4. **Compare historical metrics** in Grafana.

---

### **Q11: How do you determine which process is sending traffic on a specific port?**

**Step-by-Step**

```bash
ss -u -p 'sport = :5000'
```

Output:

```
UNCONN 0 0 239.192.0.1:5000 *:* users:(("feedhandler",pid=2435,fd=12))
```

---

### **Q12: How do you capture only dropped TCP packets for analysis?**

**Concept**
Dropped packets may indicate congestion or bad NIC tuning.

**Example**:

```bash
tcpdump 'tcp[tcpflags] & (tcp-rst) != 0'
```

Or with `ethtool`:

```bash
ethtool -S eth0 | grep drop
```

---

## **B.3 – Monitoring & Incident Response**

---

### **Q13: How do you check if a trading host is time-synced within microseconds?**

**Concept**
Time sync is critical for order timestamp accuracy.

**Step-by-Step**

1. Check PTP status:

```bash
pmc -u -b 0 "GET TIME_STATUS_NP"
```

2. See `master offset` — should be < 1000ns for trading.

3. Verify system clock matches hardware clock:

```bash
phc2sys -s eth0 -c CLOCK_REALTIME -w
```

---

### **Q14: What metrics would you alert on for market data health?**

**Critical metrics**

* Feed gap count per second
* Message processing latency (p99)
* NIC RX drops
* CPU steal time on feed cores
* PTP offset

**PromQL example**:

```promql
rate(feed_gap_total[1m]) > 0
```

---

### **Q15: How do you investigate a sudden spike in p99 latency for order acknowledgements?**

**Step-by-Step**

1. Verify **time sync** is healthy.
2. Check **gateway CPU/IRQ saturation**:

```bash
mpstat -P ALL 1
cat /proc/interrupts
```

3. Check network **queue depth** on switch.
4. Roll traffic to standby gateway if needed.

---

### **Q16: How do you create a runbook for packet loss incidents?**

**Outline**

1. Symptom detection (alerts triggered)
2. Immediate commands to run (tcpdump, ethtool, ip maddr)
3. Escalation matrix
4. Recovery steps (reroute, buffer tuning)

---

## **B.4 – Automation & Scripting**

---

### **Q17: How do you write a Python script to tail a log file and alert on errors?**

**Concept**
Log tailing is often the fastest detection method during an incident.

**CoderPad Snippet**

```python
import time, re

pattern = re.compile(r'(error|fail)', re.I)
with open('/var/log/syslog') as f:
    f.seek(0, 2)
    while True:
        line = f.readline()
        if not line:
            time.sleep(0.1)
            continue
        if pattern.search(line):
            print(f"ALERT: {line.strip()}")
```

---

### **Q18: How do you automate disabling GRO/LRO on a NIC for low latency?**

**Example Bash**

```bash
#!/bin/bash
IFACE=$1
ethtool -K $IFACE gro off lro off
```

---

### **Q19: How do you schedule a blue/green deployment in Ansible?**

**Example YAML**

```yaml
- hosts: trading_servers
  tasks:
    - name: Deploy to blue
      command: deploy_script --env blue
    - name: Switch to blue
      command: switch_traffic blue
```

---

### **Q20: How do you rollback automatically on deployment failure?**

**Python Snippet**

```python
import subprocess

try:
    subprocess.run(["deploy"], check=True)
except subprocess.CalledProcessError:
    subprocess.run(["rollback"])
```

---

## **B.5 – Domain-Specific Low-Latency Ops**

---

### **Q21: Why disable GRO/LRO for market data NICs?**

**Concept**
GRO/LRO batch packets to reduce CPU usage, but increase latency and jitter. In HFT, microseconds matter, so disable them.

**Command**

```bash
ethtool -K eth0 gro off lro off
```

---

### **Q22: How do you pin IRQs for a NIC to specific CPU cores?**

**Step-by-Step**

```bash
grep eth0 /proc/interrupts
echo 4 > /proc/irq/<IRQ>/smp_affinity
```

(4 = bitmask for CPU2)

---

### **Q23: How do you measure one-way latency between two hosts?**

**Requirements**

* PTP-synced clocks
* Capture send/receive timestamps
* Compute delta

---

### **Q24: How do you detect kernel buffer overruns on a UDP socket?**

**Command**

```bash
netstat -su | grep "packet receive errors"
```

If increasing, enlarge buffers:

```bash
sysctl -w net.core.rmem_max=26214400
```

---

### **Q25: How do you test failover between two market data links?**

**Step-by-Step**

1. Start feed on primary, monitor sequence.
2. Cut primary link.
3. Verify secondary sequence resumes without gap.

---

### **Q26: How do you detect if a process is being throttled by cgroups?**

**Command**

```bash
cat /sys/fs/cgroup/cpu/trading_app/cpu.stat
```

---

### **Q27: How do you monitor NIC queue utilization?**

**Command**

```bash
ethtool -S eth0 | grep queue
```

---

### **Q28: How do you debug a trading process crash?**

**Step-by-Step**

* Check `dmesg` for OOM kills or segfaults
* If core dump available: `gdb ./binary core`

---

### **Q29: How do you check for asymmetric routing issues?**

**Command**

```bash
tracepath <exchange-ip>
```

Compare forward/reverse paths.

---

### **Q30: How do you verify MTU alignment across network path?**

**Command**

```bash
ping -M do -s 8972 <peer>
```

---

# **SECTION C – Mock Interviews (Simulated Live)**

---

## **Mock Interview 1 – Systems & Networking Focus**

**Time:** 60 min | **Mix:** 20% behavioral, 50% Linux/network troubleshooting, 30% Python coding

---

**\[0:00 – 0:03] – Small Talk / Warm-Up**
**Interviewer:** Hey, thanks for joining. Have you had a chance to test the CoderPad link?
**You:** Yes, I opened it and confirmed it’s set to Python 3.

---

**\[0:03 – 0:10] – Behavioral**
**Q:** Tell me about a time you had to debug a system issue under pressure.
**Ideal STAR Answer (tailored to your CV)**

* **S**: In my cloud deployment project, a production VM became unresponsive right before a demo.
* **T**: I had to restore services within minutes.
* **A**: Checked system load with `top`, saw I/O wait high, ran `iotop` and found a log rotation job blocking writes. Killed process, freed disk space, restarted service.
* **R**: Restored availability in under 3 minutes, documented incident for future prevention.

---

**\[0:10 – 0:30] – Linux / Networking Troubleshooting**
**Scenario:** Market data feed shows gaps in sequence numbers every few minutes.

**Interviewer:** You’re on-call. Traders call saying the feed has gaps. What’s your step-by-step?

**You (Step Plan):**

1. Check NIC stats for drops:

```bash
ethtool -S eth0 | grep drop
```

2. Verify multicast group membership:

```bash
ip maddr show dev eth0
```

3. Capture packets to confirm arrival:

```bash
tcpdump -ni eth0 'udp and host 239.192.0.1 and port 5000'
```

4. Check `netstat -su` for UDP receive errors.
5. If kernel buffer full → tune:

```bash
sysctl -w net.core.rmem_max=26214400
```

**Interviewer Follow-Up:** What if the problem is intermittent?
**You:** Run a gap detection script (Python) over time to correlate with NIC drop counters.

---

**\[0:30 – 0:50] – CoderPad Python Task**
**Prompt:** Write a Python script that listens to a multicast feed and detects missing packets given that each packet begins with a 4-byte big-endian sequence number.

**Ideal Solution**

```python
import socket, struct

GROUP, PORT = "239.192.0.1", 5000
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', PORT))
mreq = struct.pack("4sl", socket.inet_aton(GROUP), socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

last_seq = None
while True:
    data, _ = sock.recvfrom(1500)
    seq = int.from_bytes(data[:4], 'big')
    if last_seq is not None and seq != last_seq + 1:
        print(f"Gap detected: expected {last_seq + 1}, got {seq}")
    last_seq = seq
```

**Key Talking Points:**

* Use `SO_REUSEADDR` for multiple listeners.
* `struct` for network byte order parsing.
* Gap detection logic.

---

**\[0:50 – 1:00] – Wrap-Up**
**Interviewer:** Any questions for me?
**You:** Ask about Jump’s approach to real-time monitoring and automated failover (we’ll list good questions in Section D).

---

**Rubric**

* Troubleshooting logic: **5/5**
* Linux commands knowledge: **4/5**
* Python implementation correctness: **5/5**
* Communication under pressure: **4/5**

**Improvement Notes**

* In answers, always mention **metrics correlation** (Grafana, Prometheus).
* Be quicker in recalling key sysctl parameters.

---

## **Mock Interview 2 – Incident Response & Automation Focus**

**Mix:** 20% behavioral, 30% incident triage, 50% Python scripting

---

**\[0:00 – 0:05] – Behavioral**
**Q:** Give me an example of improving a process via automation.

* **S:** Manual log checks for errors were slow.
* **T:** Automate real-time error alerts.
* **A:** Wrote a Python log tailer with regex match and Slack webhook.
* **R:** Reduced detection time from minutes to seconds.

---

**\[0:05 – 0:20] – Incident Scenario**
**Scenario:** CPU usage spikes to 90% on core 2 during market open, causing order latency.
**You:**

1. Verify with `mpstat -P 2 1`
2. Check process with `taskset -cp <PID>`
3. Inspect `/proc/interrupts` for NIC IRQ on same core → move IRQ to different core.
4. Re-pin process to isolated CPU core.

---

**\[0:20 – 0:55] – CoderPad Task**
**Prompt:** Write a Python script to monitor CPU and memory usage of a given process, alert if CPU > 70% or memory > 500MB.

**Ideal Solution**

```python
import psutil, sys, time

pid = int(sys.argv[1])
p = psutil.Process(pid)

while True:
    cpu = p.cpu_percent(interval=1)
    mem = p.memory_info().rss / (1024**2)  # MB
    if cpu > 70 or mem > 500:
        print(f"ALERT: CPU {cpu:.1f}%, Mem {mem:.1f}MB")
```

---

**Rubric**

* Python process monitoring: **5/5**
* Correct threshold handling: **5/5**
* Linux-ops integration in reasoning: **4/5**

---

## **Mock Interview 3 – End-to-End Problem Solving**

**Mix:** Full pipeline from detection → debugging → fix → prevention.

---

**Scenario:** Traders report orders not acknowledged by exchange for 2 seconds.

**You (Plan):**

1. Verify connectivity with `mtr -rw`
2. Check NIC queue drops with `ethtool -S`
3. Review order gateway logs for send/recv timestamps.
4. Cross-check with PTP offset — maybe time drift caused order timestamp rejection.
5. Roll traffic to standby gateway.

---

**CoderPad Task** – Parse a log file and print average latency per order ID.

**Example Log**:

```
order123 sent 1699999999.123
order123 ack  1699999999.223
```

**Solution**

```python
latencies = {}
with open('orders.log') as f:
    for line in f:
        parts = line.split()
        oid, ts = parts[0], float(parts[2])
        if oid not in latencies:
            latencies[oid] = [ts, None]
        elif 'ack' in line:
            latencies[oid][1] = ts

for oid, (start, end) in latencies.items():
    if end:
        print(oid, (end - start) * 1000, "ms")
```

---

# **SECTION D – Python 3 CoderPad Cheatsheet (Tech Ops Focus – Expanded)**

---

## **D.1 – Networking in Python**

---

**UDP Send & Receive**

```python
import socket

# Create UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Send
sock.sendto(b"Hello", ("10.1.1.2", 5000))

# Receive
data, addr = sock.recvfrom(1024)
print(f"Received {data} from {addr}")
```

* **When to use**: Sending heartbeat messages, quick diagnostics.
* **Gotcha**: UDP is connectionless — no delivery guarantees.

---

**Joining a Multicast Group**

```python
import struct, socket

GROUP, PORT = "239.0.0.1", 5000
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', PORT))

# Add to multicast group
mreq = struct.pack("4sl", socket.inet_aton(GROUP), socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)
```

* **Why important in Tech Ops**: Market data feeds are multicast — you must bind & join correctly.
* **Common pitfall**: Forgetting `SO_REUSEADDR` will block if another listener exists.

---

**Parsing Binary Packets**

```python
import struct

# Suppose packet starts with seq (uint32), price (float), volume (uint16)
data = b'\x00\x00\x04\xd2\x41\x20\x00\x00\x03\xe8'  # Example binary
seq, price, vol = struct.unpack(">IfH", data[:10])
print(seq, price, vol)
```

* **>** = big-endian, **I** = unsigned int, **f** = float, **H** = unsigned short.
* **Use case**: Reading wire protocol formats for market data.

---

## **D.2 – File & Log Handling**

---

**Tail a File**

```python
import time

with open('/var/log/syslog') as f:
    f.seek(0, 2)  # Go to end of file
    while True:
        line = f.readline()
        if not line:
            time.sleep(0.1)
            continue
        print(line.strip())
```

* **Use case**: Real-time incident debugging.
* **Tip**: Combine with regex to only match certain patterns.

---

**Regex Matching**

```python
import re
pattern = re.compile(r'error|fail', re.I)
if pattern.search("ERROR: connection lost"):
    print("Match found")
```

* **`re.I`** = case-insensitive search.

---

## **D.3 – Process Monitoring with psutil (Complete Guide)**

---

`psutil` = **Process and System Utilities** — allows you to monitor CPU, memory, disk, and network usage programmatically.

**Installation in CoderPad**:

```bash
pip install psutil
```

---

**CPU Usage**

```python
import psutil
print(psutil.cpu_percent(interval=1))   # % of CPU used
print(psutil.cpu_count(logical=False))  # Physical cores
print(psutil.cpu_times_percent())
```

* **interval=1** → calculates usage over 1 second.
* `cpu_times_percent` shows user/sys/iowait breakdown.

---

**Memory Usage**

```python
mem = psutil.virtual_memory()
print(mem.total, mem.used, mem.percent)
```

* Shows **total**, **used**, and **percent** usage.
* Combine with alerts if usage > 90%.

---

**Disk Usage & I/O**

```python
print(psutil.disk_usage('/'))
print(psutil.disk_io_counters())
```

* **disk\_usage**: free space monitoring.
* **disk\_io\_counters**: read/write counts & bytes.

---

**Network I/O**

```python
print(psutil.net_io_counters())
print(psutil.net_connections(kind='inet'))
```

* **net\_connections**: show open TCP/UDP sockets.

---

**Per-Process Info**

```python
p = psutil.Process(2435)
print(p.name(), p.status())
print(p.cpu_percent(interval=1))
print(p.memory_info().rss / (1024**2))  # in MB
print(p.open_files())
print(p.connections())
```

* **rss** = resident memory size (RAM).
* **connections()**: per-process network endpoints.

---

**Process Tree**

```python
for child in p.children(recursive=True):
    print(child.pid, child.name())
```

* Useful for finding orphaned subprocesses.

---

**Kill a Process**

```python
p.terminate()
```

---

## **D.4 – Parsing & Data Processing**

---

**CSV**

```python
import csv
with open('data.csv') as f:
    for row in csv.reader(f):
        print(row)
```

**JSON**

```python
import json
data = json.loads('{"key": 1}')
print(data['key'])
```

---

**Datetime & Timezones**

```python
from datetime import datetime
print(datetime.utcnow().isoformat())
```

* Important for timestamping logs in trading.

---

## **D.5 – Performance Tips for Python in Ops**

* Use **`time.perf_counter()`** for high-precision timing.
* Avoid unnecessary allocations in loops (use `bytearray` for buffers).
* For log parsing, pre-compile regex.
* Use `deque` for rolling window operations in monitoring scripts.
* For heavy computations, consider Cython or NumPy vectorization.

---

# **SECTION E – CV Deep Dive & Speaking Guide**

---

## **E.1 – STAR Stories from Your CV**

---

**Example 1 – Linux Performance Debugging**

* **S**: During my XYZ project, we saw request latency spikes.
* **T**: Identify and fix the root cause before deployment deadline.
* **A**: Used `htop`, `mpstat`, `iostat` to pinpoint CPU saturation due to logging process. Tuned logging frequency, adjusted core affinity.
* **R**: Reduced latency by 35%, avoided downtime.

---

**Example 2 – Automation via Ansible**

* **S**: Manual deployment steps caused delays in new feature rollout.
* **T**: Reduce deployment time from 30 mins to <5 mins.
* **A**: Created Ansible playbooks for config and service restarts.
* **R**: Achieved <3 min deployment time, reduced human errors.

---

**Example 3 – Cloud Ops Monitoring**

* **S**: No visibility into cloud resource usage for dev environment.
* **T**: Build lightweight monitoring stack.
* **A**: Integrated Prometheus & Grafana, wrote Python exporters.
* **R**: Improved debugging speed by 50%.

---

## **E.2 – Speaking Style in Tech Ops Interviews**

---

* Always **go from detection → isolation → fix → prevention** in answers.
* Speak in **numbers** (latency in microseconds, memory in MB, packets/sec).
* When coding, **narrate your thought process**: “I’ll open a socket, bind, then parse the header…”
* Show **ops awareness**: mention monitoring tools, rollback plans, failure modes.
* End every solution with **preventative measure** to avoid recurrence.

---

# **SECTION F – Python + Ops CoderPad Drills**

---

## **F.1 – Easy: CPU Usage Alert**

**Problem:** Write a Python script to print `ALERT` if system-wide CPU usage exceeds 80% for more than 5 seconds.

**Reasoning:**

* Use `psutil.cpu_percent()` with `interval`
* Keep a rolling counter for consecutive high values
* Alert only if sustained

**Solution:**

```python
import psutil, time

count = 0
while True:
    usage = psutil.cpu_percent(interval=1)
    print(f"CPU: {usage}%")
    if usage > 80:
        count += 1
    else:
        count = 0
    if count >= 5:
        print("ALERT: High CPU usage sustained for 5 seconds!")
        count = 0
```

**What this tests:** Basic `psutil` usage, control flow.

---

## **F.2 – Easy/Moderate: Disk Space Monitor**

**Problem:** Monitor `/var/log` disk usage; alert if >90% full.

**Solution:**

```python
import psutil

usage = psutil.disk_usage('/var/log')
if usage.percent > 90:
    print(f"ALERT: /var/log {usage.percent}% full")
```

**Command analogy:**
`df -h /var/log` → checks disk space usage.

* **df**: disk free
* **-h**: human-readable
* `/var/log`: target mount point

**What this tests:** Mapping Linux commands to Python equivalents.

---

## **F.3 – Moderate: Tail a Log and Alert on “error”**

**Problem:** Mimic `tail -f /var/log/syslog | grep error`.

**Solution:**

```python
import time, re

pattern = re.compile(r'error', re.I)
with open('/var/log/syslog') as f:
    f.seek(0, 2)
    while True:
        line = f.readline()
        if not line:
            time.sleep(0.1)
            continue
        if pattern.search(line):
            print(f"ALERT: {line.strip()}")
```

**Command analogy:**

* `tail -f`: follow file as it grows
* `grep`: search for pattern
* `re.compile(r'error', re.I)`: regex search ignoring case

---

## **F.4 – Moderate: Network Connections by Process**

**Problem:** Show all TCP connections for processes containing `feed` in their name.

**Solution:**

```python
import psutil

for p in psutil.process_iter(['pid', 'name']):
    if 'feed' in p.info['name']:
        conns = p.connections(kind='tcp')
        for c in conns:
            print(p.info['pid'], p.info['name'], c.laddr, c.raddr, c.status)
```

**Command analogy:**
`ss -p -n state established`

* **ss**: socket statistics
* **-p**: process info
* **-n**: no DNS resolution

---

## **F.5 – Moderate/Hard: Multicast Packet Counter**

**Problem:** Join multicast group `239.1.1.1:5000`, count packets, print rate per second.

**Solution:**

```python
import socket, struct, time

GROUP, PORT = "239.1.1.1", 5000
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', PORT))
mreq = struct.pack("4sl", socket.inet_aton(GROUP), socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

count, start = 0, time.time()
while True:
    sock.recv(1500)
    count += 1
    now = time.time()
    if now - start >= 1:
        print(f"{count} packets/sec")
        count, start = 0, now
```

**Command analogy:**
`tcpdump -ni eth0 'udp and host 239.1.1.1 and port 5000' | wc -l`

* **-n**: numeric output
* **-i eth0**: interface
* `wc -l`: count lines

---

## **F.6 – Hard: Detect Missing Packets in Binary Feed**

**Problem:** First 4 bytes = sequence number (big-endian). Print gaps.

**Solution:**

```python
import socket, struct

GROUP, PORT = "239.1.1.1", 5000
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', PORT))
mreq = struct.pack("4sl", socket.inet_aton(GROUP), socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

last = None
while True:
    data, _ = sock.recvfrom(1500)
    seq = struct.unpack(">I", data[:4])[0]
    if last and seq != last + 1:
        print(f"Gap detected: expected {last+1}, got {seq}")
    last = seq
```

---

## **F.7 – Hard: Rolling 1-Minute Average Latency**

**Problem:** Parse log lines with `orderID timestamp_sent timestamp_ack` and compute rolling 1-minute average latency.

**Solution:**

```python
from collections import deque
import time

window = deque()
with open('orders.log') as f:
    for line in f:
        oid, ts_s, ts_a = line.split()
        latency = float(ts_a) - float(ts_s)
        now = time.time()
        window.append((now, latency))
        while window and now - window[0][0] > 60:
            window.popleft()
        avg = sum(l for _, l in window) / len(window)
        print(f"Rolling avg latency: {avg*1000:.3f} ms")
```

---

## **F.8 – Hard: Python Version of `iftop` for One Interface**

**Problem:** Show total bytes/sec sent and received on `eth0` every second.

**Solution:**

```python
import psutil, time

old = psutil.net_io_counters(pernic=True)['eth0']
while True:
    time.sleep(1)
    new = psutil.net_io_counters(pernic=True)['eth0']
    sent_rate = (new.bytes_sent - old.bytes_sent)
    recv_rate = (new.bytes_recv - old.bytes_recv)
    print(f"TX: {sent_rate} B/s, RX: {recv_rate} B/s")
    old = new
```

**Command analogy:**
`iftop -i eth0`

* **-i**: interface to monitor

---

## **F.9 – Brutal: Recreate `ping` in Python (ICMP)**

**Problem:** Send ICMP echo requests and measure RTT. (Requires root privileges in real use.)

**Solution:**

```python
import socket, struct, time, os

def checksum(data):
    s = sum(data[i] + (data[i+1] << 8) for i in range(0, len(data), 2))
    s = (s >> 16) + (s & 0xffff)
    s += s >> 16
    return ~s & 0xffff

host = "8.8.8.8"
icmp = socket.getprotobyname("icmp")
sock = socket.socket(socket.AF_INET, socket.SOCK_RAW, icmp)
pid = os.getpid() & 0xffff

for seq in range(4):
    header = struct.pack("bbHHh", 8, 0, 0, pid, seq)
    data = b"X" * 48
    chksum = checksum(header + data)
    header = struct.pack("bbHHh", 8, 0, chksum, pid, seq)
    sock.sendto(header + data, (host, 1))
    start = time.time()
    sock.recv(1024)
    print(f"RTT: {(time.time() - start)*1000:.2f} ms")
```

---

## **F.10 – Brutal: Real-Time Latency Histogram**

**Problem:** From incoming UDP packets, measure latency in μs (timestamp in first 8 bytes, double format), print histogram every 5 seconds.

**Solution:**

```python
import socket, struct, time, numpy as np

GROUP, PORT = "239.1.1.1", 5000
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', PORT))
mreq = struct.pack("4sl", socket.inet_aton(GROUP), socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

latencies = []
last_print = time.time()

while True:
    data, _ = sock.recvfrom(1500)
    send_ts = struct.unpack(">d", data[:8])[0]
    latency_us = (time.time() - send_ts) * 1e6
    latencies.append(latency_us)
    if time.time() - last_print >= 5:
        hist, bins = np.histogram(latencies, bins=10)
        for b, h in zip(bins, hist):
            print(f"{b:.0f}us: {h}")
        latencies.clear()
        last_print = time.time()
```

---

# **SECTION G – Networking Basics to Advanced (Tech Ops Edition)**

---

## **G.1 – OSI Model and Why It Matters**

---

**The OSI 7-Layer Model:**

| Layer | Name         | Purpose in Trading/Tech Ops         | Key Examples / Tools     |
| ----- | ------------ | ----------------------------------- | ------------------------ |
| 7     | Application  | End-user protocols, feed formats    | FIX, FAST, ITCH, HTTP    |
| 6     | Presentation | Data format conversion, encryption  | TLS, SSL, JSON, XML      |
| 5     | Session      | Managing connections                | TCP session, QUIC        |
| 4     | Transport    | Data delivery (reliable/unreliable) | TCP, UDP                 |
| 3     | Network      | Routing packets                     | IP, ICMP                 |
| 2     | Data Link    | Local link addressing               | Ethernet, VLAN           |
| 1     | Physical     | Electrical/optical transport        | Fiber, Cat6, SFP modules |

**Why it matters for Tech Ops:**

* A low-latency trading issue could be at *any* layer (e.g., dropped packets at L2 vs. BGP route change at L3).
* Diagnosing requires knowing **where** to look first.

---

## **G.2 – Key Networking Terms and Metrics**

---

* **Latency:** Time taken for data to travel from source to destination (μs, ms).

  * *Round-trip time* (RTT) via `ping` or `mtr`.
* **Jitter:** Variation in packet arrival times (problematic for real-time feeds).
* **Packet loss:** Dropped packets → missing market data.
* **Throughput:** Data transfer rate (Mbps, Gbps).
* **Bandwidth:** Max possible throughput (physical link capacity).
* **MTU (Maximum Transmission Unit):** Max payload size for a packet (1500 bytes for standard Ethernet, 9000 for jumbo frames).
* **MSS (Maximum Segment Size):** TCP-specific payload size.
* **Flow control:** Mechanisms like TCP window scaling, pause frames (Ethernet).
* **Multicast:** One-to-many transmission, crucial for market data.
* **Unicast:** One-to-one transmission.
* **Broadcast:** One-to-all (rare in trading networks).

---

## **G.3 – Networking Protocols You Must Know for Tech Ops**

---

### **L1/L2 – Physical/Data Link**

* **Ethernet:** Base frame format, MAC addressing.
* **VLANs (802.1Q):** Logical network segmentation.
* **LACP:** Link Aggregation for bandwidth + redundancy.

---

### **L3 – Network**

* **IPv4 basics:** Addresses, subnets, CIDR notation.
* **Subnet mask:** Defines network vs host bits.
* **Routing:** Static vs dynamic (OSPF, BGP).
* **ICMP:** Ping, traceroute, network reachability.

---

### **L4 – Transport**

* **TCP:** Reliable, ordered, connection-oriented. Used for FIX order entry.
* **UDP:** Faster, no reliability overhead. Used for market data feeds.
* **Ports:** Identify specific processes (e.g., UDP 5000 for feed).
* **Ephemeral ports:** Temporary client-side ports.

---

### **L5-L7 – Application Layer**

* **FIX Protocol:** Order entry.
* **ITCH/FAST:** Market data feed formats.
* **HTTP/WebSocket:** API calls, dashboards.

---

## **G.4 – Critical Networking Tools (Linux)**

---

| Tool                 | Purpose          | Example Command                  | Output / Usage       |
| -------------------- | ---------------- | -------------------------------- | -------------------- |
| `ping`               | RTT measurement  | `ping 10.1.1.2`                  | Average latency      |
| `traceroute` / `mtr` | Path tracing     | `mtr -rw 8.8.8.8`                | Per-hop latency/loss |
| `netstat` / `ss`     | Show connections | `ss -tuna`                       | TCP/UDP sockets      |
| `tcpdump`            | Packet capture   | `tcpdump -ni eth0 udp port 5000` | Live feed inspection |
| `ethtool`            | NIC stats        | `ethtool -S eth0`                | Drops, errors, speed |
| `ip`                 | Interface config | `ip addr show`                   | IP, MAC, link state  |

---

**Command breakdown example – tcpdump**:

```bash
tcpdump -ni eth0 udp port 5000
```

* `-n` → no DNS resolution (faster)
* `-i eth0` → listen on interface eth0
* `udp port 5000` → capture only UDP packets to/from port 5000

---

## **G.5 – Advanced Networking Concepts for Low Latency Trading**

---

### **1. Multicast Operations**

* Market data often sent over UDP multicast to all subscribers.
* **IGMP (Internet Group Management Protocol)** joins a host to multicast group.
* Linux command to check membership:

```bash
ip maddr show dev eth0
```

* Caveat: Missed IGMP joins can silently drop data.

---

### **2. NIC Offloads and Kernel Bypass**

* **TSO/LRO:** TCP segmentation/large receive offload — can cause unwanted batching.
* Disable with:

```bash
ethtool -K eth0 gro off lro off
```

* **Kernel bypass**: Using DPDK/Solarflare Onload to skip kernel overhead.

---

### **3. Clock Synchronization (PTP/NTP)**

* **PTP (Precision Time Protocol):** Sub-microsecond sync for timestamping trades.
* **NTP:** Millisecond-level accuracy (not enough for HFT).
* Check offset:

```bash
pmc -u -b 0 "GET TIME_STATUS_NP"
```

---

### **4. Network Performance Tuning**

* Increase receive buffer for market data:

```bash
sysctl -w net.core.rmem_max=26214400
```

* Pin IRQs to specific CPU cores:

```bash
cat /proc/interrupts
echo 2 > /proc/irq/45/smp_affinity
```

---

### **5. Troubleshooting Path**

**Symptom:** Packet drops.

1. Check NIC stats (`ethtool -S`).
2. Check CPU softirq saturation (`/proc/softirqs`).
3. Capture packets (`tcpdump`).
4. Verify IGMP membership.
5. Tune kernel buffers.

---

### **6. Security in Low-Latency Environments**

* Use ACLs at switches to filter only relevant market data.
* Avoid CPU-consuming firewalls inline; offload filtering to NIC.

---

## **G.6 – Networking Caveats in Finance**

---

* **Jumbo Frames:** Beneficial for throughput, but mismatched MTU between hops = packet loss.
* **Microbursts:** Short spikes of traffic → cause buffer overruns on switches/NICs.
* **Multicast Gap Handling:** Apps must gracefully detect & request retransmissions.
* **Congestion Avoidance:** TCP in high-throughput low-latency links requires careful window tuning.
* **Redundancy:** Dual links to exchanges; instant failover without packet reordering.

---

## **G.7 – Interview Tips for Networking Questions**

---

* When asked “How would you debug latency?”, always go **Layer 1 → Layer 7** in sequence.
* Mention **specific tools** and **metrics** for each layer.
* Use **cause + effect** language: “If NIC drops increase, application gaps likely follow.”
* If they ask about **multicast**: explain IGMP joins, snooping, packet ordering.

---

# **SECTION H – 10 Elite Questions to Ask Interviewers**

---

1. How does Jump’s Tech Ops team measure and enforce microsecond-level performance goals?
2. What’s the process for post-incident analysis, and how is it integrated into automation improvements?
3. How do you handle testing changes in a low-latency environment without impacting live trading?
4. What’s the balance between custom-built tooling vs open-source in Jump’s infrastructure?
5. How is failover between data centers orchestrated and tested?
6. What’s the typical signal-to-noise ratio in monitoring alerts, and how do you reduce false positives?
7. How do Tech Ops engineers collaborate with traders when diagnosing latency anomalies?
8. How does Jump validate NIC and kernel tuning for specific hardware models?
9. How do you train new hires to think in terms of both systems stability and trading strategy impact?
10. What metrics or achievements define success for an intern in the Tech Ops team?