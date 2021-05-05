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
   * To allow other google cloud services to access those messages, we create a subscription. Clicking on the three dot menu to the right of the topic that was created > click on the **":ballot_box_with_plus_sign:New subscription"** > enter **"heartratedata"** as subscription name > pull > click on **Create**.
 
    
    
    
    
    
    
    
    
