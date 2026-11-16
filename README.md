# Secure Infrastructures: Practical Assignments

Coursework for **Designing and Building Secure Infrastructures** (Secure Infrastructures), Department of Informatics Engineering, Faculty of Sciences and Technology, University of Coimbra.

Authors: Hélder Ramos and Rodrigo Manão (group 18).

## Projects

| Folder | Assignment | Status |
|--------|------------|--------|
| `TP1/` | Testbed reconnaissance, scouting and vulnerability analysis of an OT/SCADA environment | Done |
| `TP2/` | To be added | In progress |

## TP1: Testbed Reconnaissance, Scouting, and Vulnerability Analysis of an OT/SCADA Environment

IEEE-format report (LaTeX, `IEEEtran`) documenting a security assessment of a university industrial control system (ICS/SCADA) testbed. The assignment imposed two hard rules: **do not exploit any vulnerability and do not change the testbed state**, so every technique used is passive, read-only or non-destructive.

### What was done

- **Discovery:** low-impact ARP host discovery and a throttled Nmap SYN scan, to build an asset inventory without stressing fragile embedded devices.
- **Protocol fingerprinting:** read-only identification of a Schneider Modicon M340 (Modbus/TCP, `modicon-info`) and of a Siemens-branded device (S7comm, `plcscan`), plus read-only Modbus register enumeration.
- **Service verification:** banner observation on FTP, Telnet and HTTP services, without submitting any credentials.
- **Honeypot identification:** one device presenting itself as a Siemens PLC was identified as a Conpot honeypot from several independent indicators (default identification strings, HTTP status page, exposed protocols, FTP session behavior compared with the real PLC).
- **Vulnerability scanning:** OpenVAS scans with a custom configuration that removes disruptive NVT families, with the PLCs and the honeypot excluded from the thorough pass.
- **Passive detection:** Snort with the Quickdraw SCADA rule set, with a real alert confirmed.
- **Man-in-the-Middle analysis:** offline analysis of a provided capture showing bidirectional ARP poisoning between the HMI and the PLC and 116 Modbus/TCP read transactions in transit.

### Main findings

- Unauthenticated disclosure of hardware, firmware and engineering project metadata on the Modicon PLC, and unauthenticated Modbus read access.
- An end-of-life operating system and a critical remote code execution vulnerability (MS15-034) on the HMI host, together with weak encryption on RDP and an unencrypted VNC service.
- A device whose MAC vendor prefix suggested a D-Link switch turned out to be a Raspberry Pi exposing Telnet.
- No authentication or integrity protection in ARP or Modbus/TCP, which makes Man-in-the-Middle eavesdropping trivial on the same broadcast domain.

### Tools

`arp-scan`, `nmap` (and NSE scripts), `modbus` CLI, `plcscan`, `nc`, `telnet`, `curl`, OpenVAS/Greenbone, Snort 2.9.20 with Quickdraw rules, Wireshark and `tshark`.


### Scope and ethics

All activity was carried out on the course testbed, with the address ranges reserved for other student groups (`172.27.224.41-66` and `172.27.224.91-115`) excluded from probing, as required by the assignment. The techniques described are documented for educational purposes and must only be used on systems you are authorized to test.
