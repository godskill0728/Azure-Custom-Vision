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
26.  You can use tags to target groups of devices for module deployment. Add a tag value to the Initial Device Twin State if you'd like.
For example:

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Link_Device04.png)

Note. The ESRP-CSS-UNOxxxx value is for the ﬁrmware update in Device Management section. 
