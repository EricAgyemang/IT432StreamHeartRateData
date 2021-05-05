# INTRODUCTION
# IT432 Stream Heart Rate Data
This is an example of how to build a data pipeline that starts with an Internet of Things (IoT) device that captures the heart rate, leverage IoT core to securely publish the data to a message queue where it will then be transported into a data warehouse. Here, a Raspberry Pi with a heart rate sensor is used for the IoT device and components of Google cloud platform will form the data pipeline.

This example is not an officially supported Google product, does not have a SLA/SLO, and should not be used in production


# SECTION 1
# Supporting Hardware Targets
The following are currently supported hardware targets:
* Raspberry Pi 3 Model B with power supply 
* SD memorry card and case
* USB card reader
* Female-to-male breadboard wires
* Polar T34 Heart Rate Transmitter and Polar Heart Rate receiver
* A computer monitor or TV with HDMI input, HDMI cable, Keyboard and a mouse.
* A Google Cloud Platform account (New users are eligible for a $300 free trial)
* Gmail account

-------------
*Note:*

      1. The Raspberry Pi Zero W is recommended for this codelab but we use Raspberry Pi 3 Model B instead, which also works.
      2. The Raspberry Pi Zero W require the programmer to have GPIO Hammer Headers or USB hub(to allow for connecting a keyboard and mouse into the USB port ib the Raspberry Pi), and soldering iron with solder



# SECTION 2
# Getting Set Up
  - Environment Setup
    * Sign-in to [Google Cloud Platform console](http://console.cloud.google.com/) 
    * You can use the default project ("My First Project") or create a new project by using [Manage resources page](https://console.cloud.google.com/cloud-resource-manager) eg: iot-heartrate. The Project ID must be unique name accross all Google Cloud projects
 
  - Enable APIs
    * This project uses the **IoT Core, Pub/Sub, Dataflow, and Compute Engine**, so enable the Cloud IoT API by opening the [Google Cloud IoT Core console](http://console.cloud.google.com/iot/).
    * Repeat this process for the **Pub/Sub, Dataflow, and Compute Engine** by clicking the **Enable API** tab.
    
     
# SECTION 3
# Create a BigQuery Table
* BigQuery is a serverless, highly scalable, low cost enterprise data warehouse to store data being stream from IoT devices.
* Create a table that will hold all of the IoT heart rate data.
    * From the **cloud console** select **BigQuery** > Click on the down arrow icon next to project name > select **Create new dataset** > **Enter heartRateData** > click **Ok**.
    * Click **"+"** sign next to your Dateset to create a new table
    * From **Source Data**, select **Create empty table**. 
    * For Destination table name, enter heartRateDataTable.
    * Under **Schema**, click the **Add Field** botton until there are a total of 4 fields. Fill in the fields by making sure to also select the appropriate **Type** for each field. 
    * Next, Click on the **Create Table** button.

----------------------------
*Note:* 
 
      1. Any spelling errors for the field names or the table name can cause issues when Cloud Functions attempts to add data later in this codelab.
      2. If you did make a mistake in creating the table, hover over the table name of the left side if the window, click the down arrow icon > click on Delete Table. 
      3. Repeate the steps in SECTION 3 above to create the table.
    
    
 # SECTION 4
 # Create a Pub/Sub Topic and Subscription
   Cloud Pub/Sub is a simple, reliable, scalable foundation for streaming data and event-driven computing systems. This will be used to pull IoT messages from.
   * From the [Google Cloud Console](https://console.cloud.google.com), select **Pub/Sub** > **Topics** > **Create a topic** > enter **"heartratedata"** as a topic name > click **Create**.
   * To allow other google cloud services to access those messages, we create a subscription. Clicking on the three dot menu to the right of the topic that was created > click on the **" :heavy_plus_sign: New subscription"** > enter **"heartratedata"** as subscription name > pull > click on **Create**.
 
    
# SECTION 4 
# Dataflow Template
A Dataflow template is used to create a process that monitors a Pub/Sub topic for incoming messages and then move messages to the BigQuery.
To create a Dataflow: 
* from the [Google Cloud Console](https://console.cloud.google.com), select Storage > **Browser.** > click **Bucket** button > **Create bucket**.
* Choose a name for the storage bucket (must be globally unique accross all Google Cloud).
* Select **Reginal** and make sure it is the sane region as the rest of your project's services > choose Location.
* Click **Create** button.
* Select **Dataflow > Create Job from Template > enter a job name** > select **Pub/Sub Subscription to BigQuery template**
* The reginal endpoint must match where the rest of the project resources are located. 
* Fill in the remainder of the field and make sure they are align with the name of your storage bucket, Pub/Sub subscription, and BigQuery dataset and tablename.
* Click on **Run job**

    
# SECTION 5
# Create an IOT Core registry
The cloud IoT Core is a fully managed service that allows the user to connect, manage, and ingest data from millions of global dispersed device. 
From the [Cloud Console](https://console.cloud.google.com),
* Select **Cloud IoT Core** > click on **device registry** > enter "heartrate" as the **Rsgistry ID**.
* Select a **Region** close to you > disable HTTP protocol > select Pub/Sub topic that was created in Section 4 above.
* Check the **MQTT** in the **Protocol** section. 
* Click on **Create** button.
    
# Assemble Raspberry Pi and Heart Rate Reciever
  * If there are more than three pins, trim the header down to only three pins.
  * Solder the header pins to the sensor board.
  * Insall the hammer header pins into the Raspberry Pi.
  * Format the SD card and install the New Out Of Box Softwafare (NOOBS) installer following the steps [here](https://www.raspberrypi.org/documentation/installation/noobs.md).
  * Insert the SD card into the Raspberry Pi(I recommend SD card with 32 GB).
  * Place the Raspberry Pi into its case.
  * Use the breadboard wires to connect the heart rate receiver to the Raspberry Pi.
 
 
  Raspberry Pi pin  | Receiver Connection 
  ------------------|--------------------
  Pin 16 (GPIO23)   | <Not labelled>
  Pin 17 (3V3)      | <Not labelled>
  Pin 20 (GND)      | GND
                    
    
