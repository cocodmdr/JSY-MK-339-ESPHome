# JSY-MK-339 ESPHome Component

This repository provides an ESPHome configuration package for the JSY-MK-339 three-phase DIN-rail Modbus energy meter.

![JSY-MK-339 Energy Meter](docs/jsy-mk-339.jpg)

## Features

- Real-time monitoring of voltage, current, power, frequency, and energy counters.
- Alarm status decoding for phase sequence, over-voltage, over-current, and leakage alarm bits.
- Configurable limits for voltage, current, and leakage current.
- Wiring mode configuration (3-phase 3-wire / 3-phase 4-wire).
- Multi-device support on the same RS-485 bus.

> Note: This is not a protection device. Use it for monitoring and automation with proper safeguards.

## Manufacturer Documentation

Technical details and register mapping come from the JSY-MK-339 user manual.

[JSY-MK-339 User Manual (PDF)](docs/jsy-mk-339-user-manual.pdf)

## ESPHome Integration

### Prerequisites

Ensure UART and Modbus are configured in your ESPHome node.

```yaml
uart:
  - id: mod_bus
    tx_pin: GPIO14
    rx_pin: GPIO12
    baud_rate: 9600

modbus:
  - id: modbus1
    uart_id: mod_bus
```

### Single Meter Setup

```yaml
packages:
  meter: !include
    file: jsy-mk-339.yaml
    vars:
      meter_name: "Main Meter"
      meter_id: jsy339_main
      meter_address: "1"
```

### Multiple Meters on One RS-485 Bus

```yaml
packages:
  meter1: !include
    file: jsy-mk-339.yaml
    vars:
      meter_name: "Grid"
      meter_id: jsy339_grid
      meter_address: "1"

  meter2: !include
    file: jsy-mk-339.yaml
    vars:
      meter_name: "Load"
      meter_id: jsy339_load
      meter_address: "2"
```

### Using `jsy-mk-339.yaml` directly from GitHub

```yaml
packages:
  meter1:
    url: https://github.com/cocodmdr/JSY-MK-339-ESPHome
    files:
      - path: jsy-mk-339.yaml
        vars:
          meter_name: "Grid"
          meter_id: jsy339_grid
          meter_address: "1"
```

## Repository Layout

- `jsy-mk-339.yaml`: Main reusable package.
- `docs/example.yaml`: Example with two meters on one RS-485 bus.

## Known Issues

- Register behavior can vary slightly between hardware revisions.
- If communication is unstable, reduce polling density or improve RS-485 wiring/termination.
