# Idle Timeout

Sets how long the printer sits idle, with no activity, before Klipper powers down its stepper
motors and heaters. You choose the timeout in seconds.

## What it does

- Overrides Klipper's `[idle_timeout] timeout` with the value you choose, so the setting sticks
  across prints instead of reverting to the printer's short default.
- Leaves `timeout_on_pause` and the rest of the printer's config untouched.

## Why you might want it

- The Snapmaker U1 ships with a 5 minute (300 second) idle timeout, which can power the motors
  down mid-task while you swap filament, level by hand, or step away for a moment. On the U1 the
  firmware re-applies the configured timeout after every print, so setting it in the config (what
  this plugin does) is what makes a longer timeout actually stick.
- A longer timeout keeps the printer ready; a shorter one saves power and wear.

## Configuration

- **Idle timeout (seconds)**: seconds of inactivity before the motors power down. Default is 7200
  (2 hours). 3600 is 1 hour, 1800 is 30 minutes. Must be a positive whole number.

## Notes

- Takes effect after the plugin installs and Klipper restarts.
