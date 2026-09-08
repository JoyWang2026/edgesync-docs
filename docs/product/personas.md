# EdgeSync User Personas

## 1. Overview

EdgeSync supports three primary user types:

1. Network Administrator
2. Network Engineer
3. Developer

Each user type has different goals, tasks, and documentation needs.

---

## 2. Network Administrator

### Role

The Network Administrator is responsible for managing EdgeSync devices, synchronization configurations, and alarms.

### Primary Goals

- Register and manage network devices
- Configure synchronization sources
- Monitor device status
- Manage synchronization-related alarms

### Typical Tasks

| Task | Expected Outcome |
|---|---|
| Register a device | The device is added to EdgeSync |
| Configure a synchronization source | The device uses the configured source |
| View device status | The administrator can determine whether the device is healthy |
| Acknowledge an alarm | The alarm is marked as acknowledged |

### Documentation Needs

The Network Administrator primarily needs task-oriented documentation, including:

- How to register a device
- How to configure a synchronization source
- How to view device status
- How to acknowledge an alarm

---

## 3. Network Engineer

### Role

The Network Engineer monitors network synchronization and investigates synchronization-related problems.

### Primary Goals

- Monitor synchronization health
- Analyze synchronization metrics
- Identify synchronization problems
- Troubleshoot synchronization failures

### Typical Tasks

| Task | Expected Outcome |
|---|---|
| Check synchronization status | Determine the current synchronization state |
| Analyze offset | Determine whether clock offset is within an acceptable range |
| Check synchronization source | Identify the active synchronization source |
| Investigate an alarm | Determine the possible cause of the problem |
| Troubleshoot synchronization loss | Restore synchronization |

### Documentation Needs

The Network Engineer primarily needs:

- Conceptual documentation
- Monitoring documentation
- Troubleshooting guides
- Error and alarm references
- Architecture documentation

---

## 4. Developer

### Role

The Developer integrates EdgeSync with external applications and monitoring systems.

### Primary Goals

- Access EdgeSync data programmatically
- Retrieve device information
- Retrieve synchronization status
- Query alarms
- Automate monitoring tasks

### Typical Tasks

| Task | Expected Outcome |
|---|---|
| Authenticate with the API | The application can access EdgeSync |
| Retrieve device information | Device data is returned |
| Retrieve synchronization status | Current synchronization information is returned |
| Query alarms | Relevant alarms are returned |
| Handle API errors | The application can respond appropriately |

### Documentation Needs

The Developer primarily needs:

- API overview
- Authentication guide
- Quickstart
- API reference
- Request and response examples
- Error handling documentation

---

## 5. User-to-Documentation Mapping

| User | Primary Goal | Documentation Type |
|---|---|---|
| Network Administrator | Manage and configure EdgeSync | Task-oriented documentation |
| Network Engineer | Monitor and troubleshoot synchronization | Conceptual and troubleshooting documentation |
| Developer | Integrate with EdgeSync | Developer and API documentation |

---

## 6. Documentation Design Implications

The different user goals influence the structure of the EdgeSync documentation.

### Network Administrators

Documentation should:

- Focus on completing specific tasks
- Use step-by-step procedures
- Clearly identify prerequisites
- Provide expected results

### Network Engineers

Documentation should:

- Explain synchronization concepts
- Explain system behavior
- Describe synchronization states and metrics
- Provide diagnostic procedures
- Explain possible causes and solutions

### Developers

Documentation should:

- Provide a quick path to the first successful API request
- Include complete API references
- Provide request and response examples
- Document authentication and errors
- Use consistent terminology and field names

---

## 7. Key Documentation Principle

EdgeSync documentation should be organized around user goals and tasks rather than internal product components alone.

Users should be able to answer questions such as:

- How do I register a device?
- How do I check synchronization status?
- Why is a device in HOLDOVER state?
- Why is the PTP offset too high?
- How do I retrieve synchronization status through the API?
