# CURA Gcode

### Start GCODE (Load stored mesh)
```gcode
G92 E0 ; Reset Extruder

M140 S{material_bed_temperature} ; start preheating the bed
M104 S{material_print_temperature} ?T0 ; start preheating hotend

G28 ; Home all axes
G29 L1 ; Load the mesh stored in slot 1

M190 S{material_bed_temperature} ; heat to bed setting in Cura and WAIT
M109 S{material_print_temperature} ?T0 ; heat hotend to setting in Cura and WAIT

G29 J ; Probe 3 points to tilt mesh

G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0 ; Move over to prevent blob squish
```

### Start GCODE (Probe mesh before print)
```gcode
G92 E0 ; Reset Extruder

M140 S{material_bed_temperature}       ; start preheating the bed
M104 S{material_print_temperature} ?T0 ; start preheating hotend
M190 S{material_bed_temperature}       ; heat to bed setting in Cura and WAIT
M109 S{material_print_temperature} ?T0 ; heat hotend to setting in Cura and WAIT

G4 S120             ; wait 120s for everything to get nice and toasty
G29 F 0             ; disable fade height
G28                 ; home all axes
G29 P1              ; automatic mesh probing using ABL
G29 P3 T            ; extrapolate unreachable mesh values
G29 P3 T            ; extrapolate unreachable mesh values
G29 P3 T            ; extrapolate unreachable mesh values
G29 P3 T            ; extrapolate unreachable mesh values
G29 F5.0            ; re-enable fade height at 5.0mm
G29 S0              ; save mesh to slot 0
M420 S1 V           ; enable and output updated mesh values
G4 S10              ; wait 10s for user to read mesh status
G29 A               ; Ensure UBL is activated
G29 L1              ; Load the mesh stored in slot 0
M500                ; save the new mesh to EEPROM

G1 Z2.0 F3000                   ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X0.1 Y20 Z0.3 F5000.0        ; Move to start position
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
G1 X0.4 Y200.0 Z0.3 F5000.0     ; Move to side a little
G1 X0.4 Y20 Z0.3 F1500.0 E30    ; Draw the second line
G92 E0                          ; Reset Extruder
G1 Z2.0 F3000                   ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0          ; Move over to prevent blob squish
```

Stop GCODE
```gcode
G91                 ; Relative positioning
G1 E-10 Z0.2 F2400  ; Retract and raise Z
G1 X5 Y5 F3000      ; Wipe out

G1 Z10    ; Raise Z more
G90       ; Absolute positionning

M106 S0   ; Turn-off fan
M104 S0   ; Turn-off hotend
M140 S0   ; Turn-off bed

M84 X Y E ; Disable all steppers but Z
```
