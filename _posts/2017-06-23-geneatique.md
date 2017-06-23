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

### Overall Architecture

As the main objective of this initiative is to enhance the existing Mandarine services, the architecture of this deployment will take into account all the legacy components technologies used by Mandarine services.
For instance, the database server is based on MySQL Server and this server is hosted in Azure's datacenter. These training video files are hosted on a third-party streaming platform.</p>
On the technologies side, the servers are running Ubuntu, the Web Servers are based on Apache/PHP/Symfony. The backend services are developed using Node.js.The Web Servers will be hosted in Azure. 
The client application is actually a Web Application running in any browsers.</p>


1. Install the latest RS2 (Creator Update) Build 15063 on your PC running Windows 10
2. Install Windows 10 SDK (10.0.15063.0)https://developer.microsoft.com/en-us/windows/downloads/sdk-archive 

3. Install DesktopAppConverter from Windows Store
4. Download the base image 15063 from there http://aka.ms/converterimages

5. Install the base Image 15063   Launch DesktopAppConverter.exe and enter the following command in the command shell window:   DesktopAppConverter.exe -Setup -BaseImage C:\projects\VLCCentennial\inputs\BaseImage-15063.wim

6. Download the latest Win32 VLC build (vlc-3.0.0-<YYYYMMDD>-<XXXX>-git-win32.exe) and Win64 VLC build (vlc-3.0.0-<YYYYMMDD>-<XXXX>-git-win64.exe) from respectively   Win32   http://nightlies.videolan.org/build/win32/   Win64   http://nightlies.videolan.org/build/win64/

7. Create Win32 and Win64 Appx    Launch DesktopAppConverter.exe and enter the following command in the command shell window:   Win32DesktopAppConverter.exe -Installer C:\projects\VLCCentennial\inputs\binaries\vlc-3.0.0-20170517-0254-git-win32.exe -InstallerArguments "/S /V/qn" -AppExecutable "C:\Program Files (x86)\VideoLAN\VLC\VLC.exe" -Destination "C:\projects\VLCCentennial\outputs\17052017\VLCWin32" -PackageName VLC -Publisher CN=videolan -Version 0.0.0.1 -MakeAppx   Win64DesktopAppConverter.exe -Installer C:\projects\VLCCentennial\inputs\binaries\vlc-3.0.0-20170517-0454-git-win64.exe -InstallerArguments "/S /V/qn" -AppExecutable "C:\Program Files\VideoLAN\VLC\VLC.exe" -Destination "C:\projects\VLCCentennial\outputs\17052017\VLCWin64" -PackageName VLC -Publisher CN=videolan -Version 0.0.0.1 -MakeAppx

8. Update the package logos under (Add logos):    VLCWin32\VLC\PackageFiles\Assets   VLCWin64\VLC\PackageFiles\Assets

9. Update the Win32 and Win64 Appx with the new logos   In the command Window launch the following commands:   Win32"C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe" pack /d "C:\projects\VLCCentennial\outputs\17052017\VLCWin32\VLC\PackageFiles" /p "C:\projects\VLCCentennial\outputs\17052017\VLCWin32\VLC\VLC.appx“   Win64"C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe" pack /d "C:\projects\VLCCentennial\outputs\17052017\VLCWin64\VLC\PackageFiles" /p "C:\projects\VLCCentennial\outputs\17052017\VLCWin64\VLC\VLC.appx“
10. Create the certificate
"C:\Program Files (x86)\Windows Kits\10\bin\x64\makecert.exe" -r -h 0 -n "CN=videolan" -eku 1.3.6.1.5.5.7.3.3 -pe -sv C:\projects\VLCCentennial\outputs\17052017\TempCert.pvk C:\projects\VLCCentennial\outputs\17052017\TempCert.cer

11. Create the pfx file
"C:\Program Files (x86)\Windows Kits\10\bin\x64\pvk2pfx.exe" -pvk  C:\projects\VLCCentennial\outputs\17052017\TempCert.pvk -spc C:\projects\VLCCentennial\outputs\17052017\TempCert.cer -pfx C:\projects\VLCCentennial\outputs\17052017\TempCert.pfx

12. Import the certificat in « local machine » « trusted people » 

13. Sign the Win32 and Win64 Appx    In the command Window launch the following commands:    Win32"C:\Program Files (x86)\Windows Kits\10\bin\x64\signtool.exe" sign -f C:\projects\VLCCentennial\outputs\17052017\TempCert.pfx -fd SHA256 -v C:\projects\VLCCentennial\outputs\17052017\VLCWin32\VLC\VLC.appx    Win64 "C:\Program Files (x86)\Windows Kits\10\bin\x64\signtool.exe" sign -f C:\projects\VLCCentennial\outputs\17052017\TempCert.pfx -fd SHA256 -v C:\projects\VLCCentennial\outputs\17052017\VLCWin64\VLC\VLC.appx 

14. Install the Win32 and Win64 Appx    In the Powershell command Window launch the following commands:    Win32    Add-AppxPackage -Path C:\projects\VLCCentennial\outputs\17052017\VLCWin32\VLC\VLC.appx     Win64    Add-AppxPackage -Path C:\projects\VLCCentennial\outputs\17052017\VLCWin64\VLC\VLC.appx 

Préparation du Setup : 
 
1. le setup d’installation de l’application doit être généré en un seul fichier ( et non plusieur binaire) 2. Tous les modules exécutés depuis votre setup d'installation (Section [run] de l’iss si vous utilisez ‘Inno Setup’) doivent être lancer en mode SILENT et NORESTART, sinon la génération de l’appx reste bloquée. 
 
Configuration système requise : 
 
Pour générer un Appx vous aurez besoin d'un version de Windows 10 (Entreprise ou pro). Assurez-vous que vous utilisez la mise à jour anniversaire de Windows 10 (14393.0 ou versions ultérieures). 
 
Préparation de l'environnement : 
 
1. Installer DesktopConverter disponible ici : https://www.microsoft.com/en-us/store/p/desktopappconverter/9nblggh4skzw .  2. Installer Windows container en suivant les instructions : https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-st art-windows-10 3. Télécharger une image de base compatible avec votre version de windows installé  4. Exécuter "Desktop Bridge Converter" en tant qu'admin 5. Commencer par l'installation de l'environnement en utilisant l'image de base téléchargé     DesktopAppConverter.exe -Setup -BaseImage "C:\Téléchargement\BaseImage-<version>.win"  
Génération de l'appx : 
 1. Générer votre APPX avec la commande :  DesktopAppConverter.exe -Installer "C:\Votresetup.exe" -InstallerArgument "/SILENT /NORESTART" -Destination "c:\VotreSortie" -PackageName "Geneatique2017" -PackageDisplayName "Généatique 2017" -AppDisplayName "Généatique 2017" -AppDescription  "Logiciel de Généalogie : Généatique 2017" -AppExecutable "Geneatique.exe" -Version 1.0.8.0 -Publisher "CN=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, OU=Secure Application Development, O=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, L=OSNY, S=Val-d''Oise, C=FR" - MakeAppx -Verbose  
 
Une fois la génération de l'appx terminée. Dans le dossier de sortie vous aurez - Un dossier  "log" , - Un dossier "PackageFiles" , - Geneatique.appx > Le dossier "PackageFiles" contient l'ensemble des fichiers nécessaire à la génération de l'appx. Tous les autres appx seront généré uniquement à partire de ce dossier. 

 
Mise à jour de l'appx : 
 
 - Placer dans le dossier "PackageFiles" tous les fichiers modifiés puis régénérer l'appx avec la commande "MakeAppx"   makeappx pack -d "c:\VotreSortie\PackageFiles" -p "C:\VotreSortie\Geneatique.appx" 
 
Signature de l'appx: 
 
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