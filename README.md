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

# Azure Services
## Creating the Classification Model
The [Azure Custom Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/?WT.mc_id=devto-blog-dglover)
 service is a simple way to create an image classification machine learning model without having to be a data science or machine learning expert. You simply upload multiple collections of labelled images. For example, you could upload a collection of banana images and label them as 'banana'. In this demo we create the people modules. So you need to create your own module before building this demo project.

To create your own classification model read [How to build a classifier with Custom Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier?WT.mc_id=devto-blog-dglover) for more information. It is important to have a good variety of labelled images so be sure to read [How to improve your classifier](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/getting-started-improving-your-classifier?WT.mc_id=devto-blog-dglover).

## Exporting an Azure Custom Vision Model
This "Image Classification" module includes a simple fruit classification model that was exported from Azure Custom Vision. For more information read how to [Export your model for use with mobile devices](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/export-your-model?WT.mc_id=devto-blog-dglover). It is important to select one of the "compact" domains from the project settings page otherwise you will not be able to export the model.

Follow these steps to export your Custom Vision project model.
1. From the Performance tab of your Custom Vision project click Export.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/exportmodel.png)

2. Select Dockerfile from the list of available options

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/export-as-docker.png)

3. Then select the Linux version of the Dockerfile.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/export-choose-your-platform.png)

4. Download the docker file and unzip and you have a ready-made Docker solution with a Python Flask REST API.

## Azure Speech Services
[Azure Speech Services](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/overview/?WT.mc_id=devto-blog-dglover) supports both "speech to text" and "text to speech". For this solution, I'm using the text to speech (F0) free tier which is limited to 5 million characters per month. You will need to add the Speech service using the Azure Portal and "Grab your key" from the service.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Speech_Key01.png)

Open the deployment.template.json file and update the BingKey with the key you copied from the Azure Speech service.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Speech_Key02.png)

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


# How to build docker image and deploy to edge device

1. Clone this GitHub repository.
git clone https://github.com/godskill0728/Azure-Custom-Vision.git

2. Install the Azure IoT Edge runtime on your Linux desktop or device (eg Raspberry Pi).
Follow the instructions to [Deploy your first IoT Edge module to a Linux x64 device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux?WT.mc_id=devto-blog-dglover).

3. Install the following software development tools.
	+ [Visual Studio Code](https://code.visualstudio.com/)
	+ Plus, the following Visual Studio Code Extensions
		+ [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
		+ [JSON Tools](https://marketplace.visualstudio.com/items?itemName=eriklynd.json-tools) useful for changing the "Create Options" for a module.

4. With Visual Studio Code, open the IoT Edge solution you cloned from GitHub to your developer desktop.

# Projhect Atchitecture
1. There are two modules: CameraCaptureOpenCV and ImageClassifierService.
2. The module.json file defines the Docker build process, the module version, and your docker registry. Updating the version number, pushing the updated module to an image registry, and updating the deployment manifest for an edge device triggers the Azure IoT Edge runtime to pull down the new module to the edge device.
3. The deployment.template.json file is used by the build process. It defines what modules to build, what message routes to set up, and what version of the IoT Edge runtime to run.
4. The deployment.json file is generated from the deployment.template.json and is the [Deployment Manifest](https://docs.microsoft.com/en-us/azure/iot-edge/module-composition?WT.mc_id=devto-blog-dglover)
5. The version.py in the project root folder is a helper app you can run on your development machine that updates the version number of each module. Useful as a change in the version number is what triggers Azure IoT Edge runtime to pull the updated module and it is easy to forget to change the module version numbers
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Project_Architecture.png)

# Building the Solution
You need to ensure the image you plan to build matches the target processor architecture specified in the deployment.template.json file.
1. Specify your Docker repository in the module.json file for each module. If you are using a supported Linux Azure IoT Edge Distribution, such as Ubuntu 16.04 as your development machine and you have Azure IoT Edge installed locally then I strongly recommend setting up a Azure Container Register space. It will significantly speed up your development, deployment and test cycle.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Container_Registry.png)

Please refer [How to create Azure Container Registry](https://docs.microsoft.com/zh-tw/azure/container-registry/container-registry-get-started-portal) to create your own Azure Container Registry.

2. We will build our own docker images and upload to Azure Container Registry. Then we will deploy these docker images into target device.
3. Confirm processor architecture you plan to build for. From the Visual Studio Code bottom bar click the currently selected processor architecture, then from the popup select the desired processor architecture.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Processor_Architecture.png)

4. Next, Build and Push the solution to Docker by right mouse clicking the deployment.template.json file and select "Build and Push IoT Edge Solution". The first build will be slow as Docker needs to pull the base layers to your local machine. If you are cross compiling to arm32v7 then the first build will be very slow as OpenCV and Python requirements need to be compiled. On a fast Intel i7-8750H processor cross compiling this solution will take approximately 40 minutes.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Build_Solution.png)

# Deploying the Solution
When the Docker Build and Push process has completed select the Azure IoT Hub device you want to deploy the solution to. Right mouse click the deployment.json file found in the config folder and select the target device from the drop-down list.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Deployment.png)

# Monitoring the Solution on the IoT Edge Device
Once the solution has been deployed you can monitor it on the IoT Edge device itself using the iotedge list command.

``` iotedge list ```

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/iotedge_list.png)

# Monitoring the Solution from the Azure IoT Edge Blade
You can monitor the state of the Azure IoT Edge module from the Azure IoT Hub blade on the Azure Portal.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Hub_Device.png)

Click on the device from the Azure IoT Edge blade to view more details about the modules running on the device.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/IoT_Hub_Module.png)

# Result
When the solution is finally deployed to the IoT Edge device the system will start telling you what items it thinks have been scanned.
