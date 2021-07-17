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
1. Install Home Assistant. Use any flavor or method. SUPERVISOR REQUIRED: https://www.home-assistant.io/installation/ . 
   This snapshot was taken on a RPI4 running HASSOS. You might need to reinstall DOODS when setting this up on a different platform. 
   You can do this through Supervisor>Addon Store. Settings should remain the same.
2. Download this snapshot, and place it somewhere on the media/pc you installed HA on.
3. Boot Up Home Assistant
4. Follow these instructions on restoring this snapshot, starting at #3: https://community.home-assistant.io/t/how-to-restore-a-snapshot/227688
5. Default login details are as follows:
   Username: steve
   Password: password1234$
5. Browse to Configuration>Users>steve AND CHANGE THE PASSWORD!! You can now add your own user here if you wish.

## Setup

The Snapshot has a single camera(named backyard) set up from end to end. it would be best to only change this cameras settings in MotionEye, **keeping the Camera Name as backyard**, and see if all other setup works as is. If so, then go ahead and start cloning/editing these settings/config/automations for your other cameras.

If it works as expected, you should be seeing notifications pop up in the notifications tab, as well as images showing up in the Media Browser tab when motion, persons, or cars are detected.

![image](https://user-images.githubusercontent.com/46109936/126046226-689338ce-00e8-455f-8f53-ec3c3a5e6c1c.png)

![image](https://user-images.githubusercontent.com/46109936/126046244-9e2d1908-59d9-42bf-863a-dde58881cf02.png)



The #1 thing to check if something isnt working, is if you are using the right local Home Assistant URL in all files and configurations. 

These are the common URLs to use: http://homeassistant.local:8123, http://homeassistant.localdomain:8123, http://homeassistant:8123, http://< YourLocalHAIPAddress >:8123.

This should be checked in the following locations: Configuration.yaml,Automations.yaml, MotionEye Motion Notification Web Hook URL
   
Onto the config:

   ### MotionEye: 
   
   ![image](https://user-images.githubusercontent.com/46109936/126047628-67f429c2-722d-4bbf-bb1b-f9ac19c222a6.png)

   #### Guide: https://www.youtube.com/watch?v=e27jyEcE5lU
   Please Change the password on default credentials once logged in:
   
   Username: admin
   
   Password: password1234$
   #### Add Network Camera
   
   ![image](https://user-images.githubusercontent.com/46109936/126047664-6a9b4913-5172-464f-8fd3-7b7dab353f77.png)

   ![image](https://user-images.githubusercontent.com/46109936/126047674-eef03e13-d7e4-4d41-9e43-3e26f03f3491.png)

   The URL below would be unique to your camera. A google search should be able to help you find it. 
   Here is some info on Dahua cams: https://dahuawiki.com/Remote_Access/RTSP_via_VLC
   
      URL: rtsp://<CamOrNVR_RTSP_URL>
      Username: <CamOrNVR Authentication Username>
      Password: <CamOrNVR Authentication Pass>
      Camera: UDP or TCP should work if the option is presented
   
   ![image](https://user-images.githubusercontent.com/46109936/126047711-63d529cc-9208-4c62-9f83-4077c28cae7e.png)

   #### Change Camera Settings:
   ##### Video Device:
      Camera Name: <Something Descriptive>
      Video Resolution: <Match your Cam/NVR Resolution>
      Frame Rate: <25 is a good Start. Match your Cam/NVR if Possible>
   
   ![image](https://user-images.githubusercontent.com/46109936/126047749-4ab69959-9c84-4533-98af-8336ca79c55d.png)

   ##### Motion Detection:
      Auto Threshold Tuning: ON  (You can manually tune this if Auto doesnt work for you)
      Motion Gap: 10 (Adjust this depending on how much movement this camera sees)
      Captured Before/After: 1
   
   ![image](https://user-images.githubusercontent.com/46109936/126047764-f479a734-37d8-426e-8dd3-48b79245bd1d.png)

   ##### Motion Notification:
      Call a Web Hook: http://homeassistant.local:8123/api/webhook/<camera_name>_motion  (This needs to match the webhook set in Home Assistant Automations)
      HTTP Method: Post (form)
   
   ![image](https://user-images.githubusercontent.com/46109936/126047786-9a7f8667-659e-4b81-80ce-3d7c7536028d.png)

   ### Home Assistant Configuration
   * You will need to restart Home Assistant after each camera is added to MotionEye in order for the relevant entities to pull through.
     
     To restart: Configuration>Server Controls>Restart
     
     ![image](https://user-images.githubusercontent.com/46109936/126047984-3dd0f2f4-04f3-4223-973b-e1f7e23529fe.png)

   
   * Your camera entities should now show up under: Developer Tools>Current Entities>[search for camera]

     ![image](https://user-images.githubusercontent.com/46109936/126048013-ac4c89e8-edb0-4733-92ab-c2530d57ffe6.png)

   * Note Down these camera entities, and add them to the configuration.yaml, under **image_processing**

     ![image](https://user-images.githubusercontent.com/46109936/126048052-70e8fc2a-5d27-4a5c-bfdb-9630e2c7927f.png)
     
   * Save this file(ctrl+s on your keyboard)
   * Head over to Configuration>Server Controls, and hit Check Configuration. Address any issues with your config if its raised.

     ![image](https://user-images.githubusercontent.com/46109936/126048066-e3ce943e-47bb-44c8-a045-354b1ae49cf5.png)

   * If your Config is Valid, Hit the restart button, and wait for Home assistant to restart.
   * Once HA has restarted, head back to Developer Tools>Current Entities. You should see a **image_processing** entity for each camera you have added. If not, double check your configuration for incorrect capitalization or spelling.

   ![image](https://user-images.githubusercontent.com/46109936/126048091-c5d8826a-ba06-48ac-8d9e-9efad66832d3.png)

   
   ### Home Assistant Automation Updates
   These base automations handle image processing upon motion detection, as well as sending a notification, and creating a backup of the image if an object was detected.
   
   You can either clone and edit these automations using the Visual Studio Code editor included(note the comments in these files) if you are comfortable with yaml syntax, or you can edit/clone these automations using Home Assistants UI. 
   
   I will make use of the UI for this guide.
   
   * There are 3 base Automations that handle Object Detection:

     ![image](https://user-images.githubusercontent.com/46109936/126048108-c38aadbd-fc7a-484f-8830-df58fd8d24a3.png)

   
     **Object Detection,< Camera Name >:** This Automation gets triggered by the webhook set up in MotionEye for the relevant camera. It Calls **image_processing(DOODS)** which analyzes the image, and see if any of the objects(labels) stated in the configuration.yaml are found.
   
     **Notification, < Camera Name >,Car Detected:** This Automation is triggered when the **image_processing** entity related to the specific Camera returns **car** or **truck**. It then sends a notification to the specified device(in this case, just Home Assistant), as well as copies the processed image to the shared folder with a date tag.
     
     **Notification, < Camera Name >, Person Detected:** Same as above, but just for a person.
   
   * To clone an Automation go to Configuration>Automation>[ Edit on Relevant Automation ]>[3Dots top right hand corner]>Duplicate Automation

     ![image](https://user-images.githubusercontent.com/46109936/126048142-c4791eba-ead5-4140-8b99-aa9a29c66a50.png)
     
     ![image](https://user-images.githubusercontent.com/46109936/126048151-7e7b2c85-e999-4e66-8c76-532b5982649e.png)


   * Update the Automation Name, Description, Trigger and Action to the entities relevant to the camera you are targeting.
   
     Example: If you are cloning Object Detection for using with a camera called Frontdoor:
   
         Trigger:
         
            Trigger ID: frontdoor_motion
   
            Webhook ID: frontdoor_motion
   
            
         Action:
            Image Processing Target: Doods frontdoor
            Notification Message: Motion Detected At Front Door
            
## Additional Info:
#### Phone Notifications: https://www.youtube.com/watch?v=v8fcwhko1k4
#### Telegram Notifications: https://www.youtube.com/watch?v=7D6-IP_qKEw
#### License Plate Detection: https://www.youtube.com/watch?v=t-XxCrdj_94
HACS as well as Plate Recognition integration is already installed. You just need to add HACS integration by going to: Configuration>Integration>Add Integration>[ Search HACS ]
   
