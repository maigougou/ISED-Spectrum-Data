# ISED source column selection · schema v2

Column letters refer to the original CSV opened in Excel, A–BI. Match by header name during processing.

**Retain exactly 32 columns in `source_projection`; exclude the 29 listed columns from that projection.** Shared values are normalized, and differing values in any retained column remain represented. Source P is stored once in `source_cells` and referenced by physical `radio_records`; the `radios` LEFT JOIN view restores its original text without dropping unknown Cell IDs or merging configurations across sites/technologies.

Schema 2 deliberately preserves sparse exceptions for AC/AG outside this projection in `radio_equipment_exceptions(rx_model, rx_certification_no)`. RX/TX values are compared exactly while both are available. Matching values share TX storage; within an exception NULL inherits TX and `''` preserves an explicitly empty RX. The exception tuple participates in radio deduplication, so conflicting RX values keep their own filing and warning associations. This is a narrowly scoped exception to physical field exclusion, not an extra projection column or a guessed RX value. All other excluded fields remain absent; their permitted diagnostic/correction codes may survive. The validator permits `rx_certification_no` only in this sparse table and rejects excluded column names elsewhere.

## Retained

| Original column | Official field |
|---|---|
| B | `licence_number` |
| E | `licence_category*` |
| F | `area_id*` |
| G | `area_name*` |
| I | `licensee_name*` |
| O | `technology` |
| P | `cell_id` |
| Q | `physical_id` |
| R | `province_code` |
| S | `latitude` |
| T | `longitude` |
| U | `site_type` |
| V | `structure_height` |
| W | `structure_type` |
| X | `date_last_changed` |
| Z | `tx_frequency` |
| AA | `rx_frequency` |
| AB | `tx_radio_model_number` |
| AD | `tx_radio_manufacturer_code` |
| AF | `tx_certification_no` |
| AH | `bandwidth` |
| AK | `downlink_allocation` |
| AN | `tx_number_antennas` |
| AO | `rx_number_antennas` |
| AP | `tx_ant_model_no` |
| AR | `tx_ant_manufacturer` |
| AT | `tx_ant_height` |
| AV | `tx_ant_omni_indicator` |
| AX | `tx_ant_horiz_beamwidth` |
| AZ | `tx_ant_vert_beamwidth` |
| BB | `tx_ant_azimuth` |
| BD | `tx_ant_elevation_angle` |

## Excluded

| Original column | Official field |
|---|---|
| A | `upload_date*` |
| C | `reference_number` |
| D | `subservice_id*` |
| H | `account_number*` |
| J | `contact_name` |
| K | `business_telephone` |
| L | `email_address` |
| M | `location` |
| N | `station_type` |
| Y | `record_id` |
| AC | `rx_radio_model_number` |
| AE | `rx_radio_manufacturer_code` |
| AG | `rx_certification_no` |
| AI | `class_emission` |
| AJ | `tx_power` |
| AL | `tx_ant_type` |
| AM | `rx_ant_type` |
| AQ | `rx_ant_model_no` |
| AS | `rx_ant_manufacturer` |
| AU | `rx_ant_height` |
| AW | `rx_ant_omni_indicator` |
| AY | `rx_ant_horiz_beamwidth` |
| BA | `rx_ant_vert_beamwidth` |
| BC | `rx_ant_azimuth` |
| BE | `rx_ant_elevation_angle` |
| BF | `tx_ant_gain` |
| BG | `rx_ant_gain` |
| BH | `tx_line_loss` |
| BI | `rx_line_loss` |

## Known source repair

The 2026-09-02 source contains four Skysurf Canada Communications Inc. rows for licence 010282259-001 / record SK02 with one extra opening double quote in M (`Hwy 201 & TransCanada Hwy, Broadview, S`). The parser removes only that extra quote, under exact licensee/licence/record/text guards. It preserves the four azimuths (40°, 160°, 220°, 340°). M and Y remain excluded from the dataset; repair is needed to align the retained fields. `quality-report.json` reports repaired rows separately from rejected rows. Other CSV quoting is untouched.
