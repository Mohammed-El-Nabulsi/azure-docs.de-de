---
title: Neuigkeiten in Azure RemoteApp | Microsoft Docs
description: "Erfahren Sie, welche Änderungen und Verbesserungen an Azure RemoteApp vorgenommen wurden."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 40f1ba79-80f1-47bd-bf39-f86c03e2306a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: e4d94d3f9736378d93e93be6645ed04ade763ca3
ms.openlocfilehash: 05487c4bbdce24ea9f31ed451026531c16ead7c4


---
# <a name="whats-new-in-azure-remoteapp"></a>Neuigkeiten in Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Einer der Vorteile von Azure RemoteApp besteht darin, dass wir ständig an Verbesserungen arbeiten. Hier werden Sie über solche Änderungen informiert.

## <a name="future-updates"></a>Zukünftige Updates
Wussten Sie, dass das Azure RemoteApp-Team monatlich Updates im RDS-Blog veröffentlicht? Sie erfahren nicht nur, was sich in Azure RemoteApp ändert, sondern erhalten auch andere Informationen zur Verwendung von RDS. Weitere Informationen finden Sie im [Remote Desktop Services-Blog](https://blogs.msdn.microsoft.com/rds/). Vor einigen Wochen wurde z.B. ein Eintrag zum [Anheben und Verschieben von Workloads mit Azure RemoteApp und Azure AD](https://blogs.msdn.microsoft.com/rds/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services/) veröffentlicht.

## <a name="september-2015"></a>September 2015
* InfoPath wurde der Microsoft Office 365-Vorlage und dem Katalogimage hinzugefügt. Wenn Sie InfoPath freigeben möchten, müssen Sie unbedingt Ihre Sammlungen mit dem aktuellen Image aktualisieren.
* Clientupdates:
  * Der Windows-Client wurde aktualisiert, damit Benutzer Feedback abgeben können, insbesondere bei Verbindungsproblemen.
  * Der iOS-Client wurde aktualisiert, sodass Fehlermeldungen korrigiert wurden und ein Problem behoben wurde, bei dem Ihre Anmeldeinformationen früher als erwartet abliefen.
* Wir arbeiten daran, den Support für Office 2016 zu testen. Sobald dies abgeschlossen ist, können Sie nach aktualisierten Images Ausschau halten.
* Ein neuer Artikel zu den [Unterschieden zwischen Cloud- und Hybridsammlungen](remoteapp-collections.md) wurde veröffentlicht. Dieser hilft beim Auswählen des Sammlungstyps, der für Ihre Apps am besten geeignet ist: nur Cloud, Cloud + VNET oder Hybrid.
* Möchten Sie QuickBooks mithilfe von Azure RemoteApp freigeben, sind sich jedoch nicht über den Ablauf sicher? Sehen Sie sich [Erics neuen Artikels](remoteapp-quickbooks.md) an, in dem Sie die genauen Schritte erfahren.

## <a name="august-2015"></a>August 2015
Im August gab es große Änderungen – hier sind die wichtigsten:

* Sie können Azure-VNETs nun mit einer Cloudsammlung verwenden! Sehen Sie sich die [Anleitung zur Clouderstellung](remoteapp-create-cloud-deployment.md) an, in der Sie die neuen Schritte erfahren.
* Es wurde ermöglicht, dem **Startmenü **Apps für den Windows RemoteApp-Client hinzuzufügen. Apps werden in der Anwendungsliste angezeigt, und Sie können sie dem **Startmenü **in Windows anheften.
* Ein neues Image wurde dem Azure-VM-Katalog hinzugefügt – Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus.
* Der Mac-Client wurde korrigiert, sodass Apps mit modalen Fenstern nicht mehr einfrieren.
* Die Verwendung Ihres [Office 365 ProPlus-Abonnements](remoteapp-officesubscription.md) mit Azure RemoteApp wurde dokumentiert.
* Es wird ausführlich erläutert, wie Sie [Apps und Daten](remoteapp-secure.md) in Ihrer Azure RemoteApp-Sammlung sichern können.

## <a name="july-2015"></a>Juli 2015
Im Juli wurden die für August geplanten Änderungen vorbereitet, deshalb gibt es neben verschiedenen Aktualisierungen an der Dokumentation nicht viel zu berichten. Nachfolgend werden die neuesten Änderungen aufgeführt:

* Dem Portal wurde eine Registerkarte **Support** hinzugefügt, um einen leichteren Zugang zu Supportressourcen (z. B. den Foren) zu ermöglichen.
* Die Informationen zur Problembehandlung beim Erstellen einer Hybridsammlung wurden überarbeitet. Sehen Sie sich die [neuesten und besten](remoteapp-hybridtrouble.md) Tipps zur Problembehandlung an, beispielsweise Informationen dazu, wie Sie die richtigen Ports für Ihr VNET ermitteln.
* Die Dokumentation wurde um Informationen dazu ergänzt, wie [Benutzerdaten](remoteapp-upd.md) in Azure RemoteApp erstellt und gespeichert werden.
* Die Dokumentation wurde um Informationen dazu ergänzt, wie [Apps gesperrt werden](remoteapp-secure.md).
* Die [Cmdlets für Azure RemoteApp](https://msdn.microsoft.com/library/mt428031.aspx)wurden veröffentlicht.
* Und schließlich wurde eine Konversation über die Terminologie mit einigen Azure RemoteApp-Benutzern gestartet. Sehen Sie sich an, inwiefern die Verweise auf verschiedene Sammlungsoptionen geändert wurden.

## <a name="june-2015"></a>Juni 2015
Es gibt so viele Änderungen! Das Team war im Juni sehr aktiv:

* Die [Angebotsseite](https://www.remoteapp.windowsazure.com/) von Azure RemoteApp wurde überarbeitet - sehen sie selbst!
* Die Software wurde in allen Images aktualisiert, die Ihnen im Rahmen Ihres Abonnements zur Verfügung stehen.
* An Hybridsammlungen wurden Verbesserungen vorgenommen, einschließlich der Unterstützung von erzwungener Tunnelung und der Überprüfung der Größe von IP-Subnetzen vor dem Erstellen der Sammlung.
* Es wurde festgestellt, dass der Platzhalter * für Webcams nicht funktioniert. Stattdessen müssen Sie die Instanz-ID oder GUID angeben. Die Umleitungsinformationen werden entsprechend aktualisiert.
* Sie können dem Image benutzerdefinierte Antivirensoftware hinzufügen, wenn Sie ein Vorlagenimage aus dem Azure-Katalog erstellen.

Weitere Änderungen sind für Juli geplant, sodass es bald ein neues Update gibt.

## <a name="may-2015"></a>Mai 2015
Die Erstellung dieses Themas ist bereits einige Erweiterungen (und Monate) her, sodass diese Liste eigentlich die Änderungen von Anfang März bis Mai enthält. Sehen Sie sich diese neuen Features an:

* Automatisieren Sie alles: Azure RemoteApp verfügt jetzt über [Cmdlets im Azure PowerShell-Modul](remoteapp-tutorial-arawithpowershell.md).
* [Erstellen Sie ein Azure RemoteApp-Image von einem virtuellen Azure-Computer](remoteapp-image-on-azurevm.md). Dadurch wird das Hochladen Ihres benutzerdefinierten Images beschleunigt.
* Verwenden Sie ein Azure-VNET statt eines RemoteApp-VNET, um die Ressourcen Ihres Unternehmensnetzwerks mit Azure zu verbinden. Die [Anweisungen für Hybridsammlungen](remoteapp-create-hybrid-deployment.md) wurden aktualisiert, um Ihnen die Erstellung eines Azure-VNET zu erläutern (Schritt 1).
* Apropos VNETs, sehen Sie sich die [neuen Anleitungen](remoteapp-vnetsizing.md) im Zusammenhang mit VNET-Größenlimits und -Einschränkungen an.
* Und bezüglich Einschränkungen: Was sind die [Diensteinschränkungen und Standardwerte](../azure-subscription-service-limits.md)?

Sie möchten mehr über Azure RemoteApp erfahren? Das RemoteApp-Team war vor einigen Wochen auf der Ignite. Sehen Sie sich das Video von Eric an: [The Fundamentals of Microsoft Azure RemoteApp Management and Administration](http://channel9.msdn.com/Events/Ignite/2015/BRK3868)(in englischer Sprache).

Möchten Sie Azure RemoteApp in der Praxis testen? Sehen Sie sich das Tutorial [Ausführen einer beliebigen App auf einem beliebigen Gerät](remoteapp-anyapp.md) an. Es zeigt, wie Access für die Benutzer freigegeben wird, einschließlich der Freigabe von Datenbankdateien. Es gibt auch ein Tutorial, das zeigt, wie [Office 365](remoteapp-tutorial-o365anywhere.md) auf jedem Gerät gleich ausgeführt wird.

Vielen Dank, dass Sie uns treu bleiben. Im nächsten Monat gibt es weitere Updates.

### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen zu helfen
Wussten Sie schon, dass Sie diesen Artikel im Bereich unten nicht nur bewerten und kommentieren, sondern ihn auch selbst ändern können? Fehlt etwas? Ist etwas nicht ganz richtig? Habe ich etwas geschrieben, das eher verwirrend ist? Scrollen Sie nach oben, und klicken Sie auf **Edit on GitHub**, um die gewünschten Änderungen vorzunehmen. Ihr Vorschlag wird uns vorgelegt, und wenn wir ihn bestätigt haben, werden Ihre Änderungen und Verbesserungen hier angezeigt.




<!--HONumber=Dec16_HO2-->


