# Introduction

## What is EdgeSync?

EdgeSync is a fictional network synchronization monitoring and management platform for 5G and edge networks.

EdgeSync provides a centralized interface for monitoring synchronization health, managing synchronization sources, investigating synchronization problems, and responding to synchronization-related alarms.

EdgeSync is designed to help network teams understand the synchronization state of their network devices and identify potential synchronization problems.

## What can you do with EdgeSync?

With EdgeSync, you can:

- Register and manage network devices.
- Configure synchronization sources.
- Monitor synchronization status and metrics.
- View and manage synchronization-related alarms.
- Investigate synchronization problems.
- Access synchronization information through REST APIs.

## Who is EdgeSync for?

EdgeSync supports three primary user types.

### Network Administrators

Network Administrators use EdgeSync to:

- Register network devices.
- Configure synchronization sources.
- Monitor device status.
- Manage alarms.

### Network Engineers

Network Engineers use EdgeSync to:

- Monitor synchronization health.
- Analyze synchronization metrics.
- Investigate synchronization problems.
- Troubleshoot synchronization failures.

### Developers

Developers use EdgeSync to:

- Access EdgeSync data programmatically.
- Retrieve device information.
- Retrieve synchronization status.
- Query alarms.
- Integrate EdgeSync with external applications.

## How EdgeSync works

At a high level, EdgeSync collects synchronization information from network devices and synchronization sources.

The platform processes this information and presents synchronization status, metrics, events, and alarms through the web interface and REST API.

A simplified data flow is:

```text
Network Devices
       │
       │ Synchronization Data
       ▼
   EdgeSync
       │
       ├── Synchronization Status
       ├── Metrics
       ├── Events
       └── Alarms
       │
       ├───────────────┐
       ▼               ▼
    Web UI          REST API
       │               │
       ▼               ▼
Administrators      Developers
Engineers
```

## Key concepts

Before working with EdgeSync, you should understand the following concepts:

- Device — A network device monitored by EdgeSync.
- Synchronization Source — A source that provides timing information, such as a PTP Grandmaster or NTP server.
- Synchronization State — The current synchronization condition of a device.
- Offset — The difference between the device clock and its reference clock.
- Holdover — A state in which a device maintains synchronization using its local clock after losing its reference source.

For more information, see the [Concepts](../concepts/index.md) section.

## Where should I start?

Choose the path that matches your role:

- Network Administrator: Start with the [Administrator Guide](../administrator/index.md).
- Network Engineer: Start with [Network Synchronization](../concepts/network-synchronization.md).
- Developer: Start with the [Developer Guide](../developer/index.md).

If you are new to EdgeSync, continue with the [Quick Start](../getting-started/quickstart.md).
