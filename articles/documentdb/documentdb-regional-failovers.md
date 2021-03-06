---
title: Regionale Failover in Azure DocumentDB | Microsoft-Dokumentation
description: Informationen zur Funktionsweise von manuellen und automatischen Failovern in Azure DocumentDB.
services: documentdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 4af4d30a3378e1aea66309a1d757be1c1da2ea0d
ms.lasthandoff: 03/07/2017


---
# <a name="automatic-regional-failovers-for-business-continuity-in-documentdb"></a>Automatisches regionales Failover für die Geschäftskontinuität in DocumentDB
Azure DocumentDB vereinfacht die globale Verteilung von Daten, indem es vollständig verwaltete [Datenbankkonten in mehreren Regionen](documentdb-distribute-data-globally.md) bietet, die für eine sorgfältig austarierte Balance zwischen Konsistenz, Verfügbarkeit und Leistung mit den entsprechenden Garantien sorgen. DocumentDB-Konten bieten hohe Verfügbarkeit, Latenzen im einstelligen Millisekundenbereich, mehrere [klar abgegrenzte Konsistenzebenen](documentdb-consistency-levels.md), transparentes regionales Failover mit Multihosting-APIs sowie die Fähigkeit, Durchsatz und Speicher für alle Konten weltweit elastisch zu skalieren. 

Azure DocumentDB unterstützt sowohl explizite als auch richtlinienbasiere Failover, mit denen Sie das Verhalten des gesamten Systems im Fall von Fehlern steuern können. In diesem Artikel wird Folgendes erläutert:

* Wie funktionieren manuelle Failover in DocumentDB?
* Wie funktionieren automatische Failover in DocumentDB?
* Wie können Sie manuelle Failover in Anwendungsarchitekturen verwenden?

Informationen zu regionalen Failovern erhalten Sie auch in diesem Azure Friday-Video mit Scott Hanselman und Karthik Raman, dem Principal Engineering Manager.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Konfigurieren von Anwendungen für mehrere Regionen
Bevor wir uns mit Failovermodi beschäftigen, sehen wir uns an, wie Sie eine Anwendung so konfigurieren, dass diese von der Verfügbarkeit in mehreren Regionen profitiert und im bei einem regionalen Failover nicht ausfällt.

* Zuerst stellen Sie die Anwendung in mehreren Regionen bereit.
* Um einen Zugriff mit geringer Latenz aus jeder Region sicherzustellen, in der Ihre Anwendung bereitgestellt ist, konfigurieren Sie die entsprechende [Liste bevorzugter Regionen](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) für jede Region über eines der unterstützten SDKs.

Der folgende Codeausschnitt zeigt, wie Sie eine Anwendung in mehreren Regionen initialisieren. Hier wird das DocumentDB-Konto `contoso.documents.azure.com` für zwei Regionen konfiguriert: „USA, Westen“ und „Europa, Norden“. 

* Die Anwendung wird in der Region „USA, Westen“ bereitgestellt (z.B. mithilfe von Azure App Services). 
* Sie wird mit `West US` als erste bevorzugte Region für Lesevorgänge mit geringer Latenz konfiguriert.
* Sie wird mit `North Europe` als zweite bevorzugte Region für hohe Verfügbarkeit während regionaler Failover konfiguriert.

In .NET sieht die Konfiguration wie der folgende Codeausschnitt aus:

    ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
    { 
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    };

    usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
    usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient usClient = new DocumentClient(
        new Uri("https://contosodb.documents.azure.com"),
        "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
        usConnectionPolicy);

Die Anwendung wird auch in der Region „Europa, Norden“ bereitgestellt, mit umgekehrter Reihenfolge der bevorzugten Regionen. Es wird also „Europa, Norden“ als erste Region für Lesevorgänge mit geringer Latenz konfiguriert. Danach wird „USA, Westen“ als zweite bevorzugte Region für hohe Verfügbarkeit während regionaler Failover konfiguriert.

Das folgende Architekturdiagramm zeigt eine Anwendungsbereitstellung in mehreren Regionen, in der DocumentDB und die Anwendung so konfiguriert sind, dass sie in vier geografischen Azure-Regionen verfügbar sind.  

![Global verteilte Anwendungsbereitstellung mit Azure DocumentDB](./media/documentdb-regional-failovers/app-deployment.png)

Sehen wir uns nun an, wie der DocumentDB-Dienst regionale Ausfälle mithilfe automatischer Failover verarbeitet. 

## <a id="AutomaticFailovers"></a>Automatische Failover
Im seltenen Fall des Ausfalls einer Azure-Region löst DocumentDB automatisch ein Failover aller DocumentDB-Konten aus, die sich in der betroffenen Region befinden. 

**Was passiert beim Ausfall einer Leseregion?**

DocumentDB-Konten mit einer Leseregion in einer der betroffenen Regionen werden automatisch von ihrer Schreibregion getrennt und als offline gekennzeichnet. Die DocumentDB-SDKs implementieren ein Protokoll für die Regionsermittlung, mit dessen Hilfe sie automatisch erkennen, wenn eine Region verfügbar ist, und Leseaufrufe an die nächste verfügbar Region in der Liste der bevorzugten Regionen umleiten können. Wenn keine der Regionen in der Liste verfügbar ist, wird für die Aufrufe automatisch ein Fallback zur aktuellen Schreibregion durchgeführt. Zur Verarbeitung regionaler Failover sind keine Änderungen an Ihrem Anwendungscode erforderlich. Während des gesamten Prozesses werden Konsistenzgarantien von DocumentDB weiterhin erfüllt.

![Ausfälle von Leseregionen in Azure DocumentDB](./media/documentdb-regional-failovers/read-region-failures.png)

Sobald die betroffene Region nach dem Ausfall wiederhergestellt wurde, werden alle betroffenen DocumentDB-Konten in der Region vom Dienst automatisch wiederhergestellt. DocumentDB-Konten, die über eine Leseregion in der betroffenen Region verfügen, werden automatisch mit der aktuellen Schreibregion synchronisiert und wieder online geschaltet. Die DocumentDB-SDKs ermitteln die Verfügbarkeit der neuen Region und bewerten anhand der von der Anwendung konfigurierten Liste der bevorzugten Regionen, ob die Region als aktuelle Leseregion ausgewählt werden soll. Nachfolgende Lesevorgänge werden an die wiederhergestellte Region weitergeleitet, ohne dass Änderungen an Ihrem Anwendungscode erforderlich sind.

**Was passiert beim Ausfall einer Schreibregion?**

Wenn die betroffene Region die aktuelle Schreibregion für ein bestimmtes Azure DocumentDB-Konto ist, wird die Region automatisch als offline gekennzeichnet. Danach wird eine andere Region zur Schreibregion für jedes betroffene DocumentDB-Konto hochgestuft. Sie können die Reihenfolge der Regionsauswahl für Ihre DocumentDB-Konten vollständig über das Azure-Portal oder [programmgesteuert](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange) steuern. 

![Failoverprioritäten für Azure DocumentDB](./media/documentdb-regional-failovers/failover-priorities.png)

Während eines automatischen Failovers wählt DocumentDB anhand der angegebenen Prioritätsreihenfolge automatisch die nächste Schreibregion für ein bestimmtes Azure DocumentDB-Konto. 

![Ausfälle von Schreibregionen in Azure DocumentDB](./media/documentdb-regional-failovers/write-region-failures.png)

Sobald die betroffene Region nach dem Ausfall wiederhergestellt wurde, werden alle betroffenen DocumentDB-Konten in der Region vom Dienst automatisch wiederhergestellt. 

* DocumentDB-Konten, deren vorherige Schreibregion sich in der betroffenen Region befand, bleiben auch nach der Wiederherstellung der Region in einem Offlinemodus mit Leseverfügbarkeit. 
* Sie können diese Region abfragen, um während des Ausfalls nicht replizierte Schreibvorgänge zu verarbeiten, indem diese mit den in der aktuellen Schreibregion verfügbaren Daten verglichen werden. Je nach den Anforderungen Ihrer Anwendung können Sie Daten zusammenführen und/oder Konflikte auflösen und den finalen Satz an Änderungen dann in die aktuelle Schreibregion schreiben. 
* Nachdem Sie das Zusammenführen von Änderungen abgeschlossen haben, können Sie die betroffene Region wieder online schalten, indem Sie die Region aus Ihrem DocumentDB-Konto entfernen und diesem danach wieder hinzufügen. Sobald die Region wieder hinzugefügt wurde, können Sie sie erneut als Schreibregion konfigurieren, indem Sie über das Azure-Portal oder [programmgesteuert](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate) ein manuelles Failover durchführen.

## <a id="ManualFailovers"></a>Manuelle Failover

Zusätzlich zu automatischen Failovern kann die aktuelle Schreibregion eines bestimmten DocumentDB-Kontos manuell und dynamisch in eine der vorhandenen Leseregionen geändert werden. Manuelle Failover können über das Azure-Portal oder [programmgesteuert](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate) initiiert werden. 

Manuelle Failover stellen sicher, dass **kein Datenverlust** und **kein Verfügbarkeitsverlust** auftreten, und übertragen den Schreibzugriff ordnungsgemäß aus der alten Schreibregion in die neue Schreibregion des angegebenen DocumentDB-Kontos. Ähnlich wie bei automatischen Failovern verarbeitet das Azure DocumentDB-SDK Änderungen bei Schreibregionen während eines manuellen Failovers automatisch und stellt sicher, dass Aufrufe automatisch an die neue Schreibregion umgeleitet werden. Zum Verwalten von Failovern sind keine Code- oder Konfigurationsänderungen in Ihrer Anwendung erforderlich. 

![Manuelle Failover in Azure DocumentDB](./media/documentdb-regional-failovers/manual-failovers.png)

Im Folgenden finden Sie einige häufige Szenarien, in denen ein manuelles Failover hilfreich sein kann:

**„Follow the clock“-Modell**: Wenn Ihre Anwendungen über vorhersagbare Datenverkehrsmuster basierend auf der Tageszeit verfügen, können Sie den Schreibstatus in regelmäßigen Abständen auf die aktivste geografische Region ändern, ebenfalls basierend auf der Tageszeit.

**Dienstupdate**: Bestimmte global verteilte Anwendungsbereitstellungen umfassen möglicherweise eine Weiterleitung des Datenverkehrs an eine andere Region über den Traffic Manager während geplanter Dienstupdates. In diesen Anwendungsbereitstellungen können jetzt manuelle Failover durchgeführt werden, um den Schreibstatus in der Region beizubehalten, in der während des Dienstupdatefensters aktiver Datenverkehr stattfinden wird.

**BCDR-Tests (Business Continuity und Disaster Recovery)**: Die Entwicklungs- und Veröffentlichungsprozesse der meisten Unternehmensanwendungen umfassen Business Continuity-Tests. BCDR-Tests sind häufig ein wichtiger Schritt bei Kompatibilitätszertifizierungen und beim Garantieren der Dienstverfügbarkeit bei Regionsausfällen. Sie können die BCDR-Fähigkeit Ihrer Anwendungen, die DocumentDB für die Speicherung verwenden, testen, indem Sie ein manuelles Failover Ihres DocumentDB-Kontos auslösen und/oder eine Region dynamisch hinzufügen oder entfernen.

In diesem Artikel wurde erläutert, wie manuelle und automatische Failover in Azure DocumentDB funktionieren und wie Sie Ihre DocumentDB-Konten und -Anwendungen so konfigurieren, dass sie global verfügbar sind. Mithilfe der globalen Replikationsunterstützung von Azure DocumentDB können Sie die End-to-End-Latenz verbessern und sicherstellen, dass die Hochverfügbarkeit auch bei Regionsausfällen erhalten bleibt. 

## <a id="NextSteps"></a>Nächste Schritte
* Erfahren Sie, wie DocumentDB die [globale Verteilung](documentdb-distribute-data-globally.md) unterstützt.
* Erfahren Sie mehr über die [globale Konsistenz bei DocumentDB](documentdb-consistency-levels.md).
* Entwickeln Sie Ihre Umgebung mit mehreren Regionen mit dem [Azure DocumentDB-SDK](documentdb-developing-with-multiple-regions.md).
* Erfahren Sie, wie Sie mit Azure DocumentDB [Schreibarchitekturen mit mehreren Regionen](documentdb-multi-region-writers.md) erstellen.


