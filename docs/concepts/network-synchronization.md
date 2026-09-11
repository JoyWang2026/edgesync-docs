# Network Synchronization

Network synchronization ensures that network devices maintain a consistent view of time and frequency.

In 5G and edge networks, synchronization is important for functions that depend on accurate timing, including radio coordination, time-sensitive services, and network monitoring.

EdgeSync monitors synchronization information from network devices and their configured synchronization sources. It helps network engineers understand synchronization health and investigate synchronization problems.

## Why Synchronization Matters

Network devices use clocks to coordinate operations and maintain consistent timing.

A device may obtain timing information from an external synchronization source, such as a PTP Grandmaster, NTP server, or GNSS-based clock.

When synchronization is operating normally, the device can maintain its clock within the expected accuracy range.

When synchronization degrades or is lost, the device may enter a different synchronization state and eventually generate an alarm.

EdgeSync provides visibility into these conditions.

## Synchronization in Edge Networks

An edge network may contain many devices distributed across different locations.

A simplified synchronization architecture is:

```text
                 ┌──────────────────┐
                 │  GNSS / Reference│
                 │      Clock       │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │  PTP Grandmaster │
                 └────────┬─────────┘
                          │
                  PTP timing data
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          ▼               ▼               ▼
   ┌────────────┐  ┌────────────┐  ┌────────────┐
   │  Edge      │  │  Edge      │  │  Edge      │
   │  Device 1  │  │  Device 2  │  │  Device 3  │
   └────────────┘  └────────────┘  └────────────┘
          │               │               │
          └───────────────┼───────────────┘
                          │
                          ▼
                  ┌──────────────┐
                  │   EdgeSync   │
                  │   Monitoring │
                  └──────────────┘
```

The exact synchronization topology depends on the network design and the capabilities of the devices.

EdgeSync focuses on monitoring synchronization information and relationships between network devices and their configured reference sources. It does not represent the underlying physical timing infrastructure.

## Synchronization Sources

A synchronization source provides timing information that a network device uses as a reference.

EdgeSync monitors the following synchronization source types:

- PTP Grandmaster
- NTP Server

A GNSS reference may also be part of the synchronization infrastructure. It can provide timing information to a PTP Grandmaster, clock, or other timing equipment.

A device may have one or more configured sources.

The device uses the configured synchronization mechanism to select or obtain timing information from an available source.

### PTP Grandmaster

A PTP Grandmaster provides precise time information using the Precision Time Protocol (PTP).

PTP is commonly used when network devices require more precise time synchronization than traditional NTP deployments can provide.

In EdgeSync, a PTP source includes information such as:

- Source ID
- Source address
- Priority
- Source status
- Last update time

### NTP Server

An NTP server provides time synchronization using the Network Time Protocol (NTP).

NTP is widely used for general-purpose network time synchronization.

In EdgeSync, an NTP source includes information such as:

- Server address
- Priority
- Source status
- Last synchronization time

### GNSS Reference

A GNSS-based reference obtains timing information from a Global Navigation Satellite System.

A GNSS reference can provide a time or frequency reference to synchronization infrastructure, such as a PTP Grandmaster or other timing equipment.

In EdgeSync, a GNSS reference is treated as part of the synchronization infrastructure rather than as a network synchronization protocol.

## Synchronization State

EdgeSync defines the following synchronization states for monitoring purposes:

| State      | Description                                                                                           |
| ---------- | ----------------------------------------------------------------------------------------------------- |
| `LOCKED`   | The device is synchronized to an available reference source.                                          |
| `HOLDOVER` | The device has temporarily lost its reference source but is maintaining timing using its local clock. |
| `FREERUN`  | The device is operating without an active external synchronization reference.                         |
| `FAILED`   | EdgeSync determines that the device is no longer meeting the configured synchronization requirements.    |

These states are EdgeSync monitoring states. The exact synchronization states reported by an underlying network device may vary by device implementation.

For detailed information about each state, see [Synchronization States](synchronization-states.md).

## Synchronization Metrics

Synchronization state alone may not provide enough information to diagnose a problem.

EdgeSync therefore exposes synchronization metrics that help engineers evaluate synchronization quality.

### Offset

Offset represents the time difference between a device clock and its reference.

EdgeSync reports offset in nanoseconds:

> offsetNs

For example:

```JSON
{
  "offsetNs": 35
}
```

A larger offset may indicate synchronization degradation or a problem with the synchronization path.

The acceptable offset range depends on the device, network design, and configured operational requirements.

### Frequency Offset

Frequency offset represents the difference between the device clock frequency and its reference.

EdgeSync reports frequency offset in parts per billion (ppb):

> frequencyOffsetPpb

For example:

```JSON
{
  "frequencyOffsetPpb": 0.8
}
```

Frequency offset can help engineers determine whether a device clock is drifting relative to its reference.

### Jitter

Jitter describes short-term variation in timing measurements.

EdgeSync reports jitter in nanoseconds:

> jitterNs

For example:
```JSON
{
  "jitterNs": 12
}
```

Jitter should be interpreted together with other synchronization metrics rather than as an isolated indicator.

### Last Updated

The `lastUpdated` field indicates when the synchronization information was most recently updated.

Example:

```JSON
{
  "lastUpdated": "2026-09-08T08:30:00Z"
}
```

A stale update time may indicate that the device or synchronization source is no longer reporting current information.

## How EdgeSync Evaluates Synchronization

At a high level, EdgeSync receives synchronization information from monitored network devices and makes the information available for monitoring and alarm management.

```text
Network Device
      │
      │ Synchronization data
      ▼
   EdgeSync
      │
      ├── Synchronization State
      ├── Active Source
      ├── Offset
      ├── Frequency Offset
      ├── Jitter
      └── Last Updated
      │
      ├───────────────┐
      ▼               ▼
 Monitoring        Alarms   
```

EdgeSync uses the reported synchronization information and configured monitoring conditions to help identify degraded or failed synchronization.

The monitoring information is available through the EdgeSync user interface and REST API.

## Example Synchronization Status

The following example shows a device that is currently in the `LOCKED` state:

```JSON
{
  "deviceId": "edge-001",
  "state": "LOCKED",
  "source": "ptp-gm-01",
  "offsetNs": 35,
  "frequencyOffsetPpb": 0.8,
  "jitterNs": 12,
  "lastUpdated": "2026-09-08T08:30:00Z"
}
```

In this example:

- `edge-001` is the monitored device.
- `LOCKED` indicates that the device is synchronized to a reference.
- `ptp-gm-01` is the currently active synchronization source.
- `offsetNs` is the measured clock offset.
- `frequencyOffsetPpb` is the measured frequency offset.
- `jitterNs` represents short-term timing variation.
- `lastUpdated` indicates when the information was last updated.

The example does not by itself indicate whether the reported metrics meet operational requirements. Those requirements depend on the configured thresholds and synchronization requirements of the device.

## Interpreting Synchronization Data

When investigating a synchronization issue, do not rely on a single metric.

Use the synchronization state, active source, metrics, and alarms together.

For example:

| Observation | Possible indication |
|---|---|
| `LOCKED` with low offset | Synchronization is operating normally. |
| `LOCKED` with increasing offset | Synchronization may be degrading. |
| `HOLDOVER` with a recent source-loss alarm | The reference source may be unavailable. |
| `FREERUN` | No active synchronization reference is currently available. |
| `FAILED` | The device is not meeting the configured synchronization requirements. |

These observations do not identify the root cause by themselves. Use the troubleshooting procedures to investigate the underlying problem.

For example, a device may transition through the following states:

```text
LOCKED
   │
   │ Reference becomes unavailable
   ▼
HOLDOVER
   │
   │ Holdover period expires
   ▼
FREERUN
   │
   │ Synchronization condition continues to degrade
   ▼
FAILED
```

The actual state transition depends on device behavior and the synchronization conditions configured for the EdgeSync deployment.

## Investigating Synchronization Issues

Network engineers can use EdgeSync to monitor:

- Current synchronization state
- Active synchronization source
- Clock offset
- Frequency offset
- Jitter
- Last update time
- Synchronization-related alarms

A typical monitoring workflow is:

1. Identify the affected device.
2. Check the current synchronization state.
3. Identify the active synchronization source.
4. Review synchronization metrics.
5. Check for related alarms.
6. Investigate the synchronization path.
7. Follow the appropriate troubleshooting procedure.

For troubleshooting procedures, see [Troubleshooting](../troubleshooting/index.md).

### Synchronization Source Unavailable

The configured synchronization source cannot be reached or does not provide usable timing information.

Possible causes include:

- Network connectivity problems
- Source failure
- Incorrect source configuration
- PTP or NTP communication problems

See [Synchronization Source Unavailable](../troubleshooting/source-unavailable.md).

### High PTP Offset

The device remains synchronized, but the measured PTP offset exceeds the expected range.

Possible causes include:

- Network delay variation
- Packet loss
- Incorrect PTP configuration
- Timing path problems
- Device performance issues

See [PTP Offset Too High](../troubleshooting/high-ptp-offset.md).

### Synchronization Lost

The device can no longer maintain synchronization with its reference.

The device may transition from `LOCKED` to `HOLDOVER` and eventually to another state if the reference is not restored.

See [Synchronization Lost](../troubleshooting/synchronization-lost.md).

## Related Documentation

- [Synchronization States](synchronization-states.md)
- [Synchronization Sources](synchronization-sources.md)
- [PTP](ptp.md)
- [NTP](ntp.md)
- [Troubleshooting](../troubleshooting/index.md)
