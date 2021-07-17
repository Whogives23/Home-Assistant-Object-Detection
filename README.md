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
#### Installation Guide: https://www.youtube.com/watch?v=SHg6fa0x7OA
1. Install Home Assistant. Use any flavor or method. SUPERVISOR REQUIRED: https://www.home-assistant.io/installation/
2. Download this snapshot, and place it somewhere on the media/pc you installed HA on.
3. Boot Up Home Assistant
4. Follow these instructions on restoring this snapshot, starting at #3: https://community.home-assistant.io/t/how-to-restore-a-snapshot/227688
5. Default login details are as follows:
   Username: steve
   Password: password1234$
5. Browse to Configuration>Users>steve AND CHANGE THE PASSWORD!! You can now add your own user here if you wish.

## Setup
   ### MotionEye: 
   #### Guide: https://www.youtube.com/watch?v=e27jyEcE5lU
   #### Add Network Camera
   The URL below would be unique to your camera. A google search should be able to help you find it. 
   Here is some info on Dahua cams: https://dahuawiki.com/Remote_Access/RTSP_via_VLC
   
      URL: rtsp://<CamOrNVR_RTSP_URL>
      Username: <CamOrNVR Authentication Username>
      Password: <CamOrNVR Authentication Pass>
      Camera: UDP or TCP should work
   #### Change Camera Settings:
   ##### Video Device:
      Camera Name: <Something Descriptive>
      Video Resolution: <Match your Cam/NVR Resolution>
      Frame Rate: <25 is a good Start. Match your Cam/NVR if Possible>
   ##### Motion Detection:
      Auto Threshold Tuning: ON  (You can manually tune this if Auto doesnt work for you)
      Motion Gap: 10 (Adjust this depending on how much movement this camera sees)
      Captured Before/After: 1
   ##### Motion Notification:
      Call a Web Hook: http://homeassistant.local:8123/api/webhook/<camera_name>_motion  (This needs to match the webhook set in Home Assistant Automations)
      HTTP Method: Post (form)
