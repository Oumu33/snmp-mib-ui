# API Reference

The platform provides a comprehensive RESTful API for integration with third-party systems.

## Core API Endpoints

### Device Management
- `GET /api/v1/devices`: Get a list of all devices.
- `POST /api/v1/devices`: Add a new device.
- `GET /api/v1/devices/{id}`: Get details for a specific device.
- `PUT /api/v1/devices/{id}`: Update a device's information.
- `DELETE /api/v1/devices/{id}`: Remove a device.

### SNMP Operations
- `POST /api/v1/snmp/get`: Perform an SNMP GET operation.
- `POST /api/v1/snmp/walk`: Perform an SNMP WALK operation.
- `POST /api/v1/snmp/test`: Test SNMP connectivity to a device.

### MIB Management
- `GET /api/v1/mibs`: List all uploaded MIB files.
- `POST /api/v1/mibs/upload`: Upload a new MIB file.
- `DELETE /api/v1/mibs/{id}`: Delete a MIB file.

### Configuration Generation
- `POST /api/v1/configs/generate`: Generate monitoring configurations (e.g., for Prometheus).
- `POST /api/v1/configs/validate`: Validate a configuration file.

### Alert Rules
- `GET /api/v1/alert-rules`: Get all configured alert rules.
- `POST /api/v1/alert-rules`: Create a new alert rule.
- `POST /api/v1/alert-deployment/deploy`: Deploy alert rules to the monitoring system.

### Monitoring Component Management
- `GET /api/v1/monitoring/components`: Get a list of available monitoring components.
- `POST /api/v1/monitoring/install`: Install a monitoring component on a remote host.
- `GET /api/v1/monitoring/status`: Check the status of installed components.

### System Health
- `GET /api/v1/health`: Check the health status of the backend and frontend services.
- `GET /api/v1/system/health`: Get system resource usage (CPU, memory).
- `GET /api/v1/cache/stats`: View statistics for the in-memory cache.
- `GET /api/v1/database/stats`: View statistics for the SQLite database.
