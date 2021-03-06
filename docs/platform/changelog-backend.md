# Changelog

**Application:** Lobaro Backend

## unreleased

## 1.7.0 - 2020-06-04

### Added
- Allow sending user invitation mails with password reset link
- Allow user to restore a lost password
- GraphQL API: Devices can be filtered by DeviceTypeId
- Verify that server.frontendUrl is set in configuration
- Add TTN REST Endpoint /api/ttn/data

### Changed
- Prevent spaces in device addresses

## 1.6.0 - 2020-04-09

### Added
- Allow org-admin to edit wMbus Keys
- Allow org-admin to upload organisation logos
- Documentation for REST API endpoint `/api/devices`
- Parser: Record.getReceivedAt() to access the uplink message received_at date

### Changed
- Chirpstack device synchronisation reports more details

### Removed
- Gateway entity that is no longer used
- Hardcoded MQTT publishers that are no longer used

### Fixed
- Allow to remove App in device via GraphQL API
- NB-IoT Data is saved for all matching devices, not just one
- Downlink messages created_at was set to null after update
- Allow to update config values that are not defined in Device Type
- Allow org-Admin to edit wMbus Keys

## 1.5.0

### Added
- Synchronization of devices with Chirpstack
