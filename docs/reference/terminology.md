# EdgeSync Terminology

This document defines the canonical terminology used across the EdgeSync documentation.

Use the terms defined here consistently across conceptual documentation, administrator guides, developer documentation, API references, and troubleshooting content.

## Terminology Principles

The EdgeSync documentation distinguishes between:

- **Synchronization sources** — sources that a network device uses for synchronization.
- **Timing references** — references that provide timing information to synchronization infrastructure.
- **Synchronization states** — high-level states used by EdgeSync to describe a device's synchronization condition.
- **Synchronization metrics** — measurements used to evaluate synchronization behavior.
- **Source status** — the availability of a configured synchronization source.
- **Alarms** — notifications that a monitored condition requires attention.

Do not use these terms interchangeably.

---

## Synchronization Concepts

| Canonical term | Definition | Usage notes |
|---|---|---|
| **Network Synchronization** | The process of maintaining a consistent timing relationship between network devices and their synchronization references. | Use this term for the overall synchronization concept. |
| **Synchronization Source** | A source that a network device uses as a reference for synchronization. | In EdgeSync, the primary network synchronization source types are PTP Grandmaster and NTP Server. |
| **Timing Reference** | A timing reference that provides time or frequency information to synchronization infrastructure or a synchronization system. | Use this term when referring to a reference at a broader infrastructure level. |
| **Synchronization State** | The high-level state that EdgeSync uses to describe the current synchronization condition of a monitored device. | Do not use "synchronization status" when referring to `LOCKED`, `HOLDOVER`, `FREERUN`, or `FAILED`. |
| **Synchronization Metric** | A measurement used to evaluate synchronization behavior or quality. | Includes Offset, Frequency Offset, Jitter, and Last Update Time. |
| **Synchronization Requirement** | A configured requirement used to determine whether synchronization conditions are acceptable. | Avoid implying that a single universal threshold applies to all devices or deployments. |

---

## Synchronization Sources

### PTP Grandmaster

**Canonical term:** `PTP Grandmaster`

A PTP Grandmaster provides the reference time used by devices in a PTP domain.

Use:

- PTP Grandmaster
- selected PTP Grandmaster
- configured PTP Grandmaster

Avoid:

- PTP Master
- Grand Master
- Master Clock
- PTP GM

Use `PTP Grandmaster` consistently in documentation prose, diagrams, and UI descriptions unless a product-specific API field requires a different representation.

---

### NTP Server

**Canonical term:** `NTP Server`

An NTP Server provides time synchronization information to network devices using NTP.

Use:

- NTP Server
- configured NTP Server
- available NTP Server

Avoid using the following as synonyms in formal documentation:

- Time Server
- NTP Source
- NTP Source Server

An NTP Server can be configured as a **Synchronization Source**.

---

### GNSS Reference

**Canonical term:** `GNSS Reference`

A GNSS Reference provides timing information to synchronization infrastructure or timing equipment.

In the EdgeSync conceptual model, GNSS is treated as a timing reference rather than a network synchronization protocol.

Use:

- GNSS Reference
- GNSS timing reference
- GNSS-based timing reference

Avoid:

- GNSS Source
- GNSS Server
- GNSS Synchronization Source

A GNSS Reference may provide timing to a PTP Grandmaster, clock, or other timing equipment.

---

## Active Synchronization Source

**Canonical term:** `Active Synchronization Source`

The Active Synchronization Source is the synchronization source currently selected or used by the monitored network device.

Use:

- Active Synchronization Source
- active source, when the context is already clear

Example:

```text
The device is currently synchronized to the active synchronization source.
```

In EdgeSync API examples, the `source` field represents the Active Synchronization Source.

Example:

```json
{
  "deviceId": "edge-001",
  "state": "LOCKED",
  "source": "ptp-gm-01"
}
```

### Important distinction

Do not use source to mean both the current active source and a previously used source.

If a device has no active synchronization source, source should be represented as:

```json
"source": null
```

If the product later needs to record a previously used source, use a separately defined term such as **Last Synchronization Source**.

## Primary and Secondary Sources

**Primary Source** and **Secondary Source** describe source priority or configuration within a device's synchronization configuration.

Use:

- Primary Source
- Secondary Source
- source priority
- source selection

Do not assume that Primary Source means PTP Grandmaster.

> **Note:**
> "Primary Source" describes source priority in the synchronization configuration. It does not define the PTP role of the source.

A PTP Grandmaster may be configured as either a primary or secondary synchronization source.

## Source Status

**Canonical term:** `Source Status`

Source Status indicates whether a configured synchronization source is currently available for use.

The supported source status values are:

| Status        | Meaning                                              |
| ------------- | ---------------------------------------------------- |
| `AVAILABLE`   | The source is available for synchronization.         |
| `UNAVAILABLE` | The source cannot currently be used.                 |
| `UNKNOWN`     | EdgeSync cannot determine the current source status. |

### State vs. Status

Do not use **Source Status** and **Synchronization State** interchangeably.

| Term                      | Answers                                                 |
| ------------------------- | ------------------------------------------------------- |
| **Source Status**         | Can this synchronization source currently be used?      |
| **Synchronization State** | What is the device's current synchronization condition? |

For example, a source can have an `AVAILABLE` status while the device is not currently synchronized to that source.

## Synchronization States

The following four values are the canonical EdgeSync synchronization states:

```text
LOCKED
HOLDOVER
FREERUN
FAILED
```

These are **EdgeSync monitoring states**. They do not necessarily correspond directly to every state reported by an underlying network device.

### LOCKED

`LOCKED` indicates that the device is synchronized to an available synchronization reference.

Use:

```text
Synchronization State: LOCKED
```

Do not write:

- Locked state
- LOCK state
- Synchronized state

when referring to the canonical state value.

### HOLDOVER

`HOLDOVER` indicates that the device has temporarily lost its usable synchronization reference but continues maintaining timing using its local clock.

Use:

```text
Synchronization State: HOLDOVER
```

`HOLDOVER` does not by itself identify the cause of the synchronization problem.

Possible causes include:

- Synchronization source unavailable
- Network connectivity problems
- PTP or NTP configuration problems
- Timing source failure

### FREERUN

`FREERUN` indicates that no usable external synchronization reference is currently available.

Use the exact spelling:

```text
FREERUN
```
Avoid:

- FREE-RUN
- FREE RUN
- Free Run

### FAILED

`FAILED` indicates that EdgeSync determines that the device is no longer meeting the configured synchronization requirements.

Use:

```text
Synchronization State: FAILED
```

The `FAILED` state describes the synchronization condition. It does not identify the root cause.

Possible causes may include:

- Synchronization source failure
- Network connectivity problems
- Incorrect PTP or NTP configuration
- Excessive packet delay or packet loss
- Device clock or timing subsystem problems
- Synchronization metrics exceeding configured requirements

## Synchronization Metrics

EdgeSync uses the following synchronization metrics.

### Offset

**Canonical term:** `Offset`

Offset represents the estimated time difference between the device clock and its synchronization reference.

API field:

```text
offsetNs
```

Unit:

```text
nanoseconds (ns)
```

Use:

- Offset
- clock offset
- measured offset

Do not assume that a particular offset value is universally acceptable. Acceptable offset depends on the device, network, application, and configured synchronization requirements.

### Frequency Offset

**Canonical term:** `Frequency Offset`

Frequency Offset represents the difference between the rate of the local clock and the synchronization reference.

API field:

```text
frequencyOffsetPpb
```

Unit:

```text
parts per billion (ppb)
```

Use:

- Frequency Offset
- frequency offset

Avoid:

- Frequency Difference
- Clock Frequency Difference
- Frequency Deviation

unless a specific technical context requires those terms.

### Jitter

**Canonical term:** `Jitter`

Jitter represents variation in synchronization timing measurements over time.

API field:

```text
jitterNs
```

Unit:

```text
nanoseconds (ns)
```

The interpretation of jitter depends on the device implementation and configured monitoring requirements.

### Last Update Time

**Canonical term:** `Last Update Time`

Last Update Time indicates when the synchronization information was most recently updated.

API field:

```text
lastUpdated
```

Avoid using multiple names for the same concept, such as:

- Last Synchronization Time
- Last Updated Time
- Update Timestamp

## Monitoring Terminology

### Monitoring

**Canonical term:** `Monitoring`

EdgeSync monitors synchronization information reported by supported network devices.

Use:

```text
EdgeSync monitors synchronization information.
```

Avoid describing EdgeSync as the PTP Grandmaster or as the underlying synchronization mechanism.

### Monitoring State

**Canonical term:** `Monitoring State`

Use this term when emphasizing that a state is defined by the EdgeSync monitoring model.

For example:

```text
`LOCKED` is an EdgeSync monitoring state.
```

Where possible, prefer **Synchronization State** in user-facing documentation because it is the primary product terminology.

### Root Cause

**Canonical term:** `Root Cause`

A synchronization state describes what is happening but does not necessarily explain why it is happening.

For example:

```text
Synchronization State: HOLDOVER
```

Possible causes:
- PTP Grandmaster unavailable
- Network connectivity problem
- PTP configuration error
- Timing source failure

Do not describe a synchronization state as a root cause.

## Alarm Terminology

### Alarm

**Canonical term:** `Alarm`

An alarm indicates that a monitored condition requires attention.

Use:

- Alarm
- Synchronization Alarm
- Active Alarm
- Alarm History

Do not use `Alarm`, `Event`, and `State` interchangeably.

### Active Alarm

**Canonical term:** `Active Alarm`

An alarm that currently requires attention.

### Alarm History

**Canonical term:** `Alarm History`

Historical synchronization and monitoring alarms recorded by EdgeSync.

### Acknowledge an Alarm

**Canonical action:** `Acknowledge an Alarm`

Use this wording for the user action that indicates an operator has reviewed an alarm.

Avoid:

- Confirm an alarm
- Accept an alarm
- Close an alarm

unless the product explicitly defines those as different actions.

### Clear an Alarm

**Canonical action:** `Clear an Alarm`

Use this wording when an alarm is cleared after the monitored condition is no longer present or the applicable clear condition has been met.

Do not use "acknowledge" and "clear" as synonyms.

### Events

**Canonical term:** `Event`

An event represents something that occurred and may provide useful context when investigating synchronization problems.

Events and alarms are related but different concepts:

| Term                      | Meaning                                        |
| ------------------------- | ---------------------------------------------- |
| **Event**                 | Something that occurred.                       |
| **Alarm**                 | A monitored condition that requires attention. |
| **Synchronization State** | The current synchronization condition.         |

Use **Recent Events** when referring to events relevant to troubleshooting.

## PTP Terminology

### Precision Time Protocol (PTP)

Use the full name on first occurrence:

```text
Precision Time Protocol (PTP)
```

Use `PTP` thereafter.

### PTP Domain

**Canonical term:** `PTP Domain`

A PTP Domain is a logical synchronization environment in which PTP clocks participate.

Use:

- PTP Domain
- PTP domain

Do not introduce alternative names such as "PTP synchronization domain" unless required by a specific technical source.

### PTP Profile

**Canonical term:** `PTP Profile`

A PTP Profile defines or constrains PTP behavior for a particular application environment.

The configured PTP Profile is part of the device and network configuration and may affect how PTP behavior and synchronization metrics are interpreted.

### PTP-capable Device

**Canonical term:** `PTP-capable device`

A network device that participates in PTP synchronization.

Use lowercase `device` unless the term appears at the beginning of a sentence or in a title.

### Boundary Clock

**Canonical term:** `Boundary Clock`

A PTP device that receives timing from an upstream source and provides timing to downstream devices.

### Transparent Clock

**Canonical term:** `Transparent Clock`

A PTP device that forwards PTP messages while accounting for the time spent traversing the device.

## PTP Message Terminology

Use the canonical PTP message names:

```text
Sync
Follow_Up
Delay_Req
Delay_Resp
```

When explaining the message exchange, preserve the exact message names used by the PTP terminology.

Example:

```text
The PTP Grandmaster sends a Sync message followed by a Follow_Up message.
```
Do not rename these messages for readability.

## Device Terminology

### Network Device

**Canonical term:** `Network Device`

Use `Network Device` when referring to a device in the network synchronization architecture.

Examples:

- Network Device
- monitored network device
- configured network device

In EdgeSync product documentation, `Device` may be used as a shorter form when the context is already clear.

Examples:

- Device List
- Device Details
- Device Configuration
- Register a Device

Avoid introducing alternative terms such as:

- Network Node
- Network Element
- Endpoint
- Edge Node

unless they represent distinct product concepts.

## API Terminology

The following API fields are canonical:

| Concept                       | API field            |
| ----------------------------- | -------------------- |
| Device ID                     | `deviceId`           |
| Synchronization State         | `state`              |
| Active Synchronization Source | `source`             |
| Offset                        | `offsetNs`           |
| Frequency Offset              | `frequencyOffsetPpb` |
| Jitter                        | `jitterNs`           |
| Last Update Time              | `lastUpdated`        |

Example:

```json
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

The terminology used in API documentation should match the terminology used in conceptual and user documentation.

## Synchronization vs. Sync

Use **synchronization** as the default term in formal documentation.

Preferred:

```text
The device is synchronized to the PTP Grandmaster.
```

Avoid using informal abbreviations in formal prose:

```text
The device is synced to the PTP Grandmaster.
```

The abbreviation `sync` may be used when it is part of an established technical term or protocol message name, such as the PTP `Sync` message.

## Terminology Quick Reference

The following table provides the primary terms that should be checked during documentation review.

| Use this                      | Do not use as a synonym          |
| ----------------------------- | -------------------------------- |
| Synchronization Source        | Sync Source, Timing Source       |
| Timing Reference              | Synchronization Source           |
| PTP Grandmaster               | PTP Master, Master Clock         |
| NTP Server                    | Time Server                      |
| GNSS Reference                | GNSS Source                      |
| Active Synchronization Source | Current Source, Active Reference |
| Source Status                 | Source State                     |
| Synchronization State         | Synchronization Status           |
| Synchronization Metric        | Sync Metric                      |
| Offset                        | Time Difference                  |
| Frequency Offset              | Frequency Difference             |
| Last Update Time              | Last Synchronization Time        |
| Alarm                         | Event, State                     |
| PTP Domain                    | PTP Network Domain               |
| Network Device                | Network Node, Network Element    |

## Documentation Rules

When writing or reviewing EdgeSync documentation:

1. Use the canonical term consistently after it has been introduced.
2. Do not use two terms as synonyms when they represent different concepts.
3. Preserve the capitalization of product-defined states and status values.
4. Use the exact API field names in API documentation and examples.
5. Use Synchronization State for LOCKED, HOLDOVER, FREERUN, and FAILED.
6. Use Source Status for AVAILABLE, UNAVAILABLE, and UNKNOWN.
7. Use Active Synchronization Source for the source currently selected or used by a device.
8. Use GNSS Reference when referring to GNSS within the synchronization infrastructure.
9. Do not assume that synchronization states, thresholds, or source-selection behavior are universal across all devices.
10. When a new product concept is introduced, define its canonical terminology before using alternative names across multiple documents.
