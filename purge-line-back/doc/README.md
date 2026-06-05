# Purge Line at the Back

Moves the print-start purge line to the back of the bed, out of the way of your part and
your view, instead of running it across the front.

## What it does

- Overrides the stock print-start purge-line macros so the priming line is drawn at the
  back of the bed.
- Adds a `DK_PRINT_START_LINE` macro that draws the relocated line.

## Using it

Install the plugin; the next print's purge line is drawn at the back automatically. No
slicer changes are needed if your start g-code already calls the Snapmaker print-start
sequence.

## Notes

- Klipper restarts when this plugin is installed or removed.
- Snapmaker U1 only (it overrides the stock U1 print-start macros).
