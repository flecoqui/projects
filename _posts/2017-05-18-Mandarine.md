---
layout: post
title:  "Enhancing Online Training services with Azure Media Indexer, Cognitive Services and Microsoft Bot Framework"
author: "Fred Le Coquil"
author-link: "https://www.github.com/flecoqui"
#author-image: "{{ site.baseurl }}/images/authors/flecoqui.jpg"
date:   2017-02-14
categories: [Cognitive Services]
color: "blue"
#image: "{{ site.baseurl }}/images/mandarine_logo.png" #should be ~350px tall
excerpt: This article describes how a Web site has been updated to support automated multi-languages substiles generation for training videos and to personalize the journey of each user amongst the catalog of MOOCs.
language: English
verticals: Training Services
geolocation: France
permalink: https://microsoft.github.io/techcasestudies/mandarine.html
---

Microsoft teamed up with Mandarine Academy, a Microsoft's partner, delivering online training services to update the current Mandarine's backend to support:</p>
- an automated multi-languages subtitles generation for training videos</p>
- a personalized journey for each user amongst the catalog of MOOCs</p>

The solution relies on :</p>
- [Azure Media Services Indexer](https://docs.microsoft.com/en-us/azure/media-services/media-services-process-content-with-indexer2) : Used to generate the subtitles in the Speech language of the videos </p>
- [Cognitive Services - Text Translator APIs](https://azure.microsoft.com/en-gb/services/cognitive-services/translator-text-api/) : Used to translated the original subtitles in other languages subtitles</p>
- [Azure Search](https://azure.microsoft.com/en-us/services/search/) : Used to index subtitles in different languages</p>
- [Microsoft Bot Framework](https://dev.botframework.com/) : Used to implement Mandarine Academy Bot (called Mike) and supporting Web Chat and Skype connectors </p>
 

 ![Team](/images/2017-05-18-Mandarine/picture2.png)


Mandarine's backend is currently based on:</p>
- Ubuntu virtual Machines running in Azure</p>
- Database: MySQL Server</p>
- Web Server: Apache, PHP, Symfony, Node JS</p>


**Core Team:** </p>
- Florent Petit - CTO, Mandarine Academy</p>
- Alain Damanti - Media Team Laeder, Mandarine Academy</p>
- Rachid Hammaoui - Engineering Team Leader, Mandarine Academy</p>
- Eddy Guerin - Software Engineer, Mandarine Academy</p>
- Fred Le Coquil - Technical Evangelist, Microsoft</p>
- Pierre-Louis Xech - Technical Evangelist, Microsoft</p>
- Benoit Le Pichon - Technical Evangelist, Microsoft</p>

 ![Team](/images/2017-05-18-Mandarine/picture1.png)
 ![Team](/images/2017-05-18-Mandarine/picture3.png)


## Customer profile ##

**Mandarine Academy**

 ![Team](/images/mandarine_logo.png)

 [Mandarine Web Site](https://mandarine.academy/en/)
 
 Mandarine Academy guides companies in their digital transformation. From free MOOC services to personalized corporate serivdes, Mandarine Academy provides the best tailored services to train their employees. The solution is based on a combination of the latest technologies, such as video conferencing, web conferencing, e-learning and machine learning.

 ![Team](/images/2017-05-18-Mandarine/mandarine.png)
 
## Problem statement ##

Mandarine Academy has already published several hundred of MOOCs, they would like to add subtitles (SRT, WEBVTT, TTML) for each MOOC.
Azure Media Indexer V2 seems the best approach and easiest approach to generate the subtitles directly from the original video.
Once the native subtitles are available in the native language, using Cognitive Services Text Translator API, it’s possible to generate subtitles in different languages.
Each MOOC Video will support up-to 8 subtitles languages. 
For the first phase of this project, Mandarine won't use Adaptiuve Streaming protocol like Smooth Streaming, Dash or HLS. The MOOC Videos will be MP4 files delivered to the clients  in  progressive download mode.
Azure Media Services Streaming End Point won't be used during this phase, the application will only use Azure Media Services SAS Locator to delvier the videos.
As today the generation of subtitles can't be fully automated, after each step, the Mandarine operator needs to check the generated subtitles:
- subtitles generated with Azure Media Services Indexer V2
- subtitles generated with Cognitive Services Text Translator API  
After each step, the Manadarine operator must be able to update manually the subtitles if necessary '

Beyond the enhancement of their video content, Mandarine wants to improve the interactivity of their current Web Site. They want to personalize the journey of each user amongst the catalog of MOOCs. 
For instance, they want to automatically refine the profile of each user.
Their Bot (called Mike) will ask questions to complete the profile of each user.
The Bot will also send notifications to users for instance when they are about to complete a level of training. 
The Bot will address the subscribers connected to Mandarine Web Site and to Skype. 

 
## Solution and steps ##

### Overall Architecture

As the main objectif of this initiative is to enhance the existing Mandarine services, the architecture of this deployment will take into account all the legacy components technologies used by Mandarine services.
For instance, the database server is based on MySQL Server and this server is hosted in Mandarine's datacenter. This training video files are hosted on a third party streaming platform.</p>
On the technologies side, the servers are running Ubuntu, the Web Servers are based on Apache/PHP/Symfony. The backend services are developped using Node.js.THe Web Servers will be hosted in Azure. 
The client application is actually a Web Application running in any browsers.

Beyond those legacy components the following Azure Serivces will be deployed:
1. Azure Azure Media Services </p>
2. Azure Cognitive Services Text Translator key</p>
3. Azure Search Services Account </p>
4. Azure Web App </p>
5. Microsoft Bot Framework (Node.js) </p>
6. Azure Virtual Machines running the Ubuntu and Apache/PHP/Symfony. </p>

Below the architecture of Manadarine services using Azure:

 ![Team](/images/2017-05-18-Mandarine/0-architecture.png)


### Subtitles generation 

#### Installing the bakcend services in Azure:

In order to generate the subtitles you need to install the Azure backend with all the services associated.
You can either use the Azure Portal to deploy manually all the Azure Services:

https://portal.azure.com
 
or you can use directly the Azure Resource Manager template available there:

https://github.com/flecoqui/azure/tree/master/azure-quickstart-templates/101-media-search-cognitive

This template allows you to deploy  a Web App, an Azure Media Services Account, an Azure Search Account and an Azure Text Translator service in the same region as the resource group.
As Azure Media Services, Search Service and Cognitive Services are not deployed in all regions, it's recommanded to use the following regions:
West US, West Europe,Southeast Asia,West Central US 
This template is associated with Windows Application which is used to generate automatically video subtitles in different languages fron an orginal video or audio file. Once generated the subtitles are stored in Azure Search to allow the users to find all the videos associated with a specific key word.
This Application called TestAzureMediaIndexer is available there:
https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer 


![](/images/2017-05-18-Mandarine/1-architecture.png)

Using the two Azure CLI command lines below, you can deploy automatically all the Azure Services required for the Mandarine deployment: 

    azure group create testamsseacog northeurope
	azure group deployment create testamsseacog depiperftest -f azuredeploy.json -e azuredeploy.parameters.json -vv

Using the following parameters:

Name prefix which will be used to create Azure Media Serivces Account, Azure Storage Account,  Azure Search Account, Azure Cognitive Services Text Translation Account:

    "namePrefix": {
      "type": "string",
      "minLength": 2,
      "maxLength": 50,
      "metadata": {
        "description": "Service name prefix must only contain lowercase letters, digits or dashes, cannot use dash as the first two or last one characters, cannot contain consecutive dashes, and is limited between 2 and 50 characters in length."
      }

Azure Storage SKU associated with Azure Media Services, used to store video and audio files:

    "mediaStorageSku": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "This is  Storage Account SKU associated with Azure Media Services"
      }
    },

Azure Search SKU:

    "searchSku": {
      "type": "string",
      "defaultValue": "free",
      "allowedValues": [
        "free",
        "basic",
        "standard",
        "standard2",
        "standard3"
      ],
      "metadata": {
        "description": "The SKU of the search service you want to create. E.g. free or standard"
      }
    },

Azure Cognitive Services SKU:

    "cognitiveSku": {
      "type": "string",
      "defaultValue": "S1",
      "allowedValues": [
        "F0",
        "P0",
        "P1",
        "P2",
        "S0",
        "S1",
        "S2",
        "S3",
        "S4",
        "S5",
        "S6"
      ],
      "metadata": {
        "description": "The SKU of the search service you want to create. E.g. free or standard"
      }
    },

Azure Web App SKU:

    "webSku": {
      "type": "string",
      "defaultValue": "F1",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "metadata": {
        "description": "The SKU of the Web service you want to create. E.g. free or standard"
      }
    }


Once the backend services are installed, the storage account where the video files and subtiles files will be stored needs to be manually configured to support CORS (Cross-Origin Resource Sharing) using the Azure portal:

1. Select the Storage Account of your new resource group:</p>
![](/images/2017-05-18-Mandarine/cors-0.png)
2. Select CORS on the page of your Storage Account</p>
![](/images/2017-05-18-Mandarine/cors-1.png)
3. Click on the button Add to Add a CORS rule. Enter  * for **Allowed origins**, GET,POST and PUT for **Allowed verbs**, * for **Allowed headers**, * for **Exposed headers** and 5 for **Maximum age**.
Click on the Add button to Create the new rule.</p>
![](/images/2017-05-18-Mandarine/cors-2.png)
4. Click on the Add button to Create the new rule. Once the rule is created the Web Player will be able to play audio/video files and subtitles files stored on the Storage Account.</p>
![](/images/2017-05-18-Mandarine/cors-3.png)

Moreover, the media files will be streamed from SAS locators which returns "Accept-Ranges: bytes" in the http headers. This http header is mandatory to play MP4 video files or MP3 audio files over http with Chrome browser.

#### Testing the backend services with the applicaiton TestAzureMediaIndexer:

This template is associated with Windows Application which is used to generate automatically video subtitles in different languages fron an orginal video or audio file. Once generated the subtitles are stored in Azure Search to allow the users to find all the videos associated with a specific key word.
This Application called TestAzureMediaIndexer is available there:
https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer 

**Prerequisite: Visual Studio 2015 or 2017**

1. If you download the samples ZIP, be sure to unzip the entire archive, not just the folder with the sample you want to build. 
3. Start Microsoft Visual Studio 2015 or 2017 and select **File** \> **Open** \> **Project/Solution**.
3. Starting in the folder where you unzipped the samples, go to the Samples subfolder, then the subfolder for this specific sample. Double-click the Visual Studio 2015/2017 Solution (.sln) file.
4. Press Ctrl+Shift+B, or select **Build** \> **Build Solution**.

**Deploying and running the sample**
1.  To debug the sample and then run it, press F5 or select **Debug** \> **Start Debugging**. To run the sample without debugging, press Ctrl+F5 or select **Debug** \> **Start Without Debugging**.

**Downloading the binary**
The binary associted with the application is available there:
[ZIP file with the Application](https://github.com/flecoqui/azure/master/Samples/TestAzureMediaIndexer/Releases/Latestrelease.zip)

1. You can download the zip file.</p>
2. Unzip the LatestRelease.zip file</p>
3. Run locally TestAzureMediaIndexer.exe</p>

## Using the application TestAzureMediaIndexer
This sample application is a basic Windows Application with one single page:

![](/images/2017-05-18-Mandarine/ui-0.png)


#### Connect the application to the Azure Backedn
In order to use the application you need to provide the following paramters to establish a connection with your backend in Azure:
1. The Azure Media Serivces Account name</p>
2. The Azure Media Serivces Account key</p>
3. The Azure Cognitive Services Text Translator key</p>
4. The Azure Search Serivces Account name</p>
5. The Azure Search Serivces Account key</p>
6. The url of the Web Player application hosted on the Web site of your backend, the url should be close to this format: http://YourWebAppName.azurewebsites.net/player.html

![](/images/2017-05-18-Mandarine/1-architecture-step-1.png)

You can retrieve all the parameters below from the [Azure Portal:](https://portal.azure.com) </p>
1. The Azure Media Serivces Account name</p>
2. The Azure Media Serivces Account key</p>
3. The Azure Cognitive Services Text Translator key</p>
4. The Azure Search Serivces Account name</p>
5. The Azure Search Serivces Account key</p>
6. The url of the Web Player application hosted on the Web site </p>

Follow the steps below to read the account names and the keys: </p>

1. On the Azure portal, select the new Resource Group associated with the backend services. Select the service (Media, Search, Cognitive) you want to read the keys.</p>
![](/images/2017-05-18-Mandarine/mediasearchcognitive.png)
2. For Azure Media Services, select the **Account Keys** menu to display the Account name and the keys.</p>
![](/images/2017-05-18-Mandarine/media-key.png)
3. For Azure Search Services, select the **Keys** menu to display the Account name and the keys.</p>
![](/images/2017-05-18-Mandarine/search-key.png)
4. For Cognitive Services, select the **Keys** menu to display the keys.</p>
![](/images/2017-05-18-Mandarine/cognitive-key.png)

Once all the parameters are ready click on the button "Connect" to establish the connection with your backend:
![](/images/2017-05-18-Mandarine/ui-1.png)

#### Updload the audio and video assets
Once the applicaiton is connected, the first step consists in uploading video or audio assets on your backend in Azure.

![](/images/2017-05-18-Mandarine/1-architecture-step-2.png)

1. Click on the **Add Asset** button</p>
2. Select the audio file or video file you want to upload</p>

![](/images/2017-05-18-Mandarine/ui-2.png)

3. Once the file is uploaded, a new asset and a new file is displayed in the lists below:</p>

![](/images/2017-05-18-Mandarine/ui-2-1.png)


#### Generate the subtititles from audio and video assets
Once the video or audio file is uploaded, you can generate the subtitles with Azure Media Services Indexer 

![](/images/2017-05-18-Mandarine/1-architecture-step-3.png)

1. Select the spoken language of your audio or video file</p>
2. Click on the button **Generate subtitle** </p>

![](/images/2017-05-18-Mandarine/ui-3.png)

3. A Job to generate the subtitles is launched in Azure after few minutes, the new subtitle file is available.</p>

![](/images/2017-05-18-Mandarine/ui-3-1.png)


#### Update the generated subtitles with the Web Application 
Once the subtile file is available, it's possible to update the subtitle file. The native format is WEBVTT, but it's still possible to convert a WEBVTT subtitle file into a TTML subtitle file.

![](/images/2017-05-18-Mandarine/1-architecture-step-4.png)

1. Click on the button **Play Video/Subtile** or **Play Audio/Subtile** </p> 
The Windows Application launches the Web Player Application 
![](/images/2017-05-18-Mandarine/ui-4.png)

2. Your default browser on your computer running Windows is displaying a page playing the video or audio file along with the subtitles.</p>

![](/images/2017-05-18-Mandarine/ui-4-1.png)

3. As sometimes the generated subtiles file needs to be updated, you can update each subtitle. Click on the **Pause** button.</p>
4. Update the subtile, click on the button **Save**.</p>

![](/images/2017-05-18-Mandarine/ui-4-2.png)

5. When all the subtitles are updated, you can save the subtile file on your machine in WEBVTT or TTML format.</p>

![](/images/2017-05-18-Mandarine/ui-4-3.png)

6. Once the subtile file is stored locally on your machine, you can update the subtitle file on Azure Storage when clicking on button **Update Subtitle**.</p>

![](/images/2017-05-18-Mandarine/ui-4-4.png)


#### Translated the generated subtitles with Cognitive Services Text Translator
Once the first subtile file associted with your audio or video file are correct, you can generate more subtitle files in different languages.
 
![](/images/2017-05-18-Mandarine/1-architecture-step-5.png)

1. Select the source subtile in the list box **List of Subtitle Assets**.</p>
2. Select the language of your new subtitle file.</p>
3. Click on the button **Translate subtitle** </p>

![](/images/2017-05-18-Mandarine/ui-5.png)

4. After few seconds the new subtile file is displayed in the list box **List of Subtitle Assets**</p>

![](/images/2017-05-18-Mandarine/ui-5-1.png)

5. If you click on the button **Play Video/Subtile** or **Play Audio/Subtile** , you can playback the new subtitles.</p>

![](/images/2017-05-18-Mandarine/ui-5-2.png)

6. Finally, you can download or display the new subtitle file.</p>

![](/images/2017-05-18-Mandarine/ui-5-3.png)


#### Using Azure Search to Index the subtitles files associated with the audio and video assets
Once all the subtitles associated with your video or audio files are generated, you can store the subtitles in the Azure Search service.</p>
 
![](/images/2017-05-18-Mandarine/1-architecture-step-6.png)

1. First you need to create the Index associated with the subtiles. Click on the button **Create Index** to create the Index. This step is only required once. If you want to clear the Azure Search database you can click on **Delete Index** button.</p>

![](/images/2017-05-18-Mandarine/ui-6.png)

2. Once the Index is created, select the subtile file you want to import and click on the button **Populate the Index with subtiles**. You can repeat this step for each subtitle file.</p>

![](/images/2017-05-18-Mandarine/ui-6-1.png)

3. Now you can test the Azure Search database, enter a word in the search edit box and click on the button **Search**.</p>

![](/images/2017-05-18-Mandarine/ui-6-2.png)

4. If this word is present in any subtitle, the search list box is populated with all the subtile where this word has been pronounced.</p>
5. Select the subtitle in the list box and click on the button **Play Search**.</p>

![](/images/2017-05-18-Mandarine/ui-6-3.png)

6. The Web Player will play the audio or video files along with the subtitles when the word has been pronounced.</p>

![](/images/2017-05-18-Mandarine/ui-6-4.png)


### Mandarine Bot

"{to be completed}"


## Technical delivery ##

### SDKs ###

The differents components used for this deployment are based on the following SDKs:

- [Azure SDK for PHP](https://github.com/Azure/azure-sdk-for-php)
- [Azure Search REST API](https://docs.microsoft.com/fr-fr/rest/api/searchservice/?redirectedfrom=MSDN)
- [Cognitive Services REST Text Translator API](http://docs.microsofttranslator.com/text-translate.html)

### Azure Media Services APIs ###

WEBVTT subtitles:
The Azure Media Services are used to generate WEBVTT subtitles files from input video files (MP4 files) or input audio files (MP3 files).
As the Azure Media Player supports natively the WEBVTT subtitles, this format is used a pivot format for subtitles.

From this format it's possible to generate TTML subtitle file.

- [Check the DisplayVTTSource on github](https://github.com/flecoqui/azure/blob/master/Samples/TestAzureMediaIndexer/AzureWebPlayer/assets/js/loadplayer.js#L458)
- [Check the DisplayTTMLSource on github](https://github.com/flecoqui/azure/blob/master/Samples/TestAzureMediaIndexer/AzureWebPlayer/assets/js/loadplayer.js#L472)


Below an extract of a WEBVTT file:


        WEBVTT
           00:00.100 --> 00:12.020
        I am delighted we could share in the serenity and joy of this beautiful day as we come together to celebrate the commitment of the sort of mind moving your enormous phone you mean enormously awesome Galaxy.
        
        00:13.230 --> 00:15.300
        Search one 



Below an extract of a TTML file: 



        <?xml version="1.0" encoding="utf-8"?>
        <tt xml:lang="en-us" xmlns="http://www.w3.org/ns/ttml" xmlns:tts="http://www.w3.org/ns/ttml#styling" xmlns:ttm="http://www.w3.org/ns/ttml#metadata">
        <head>
        <metadata>
         <ttm:title>switch_aud_SpReco</ttm:title>
         <ttm:copyright>Copyright (c) 2013 Microsoft Corporation.  All rights reserved.</ttm:copyright>
        </metadata>
        <styling>
         <style xml:id="Style1" tts:fontFamily="proportionalSansSerif" tts:fontSize="0.8c" tts:textAlign="center" tts:color="white" />
        </styling>
        <layout>
        <region style="Style1" xml:id="CaptionArea" tts:origin="0c 12.6c" tts:extent="32c 2.4c" tts:backgroundColor="rgba(0,0,0,160)" tts:displayAlign="center" tts:padding="0.3c 0.5c" />
        </layout>
        </head>
        <body region="CaptionArea">
        <div>
         <p begin="00:00:00.100" end="00:00:12.020">I am delighted we could share in the serenity and joy of this beautiful day as we come together to celebrate the commitment of the sort of mind moving your enormous phone you mean enormously awesome Galaxy.</p>
         <p begin="00:00:13.230" end="00:00:15.300">Search one </p>
	     .
	     .
	     .
        </div>
        </body>
        </tt>


Once generated the subtitles file and the input video/audio file are stored on a SAS (Shared Access Signature) locator in the Azure Storage Account associated with the Azure Media Services Account.
As the Mandarine's deployment doesn't require Adaptive Streaming, it's not necessary to use the Azure Media Services Streaming End Point.
In order to play correctly a MP4 video file or MP3 audio file over http with a Web Application running in Chrome, the server hosting the MP4 file needs to return the http header "Accept-Ranges: bytes".
It's the case if you store your MP4 files or MP3 files in a SAS locator.

If necessary the WEBVTT subtitle file will be updated on store in the same SAS locator.

- [Check the WEBVTT Web Player/Editor on github](https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer/AzureWebPlayer)


### Cognitive Services APIs ###

The component which translates the subtitle in different languages relies on Cognitive Services Text Translator	API:

The request uri is 	https://api.microsofttranslator.com/V2/Http.svc/Translate 

Information about the method here:

http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate 


As the size of the input text must not exceed 10000 characters, the subtitiles are translated one by one.
Once the input WEBVTT file has been parsed, each subtitle is transmitted to the Cognitive Service as an input text.

Below a sample code in C# which calls the Translator service for each subtitle:



        async Task<string> GetTranslatedWEBVTT(string uri, string inputLanguage, string outputLanguage)
        {
            string translatedContent = string.Empty;
            if (!string.IsNullOrEmpty(uri) && !string.IsNullOrEmpty(inputLanguage) && !string.IsNullOrEmpty(outputLanguage))
            {
                string content = await GetContent(uri);
                if (!string.IsNullOrEmpty(content))
                {
                    TextBoxLogWriteLine("Original Subtitles downloaded");
                    List<SubtitileItem> SubtitleList = ParseWEBVTT(content);
                    if (SubtitleList.Count > 0)
                    {
                        TextBoxLogWriteLine("Original Subtitles parsed");
                        translatedContent += "\xFEFF";
                        translatedContent += "WEBVTT\r\n";
                        SubtitileItem newItem = new SubtitileItem("", "", "");
                        bool bError = false;
                        foreach (SubtitileItem item in SubtitleList)
                        {
                            newItem.startTime = item.startTime;
                            newItem.endTime = item.endTime;
                            if (!string.IsNullOrEmpty(item.subtitle))
                            {
                                newItem.subtitle = await _ttc.Translate(item.subtitle, inputLanguage, outputLanguage);
                                if (!string.IsNullOrEmpty(newItem.subtitle))
                                {
                                    translatedContent += newItem.ToString();
                                }
                                else
                                {
                                    bError = true;
                                    break;
                                }
                            }
                        }
                        if (bError == true)
                            TextBoxLogWriteLine("Error while translating subtitles at " + newItem.startTime);
                        else
                            TextBoxLogWriteLine("Translating subtitles done:" + translatedContent);
                    }
                }
                else
                    TextBoxLogWriteLine("Error while downloading subtitles: subtitle string empty");
            }
            return translatedContent;
        }



The same source code is available there on  github:

https://github.com/flecoqui/azure/blob/master/Samples/TestAzureMediaIndexer/TestAzureMediaIndexer/Subtitle.cs#L205 

### Azure Storage and SAS locator ###

As mentioned before, the media files (audio, video and subtitle file) are streamed from SAS locators which returns "Accept-Ranges: bytes" in the http headers. 
This http header is mandatory to play MP4 video files or MP3 audio files over http with Chrome browser.

The SAS locators are created using the Azure Media Services SDK for a period of time.

                    try
                    {
                        tempLocator = _context.Locators.Create(LocatorType.Sas, asset, AccessPermissions.Read, TimeSpan.FromHours(DurationinHours));
                        tempAsset = asset;
                    }
                    catch (Exception ex)
                    {
                        TextBoxLogWriteLine(string.Format("Exception when creating a SAS Locator for asset Id: '{0}' Exception: {1} ", asset.Id, ex.Message), true);
                        tempAsset = null;
                    }


As so far the application will only play MP4, MP3 and subtitile files (WEBVTT), it's not necessary to activate the Azure Media Services Streaming End Point, the files will be streamed directly from the SAS locator which improve the business model associated with the application.


### Azure Search APIs ###

Once the subtitles are translated in the different languages, it's valuable to use Azure Search to index all the subtitiles files in the different languages.

The following fields have been indexed:

1. The name of the input video or audio file
2. The url of the input video or audio file in the SAS locator
3. A flag which indicates whether the content is an audio file in order to use the audio player instead of Azure Media Player
4. The url of the subitile file in the SAS locator
5. The language of the subtitle File
6. The start time of the Subtitle
7. The end time of the Subtitle
8. The subtitile

Sample source code below in C#:


                var definition = new Microsoft.Azure.Search.Models.Index()
                {
                    Name = "media",
                    Fields = new[]
                    {
                        new Microsoft.Azure.Search.Models.Field("keyId", Microsoft.Azure.Search.Models.DataType.String)                       {   IsKey = true  },
                        new Microsoft.Azure.Search.Models.Field("mediaId", Microsoft.Azure.Search.Models.DataType.String)                       {   IsFilterable = true,  IsSortable = true  },
                        new Microsoft.Azure.Search.Models.Field("mediaName", Microsoft.Azure.Search.Models.DataType.String)                     { IsFilterable = true,  IsSortable = true },
                        new Microsoft.Azure.Search.Models.Field("mediaUrl", Microsoft.Azure.Search.Models.DataType.String)                       { IsFilterable = true},
                        new Microsoft.Azure.Search.Models.Field("isAudio", Microsoft.Azure.Search.Models.DataType.Boolean)                       {IsFilterable = true},
                        new Microsoft.Azure.Search.Models.Field("subtitleUrl", Microsoft.Azure.Search.Models.DataType.String)                       { IsFilterable = true},
                        new Microsoft.Azure.Search.Models.Field("subtitleLanguage", Microsoft.Azure.Search.Models.DataType.String)                   { IsFilterable = true},
                        new Microsoft.Azure.Search.Models.Field("subtitleStartTime", Microsoft.Azure.Search.Models.DataType.String)                   { IsSortable = true},
                        new Microsoft.Azure.Search.Models.Field("subtitleEndTime", Microsoft.Azure.Search.Models.DataType.String)                   { IsSortable = true},
                        new Microsoft.Azure.Search.Models.Field("subtitleContent", Microsoft.Azure.Search.Models.DataType.String)                   { IsSearchable = true}
                    }
                };

                try
                {


                    var response = _searchContext.Indexes.CreateWithHttpMessagesAsync(definition);
                    if (response != null)
                    {
                        response.Wait();
                        if ((response.Result != null) && (response.Result.Response != null))
                        {
                            if (response.Result.Response.StatusCode == System.Net.HttpStatusCode.Created)
                            {
                                _indexClient = _searchContext.Indexes.GetClient("media");
                                result = true;
                            }
                        }
                    }


                }
                catch (Exception ex)
                {
                    TextBoxLogWriteLine("Exception while creating Azure Search Index: " + ex.Message + " " + ex.InnerException.Message, true);
                }



With Azure Search the application offers the following use case to the operator:
The operator search for a specific in the subtitle database, the service will return all the subtitles where this word has been pronounced.
The information returned by the service:
1. The url of the input video or audio file in the SAS locator
2. The url of the subitile file in the SAS locator
3. The start time of the Subtitle
allow the operator to play the video or audio file at the timestamp when the word has been pronounced.


### Microsoft Bot Framework ###
"{to be completed}"

### Learnings from the Mandarine's team

Florent Petit Mandarine's CTO: For instance -> *With Azure Media Services and Cognitive Services Text Translator API, we decreased dramatically the time to publish our online training courses for our customers!*
"{to be completed}"

## Conclusion ##

The main findings of this project are:

- Azure was flexible enough to interoperate with legacy services (existing on premises database, third party video streamer)
- Thanks to the usage of ARM template, the Mandarine IT team can redelpoy the Backend Services in few minutes.
- As the Azure Media Player doesn't support playback of audio files (MP3, WMA), the solution supports not only subtitles with Video files (MP4, WMV) but also with Audio files (MP3, WMA) using the Web Player here: https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer  
- Thanks to Azure Media Services Indexer V2 and Cognitive Services Text Translator the generation of subtitles has been almost automated, which has decreased dramatically the time to publish the videos.
- Regarding Mike the Mandarine's Bot, it's too early to get any insights as the service will be online by the end of June.


### Next Steps

This project was the first step of the enhancement of Mandarine services. 
Below a list of services which could improve Mandarine's services:

1. The Mandarine's Web Site could be extended to propose to each user the most appropriate MOOC based on his journey on the Web Site. A recommendation service based on Azure Machine Learning could improve the user experience.</p>
2. During Live events, Mandarine would like to enhance their Live MOOC videos with live subtitles as well. The support of this feature will require the developpment of a specific component based on Azure Media Services.</p>
3. Following the Build 2107 announcement and the support of Skype for Business connector with Microsoft Bot Framework, Mandarine's services could address all the users using Skype For Business.


## Additional resources ##
Below a list of links to resources used by the team:
- Azure Media Services Explorer:</p>
https://github.com/Azure/Azure-Media-Services-Explorer </p>
- Azure ARM template to deploy Azure Media Services, Cognitive Services - Translator Text API, Azure Search: </p>
https://github.com/flecoqui/azure/tree/master/azure-quickstart-templates/101-media-search-cognitive </p>
- Sample Application to generate automatically subtitles files in several languages from an original video or audio file using Azure Services:</p>
https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer </p>
- Speech-To-Text UWP Sample Application:</p>
https://github.com/flecoqui/Windows10/tree/master/Samples/SpeechToTextUWPSampleApp </p>
- Text Translator UWP Sample Application:</p>
https://github.com/flecoqui/Windows10/tree/master/Samples/TranslatorTextUWPSampleApp </p>
- Speech-To-Text Javascript Sample Application:</p>
https://github.com/davrous/BingSpeech </p>
- PHP Text Translation Sample </p>
https://github.com/MicrosoftTranslator/HTTP-Code-Samples </p>