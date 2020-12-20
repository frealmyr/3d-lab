# Extruder steps/mm
Load up with cheap light-coloured filament.

1. Use a marker to mark 120mm on the filament from the extruder.

2. Heat up hotend
```gcode
M104 S200
```

3. Enter relative mode
```gcode
M83
```

4. Extrude 100mm of filament
```gcode
G1 E100 F100
```

5. Calculate new extruder steps/mm

```c
120 – [length from extruder to mark] = [actual length extruded]

[steps/mm value] x 100 = [steps taken]

[steps taken] / [actual length extruded] = [accurate steps/mm value]
```

6. Store new extruder steps/mm to firmware
```gcode
M92 E100.34
M500
```

7. Redo steps 4-6 until you hit 20mm from the extruder.

# UBL Mesh with BLTouch

1. Heat up bed and hotend
```gcode
M190 S55
M109 S205 T0
```

2. Disable fade height (Optional as it should be fixed in Marlin now, but I'm scarred for life.)
```gcode
G29 F 0
```

3. Home endstops, then create mesh using ABL
```gcode
G28
G29 P1
```

>You can check the current progress in terminal output or LCD, it will however never reach 100/100 mesh points due to probe being on the side of hotend.

4. Check if there are any missing mesh points, run `G29 P3 T` until all points are filled in the matrix.
```gcode
G29 T
G29 P3 T
```

5. Save mesh to slot 0 in EEPROM, set fade height, activate UBL, save configuration
```gcode
G29 S0
G29 F 5.0
G29 A
M500
```

# PID Autotune

Ensure that the nozzle and heatbed is cold before starting

1. Start nozze PID autotuning, 12 cycles

```gcode
M303 E0 S205 C12
```

2. Update the nozzle PID configuration with values outputted in terminal

```gcode
M301 P21.92 I1.66 D72.47
```

3. Start heatbed PID autotuning, 12 cycles

```gcode
M303 E-1 S55 C12
```

4. Update the heatbed PID configuration with values outputted in terminal

```gcode
M304 P57.85 I11.11 D200.85
```
