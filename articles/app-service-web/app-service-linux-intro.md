---
title: "Einführung in App Service unter Linux | Microsoft Docs"
description: "Enthält Informationen zu App Service unter Linux."
keywords: Azure App Service, Linux, OSS
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 9ed502efadbc27ca507fa5be455f32c3bd1b9eee
ms.lasthandoff: 03/10/2017


---
# <a name="introduction-to-app-service-on-linux"></a>Einführung in App Service unter Linux
Azure App Service unter Linux befindet sich derzeit in der öffentlichen Vorschauphase und unterstützt die native Ausführung von Web-Apps unter Linux.

## <a name="overview"></a>Übersicht
Kunden können App Service unter Linux verwenden, um Web-Apps für unterstützte Anwendungsstapel nativ unter Linux zu hosten. Im folgenden Abschnitt sind die Anwendungsstapel aufgeführt, die derzeit unterstützt werden. 

## <a name="features"></a>Features
App Service unter Linux unterstützt derzeit die folgenden Anwendungsstapel:

* Node.js
    * 4.5.0
    * 4.4.7
    * 6.2.2
    * 6.6.0
    * 6.9.3
* PHP
    * 5.6.23
    * 7.0.6
* .NET Core
    * 1,0
* Ruby
    * 2.3

Kunden haben folgende Möglichkeiten, um ihre Anwendungen bereitzustellen:

* FTP
* Lokales Git
* GitHub
* Bitbucket

Für die Anwendungsskalierung:

* Kunden können Web-Apps zentral hoch- und herunterskalieren, indem sie die Ebene in ihrem App Service-Plan ändern.
* Kunden können Anwendungen horizontal hochskalieren und mehrere App-Instanzen innerhalb der Grenzen ihrer SKU ausführen.

Für Kudu können einige grundlegende Funktionen für Folgendes verwendet werden:

* Umgebungen
* Bereitstellungen
* Grundlegende Konsolen

## <a name="limitations"></a>Einschränkungen
Im Azure-Portal werden nur Features angezeigt, die für App Service unter Linux derzeit funktionieren. Die restlichen Features werden ausgeblendet. Sobald wir weitere Features aktivieren, werden sie im Portal angezeigt.

Einige Features, z.B. die Integration virtueller Netzwerke, Azure Active Directory/Drittanbieter-Authentifizierung oder Kudu-Websiteerweiterungen, sind noch nicht fertiggestellt. Sobald sich dies ändert, aktualisieren wir die Dokumentation entsprechend und veröffentlichen Blog-Beiträge zu den Änderungen.

Diese öffentliche Vorschau ist derzeit nur in den folgenden Regionen verfügbar:

* USA (West)
* Westeuropa 
* Südostasien

Web-Apps unter Linux wird nur in den App Service-Plänen unterstützt und verfügt nicht über einen Free- oder Shared-Tarif. Außerdem schließen sich App Service-Pläne für reguläre Web-Apps und Linux-Web-Apps gegenseitig aus, sodass es nicht möglich ist, eine Linux-Web-App in einem anderen Plan zu erstellen, bei dem es sich nicht um einen Linux-App Service-Plan handelt.

Web-Apps unter Linux müssen in einer Ressourcengruppe erstellt werden, die in derselben Region keine anderen Web-Apps als Linux-Web-Apps enthält.

Web-Apps unter Linux unterstützen noch keine Bereitstellung von .NET Core-Apps aus einer nicht kompilierten Quelle. Sie müssen Ihre .NET Core-App zuerst lokal veröffentlichen/kompilieren und dann die Teile der veröffentlichten Website an Ihre App übertragen.

## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links erhalten Sie Informationen zu den ersten Schritten mit App Service unter Linux. In [unserem Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.

* [Erstellen von Web-Apps in App Service unter Linux](app-service-linux-how-to-create-a-web-app.md)
* [Verwenden eines benutzerdefinierten Docker-Image für App Service unter Linux](app-service-linux-using-custom-docker-image.md)
* [Verwenden der PM2-Konfiguration für Node.js in Web-Apps unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwendung von .NET Core in Azure App Service-Web-Apps unter Linux](app-service-linux-using-dotnetcore.md)
* [Verwendung von Ruby in Azure App Service-Web-Apps unter Linux](app-service-linux-using-ruby.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)


