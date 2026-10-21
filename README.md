# AutoDrive

A CAN bus sniffing setup for exploring a car's in-vehicle network. An Arduino with an MCP2515 CAN module captures bus traffic and streams it over serial to a PyQt5 desktop app. In the app you can watch, filter, label, save and replay packets.

This project is based on [canDrive](https://github.com/adamtheone/canDrive) by Adam Varga and is distributed under its [MIT license](LICENSE).

## Components

| File | Role |
| --- | --- |
| `canSniffer.ino` | Arduino firmware bridging the CAN bus and serial (250000 baud). `RANDOM_CAN 1` generates fake packets so you can test without a car. Set `CAN_SPEED` to match your bus (e.g. 500 kbps). |
| `main.py` | PyQt5 GUI. Scan and connect serial ports, start or stop sniffing, show only selected IDs, label IDs, save or load sessions as CSV, and send packets. |
| `canSniffer.ui` / `canSniffer_ui.py` | Qt Designer layout and the Python code generated from it (regenerate with `ui_converter.py`). |
| `SerialReader.py`, `SerialWriter.py`, `FileLoader.py`, `HideOldPackets.py` | Worker threads for serial I/O, session loading, and hiding stale packets. |

## Hardware

- An Arduino (Uno or Nano) and an MCP2515 CAN module, connected to the vehicle's CAN-H/CAN-L lines (e.g. through the OBD-II port).
- The Arduino [CAN library](https://github.com/sandeepmistry/arduino-CAN) by Sandeep Mistry. If your MCP2515 module has an 8 MHz crystal, change the clock in `MCP2515.h` from 16e6 to 8e6.

## Run

```bash
pip install -r requirements.txt
python main.py
```

To build a single-file executable, run `pyinstaller -F main.spec`.

## Safety

Sending frames on a live vehicle bus can interfere with safety-critical systems. Test with `RANDOM_CAN` or on a bench setup before connecting to a real car, and only transmit when you know exactly what a frame does.
