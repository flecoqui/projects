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

Microsoft teamed up with Mandarine Academy, a Microsoft's partner, delivering online training courses to update the current Mandarine's backend to allow new features for the frontend. The new backend does support:</p>
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
 
Created in 2008, Mandarine Academy guides companies in their digital transformation. 35 full-time employees are currently working at headquarters in Roubaix, France. From free MOOC services to personalized corporate services, Mandarine Academy provides the best tailored services to train all the employees. The solution is based on a combination of the latest technologies, such as video conferencing, web conferencing, e-learning and machine learning. 
This hackfest is aligned with Mandarine strategic move operated earlier this year (Feb 2017), that led to the creation of a new logo, name, slogan and graphic interface that increased their visibility as a well-established player in the digital change and transformation arena. They also came with a reinforced teaching team, a broader range of solution and an international dimension with the opening of new offices in the United Kingdom, Poland, Canada and the United States. 

 ![Team](/images/2017-05-18-Mandarine/mandarine.png)
 
## Problem statement ##

Mandarine Academy has already published several hundred of videos, they would like to add subtitles (SRT, WEBVTT, TTML) for each video.
Azure Media Indexer V2 seems the best approach and easiest approach to generate the subtitles directly from the original video.
Once the native subtitles are available in the native language. it becomes possible  to generate subtitles in different languages by using Cognitive Services Text Translator API,.
Each MOOC Video will support up-to 8 subtitles languages. 
For the first phase of this project, Mandarine use Adaptive Streaming based on third-party platform and player. The MOOC Videos will be MP4 files delivered to the clients through the standard player.
Azure Media Services Streaming End Point won't be used during this phase, the application will only use Azure Media Services SAS Locator to deliver the videos.
As today the generation of subtitles can't be fully automated, after each step, the Mandarine operator needs to check the generated subtitles:
- subtitles generated with Azure Media Services Indexer V2
- subtitles generated with Cognitive Services Text Translator API  
After each step, the Manadarine operator must be able to update manually the subtitles if necessary '

Beyond the enhancement of their video content, Mandarine wants to improve the interactivity of their current Web Site. They want to personalize the journey of each user amongst the catalog of MOOCs. 
For instance, they want to automatically refine the profile of each user.
Their Bot (Project Name called Mike) will ask questions to complete the profile of each user.
The Bot will also send notifications to users for instance when they are about to complete a level of training. 
The Bot will address the subscribers connected to Mandarine Web Site and to Skype. 

 
## Solution and steps ##

### Overall Architecture

As the main objective of this initiative is to enhance the existing Mandarine services, the architecture of this deployment will take into account all the legacy components technologies used by Mandarine services.
For instance, the database server is based on MySQL Server and this server is hosted in Azure's datacenter. These training video files are hosted on a third-party streaming platform.</p>
On the technologies side, the servers are running Ubuntu, the Web Servers are based on Apache/PHP/Symfony. The backend services are developed using Node.js.The Web Servers will be hosted in Azure. 
The client application is actually a Web Application running in any browsers.</p>

Beyond those legacy components, the following Azure Serivces will be deployed:
1. Azure Azure Media Services </p>
2. Azure Cognitive Services Text Translator key</p>
3. Azure Search Services Account </p>
4. Azure Web App </p>
5. Microsoft Bot Framework (Node.js) </p>
6. Azure Virtual Machines running the Ubuntu and Apache/PHP/Symfony/MySQL. </p>

Below the architecture of Manadarine services using Azure:

 ![Team](/images/2017-05-18-Mandarine/0-architecture.png)


### Subtitles generation 

#### Installing the backend services in Azure:

In order to generate the subtitles you need to install the Azure backend with all the services associated.
You can either use the Azure Portal to deploy manually all the Azure Services:

https://portal.azure.com
 
or you can use directly the Azure Resource Manager template available there:

https://github.com/flecoqui/azure/tree/master/azure-quickstart-templates/101-media-search-cognitive

This template allows you to deploy  a Web App, an Azure Media Services Account, an Azure Search Account and an Azure Text Translator service in the same region as the resource group.
As Azure Media Services, Search Service and Cognitive Services are not deployed in all regions, it is recommended to use the following regions:
West US, West Europe,Southeast Asia,West Central US 
This template is associated with Windows Application which is used to generate automatically video subtitles in different languages fron an orginal video or audio file. Once generated the subtitles are stored in Azure Search to allow the users to find all the videos associated with a specific key word.
This Application called TestAzureMediaIndexer is available there:
https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer 


![](/images/2017-05-18-Mandarine/1-architecture.png)

Using the two Azure CLI command lines below, you can deploy automatically all the Azure Services required for the Mandarine deployment: 

    azure group create testamsseacog northeurope
	azure group deployment create testamsseacog depiperftest -f azuredeploy.json -e azuredeploy.parameters.json -vv

Using the following parameters:

Name prefix which will be used to create Azure Media Services Account, Azure Storage Account,  Azure Search Account, Azure Cognitive Services Text Translation Account:

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


Once the backend services are installed, the storage account where the video files and subtitles files will be stored needs to be manually configured to support CORS (Cross-Origin Resource Sharing) using the Azure portal:

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

This template is associated with Windows Application which is used to generate automatically video subtitles in different languages fron an original video or audio file. Once generated the subtitles are stored in Azure Search to allow the users to find all the videos associated with a specific key word.
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
The binary associated with the application is available there:
[ZIP file with the Application](https://github.com/flecoqui/azure/master/Samples/TestAzureMediaIndexer/Releases/Latestrelease.zip)

1. You can download the zip file.</p>
2. Unzip the LatestRelease.zip file</p>
3. Run locally TestAzureMediaIndexer.exe</p>

## Using the application TestAzureMediaIndexer
This sample application is a basic Windows Application with one single page:

![](/images/2017-05-18-Mandarine/ui-0.png)


#### Connect the application to the Azure Backend
In order to use the application you need to provide the following parameters to establish a connection with your backend in Azure:
1. The Azure Media Services Account name</p>
2. The Azure Media Services Account key</p>
3. The Azure Cognitive Services Text Translator key</p>
4. The Azure Search Services Account name</p>
5. The Azure Search Services Account key</p>
6. The url of the Web Player application hosted on the Web site of your backend, the url should be close to this format: http://YourWebAppName.azurewebsites.net/player.html

![](/images/2017-05-18-Mandarine/1-architecture-step-1.png)

You can retrieve all the parameters below from the [Azure Portal:](https://portal.azure.com) </p>
1. The Azure Media Services Account name</p>
2. The Azure Media Services Account key</p>
3. The Azure Cognitive Services Text Translator key</p>
4. The Azure Search Services Account name</p>
5. The Azure Search Services Account key</p>
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

#### Upload the audio and video assets
Once the application is connected, the first step consists in uploading video or audio assets on your backend in Azure.

![](/images/2017-05-18-Mandarine/1-architecture-step-2.png)

1. Click on the **Add Asset** button</p>
2. Select the audio file or video file you want to upload</p>

![](/images/2017-05-18-Mandarine/ui-2.png)

3. Once the file is uploaded, a new asset and a new file is displayed in the lists below:</p>

![](/images/2017-05-18-Mandarine/ui-2-1.png)


#### Generate the subtitles from audio and video assets
Once the video or audio file is uploaded, you can generate the subtitles with Azure Media Services Indexer 

![](/images/2017-05-18-Mandarine/1-architecture-step-3.png)

1. Select the spoken language of your audio or video file</p>
2. Click on the button **Generate subtitle** </p>

![](/images/2017-05-18-Mandarine/ui-3.png)

3. A Job to generate the subtitles is launched in Azure after few minutes, the new subtitle file is available.</p>

![](/images/2017-05-18-Mandarine/ui-3-1.png)


#### Update the generated subtitles with the Web Application 
Once the subtitle file is available, it's possible to update the subtitle file. The native format is WEBVTT, but it's still possible to convert a WEBVTT subtitle file into a TTML subtitle file.

![](/images/2017-05-18-Mandarine/1-architecture-step-4.png)

1. Click on the button **Play Video/Subtitle** or **Play Audio/Subtitle** </p> 
The Windows Application launches the Web Player Application 
![](/images/2017-05-18-Mandarine/ui-4.png)

2. Your default browser on your computer running Windows is displaying a page playing the video or audio file along with the subtitles.</p>

![](/images/2017-05-18-Mandarine/ui-4-1.png)

3. As sometimes the generated subtitles file needs to be updated, you can update each subtitle. Click on the **Pause** button.</p>
4. Update the subtitle, click on the button **Save**.</p>

![](/images/2017-05-18-Mandarine/ui-4-2.png)

5. When all the subtitles are updated, you can save the subtitle file on your machine in WEBVTT or TTML format.</p>

![](/images/2017-05-18-Mandarine/ui-4-3.png)

6. Once the subtitle file is stored locally on your machine, you can update the subtitle file on Azure Storage when clicking on button **Update Subtitle**.</p>

![](/images/2017-05-18-Mandarine/ui-4-4.png)


#### Translated the generated subtitles with Cognitive Services Text Translator
Once the first subtitle file associted with your audio or video file are correct, you can generate more subtitle files in different languages.
 
![](/images/2017-05-18-Mandarine/1-architecture-step-5.png)

1. Select the source subtitle in the list box **List of Subtitle Assets**.</p>
2. Select the language of your new subtitle file.</p>
3. Click on the button **Translate subtitle** </p>

![](/images/2017-05-18-Mandarine/ui-5.png)

4. After few seconds the new subtitle file is displayed in the list box **List of Subtitle Assets**</p>

![](/images/2017-05-18-Mandarine/ui-5-1.png)

5. If you click on the button **Play Video/subtitle** or **Play Audio/subtitle** , you can playback the new subtitles.</p>

![](/images/2017-05-18-Mandarine/ui-5-2.png)

6. Finally, you can download or display the new subtitle file.</p>

![](/images/2017-05-18-Mandarine/ui-5-3.png)


#### Using Azure Search to Index the subtitles files associated with the audio and video assets
Once all the subtitles associated with your video or audio files are generated, you can store the subtitles in the Azure Search service.</p>
 
![](/images/2017-05-18-Mandarine/1-architecture-step-6.png)

1. First you need to create the Index associated with the subtitles. Click on the button **Create Index** to create the Index. This step is only required once. If you want to clear the Azure Search database you can click on **Delete Index** button.</p>

![](/images/2017-05-18-Mandarine/ui-6.png)

2. Once the Index is created, select the subtitle file you want to import and click on the button **Populate the Index with subtitles**. You can repeat this step for each subtitle file.</p>

![](/images/2017-05-18-Mandarine/ui-6-1.png)

3. Now you can test the Azure Search database, enter a word in the search edit box and click on the button **Search**.</p>

![](/images/2017-05-18-Mandarine/ui-6-2.png)

4. If this word is present in any subtitle, the search list box is populated with all the subtitle where this word has been pronounced.</p>
5. Select the subtitle in the list box and click on the button **Play Search**.</p>

![](/images/2017-05-18-Mandarine/ui-6-3.png)

6. The Web Player will play the audio or video files along with the subtitles when the word has been pronounced.</p>

![](/images/2017-05-18-Mandarine/ui-6-4.png)


### Mandarine Bot

#### Installing the Mandarine backend associated with Mandarine Bot and Mandarine Web Site in Azure:

The current Mandarine Web Site is hosted in Ubuntu virtual machines running Apache/PHP .
The Mandarine Web Site will be extended to:
- support a Web Chat control connected to the Mandarine Bot,
- support a link to add the Mandarine Bot to your Skype contacts.

In order to activate the Mandarine Bot Service, you need first to register your bot on Bot Framework Web Site https://dev.botframework.com/ and then install the Azure Bot backend with all the services associated.
You can either use the Azure Portal to deploy manually all the Azure Services:

https://portal.azure.com
 
or you can use directly the Azure Resource Manager template available there:

https://github.com/flecoqui/azure/tree/master/azure-quickstart-templates/101-samplebot-webchat

This template allows you to deploy  Deploy a Web App hosting a sample Bot with Web Chat and Skype channels and a Virtual Machine running Linux (debian, ubuntu, centos, redhat) and an Apache/PHP server with Web Chat control and a link to Skype. All those resources will be deployed in the same region as the resource group.

![](/images/2017-05-18-Mandarine/2-architecture.png)

This template allows you to deploy a simple VM running: </p>
- Debian (version 8): Apache and php with Web chat control and link to Skype,
- Ubuntu (version 16.04): Apache and php with Web chat control and link to Skype, 
- Centos (version 7.2): Apache and php with Web chat control and link to Skype,
- Red Hat (version 7.2): Apache and php with Web chat control and link to Skype.

#### REGISTERING YOUR BOT:

Before deploying this Azure Resource Manager template you need to register your bot:
 
1. With your Browser open the url: https://dev.botframework.com/  </p>
2. Click on the link "Sign In" and use your Microsoft Account. If you don't have a Microsoft Account you can sign up there: https://signup.live.com/ </p>
3. Once connected to the botframework web site you can see the menu bar below::</p>
![](/images/2017-05-18-Mandarine/1-install.png)
4. Click on the menu "My bots", you can see the page below. </p>
![](/images/2017-05-18-Mandarine/2-install.png)
5. Click on the button "Register", the page is displayed and fill the different fields like Display Name, Bot handle, Long description.</p>
![](/images/2017-05-18-Mandarine/3-install.png)
6. In the "Configuration" section, click on "Create Microsoft App ID and password" button to register your App ID and password.</p>
![](/images/2017-05-18-Mandarine/4-install.png)
7. On the page below, enter your "App name" and click on the "Generate an App  password to continue" button.</p>
![](/images/2017-05-18-Mandarine/5-install.png)
8. On the subsequent page, copy the new password, you'll need this password to deploy your Azure template.</p>
![](/images/2017-05-18-Mandarine/6-install.png)
9. Copy your App ID as well, you'll need this ID to deploy your Azure template.</p>
![](/images/2017-05-18-Mandarine/7-install.png)
10. Check the "Terms of use" box and click on the "Register" button to register your bot.</p>
![](/images/2017-05-18-Mandarine/8-install.png)
11. During this registration phase, we leave the field "Messaging endpoint" empty, we will fill this field when the bot will be deployed.</p>
![](/images/2017-05-18-Mandarine/9-install.png)
12. Once the box is created the message below is displayed.</p>
![](/images/2017-05-18-Mandarine/10-install.png)
13. By default 2 channels are created, the Skype channel and the Web Chat channel. Click on  the "Edit" link associated with the Web Chat channel</p>
![](/images/2017-05-18-Mandarine/11-install.png)
14. The page "Configure Web Chat" is displayed, click on the "Add new site" link.</p>
![](/images/2017-05-18-Mandarine/13-install.png)
15. Fill the field "Site name" and click on the "Done" button .</p>
![](/images/2017-05-18-Mandarine/14-install.png)
16. On the subsequent page, copy the "Secret keys" clicking on the "Show" link. You'll need this "Secret key" to deploy your Azure template.</p>
![](/images/2017-05-18-Mandarine/15-install.png)
17. The registration of your bot is now done, you know the "App ID" and the "Password" of your bot, and the "Secret Key" of the Web Chat channel. You are now ready to deploy the Azure template.

#### CREATE RESOURCE GROUP:

Using Azure CLI you can run the following command to create the resource group associated with your deployment:

azure group create "ResourceGroupName" "RegionName"

For instance:

    azure group create testbotgrp northeurope

#### DEPLOY THE SERVICES:

Once the resource group is created, you can launch the deployment using Azure CLI:

azure group deployment create "ResourceGroupName" "DeploymentName"  -f azuredeploy.json -e azuredeploy.parameters.json

For instance:

    azure group deployment create testbotgrp depiperftest -f azuredeploy.json -e azuredeploy.parameters.json -vv
 
You can also launch the deployment with the Azure portal clicking on the button below: 

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fflecoqui%2Fazure%2Fmaster%2Fazure-quickstart-templates%2F101-samplebot-webchat%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Through the portal or Azure CLI you'll need to define the following parameters before launching the deployment. If you use Azure CLI you'll need to fill the file azuredeploy.parameters.json, if you use the Azure Portal, you'll fill directly the fields on the portal:

The Bot Name prefix which will be used to deploy your bot in a Web App. </p>
The url of the Web App will be : https://[Bot Name Prefix]bot.azurewebsites.net/ .</p>
The messaging endpoint url will be : https://[Bot Name Prefix]bot.azurewebsites.net/api/messages :

    "namePrefix": {
      "defaultValue": "Bot Name prefix",
      "type": "string",
      "metadata": {
        "description": "Bot Name prefix"
      }
    }

The Bot sku :

    "sku": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Shared",
        "Basic",
        "Standard"
      ],
      "defaultValue": "Free",
      "metadata": {
        "description": "Bot Sku: Free, Shared, Basic, Standard"
      }
    }

The Bot worker size:

    "workerSize": {
      "type": "string",
      "allowedValues": [
        "0",
        "1",
        "2"
      ],
      "defaultValue": "0",
      "metadata": {
        "description": "Bot Worker Size: 0, 1, 2"
      }
    }


The Microsoft App ID associated with your Bot, you got this parameter after the registration phase:


    "MICROSOFT_APP_ID": {
      "type": "string",
      "metadata": {
        "description": "Bot Application ID"
      }
    }


The Microsoft App Password associated with your Bot, you got this parameter after the registration phase:


    "MICROSOFT_APP_PASSWORD": {
      "type": "string",
      "metadata": {
        "description": "Bot Application Password"
      }
    }


The Web Chat channel secret key associated with your Bot, you got this parameter after the registration phase:


    "WEBCHAT_SECRET": {
      "type": "string",
      "metadata": {
        "description": "Bot WebChat Secret"
      }
    }


The administrator user name of the Virtual Machine running Linux and Apache/PHP:


    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Administrator User name for the Virtual Machine."
      }
    }


The administrator password of the Virtual Machine running Linux and Apache/PHP:


    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    }


The DNS name prefix of your Virtual Machine running Linux and Apache/PHP, the public DNS name of your Virtual Machine will be [dnsLabelPrefix].[Region].cloudapp.azure.com:


    "dnsLabelPrefix": {
      "type": "string",
      "metadata": {
        "description": "Unique DNS Name for the Public IP used to access the Virtual Machine. DNS name: <dnsLabelPrefix>.<Region>.cloudapp.azure.com"
      }
    }

The Linux distribution of your Virtual Machine  (debian, ubuntu, centos, redhat): 

    "configurationOS": {
      "type": "string",
      "defaultValue": "debian",
      "allowedValues": [
        "debian",
        "ubuntu",
        "centos",
        "redhat"
      ],
      "metadata": {
        "description": "The Operating System to be installed on the VM. Default value debian. Allowed values: debian,ubuntu,centos,redhat,nanoserver2016,windowsserver2016"
      }
    },




The configuration size of your virtual machine (Small: F1 and 128 GB data disk, Medium: F2 and 256 GB data disk, Large: F4 and 512 GB data disk, XLarge: F4 and 1024 GB data disk) : 

    "configurationSize": {
      "type": "string",
      "defaultValue": "Small",
      "allowedValues": [
        "Small",
        "Medium",
        "Large",
        "XLarge"
      ],
      "metadata": {
        "description": "Configuration Size: VM Size + Disk Size"
      }
    }


#### COMPLETING THE BOT REGISTRATION

Now the Web App running your Bot has been deployed, you need to associate this Web App with your Bot registration on the Bot Framework Web Site https://dev.botframework.com/ :

1. To complete the configuration of your Bot, you need to define the "Messaging Endpoint" of your bot, if you did the deployment with Azure CLI, this information has been displayed at the end of the Azure CLI. Otherwise, the syntax of this url is  https://[Bot Name Prefix]bot.azurewebsites.net/api/messages .</p>
![](/images/2017-05-18-Mandarine/0-configure.png)
2. With your Browser open the url https://dev.botframework.com/ , on the new page select your bot registered during the first step of this deployment. Click on the "Settings" menu.</p>
![](/images/2017-05-18-Mandarine/1-configure.png)
3. In the "Configuration" section, enter the "messaging endpoint" of your Web App:</p>
![](/images/2017-05-18-Mandarine/2-configure.png)
4. Click on the "Save changes" button. Your Bot is now fully configured</p>
![](/images/2017-05-18-Mandarine/3-configure.png)


#### TESTING THE BOT

1. With your Browser open the url https://dev.botframework.com/ , on the new page select your bot registered during the first step of this deployment. Click on the "Test" button on the menu bar.</p>
![](/images/2017-05-18-Mandarine/1-test.png)
2. In the Test page, enter a message in the "Type your message" field. Check that you get a response from the bot.</p>
![](/images/2017-05-18-Mandarine/2-test.png)
3. Now, let's test the Apache Server running in the Virtual Machine. </p>With your Browser open the url  http://[dnsLabelPrefix].[Region].cloudapp.azure.com/index.php. If you did the deployment with Azure CLI, this url has been displayed at the end of the Azure CLI. The page will display the public IP address of the virtual machine, the Web Chat control and the link to Skype to add the bot to your contacts.</p>
![](/images/2017-05-18-Mandarine/3-test.png)
4. In the Web Chat control enter a message, check that you get a response from the bot.</p>
![](/images/2017-05-18-Mandarine/4-test.png)
5. Click on the "Add to Skype" button to add your Bot to your Skype contacts</p>
![](/images/2017-05-18-Mandarine/5-test.png)
6. Launch Skype on your machine, you can see your bot in the list of your contacts.</p>
![](/images/2017-05-18-Mandarine/6-test.png)
7. Enter a message, check that you get a response from the bot.</p>
![](/images/2017-05-18-Mandarine/7-test.png)


## Technical delivery ##

### SDKs ###

The different components used for this deployment are based on the following SDKs:

- [Azure SDK for PHP](https://github.com/Azure/azure-sdk-for-php)
- [Azure Search REST API](https://docs.microsoft.com/fr-fr/rest/api/searchservice/?redirectedfrom=MSDN)
- [Cognitive Services REST Text Translator API](http://docs.microsofttranslator.com/text-translate.html)
- [Bot Builder SDK for Node.js](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-quickstart#install-the-sdk)

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
As Mandarine's deployment doesn't require Adaptive Streaming, it's not necessary to use the Azure Media Services Streaming End Point.
In order to play correctly a MP4 video file or MP3 audio file over http with a Web Application running in Chrome, the server hosting the MP4 file needs to return the http header "Accept-Ranges: bytes".
It's the case if you store your MP4 files or MP3 files in a SAS locator.

If necessary the WEBVTT subtitle file will be updated on store in the same SAS locator.

- [Check the WEBVTT Web Player/Editor on github](https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer/AzureWebPlayer)


### Cognitive Services APIs ###

The component which translates the subtitle in different languages relies on Cognitive Services Text Translator	API:

The request uri is 	https://api.microsofttranslator.com/V2/Http.svc/Translate 

Information about the method here:

http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate 


As the size of the input text must not exceed 10000 characters, the subtitles are translated one by one.
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
                    List<SubtitleItem> SubtitleList = ParseWEBVTT(content);
                    if (SubtitleList.Count > 0)
                    {
                        TextBoxLogWriteLine("Original Subtitles parsed");
                        translatedContent += "\xFEFF";
                        translatedContent += "WEBVTT\r\n";
                        SubtitleItem newItem = new SubtitleItem("", "", "");
                        bool bError = false;
                        foreach (SubtitleItem item in SubtitleList)
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


As so far the application will only play MP4, MP3 and subtitle files (WEBVTT), it's not necessary to activate the Azure Media Services Streaming End Point, the files will be streamed directly from the SAS locator which improve the business model associated with the application.


### Azure Search APIs ###

Once the subtitles are translated in the different languages, it's valuable to use Azure Search to index all the subtitles files in the different languages.

The following fields have been indexed:

1. The name of the input video or audio file
2. The url of the input video or audio file in the SAS locator
3. A flag which indicates whether the content is an audio file in order to use the audio player instead of Azure Media Player
4. The url of the subtitle file in the SAS locator
5. The language of the subtitle File
6. The start time of the Subtitle
7. The end time of the Subtitle
8. The subtitle

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
2. The url of the subtitle file in the SAS locator
3. The start time of the Subtitle
allow the operator to play the video or audio file at the timestamp when the word has been pronounced.


### Microsoft Bot Framework ###

The [script](https://github.com/flecoqui/azure/blob/master/azure-quickstart-templates/101-samplebot-webchat/install-software.sh) used to install and configure the Virtual Machine running Apache/PHP server is called from the Azure Resource Manager Template.
This script is called with 3 parameters:
- the first parameter is the machine hostname
- the second parameter is the Web Chat secret key associated with the bot.
- the third parameter is the App ID associated with the bot.

```
    # Parameter 2 Bot WebChat Secret 
    webchat_secret=$2
    webchat_url=https://webchat.botframework.com/embed/mynewsamplebot?s=$webchat_secret
    # Parameter 3 Bot Application ID 
    skype_appid=$3
    skype_url=https://join.skype.com/bot/$skype_appid
```

The second parameter the Web Chat secret key is used to embed the Web Chat control in the PHP page.
The third parameter the App ID is used to embed the link to the Skype page to add the bot to your Skype contats.


```
    <p>This is the home page of a VM running on Azure</p>
    <p>Below the WebChat page for the Bot: </p>
	<iframe src="$webchat_url"></iframe>
    <p></p>
    <p></p>
    <p></p>
    <p>Below the link to add the Bot to your Skype contacts: </p>
	<a href="$skype_url">
	<img src="https://dev.botframework.com/Client/Images/Add-To-Skype-Buttons.png"/>
	</a>
```

### Learnings from the Mandarine's team  

Florent Petit Mandarine's CTO: *With Azure Media Services and Cognitive Services Text Translator API, we decreased dramatically the time to publish our online training courses for our customers!*  


## Conclusion ##

The main insights of this project are:

- Azure was flexible enough to interoperate with legacy services (existing on premises database, third party video streamer)
- Thanks to the usage of ARM template, the Mandarine IT team can redeploy the Backend Services in few minutes.
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