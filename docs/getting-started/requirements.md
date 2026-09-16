# EdgeSync System Requirements

**Status:** Draft  
**Version:** 1.0  
**Audience:** Network administrators, network engineers, and developers preparing to use EdgeSync

## 1. Purpose

This document describes the information, access, and environment prerequisites needed to follow the EdgeSync Getting Started guide.

It is intentionally limited to prerequisites supported by the current EdgeSync documentation and product model. It does not define implementation-specific browser versions, operating-system versions, hardware specifications, performance limits, or deployment topology.

> **Important:** EdgeSync is a fictional portfolio project. Where the current product definition does not specify an implementation-level requirement, this document does not invent one.

## 2. Before You Begin

Before starting with EdgeSync, make sure you have:

1. Access to an EdgeSync environment.
2. At least one supported network device that can be monitored or managed by EdgeSync.
3. The management address of the device.
4. Information about the synchronization source used by the device.
5. Access appropriate to your role:
   - Web UI access for network administrators and network engineers.
   - REST API access for developers and external applications.

The exact authentication and access-control mechanism is documented separately and is not defined by this prerequisite document.

## 3. Network Device Requirements

EdgeSync manages and monitors network devices that provide device and synchronization information.

The current architecture identifies examples such as:

- gNB
- Router
- PTP-capable network device
- Timing device

The underlying network device performs the synchronization mechanism. EdgeSync monitors and manages the resulting information rather than replacing the device's synchronization function.

### 3.1 Device Information

For each device you plan to register, have the following information available:

| Information | Purpose |
|---|---|
| Device name | Identifies the device in a human-readable way. |
| Device type | Identifies the category of network device. |
| Management address | Allows EdgeSync to communicate with the device for management. |

The current Device model uses `deviceId` as the stable EdgeSync identifier. When registering a device through the API, the server manages this identifier rather than requiring it in the `DeviceCreate` request.

## 4. Synchronization Source Requirements

The current EdgeSync MVP model supports two network synchronization source types:

- PTP Grandmaster
- NTP Server

A synchronization source is represented separately from the device's synchronization status.

For the source you plan to configure, have the following information available:

| Information | Purpose |
|---|---|
| Source ID | Identifies the synchronization source. |
| Source name | Provides a human-readable source name. |
| Source type | Identifies the synchronization protocol type (`PTP` or `NTP`). |
| Source address | Identifies the network address of the source. |
| Priority | Defines the configured source priority. |

PTP and NTP documentation provides protocol-specific information. The common monitoring model defines how source information relates to synchronization state and metrics.

## 5. Synchronization Monitoring Prerequisites

To perform the first synchronization check, the device should provide synchronization information that EdgeSync can monitor.

The common monitoring model includes:

- Synchronization State
- Active Synchronization Source
- Offset
- Frequency Offset
- Jitter
- Last Update Time
- Related alarms

The current synchronization states are:

| State | Meaning |
|---|---|
| `LOCKED` | The device is synchronized to an available reference source and is meeting deployment synchronization conditions. |
| `HOLDOVER` | The device temporarily lost its usable reference but maintains timing using its local clock. |
| `FREERUN` | The device operates without an active external synchronization reference. |
| `FAILED` | EdgeSync determines that the device no longer meets configured synchronization requirements. |

Metric availability depends on the device implementation and supported monitoring interface.

## 6. Access Requirements

EdgeSync provides two primary access paths.

### Web UI

Network administrators and network engineers use the Web UI to:

- Register and manage devices
- Configure synchronization sources
- Monitor synchronization conditions
- Review alarms
- Investigate synchronization problems

### REST API

Developers and external applications use the REST API to access EdgeSync resources programmatically.

The API currently covers:

- Devices
- Synchronization status
- Synchronization sources
- Alarms

API authentication details belong in the API Reference and Developer Guide rather than in this document.

## 7. Permissions and Roles

The current product model identifies three primary personas:

| Persona | Typical Getting Started activity |
|---|---|
| Network Administrator | Register a device, configure a synchronization source, and review status. |
| Network Engineer | Review synchronization information and investigate synchronization problems. |
| Developer | Access EdgeSync resources through the REST API. |

The exact permission model and role-to-permission mapping have not yet been fully defined for the MVP.

Therefore, this document does not prescribe specific permission names or access-control rules.

## 8. Required Technical Knowledge

### Network Administrator

Basic knowledge of:

- Network devices
- Device management
- PTP/NTP synchronization concepts
- Alarm acknowledgement and clearing

### Network Engineer

In addition to the above, familiarity with:

- Network synchronization
- PTP and/or NTP
- Synchronization state
- Offset, frequency offset, and jitter
- Basic network troubleshooting

### Developer

Familiarity with:

- REST APIs
- HTTP methods and status codes
- JSON
- API authentication
- Basic command-line tools such as `curl`

These are learning prerequisites, not product-enforced requirements.

## 9. Configuration Information

Before starting, prepare the information relevant to your environment.

### Minimum device information

```text
Device name
Device type
Management address
```

### Minimum synchronization source information

```text
Source ID
Source name
Source type
Source address
Priority
```

### Monitoring information

You do not need to configure synchronization metrics manually for the first check. Metrics such as offset, frequency offset, jitter, and last update time are monitoring information reported through the synchronization model.

## 10. What This Document Does Not Specify

The following requirements are intentionally left undefined because the current EdgeSync product definition does not establish them:

- Supported browser versions
- Supported operating-system versions
- CPU or memory requirements
- Database sizing
- Network bandwidth requirements
- Maximum number of devices
- Maximum number of synchronization sources
- Performance or latency targets
- High-availability requirements
- Production deployment topology
- Exact authentication implementation
- Complete permission matrix

These items should be added only when corresponding product or implementation requirements are defined.

## 11. Getting Started Path

Once the prerequisites are available, follow this path:

```text
Prepare device information
        ↓
Register a device
        ↓
Configure a synchronization source
        ↓
Open synchronization monitoring
        ↓
Perform the first synchronization check
        ↓
Review alarms if necessary
```

The next document, [Quick Start](quickstart.md), walks through the first successful EdgeSync workflow.

For a focused monitoring procedure, see [First Synchronization Check](first-check.md).

## 12. Related Documentation

- [Introduction](introduction.md)
- [Quick Start](quickstart.md)
- [First Synchronization Check](first-check.md)
- [Device Data Model](../reference/device-data-model.md)
- [Monitoring Data Model](../reference/monitoring-data-model.md)
- [Synchronization Sources](../concepts/synchronization-sources.md)
- [Synchronization States](../concepts/synchronization-states.md)
- [PTP](../concepts/ptp.md)
- [NTP](../concepts/ntp.md)
- [Administrator Guide](../administrator/index.md)
- [Developer Guide](../developer/index.md)
