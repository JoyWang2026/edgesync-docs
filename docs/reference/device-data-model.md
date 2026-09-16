# EdgeSync Device Data Model

**Status:** Draft  
**Version:** 1.0

## 1. Purpose

This document defines the conceptual data model for a network device managed by EdgeSync.

It defines device identity, human-readable attributes, management connectivity information, device-management status, and the relationship between a device and its synchronization and alarm data.

> **Source of truth:** This document defines what Device data means. `openapi/edgesync.yaml` defines how the model is represented by the REST API.

## 2. Scope and boundaries

This model covers:

- Device identity
- Device name and type
- Management address
- Device-management status
- Relationships to synchronization status and alarms

This model does not define:

- Synchronization State semantics
- Synchronization Source semantics
- PTP or NTP protocol behavior
- Universal device status thresholds
- Database schema
- Device-specific implementation details

## 3. Device Model

A Device represents a network device registered with and managed by EdgeSync.

```json
{
  "deviceId": "edge-001",
  "name": "Edge Site 001",
  "type": "EDGE_ROUTER",
  "managementAddress": "192.0.2.10",
  "status": "ONLINE"
}
```

## 4. Fields

| Field | Type | Required | Meaning |
|---|---|---:|---|
| `deviceId` | string | Yes | Unique identifier of the network device. |
| `name` | string | Yes | Human-readable name used to identify the device. |
| `type` | string | Yes | Type of network device managed by EdgeSync. Supported device types are not yet formally enumerated. |
| `managementAddress` | string | Yes | Network address used by EdgeSync to communicate with the device for management purposes. |
| `status` | string | Yes | Current device-management connectivity status observed by EdgeSync. The status values are proposed MVP design and are not yet a locked product contract. |

## 5. Device Identity

`deviceId` is the stable identifier used to reference a device across EdgeSync resources.

Other resources use `deviceId` to associate monitoring information and alarms with the affected device.

## 6. Device Name

`name` is a human-readable identifier.

The name is not the canonical identity of the device and should not be assumed to be unique unless a future product requirement explicitly defines uniqueness.

## 7. Device Type

`type` identifies the category of network device.

The current MVP model intentionally leaves the value as a string because the existing product requirements do not define a complete supported-device-type enumeration.

Example:

```text
EDGE_ROUTER
```

A future product decision may define a controlled list of supported device types.

## 8. Management Address

`managementAddress` identifies the network address used by EdgeSync to communicate with the device for management.

The field is intentionally named `managementAddress` rather than `address` to distinguish it from addresses associated with synchronization sources.

A management address does not identify the device's synchronization source.

## 9. Device Status

Device Status describes the management connectivity condition observed by EdgeSync.

The proposed MVP values are:

| Status | Meaning |
|---|---|
| `ONLINE` | EdgeSync can currently communicate with the device for management. |
| `OFFLINE` | EdgeSync cannot currently communicate with the device for management. |
| `UNKNOWN` | EdgeSync cannot determine the current management connectivity status. |

These values are a proposed API design and should not be interpreted as an already-finalized product requirement.

### Device Status vs Synchronization State

Device Status and Synchronization State describe different aspects of a device.

For example:

```text
Device Status: ONLINE
Synchronization State: HOLDOVER
```

This is valid. A device may be reachable by EdgeSync while its synchronization condition is degraded.

Synchronization State is defined by the Monitoring Data Model and Synchronization States documentation.

## 10. Relationships

A Device can have:

- One current Synchronization Status record
- Multiple configured Synchronization Sources
- Multiple Events
- Multiple Alarms

Conceptually:

```text
Device
├── Synchronization Status
├── Configured Synchronization Sources
├── Events
└── Alarms
```

The Device resource does not embed these complete resources.

For example, synchronization information is retrieved through the synchronization status resource rather than being duplicated inside the Device object.

## 11. API Representation

The REST API represents the Device model using:

- `Device`
- `DeviceCreate`
- `DeviceUpdate`

The API contract is defined in `openapi/edgesync.yaml`.

## 12. Design Decisions

### 12.1 Use `managementAddress`

The model uses `managementAddress` instead of the generic `address` field.

This makes the purpose of the field explicit and prevents confusion with synchronization-source addresses.

### 12.2 Keep Device Status separate from Synchronization State

Device connectivity and synchronization condition are independent concepts.

Do not add `state`, `source`, `offsetNs`, or other synchronization fields to the Device resource merely to make the Device response more convenient.

### 12.3 Do not lock the device-type enumeration yet

The existing product requirements do not establish a complete list of supported device types.

The API therefore represents `type` as a string until the product model defines the supported values.

### 12.4 Treat Device Status values as proposed MVP design

`ONLINE`, `OFFLINE`, and `UNKNOWN` provide a practical initial model, but they are not currently treated as canonical product requirements.

## 13. Related Documentation

- [Monitoring Data Model](monitoring-data-model.md)
- [Synchronization States](../concepts/synchronization-states.md)
- [Synchronization Sources](../concepts/synchronization-sources.md)
- [API Reference](../api-reference/index.md)
