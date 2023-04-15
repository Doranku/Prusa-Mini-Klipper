# Config

The configuration files for a Prusa Mini. On Mainsail
these are in ~/printer_data/config/.

The main configuration file is printer.cfg. It includes
some defaults like mainsail.cfg that comes with the 
Mainsail install. prusa-mini.cfg from the klipper
repositiory: 
  https://github.com/Klipper3d/klipper/blob/master/config/printer-prusa-mini-plus-2020.cfg

Changes to be made for specific printers:

- printer.cfg: the serial device in the mcu section to point to your actual printer device.
- printer.cfg: (en/dis)able *[include rpi-mcu.cfg]* See https://www.klipper3d.org/RPi_microcontroller.html to install it.
- extruder.cfg: the pid values after tuning: e.g. *PID_CALIBRATE HEATER=extruder TARGET=215*
- printer-*nozzlesize*.cfg: Select the correct nozzle size and set the pressure_advance parameter after tuning for the specific nozzle, see https://www.klipper3d.org/Pressure_Advance.html
- sheet-*name*.cfg: Set your z-offset for your sheet(s). It is the absolute value of the z-offset in the Prusa firmware.
- bed-mesh-default.cfg: save bed level values. Could be used to skip the auto levelling at the start of each print. See https://www.klipper3d.org/Bed_Mesh.html#calibration 
- resonance-compensation.cfg: See https://www.klipper3d.org/Resonance_Compensation.html to either get the specific printer values manuall or measuring them with a accelerometer.
