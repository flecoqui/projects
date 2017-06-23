---
layout: post
title:  "Bringing Geneatique Desktop Application to Windows Store"
author: "Fred Le Coquil"
author-link: "https://www.github.com/flecoqui"
#author-image: "{{ site.baseurl }}/images/authors/flecoqui.jpg"
date:   2017-02-14
categories: [Desktop Bridge]
color: "blue"
#image: "{{ site.baseurl }}/images/geneatique_logo.png" #should be ~350px tall
excerpt: This article describes how the Geneatique Dea Web site has been updated to support automated multi-languages substiles generation for training videos and to personalize the journey of each user amongst the catalog of MOOCs.
language: English
verticals: Retail, Consumer Products & Services
geolocation: France
permalink: https://microsoft.github.io/techcasestudies/geneatique.html
---

Microsoft teamed up with CDIP, a Microsoft's partner, delivering Genealogy software.
As more and more their customers are running their current Desktop Genealogy Software called Geneatique over Windows 10, CDIP decided to distribute this Software through Windows Store</p>

 ![Company logo](/images/2017-06-23-geneatique/cdip.png)

The easiest approach was to use the Desktop Bridge App Converter to automatically convert their Desktop Application into packages distributed with Windows Store.
This effort did start last year using Windows 10 Anniversary Update, unfortunatelly the packages generated for this version of Windows 10 was instable: the application installed from those packages oftentimes crashed while the user was sing the embedded Web Browser.
Since Creator Update the packages are stable and the application is able to run the embedded Web Browser without crashing.</p>

The solution relies on :</p>
- [Desktop App Converter](https://www.microsoft.com/fr-fr/store/p/desktop-app-converter/9nblggh4skzw) : Used to automatically convert the Desktop Application into a package compliant with Windows Store</p>
- [Cognitive Services - Text Translator APIs](https://azure.microsoft.com/en-gb/services/cognitive-services/translator-text-api/) : Used to translated the original subtitles in other languages subtitles</p>
- [Azure Search](https://azure.microsoft.com/en-us/services/search/) : Used to index subtitles in different languages</p>
- [Microsoft Bot Framework](https://dev.botframework.com/) : Used to implement CDIP Bot (called Mike) and supporting Web Chat and Skype connectors </p>
 




 ![Team](/images/2017-06-23-geneatique/picture2.png)


Mandarine's backend is currently based on:</p>
- Ubuntu virtual Machines running in Azure</p>
- Database: MySQL Server</p>
- Web Server: Apache, PHP, Symfony, Node JS</p>


**Core Team:** </p>
- Aouatif Alilou - Project Manager, CDIP</p>
- Fred Le Coquil - Technical Evangelist, Microsoft</p>

 ![Team](/images/2017-06-23-geneatique/geneatique_1.png)

 ![Team](/images/2017-06-23-geneatique/geneatique_2.png)
 
 ![Team](/images/2017-06-23-geneatique/geneatique_3.png)


## Customer profile ##

**CDIP**

 ![Team](/images/2017-06-23-geneatique/cdip.png)

 [Mandarine Web Site](http://www.geneatique.com/)
 
Created in 2008, CDIP guides companies in their digital transformation. 35 full-time employees are currently working at headquarters in Roubaix, France. From free MOOC services to personalized corporate services, CDIP provides the best tailored services to train all the employees. The solution is based on a combination of the latest technologies, such as video conferencing, web conferencing, e-learning and machine learning. 
This hackfest is aligned with Mandarine strategic move operated earlier this year (Feb 2017), that led to the creation of a new logo, name, slogan and graphic interface that increased their visibility as a well-established player in the digital change and transformation arena. They also came with a reinforced teaching team, a broader range of solution and an international dimension with the opening of new offices in the United Kingdom, Poland, Canada and the United States. 

 ![Team](/images/2017-06-23-geneatique/mandarine.png)
 
## Problem statement ##

CDIP has already published several hundred of videos, they would like to add subtitles (SRT, WEBVTT, TTML) for each video.
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

? Préparation du Setup : 
 
1. le setup d’installation de l’application doit être généré en un seul fichier ( et non plusieur binaire) 2. Tous les modules exécutés depuis votre setup d'installation (Section [run] de l’iss si vous utilisez ‘Inno Setup’) doivent être lancer en mode SILENT et NORESTART, sinon la génération de l’appx reste bloquée. 
 
? Configuration système requise : 
 
Pour générer un Appx vous aurez besoin d'un version de Windows 10 (Entreprise ou pro). Assurez-vous que vous utilisez la mise à jour anniversaire de Windows 10 (14393.0 ou versions ultérieures). 
 
? Préparation de l'environnement : 
 
1. Installer DesktopConverter disponible ici : https://www.microsoft.com/en-us/store/p/desktopappconverter/9nblggh4skzw .  2. Installer Windows container en suivant les instructions : https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-st art-windows-10 3. Télécharger une image de base compatible avec votre version de windows installé  4. Exécuter "Desktop Bridge Converter" en tant qu'admin 5. Commencer par l'installation de l'environnement en utilisant l'image de base téléchargé     DesktopAppConverter.exe -Setup -BaseImage "C:\Téléchargement\BaseImage-<version>.win"  
 ?    ? Génération de l'appx : 
 1. Générer votre APPX avec la commande :  DesktopAppConverter.exe -Installer "C:\Votresetup.exe" -InstallerArgument "/SILENT /NORESTART" -Destination "c:\VotreSortie" -PackageName "Geneatique2017" -PackageDisplayName "Généatique 2017" -AppDisplayName "Généatique 2017" -AppDescription  "Logiciel de Généalogie : Généatique 2017" -AppExecutable "Geneatique.exe" -Version 1.0.8.0 -Publisher "CN=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, OU=Secure Application Development, O=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, L=OSNY, S=Val-d''Oise, C=FR" - MakeAppx -Verbose  
 
> Une fois la génération de l'appx terminée. Dans le dossier de sortie vous aurez - Un dossier  "log" , - Un dossier "PackageFiles" , - Geneatique.appx > Le dossier "PackageFiles" contient l'ensemble des fichiers nécessaire à la génération de l'appx. Tous les autres appx seront généré uniquement à partire de ce dossier. 

 
? Mise à jour de l'appx : 
 
 - Placer dans le dossier "PackageFiles" tous les fichiers modifiés puis régénérer l'appx avec la commande "MakeAppx"   makeappx pack -d "c:\VotreSortie\PackageFiles" -p "C:\VotreSortie\Geneatique.appx" 
 
? Signature de l'appx: 
 
  - Lancer Windows PowerShell et exécutez la commande :     signtool.exe sign -f "mycertif.pfx" -p password -fd SHA256 -v "appName.Appx"






## Conclusion ##

The main objective of this project was to automate the generation of subtitles associated with training courses videos in order to:
- increase the reach of Mandarine's services (up-to 8 subtitle languages per video)
- improve the time to publish the videos 

Beyond those legacy components, the architecture of this deployment is based on the following Azure's components:
1. Azure Azure Media Services </p>
2. Azure Cognitive Services Text Translator key</p>
3. Azure Search Services Account </p>
4. Azure Web App </p>
5. Microsoft Bot Framework (Node.js) </p>
6. Azure Virtual Machines running the Ubuntu and Apache/PHP/Symfony/MySQL. </p>


**Measurable impact/benefits resulting from the implementation of the solution:**</p>
With Azure Media Services Indexer V2 and Cognitive Services Text Translator the generation of subtitles has been almost automated, which has decreased dramatically the time to publish the videos.
Thanks to the usage of ARM template, the Mandarine IT team can redeploy the Backend Services in few minutes.

**General lessons:**</p>
Azure was flexible enough to interoperate with legacy services (existing on premises database, third party video streamer)
As the Azure Media Player doesn't support playback of audio files (MP3, WMA), the solution supports not only subtitles with Video files (MP4, WMV) but also with Audio files (MP3, WMA) using the [Web Player](https://github.com/flecoqui/azure/tree/master/Samples/TestAzureMediaIndexer)  
Regarding Mike the Mandarine's Bot, it's too early to get any insights as the service will be online by the end of June.


**Opportunities going forward**</p>
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