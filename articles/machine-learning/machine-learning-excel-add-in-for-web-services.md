<properties
    pageTitle="Excel add-in for Machine Learning Web services | Microsoft Azure"
    description="How to use Azure Machine Learning Web services directly in Excel without writing any code."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel Add-in for Azure Machine Learning Web services

Excel makes it easy to call Web services directly without the need to write any code.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Steps to Use an Existing Web service in the Workbook

1. Open the [sample Excel file](http://aka.ms/amlexcel-sample-2), which contains the Excel add-in and data about passengers on the Titanic.
2. Choose the Web service by clicking it - "Titanic Survivor Predictor (Excel Add-in Sample) [Score]" in this example.

    ![Select Web service][01]

3. This will take you to the **Predict** section.  This workbook already contains sample data, but for a blank workbook you can select a cell in Excel and click **Use sample data**.
4. Select the data with headers and click the input data range icon.  Make sure the "My data has headers" box is checked.
5. Under **Output**, enter the cell number where you want the output to be, for example "H1" here.
6. Click **Predict**.

    ![Predict section][02]

## <a name="steps-to-add-a-new-web-service"></a>Steps to Add a New Web service

Deploy a Web service or use an existing Web service. For more information on deploying a Web service, see [Walkthrough Step 5: Deploy the Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).

Get the API key for your Web service. Where you do this depends on whether you published a Classic Machine Learning Web service of a New Machine Learning Web service.

**Use a Classic Web service** 

1. In Machine Learning Studio, click the **WEB SERVICES** section in the left pane, and then select the Web service.

    ![Studio select a Web service][04]

2. Copy the API key for the Web service.

    ![Studio API key][05]

3. On the **DASHBOARD** tab for the Web service, click the **REQUEST/RESPONSE** link.
4. Look for the **Request URI** section.  Copy and save the URL.

>[AZURE.NOTE] It is now possible to sign into the [Azure Machine Learning Web Services](https://services.azureml.net) portal to obtain the API key for a Classic Machine Learning Web service.

**Use a New Web service**

1. In the [Azure Machine Learning Web Services](https://services.azureml.net) portal, click **Web Services**, then select your Web service. 
2. Click **Consume**.
3. Look for the **Basic consumption info** section. Copy and save the **Primary Key** and the **Request-Response** URL.


## <a name="steps-to-add-a-new-web-service"></a>Steps to Add a New Web service

1. Deploy a Web service or use an existing Web service. For more information on deploying a Web service, see [Walkthrough Step 5: Deploy the Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).
2. Click **Consume**.
3. Look for the **Basic consumption info** section. Copy and save the **Primary Key** and the **Request-Response** URL.
2. In Excel, go to the **Web Services** section (if you are in the **Predict** section, click the back arrow to go to the list of Web services).

    ![Go to Web service selection][03]
    
3. Click **Add Web Service**.
4. Paste the URL into the Excel add-in text box labeled **URL**.
5. Paste the API/Primary key into the text box labeled **API key**.
6. Click **Add**.

    ![URL and API key for a classic Web service.][06]

10. To use the Web service, follow the preceding directions, "Steps to Use an Existing Web Service."

## <a name="sharing-your-workbook"></a>Sharing Your Workbook

If you save your workbook, then the API/Primary key for the Web services you have added is also saved. That means you should only share the workbook with individuals you trust.

Ask any questions in the following comment section or on our [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
