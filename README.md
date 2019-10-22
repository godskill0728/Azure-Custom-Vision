# Connect Edge Device and Azure
1. We need to connect your physical device with a device identity that exists in an Azure IoT Hub. An overview of the process is showed in below picture.
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Azure_Device_Connecting.png)
2. Open terminal, type below command. Copy TPM RegistrationID and TPM Endorsement Key for later use. 
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/TPM_Number.png)
3. Log in to the Azure portal. 
4. Click the Create a resource button found on the upper left-hand corner of the Azure portal. 
5. Search the Marketplace for the IoT Hub.
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Hub.png)
6. Select IoT Hub and click Create. You see the first screen for creating an IoT Hub.
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_IoT_Hub01.png)
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_IoT_Hub02.png)
7. Click Create to create your new IoT Hub. Creating the hub takes a few minutes.
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_IoT_Hub03.png)
8. Now we need to create DPS (Device Provisioning Service)
9. Click the Create a resource button found on the upper left-hand corner of the Azure portal. 
10. Search the Marketplace for the Device Provisioning Service. 
11. Select IoT Hub Device Provisioning Service and click the Create button.
 ![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_DPS01.png)
12. Provide the following information for your new Device Provisioning Service instance and click Create.
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_DPS02.png)
13. Then we will add a configuration to the Device Provisioning Service instance. This configuration sets the IoT Hub for which devices will be provisioned. 
14. Click the All resources button from the left-hand menu of the Azure portal. Select the Device Provisioning Service instance that you created in the preceding section. 
15. On the Device Provisioning Service summary blade, select Linked IoT Hubs. Click the + Add button seen at the top.
16. On the Add link to IoT Hub page, provide the following information to link your new Device Provisioning Service instance to an IoT Hub. Then click Save. 
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Link_Device01.png)
17. Now you should see the selected hub under the Linked IoT hubs blade. You might need to click Refresh to show Linked IoT hubs. 
18. In the Azure portal, and navigate to your instance of IoT Hub Device Provisioning Service.
19. Under Settings, select Manage enrollments.
20. Select Add individual enrollment.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Link_Device02.png)

21. For Mechanism, select TPM.
22. Insert the Endorsement key and Registration ID that you copied from tpm_device_provision command.
23. Select Enable to declare that this virtual machine is an IoT Edge device.
24. Choose the linked IoT Hub that you want to connect your device to.
25. Provide an ID for your device if you'd like. You can use device IDs to target an individual device for module deployment.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Link_Device03.png)

26. You can use tags to target groups of devices for module deployment. Add a tag value to the Initial Device Twin State if you'd like.
For example:

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Link_Device04.png)

Note. The ESRP-CSS-UNOxxxx value is for the ﬁrmware update in Device Management section. 

27. Go back to edge device, open a terminal and run iotedgecli.  

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Edge00.png)

28. Choose dps. Then enter ID scope and Registration ID.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Edge01.png)

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Edge02.png)

29. If below screen show up, means the IoT Edge Service is strated. 

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Edge03.png)

30. Run the following command to check if IoT edge runtime is running normally. If there are any error, please follow the Common issues and resolutions for Azure IoT Edge for trouble shooting. 

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Edge04.png)

Note: The tpm_device_provision command can’t be execute when iotedge service is running. If below screen appears, please stop iotedge service first. And start it later. 

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Edge05.png)

# Custom Vision Demo
The architecture about this demo is as below:
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Demo_Architecture.png)
1. The Camera Capture Module handles scanning items using a camera. It then calls the Image Classification module to identify the item, a call is then made to the "Text to Speech" module to convert item label to speech, and the name of the item scanned is played on the attached speaker.
2. The Image Classification Module runs a Tensorflow machine learning model that has been trained with images of fruit. It handles classifying the scanned items.
3. The Text to Speech Module converts the name of the item scanned from text to speech using Azure Speech Services.
4. A USB Camera is used to capture images of items to be bought.
5. A Speaker for text to speech playback.
6. Azure IoT Hub (Free tier) is used for managing, deploying, and reporting Azure IoT Edge devices running the solution.
7. Azure Speech Services (free tier) is used to generate very natural speech telling the shopper what they have just scanned.
8. Azure Custom Vision service was used to build the model used for image classification.

# How to build docker image and deploy to edge device

1. Clone this GitHub repository.
git clone https://github.com/godskill0728/Azure-Custom-Vision.git

2. Install the Azure IoT Edge runtime on your Linux desktop or device (eg Raspberry Pi).
Follow the instructions to Deploy your first IoT Edge module to a Linux x64 device.

3. Install the following software development tools.
	+ [Visual Studio Code](https://code.visualstudio.com/)
	+ Plus, the following Visual Studio Code Extensions
		+ [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
		+ [JSON Tools](https://marketplace.visualstudio.com/items?itemName=eriklynd.json-tools) useful for changing the "Create Options" for a module.

4. With Visual Studio Code, open the IoT Edge solution you cloned from GitHub to your developer desktop.
