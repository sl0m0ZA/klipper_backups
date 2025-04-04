####################################
#				   #
#   Macro for Calibration Prints   #
#				   #
####################################

What is this macro?:
====================

This handy macro allows you to easily perform calibration prints from within your Klipper web interface. There are four available calibration prints:

	- Heat calibration
	- Flow calibration
	- Pressure advanced calibration
   	- Retraction calibration



How to use the macro?:
======================

The macro includes a drop-down menu with seven parameters:

	- TOOL
    	- EXTRUDER_TEMP
   	- BED_TEMP
	- FLOW
    	- FAN
    	- START
    	- FACTOR
    	- CALC

TOOL: This parameter allows you to select the calibration print by typing its name. Currently, there are five options: "heat," "flow," "pressure," "retraction," and "help." More details on these options will be provided later. The default is "help."

EXTRUDER_TEMP: This parameter allows you to set the extruder temperature to use for the calibration print. The default is 200°C (for PLA).

BED_TEMP: This parameter allows you to set the bed temperature for the calibration print. The default is 60°C (for PLA).

FLOW: This parameter lets you set the flow rate for these prints. The default is 100 

FAN: This parameter lets you set the fan speed, starting at layer 2. The default is 60% (for PLA).

START: This parameter is used the pressure advanced and retraction calibration print. It's for the TUNING_TOWER command and sets the pressure advanced and retraction value at which the print should start. The default is 0

FACTOR: This parameter is for the TUNING TOWER command, used the TOOLs "heat", "pressure" and "retraction" and sets the rate at wich the START value should raise or drop.

CALC: This is a simple built-in calculator you can use to calculate the new flow or pressure advanced value after taking measurements from the finished print. The default is 0. If CALC is anything other than 0, it will perform calculations and display the results as text in the console. If it is set to 0, it will start the print.

Note: Not every TOOL requires every parameter. Parameters not needed for the selected TOOL can be ignored, and they won't have any effect. More details on this will be provided later. If you enter "help" in the TOOL parameter, a lot of red text will be output to the console, providing you with quick information about the macro.



What do I need to do before using the macro?:
=============================================

Before using the macro, you need to reslice the provided .stl files, copy and paste the G-code into the G-code section of the respective Calibration_Prints/*.cfg file and make a few modifications. This is necessary because the existing G-code is tailored to my printer, an Anycubic Predator Delta printer, and may not work on your printer if it isn't a delta printer. I use Ultimaker CURA for slicing, and you should use the following slicer settings and make the following changes to the G-code after slicing:

heat_tower.stl for the heat_tower_print.cfg:

	- Extruder temperature: Doesn't matter because the macro overwrites it anyway.
	- Bed temperature: Doesn't matter because the macro overwrites it anyway.
    	- Cooling fan: Doesn't matter because the macro overwrites it anyway.
    	- Layer height: 0.2.
    	- Line width: 0.4.
    	- Flow: 100%.
	- Infill: 0%
    	- Bottom layers: 0.
    	- Top layers: 0.
    	- Use a brim.
    	- Spiralize/vase mode.
    	- Print speed: Use your printer's usual speed.
	- activate firmware retraction

Either delete the slicer's start and end G-code or replace your START_PRINT and END_PRINT macros with START_TUNING_PRINT and END_TUNING_PRINT. Change the first 'M106' to 'M106 S{printer["gcode_macro Calibration_Tool"].fan}; there should not be any other M106 command further down the G-code. Use the provided G-code inside the .cfg to visualize how these changes should look.

flow_cal_test.stl for the flow_calibration_print.cfg:

    	- Extruder temperature: Doesn't matter because the macro overwrites it anyway.
    	- Bed temperature: Doesn't matter because the macro overwrites it anyway.
    	- Cooling fan: Doesn't matter because the macro overwrites it anyway.
    	- Layer height: 0.2.
    	- Line width: 0.4.
	- Number of walls: 2.
    	- Flow: 100%.
	- Infill: 0%
    	- Bottom layers: 0.
    	- Top layers: 0.
    	- Use brim.
    	- Spiralize/vase mode.
    	- Print speed: Use your printer's usual speed.
	- activate firmware retraction

Either delete the slicer's start and end G-code or replace your START_PRINT and END_PRINT macros with START_TUNING_PRINT and END_TUNING_PRINT. Change the first 'M106' to 'M106 S{printer["gcode_macro Calibration_Tool"].fan}; there should not be any other M106 command further down the G-code. Use the provided G-code inside the .cfg to visualize how these changes should look. 
Note: The calculator used for this calibration assumes the print is sliced with 100% flow and 0.4 line width and uses a simple ratio calculation. If you want to change that, open the macro_calibration.cfg and adjust line 29: {% set Result = 0.8 * 100 / C %}.

retraction_test.stl for the retraction_tower_print.cfg:

    	- Extruder temperature: Doesn't matter because the macro overwrites it anyway.
    	- Bed temperature: Doesn't matter because the macro overwrites it anyway.
    	- Cooling fan: Doesn't matter because the macro overwrites it anyway.
    	- Layer height: 0.2.
    	- Line width: 0.4.
	- Number of walls: 2.
    	- Flow: 100%.
	- Infill: 0%
	- Use a skirt.
    	- Print speed: Use your printer's usual speed.
    	- Z Seam Alignment: User-specified.
    	- Z Seam X: Centered between the two towers, in my example 0.
    	- Z Seam Y: Centered between the two towers, in my example 0.
	- activate firmware retraction

Either delete the slicer's start and end G-code or replace your START_PRINT and END_PRINT macros with START_TUNING_PRINT and END_TUNING_PRINT. Change the first 'M106' to 'M106 S{printer["gcode_macro Calibration_Tool"].fan}; there should not be any other M106 command further down the G-code. Use the provided G-code inside the .cfg to visualize how these changes should look.

square_tower.stl for the pressure_advanced_print.cfg:

    	- Extruder temperature: Doesn't matter because the macro overwrites it anyway.
    	- Bed temperature: Doesn't matter because the macro overwrites it anyway.
    	- Cooling fan: Doesn't matter because the macro overwrites it anyway.
    	- Layer height: 0.3.
    	- Line width: 0.5.
    	- Number of walls: 2.
    	- Flow: 100%.
	- Infill: 0%
	- Use a skirt.
    	- Bottom layers: 1mm.
    	- Top layers: 0.
    	- Print speed: Use your printer's usual speed.
	- activate firmware retraction

Either delete the slicer's start and end G-code or replace your START_PRINT and END_PRINT macros with START_TUNING_PRINT and END_TUNING_PRINT. Change the first 'M106' to 'M106 S{printer["gcode_macro Calibration_Tool"].fan}; there should not be any other M106 command further down the G-code. Use the provided G-code inside the .cfg to visualize how these changes should look.

----------------------------------------------------------------------

Note: All .stl files are designed to be as simple, small, and fast to print as possible, with the exception of the square_tower.stl. You can use your own designs for the calibration prints and edit the G-code to use in the macro.
For this macro to work you have to activate firmware retraction. Cura doesnt support is natevly, you have to install a Plugin called "Printer Settings" and activate it there. You also have to add a [firmware retraction] section in your printer.cfg for more information look at the Klipper documentation,
Link:	https://www.klipper3d.org/G-Codes.html?h=fir#firmware_retraction 
	https://www.klipper3d.org/Config_Reference.html#firmware_retraction




What about the START_TUNING_PRINT and END_TUNING_PRINT?:
========================================================

The START_TUNING_PRINT and END_TUNING_PRINT are separate macros executed before and after each calibration print. They are designed to be basic and should function with most 3D printers. These macros do not include a purge line, so when slicing the mentioned G-code, consider using a skirt or brim. START_TUNING_PRINT simply heats, homes, and starts the print, while END_TUNING_PRINT turns off everything and raises the nozzle by 20mm, that's it.

You have the flexibility to customize these START_TUNING_PRINT and END_TUNING_PRINT macros according to your preferences within the macro_calibration.cfg file. When making adjustments, please ensure that you retain the necessary parameters.




How to add this macro?:
=======================

Simply place the "Calibration_Prints" folder and its contents in the same location as your printer.cfg file, and add "[include Calibration_Prints/*.cfg]" inside the printer.cfg file. After a restart, a macro called "CALIBRATION TOOL" should appear.



How do these calibration prints work?:
======================================

Heat tower: It's a one-walled hollow cube with no infill, top, or bottom layers, printed in vase mode. It has eight segments stacked on top of each other, separated with a slight indent. The print starts with the provided EXTRUDER_TEMP, and each segment or 5mm decreases the temperature by the provided FACTOR. After the print is finished, inspect each segment to see which one looks the best. Check the corners, surface quality, and use your fingernail to check for layer adhesion. This print doesn't test overhangs and bridging, which are more dependent on your printer's cooling capabilities than the print temperature.
Used parameters are: 
	- EXTRUDER_TEMP
	- BED_TEMP
	- FAN (optional, default 255)
	- FLOW (optional, default 100%)
	- FACTOR 

NOTE: The print temperature should not set to be  to low because the last segment dcould be printed at or below the minimum extruder temperature. The minimum extruder temperature is usually set to 170°C, and if it drops below that, the printer will halt. If you want to test bridging and overhangs in your heat tower, you can use and slice your own model, as mentioned above.

Flow calibration: Similar to the heat tower, it's a hollow cube with no infill, top, or bottom layers, but printed with 2 walls. It's not as tall and has no segments. After the print finishes, measure the wall thickness of all four walls and calculate the average. Once you have your number, for example, 0.83mm, you can enter "flow" in the TOOL parameter and input your measured wall thickness in CALC. Since CALC is not set to 0, the printer won't start, but instead, the console will output your new flow value. 
Used parameters are: 
	- EXTRUDER_TEMP
	- BED_TEMP
	- FAN (optional, default 255)
	- CALC (optional, after printing, inputing what you meassured

NOTE: The walls are designed to be 0,8mm and should be printed with two walls and 0,4mm line with and 100% flow. The Calculator uses a simple ratio calculation: what the wall thickness schould be / 100 (the flow rate its printed with) * what you meassured = new optimal flow value

Pressure advanced print: It's the square_tower from the Klipper documentation. Provide the START and FACTOR values. After it's printed, measure from the bottom up to the height where the corner looks the best. For example, if you measure 20mm, you can set TOOL to "pressure," START and FACTOR to the values you used for the print, and in CALC, enter your measured height, such as 20mm. Since CALC is not 0, the print won't start, but instead, the console will output your new pressure advanced value. You should first calibrate flow, before calibrating pressure advanced. Slice the .stl at 100% flow and use the macro flow parameter to adjust the flow for this print.
Used parameters are: 
	- EXTRUDER_TEMP
	- BED_TEMP
	- FAN (optional, default 255)
	- FLOW (should be set what you calibrated before)
	- START
	- FACTOR
	- CALC (optional, after printing, inputing what you meassured


Retraction calibration: It's a small print with a simple base and two towers. It has five segments each 5mm tall stacked on top of each other, separated with a slight indent. Set the START retraction value and the FACTOR by wich the retraction value should raise each segent. Look at the segment where stringing stops to find your new retraction value.
Used parameters are: 
	- EXTRUDER_TEMP
	- BED_TEMP
	- FAN (optional, default 255)
	- FLOW (should be set what you calibrated before)
	- START
	- FACTOR



CAUTION:
========

The macro, as it is with my specific G-code settings, has been tailored for my particular printer. Please do not copy, paste, and use this macro as-is, as it may likely result in damage to your printer. I am not liable for any damages that may occur to your printer.

On a side note, since the prints are executed as a macro and not as a .gcode file, you cannot cancel the print. If you need to stop the printer, you'll need to power cycle it or use the emergency stop function. It may not be elegant, but it is effective.