# CPU Temperature

Adds the U1's Rockchip SoC (the printer's main board CPU) as a temperature sensor, so you
can see it alongside your hotend and bed in Fluidd, Mainsail, and any Moonraker client.

## What it does

- Registers a `[temperature_sensor Rockchip]` that reports the SoC temperature.
- Shows up automatically in the temperature panel and graphs; no configuration needed.

## Why you might want it

- Keep an eye on board temperature in a hot enclosure or a warm room.
- Spot thermal issues before they cause instability.

## Notes

- Read-only: this sensor only reports temperature, it does not control any fan or heater.
- Specific to the Rockchip-based U1 mainboard.
