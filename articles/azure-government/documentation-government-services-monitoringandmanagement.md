---
title: "Azure Government-Überwachung und -Verwaltung | Microsoft Docs"
description: "Diese bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government."
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: 4b7720c1-699e-432b-9246-6e49fb77f497
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 3/13/2017
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 4d7de786dc902cb1c32e70a1f69bc74282de44f1
ms.lasthandoff: 03/14/2017


---
# <a name="azure-government-monitoring--management"></a>Azure Government-Überwachung und -Verwaltung
In diesem Artikel werden die Varianten und Überlegungen zu Überwachungs- und Verwaltungsdiensten für die Azure Government-Umgebung beschrieben.

## <a name="automation"></a>Automation
Automation ist allgemein in Azure Government verfügbar.

### <a name="variations"></a>Variationen
Die folgenden Automation-Features sind aktuell nicht in Azure Government verfügbar.

* Erstellen von Dienstprinzipal-Anmeldeinformationen zur Authentifizierung

Weitere Informationen finden Sie in der [öffentlichen Dokumentation zu Automation](../automation/automation-intro.md).

## <a name="backup"></a>Sicherung
Backup ist in Azure Government allgemein verfügbar.

Weitere Informationen finden Sie unter [Azure Government Backup](documentation-government-services-backup.md).

## <a name="resource-policy"></a>Ressourcenrichtlinie

[Azure-Ressourcenrichtlinien](../azure-resource-manager/resource-manager-policy.md) sind in Azure Government nicht verfügbar.

## <a name="site-recovery"></a>Site Recovery
Azure Site Recovery ist in Azure Government allgemein verfügbar.

Weitere Informationen finden Sie in der [öffentlichen Site Recovery-Dokumentation](../site-recovery/site-recovery-overview.md).

### <a name="variations"></a>Variationen
Die folgenden Site Recovery-Features sind aktuell nicht in Azure Government verfügbar:

* Azure Resource Manager-Site Recovery-Tresore
* E-Mail-Benachrichtigung

| Site Recovery | Klassisch | Ressourcen-Manager |
| --- | --- | --- |
| VMware/physisch  | Allgemein verfügbar | Allgemein verfügbar |
| Hyper-V | Allgemein verfügbar | Allgemein verfügbar |
| Standort-zu-Standort | Allgemein verfügbar | Allgemein verfügbar |

>[!NOTE]
>Die Tabelle gilt für „USA Gov Virginia“ und „USA Gov Iowa“.

Die folgenden URLs für Site Recovery lauten in Azure Government anders:

| Azure – Öffentlich | Azure Government | Hinweise |
| --- | --- | --- |
| \*.hypervrecoverymanager.windowsazure.com | \*.hypervrecoverymanager.windowsazure.us | Zugriff auf den Site Recovery-Dienst |
| \*.backup.windowsazure.com  | \*.backup.windowsazure.us | Zugriff auf Protection Service |
| \*.blob.core.windows.net | \*.blob.core.usgovcloudapi.net | Zum Speichern der VM-Momentaufnahmen |
| http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi | http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi | Zum Herunterladen von MySQL |

## <a name="log-analytics"></a>Log Analytics
Log Analytics ist allgemein in Azure Government verfügbar.

### <a name="variations"></a>Variationen
Die folgenden Log Analytics-Features und -Lösungen sind aktuell nicht in Azure Government verfügbar.

* Lösungen, die als Vorschau in Microsoft Azure enthalten sind, einschließlich:
  * Netzwerküberwachungslösung
  * Dienstzuordnung
  * Office 365-Lösung
  * Windows 10 Upgrade Analytics-Lösung
  * Application Insights-Lösung
  * Azure Networking Analytics-Lösung
  * Azure Automation Analytics-Lösung
  * Key Vault Analytics-Lösung
* Lösungen und Funktionen, die Updates von lokaler Software erfordern, einschließlich:
  * Surface Hub-Lösung
* Funktionen, die als Vorschau im öffentlichen Azure enthalten sind, einschließlich:
  * Exportieren von Daten nach Power BI
* Azure-Metriken und Azure-Diagnose
* Operations Management Suite - mobile Anwendungen

Die URLs für Log Analytics unterscheiden sich in Azure Government:

| Azure – Öffentlich | Azure Government | Hinweise |
| --- | --- | --- |
| mms.microsoft.com |oms.microsoft.us |Log Analytics-Portal |
| *workspaceId*.ods.opinsights.azure.com |*workspaceId*.ods.opinsights.azure.us |[Data collector API (Datensammler-API)](../log-analytics/log-analytics-data-collector-api.md) |
| \*.ods.opinsights.azure.com |\*.ods.opinsights.azure.us |Agent-Kommunikation – [Konfigurieren von Firewalleinstellungen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*.oms.opinsights.azure.com |\*.oms.opinsights.azure.us |Agent-Kommunikation – [Konfigurieren von Firewalleinstellungen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*.blob.core.windows.net |\*.blob.core.usgovcloudapi.net |Agent-Kommunikation – [Konfigurieren von Firewalleinstellungen](../log-analytics/log-analytics-proxy-firewall.md) |

Die folgenden Log Analytics-Features verhalten sich in Azure Government anders:

* Zum Verbinden Ihres System Center Operations Manager-Verwaltungsservers mit Log Analytics müssen Sie aktualisierte Management Packs herunterladen und importieren.
  + System Center Operations Manager 2016
    1. Installieren Sie [Updaterollup 2 für System Center 2016 Operations Manager](https://support.microsoft.com/help/3209591).
    2. Importieren Sie die Management Packs, die in Updaterollup 2 enthalten sind, in Operations Manager. Informationen zum Importieren eines Management Packs von einem Datenträger finden Sie unter [How to Import an Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) (Importieren eines Management Packs von Operations Manager).
    3. Zum Verbinden von Operations Manager mit Log Analytics führen Sie die Schritte in [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md) aus.
  + System Center Operations Manager 2012 R2 UR3 (oder höher) / Operations Manager 2012 SP1 UR7 (oder höher)
    1. Laden Sie die [aktualisierten Management Packs](http://go.microsoft.com/fwlink/?LinkId=828749) herunter, und speichern Sie sie.
    2. Entzippen Sie die heruntergeladene Datei.
    3. Importieren Sie die Management Packs in Operations Manager. Informationen zum Importieren eines Management Packs von einem Datenträger finden Sie unter [How to Import an Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) (Importieren eines Management Packs von Operations Manager).
    4. Zum Verbinden von Operations Manager mit Log Analytics führen Sie die Schritte in [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md) aus.
  
* Um [Computergruppen aus System Center Configuration Manager 2016](../log-analytics/log-analytics-sccm.md) verwenden zu können, müssen Sie die [Technical Preview 1701](https://docs.microsoft.com/en-us/sccm/core/get-started/technical-preview) oder höher verwenden.

### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
* Kann ich Daten aus Log Analytics in Microsoft Azure zu Azure Government migrieren?
  * Nein. Es ist nicht möglich, Daten oder Ihren Arbeitsbereich aus Microsoft Azure in Azure Government zu verschieben.
* Kann ich zwischen Microsoft Azure- und Azure Government-Arbeitsbereichen aus dem Operations Management Suite Log Analytics-Portal wechseln?
  * Nein. Die Portale für Microsoft Azure und Azure Government sind getrennt, und nutzen Informationen nicht gemeinsam.

Weitere Informationen finden Sie in der [öffentlichen Log Analytics-Dokumentation](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

