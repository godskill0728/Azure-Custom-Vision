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
10. Search the Marketplace for the Device Provisioning Service. Select IoT Hub Device Provisioning Service and click the Create button.

![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_DPS01.png)

11. Provide the following information for your new Device Provisioning Service instance and click Create.
![image](https://github.com/godskill0728/Azure-Custom-Vision/blob/master/docs/Create_DPS02.png)
