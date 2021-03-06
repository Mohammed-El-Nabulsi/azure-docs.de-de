---
title: "H264 Single Bitrate Low Quality SD für Android | Microsoft Docs"
description: "Das Thema enthält eine Übersicht über die Aufgabenvoreinstellung **H264 Single Bitrate Low Quality SD für Android**."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 67d3446d-e3b8-419f-bbf8-e5149c108531
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 55bfd5b283e6159e0a06b3107de31d1722796bf1
ms.openlocfilehash: 37f3a2b50b437fd675c17a26a143eb649fb6c857


---

# <a name="h264-single-bitrate-low-quality-sd-for-android"></a>H264 Single Bitrate Low Quality SD für Android
`Media Encoder Standard` definiert eine Reihe von Codierungsvoreinstellungen, die Sie beim Erstellen von Codierungsaufträgen verwenden können. Sie können mithilfe von `preset name` angeben, in welchem Format Ihre Mediendatei codiert werden soll. Oder Sie erstellen eigene JSON- oder XML-basierte Voreinstellungen (mithilfe von UTF-8- oder UTF-16-Codierung). In diesem Fall übergeben Sie die benutzerdefinierte Voreinstellung dann an den Encoder. Eine Liste aller von diesem `Media Encoder Standard`-Encoder unterstützten Voreinstellungsnamen finden Sie unter [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md) (Aufgabenvoreinstellungen für Media Encoder Standard).  
  
 In diesem Thema wird die Voreinstellung `H264 Single Bitrate Low Quality SD for Android` im XML- und JSON-Format gezeigt.  
  
 Diese Voreinstellung erzeugt eine einzelne MP4-Datei mit einer Bitrate von 56KBit/s und AAC-Stereoaudio. Ausführliche Informationen zu Profil, Bitrate, Samplingrate usw. dieser Voreinstellung finden Sie im unten aufgeführten XML- bzw. JSON-Code. Erläuterungen zur Bedeutung der einzelnen Elemente in diesen Voreinstellungen sowie gültige Werte für jedes Element finden Sie im Thema [Media Encoder Standard schema](media-services-mes-schema.md) (Media Encoder Standard-Schema).  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>56</Bitrate>  
          <Width>176</Width>  
          <Height>144</Height>  
          <FrameRate>12/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>2</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>56</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>HEAACV2</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>24</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "2",  
          "Bitrate": 56,  
          "MaxBitrate": 56,  
          "BufferWindow": "00:00:05",  
          "Width": 176,  
          "Height": 144,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "12/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "HEAACV2",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 24,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```


<!--HONumber=Nov16_HO5-->


