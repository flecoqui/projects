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

Microsoft teamed up with CDIP to convert one of their popular genealogy software, Geneatique, to the Universal Windows Platform (UWP), using Desktop Bridge.

Geneatique is a leading genealogy software in France that provides tools for research, charting and organizing your family tree easier. Geneatique.com is a very comprehensive and high-performance tool, renowned as one of the best family tree software program on the market. 
By bringing its first software to the Windows Store, CDIP saw a great opportunity to increase the number of downloads, reach more than 400 million PC around the world, provide a richer user experience and benefit from a simpler installation and update process management.

As more and more their customers are running their current Desktop Genealogy Software called Geneatique over Windows 10, CDIP decided to distribute this Software through Windows Store</p>

 ![Company logo](/images/2017-06-23-geneatique/cdip.png)

The easiest approach was to use the Desktop Bridge App Converter to automatically convert their Desktop Application into packages distributed with Windows Store.
This effort did start last year using Windows 10 Anniversary Update, unfortunatelly the packages generated for this version of Windows 10 was instable: the application installed from those packages oftentimes crashed while the user was sing the embedded Web Browser.
Since Creator Update the packages are stable and the application is able to run the embedded Web Browser without crashing.</p>

The solution relies on :</p>
- [Desktop App Converter](https://www.microsoft.com/fr-fr/store/p/desktop-app-converter/9nblggh4skzw) : Used to automatically convert the Desktop Application into a package compliant with Windows Store</p>
- [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive ) : Windows 10 SDK which inludes the tools </p>
- [Windows 10 Base Image 15063](http://aka.ms/converterimages/) : This Base Image is necessary to generate the package for Creator Update</p>
 

 ![Team](/images/2017-06-23-geneatique/geneatique_1.png)


**Core Team:** </p>
- Aouatif Alilou - Project Manager, CDIP</p>
- Maud Tournay - Business Evangelist, Microsoft</p>
- Fred Le Coquil - Technical Evangelist, Microsoft</p>

 ![Team](/images/2017-06-23-geneatique/geneatique_1.png)

 ![Team](/images/2017-06-23-geneatique/geneatique_2.png)
 
 ![Team](/images/2017-06-23-geneatique/geneatique_3.png)


## Customer profile ##

**CDIP**
CDIP is an ISV specialized in genealogy, scrapbooking and old mapping. Their main product is Geneatique, a leading genealogy software in France that provides tools for research, charting and organizing your family tree easier. Geneatique.com is a very comprehensive and high-performance tool, renowned as one of the best family tree software program on the market. It offers more than 100 different templates that you can customize.
The Geneatique software can be purchased online or in DVD.

 ![Team](/images/2017-06-23-geneatique/geneatique_4.png)

 [Geneatique Web Site](http://www.geneatique.com/)
  
## Problem statement ##

Geneatique is a classic Win32 Desktop application available from their official Web Site. The application is installed with a specific installation program. The Win32 application is not the best experience for Windows 10 users, but CDIP did not want to rewrite all the code to create a new Universal Windows Platform (UWP) app to leverage Windows Store capabilities. They have been trying to integrate Geneatique.com to the Windows Store since December 2016 and but they were facing deployments issues.

The packages generated for this version of Windows 10 was instable: the application installed from those packages oftentimes crashed while the user was sing the embedded Web Browser.
Since Creator Update the packages are stable and the application is able to run the embedded Web Browser without crashing.

 
## Solution and steps ##

As the main objective of this initiative is to generate Application packages for the latest version of Windows 10 aka Creator Update, you need to prepare your configuration to support Windows 10 Creator Update.</p>


1. Install the latest version of Windows 10 (Creator Update) Build 15063 on your machine

2. Install on the same machine Windows 10 SDK (10.0.15063.0)


      https://developer.microsoft.com/en-us/windows/downloads/sdk-archive 


3. Install [DesktopAppConverter from Windows Store](https://www.microsoft.com/en-us/store/p/desktopappconverter/9nblggh4skzw) 

4. Download the base image 15063 from there http://aka.ms/converterimages

5. Install the base Image 15063. Launch DesktopAppConverter.exe and enter the following command in the command shell window:</p>


       DesktopAppConverter.exe -Setup -BaseImage C:\temp\BaseImage-15063.wim


6. Your machine is now configured to generate Application packages for Windows 10 Creator Update.


You can now generate the package for Geneatique application. As the application is only available as a Win32 (32 bits), the generated package will only support 32 bits binaries.</p> 

1. Copy the latest version of Geneatique Installation Application on your machine.</p>

2. Create the Win32 package with DesktopAppConverter, enter the following command in the command shell window:</p>


       DesktopAppConverter.exe -Installer C:\projects\geneatique\inputs\setup-geneatique2017.exe -InstallerArguments "/SILENT /NORESTART" -Destination "C:\projects\geneatique\outputs" -AppExecutable "Geneatique.exe" -PackageName "Geneatique2017" -PackageDisplayName "Généatique 2017" -AppDisplayName "Généatique 2017" -AppDescription  "Logiciel de Généalogie : Généatique 2017" -AppExecutable "Geneatique.exe" -Version 1.0.8.0 -Publisher "CN=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, OU=Secure Application Development, O=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, L=OSNY, S=Val-d''Oise, C=FR" - MakeAppx -Verbose  



3. After few minutes the new package is available under "C:\projects\geneatique\outputs\Geneatique2017.appx" . The appx file is built using the files under "C:\projects\geneatique\outputs\PackageFiles". For troubleshooting, you can use the log files under "C:\projects\geneatique\outputs\Logs".

4. You can update the package file for instance to use updated logos. In the command Window launch the following commands:</p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe" pack /d "C:\projects\geneatique\outputs\PackageFiles" /p "C:\projects\geneatique\outputs\Geneatique2017.appx"

5. The Appx file is now ready to be published, you only need to sign the package wit the company certificate using the following command line: </p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\signtool.exe" sign -f C:\projects\geneatique\outputs\Certificate.pfx -p [password] -fd SHA256 -v C:\projects\geneatique\outputs\Geneatique2017.appx


6. Now the Appx file is ready to be published. You can test the installation using the following Powershell command: </p>


       Add-AppxPackage -Path C:\projects\geneatique\outputs\Geneatique2017.appx



If you don't have a company certificate you can create your own test certificate using the command lines below:


1. Create a certificate using the command lines: </p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\makecert.exe" -r -h 0 -n "CN=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, OU=Secure Application Development, O=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, L=OSNY, S=Val-d''Oise, C=FR"  -eku 1.3.6.1.5.5.7.3.3 -pe -sv C:\projects\Geneatique\outputs\TempCert.pvk C:\projects\Geneatique\outputs\TempCert.cer


2. Create the pfx file using the command lines:</p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\pvk2pfx.exe" -pvk  C:\projects\Geneatique\outputs\TempCert.pvk -spc C:\projects\Geneatique\outputs\TempCert.cer -pfx C:\projects\Geneatique\outputs\TempCert.pfx


3. Once the pfx file is created, you can import the certificat. Double-click on the pfx file, select the « local machine »  for the Store Location and click on the Next button.</p>

![](/images/2017-06-23-geneatique/geneatique_ux_7.png)


4. Enter the password associated with your certificate and click on the Next button.</p>

![](/images/2017-06-23-geneatique/geneatique_ux_9.png)


5. Finally select « trusted people » certificate store, click on the Next button and Finish button to complete the import.</p>

![](/images/2017-06-23-geneatique/geneatique_ux_8.png)







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