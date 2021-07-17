# Home Assistant Object Detection Image

A Home Assistant Snapshot that contains the basic framework for Object Detection and Alerts. 
This is aimed at being used on basic CCTV/NVR setups. But pretty much any camera that can integrate with HA can be used.

This Snapshot makes use of the following Technologies:
* DOODS: https://www.home-assistant.io/integrations/doods/
* MotionEye: https://github.com/hassio-addons/addon-motioneye
* Plate Recognizer(Cloud): https://github.com/robmarkcole/HASS-plate-recognizer
* Shell Commands(HA Native): https://www.home-assistant.io/integrations/shell_command
* Automations (HA Native): https://www.home-assistant.io/docs/automation/

## Installing:
1. Install Home Assistant. Use any flavor or method. SUPERVISOR REQUIRED: https://www.home-assistant.io/installation/
2. Download this snapshot, and place it somewhere on the media/pc you installed HA on.
3. Boot Up Home Assistant
4. Follow these instructions on restoring this snapshot, starting at #3: https://community.home-assistant.io/t/how-to-restore-a-snapshot/227688
5. Default login details are as follows:
   Username: steve
   Password: password1234$
5. Browse to Configuration>Users>steve AND CHANGE THE PASSWORD!! You can now add your own user here if you wish.

## Setup
