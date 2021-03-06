---
title: Aktivieren von Speichermetriken im Azure-Portal | Microsoft Docs
description: "Aktivieren von Speichermetriken für die Blob-, Warteschlangen-, Tabellen- und Dateidienste"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 2fb5b229-f099-4334-92be-4e0e7dd257d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 3c3c7924655f5b932be2a3d947a4df786fc0630d
ms.lasthandoff: 03/06/2017


---
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Aktivieren der Speichermetriken und Anzeigen von Metrikdaten
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Übersicht
Speichermetriken sind standardmäßig aktiviert, wenn Sie ein neues Speicherkonto erstellen. Sie können die Überwachung mithilfe des [klassischen Azure-Portals](https://manage.windowsazure.com), über Windows PowerShell oder programmgesteuert über eine Speicher-API konfigurieren.

Wenn Sie Speichermetriken aktivieren, müssen Sie einen Aufbewahrungszeitraum für die Daten auswählen: Dieser Zeitraum bestimmt, wie lange der Speicherdienst die Metriken beibehält und Speicherplatz abgerechnet wird, der für ihre Speicherung erforderlich ist. Normalerweise sollten Sie einen kürzeren Aufbewahrungszeitraum für minütliche Metriken als für stündliche Metriken auswählen, weil für minütliche Metriken eine erhebliche Menge an zusätzlichem Speicherplatz erforderlich ist. Sie sollten den Aufbewahrungszeitraum so auswählen, dass ausreichend Zeit zum Analysieren der Daten und zum Herunterladen von Metriken verfügbar ist, die Sie für die Offlineanalyse oder zur Berichterstellung verwenden möchten. Denken Sie daran, dass auch für das Herunterladen von Metrikdaten aus Ihrem Speicherkonto Kosten anfallen.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Aktivieren von Speichermetriken über das klassische Azure-Portal
Im [klassischen Azure-Portal](https://manage.windowsazure.com)verwenden Sie die Seite "Konfigurieren" für ein Speicherkonto, um die Speichermetriken zu steuern. Für die Überwachung können Sie für Blobs, Tabellen und Warteschlangen jeweils eine Stufe sowie einen Aufbewahrungszeitraum in Tagen festlegen. In jedem Fall sind die folgenden Stufen verfügbar:

* Aus – Es werden keine Metriken erfasst.
* Minimal – Die Speichermetriken erfassen eine Gruppe von Basismetriken, etwa Eingang/Ausgang, Verfügbarkeit, Latenz und Erfolg in Prozent, die für die Blob-, Tabellen- und Warteschlangendienste aggregiert werden.
* Ausführlich – Die Speichermetriken erfassen einen vollständigen Satz an Metriken, die zusätzlich zu den Metriken auf Dienstebene die gleichen Metriken für jeden API-Speichervorgang umfasst. Ausführliche Metriken ermöglichen eine genauere Analyse von Problemen, die bei Anwendungsvorgängen auftreten.

Beachten Sie, dass das klassische Azure-Portal zurzeit die Konfiguration von minütlichen Metriken in Ihrem Speicherkonto nicht unterstützt. Sie müssen minütliche Metriken mithilfe von PowerShell oder programmgesteuert aktivieren.

## <a name="how-to-enable-storage-metrics-using-powershell"></a>Aktivieren von Speichermetriken mithilfe von PowerShell
Sie können PowerShell auf Ihrem lokalen Computer zum Konfigurieren der Speichermetriken in Ihrem Speicherkonto verwenden, indem Sie das Azure PowerShell-Cmdlet "Get-AzureStorageServiceMetricsProperty" ausführen, um die aktuellen Einstellungen abzurufen. Mithilfe des Cmdlets "Set-AzureStorageServiceMetricsProperty" können Sie die aktuellen Einstellungen ändern.

Die Cmdlets zur Steuerung der Speichermetriken verwenden die folgenden Parameter:

* MetricsType – Mögliche Werte sind Hour und Minute.
* ServiceType – Mögliche Werte sind „Blob“, „Queue“ und „Table“.
* MetricsLevel – Mögliche Werte sind None (identisch mit "Aus" im klassischen Azure-Portal), Service (identisch mit "Minimal" im klassischen Azure-Portal) und "ServiceAndApi" (identisch mit "Ausführlich" im klassischen Azure-Portal).

Der folgende Befehl aktiviert z. B. minütliche Metriken für den Blob-Dienst in Ihrem Standardspeicherkonto mit einem Aufbewahrungszeitraum, der auf fünf Tage festgelegt ist:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
```
Der folgende Befehl ruft die aktuelle stündliche Metrikstufe und die Aufbewahrungstage für den Blob-Dienst in Ihrem Standardspeicherkonto ab:

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```
Informationen zum Konfigurieren der Azure PowerShell-Cmdlets für Ihr Azure-Abonnement sowie zum Auswählen des zu verwendenden Standardspeicherkontos finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Programmgesteuertes Aktivieren von Speichermetriken
Der folgende C#-Codeausschnitt zeigt, wie Metriken und Protokollierung für den Blob-Dienst mithilfe der Speicherclientbibliothek für .NET aktiviert werden:

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy to 10 days. 
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at the same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set the default service version to be used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set the service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a>Anzeigen von Speichermetriken
Nachdem Sie Speichermetriken zum Überwachen Ihres Speicherkontos konfiguriert haben, werden die Metriken in bekannten Tabellen in Ihrem Speicherkonto erfasst. Sie können die Seite "Überwachen" für Ihr Speicherkonto im klassischen Azure-Portal zum Anzeigen der stündlichen Metriken in einem Diagramm verwenden, sobald diese verfügbar werden. Auf dieser Seite des klassischen Azure-Portals können Sie folgende Aktionen ausführen:

* Auswählen, welche Metriken im Diagramm dargestellt werden sollen (die Auswahl der verfügbaren Metriken hängt davon ab, ob Sie ausführliche oder minimale Überwachung für den Dienst auf der Seite "Konfigurieren" ausgewählt haben).
* Auswählen des Zeitraums für die im Diagramm angezeigten Metriken.
* Auswählen der Verwendung einer absoluten oder relativen Skala zum Darstellen der Metriken.
* Konfigurieren von E-Mail-Benachrichtigungen bei Erreichen eines bestimmten Werts einer angegebenen Metrik.

Wenn Sie die Metriken zur langfristigen Speicherung oder für eine lokale Analyse herunterladen möchten, müssen Sie ein Tool verwenden oder Code zum Lesen der Tabellen schreiben. Sie müssen die minütlichen Metriken für die Analyse herunterladen. Die Tabellen werden nicht angezeigt, wenn Sie alle Tabellen in Ihrem Speicherkonto auflisten. Sie können jedoch direkt anhand des Namens darauf zugreifen. Zahlreiche Drittanbietertools zum Durchsuchen des Speichers erkennen diese Tabellen und ermöglichen die direkte Anzeige (im Blogbeitrag [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) (Microsoft Azure-Speicher-Explorer) finden Sie eine Liste der verfügbaren Tools).

### <a name="hourly-metrics"></a>Stundenmetriken
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minutenmetriken
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapazität
* $MetricsCapacityBlob

Die vollständigen Details der Schemas für diese Tabellen finden Sie unter [Schema der Tabellen für Speicheranalysemetriken](https://msdn.microsoft.com/library/azure/hh343264.aspx). Die Beispielzeilen unten zeigen nur eine Teilmenge der verfügbaren Spalten. Sie zeigen jedoch einige wichtige Funktionen, wie die Speichermetriken diese Metriken speichern:

| PartitionKey | RowKey | Timestamp | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Availability | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |user;All |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104,4286 |6,857143 |100 |
| 20140522T1100 |user;QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143,8 |7,8 |100 |
| 20140522T1100 |user;QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |user;UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

In diesen minütlichen Metrikbeispieldaten verwendet der Partitionsschlüssel die Auflösung "Uhrzeit zur Minute". Der Zeilenschlüssel identifiziert den Informationstyp, der in der Zeile gespeichert ist. Dieser besteht aus zwei Informationseinheiten: dem Zugriffstyp und dem Anforderungstyp:

* Der Zugriffstyp ist "user" oder "system". Dabei bezieht sich "user" auf alle Benutzeranforderungen des Speicherdiensts und "system" auf Anforderungen, die von der Speicheranalyse vorgenommen werden.
* Der Anforderungstyp ist entweder "all", in diesem Fall handelt es sich um eine Zusammenfassungszeile, oder er identifiziert die jeweilige API, z. B. "QueryEntity" oder "UpdateEntity".

Die Beispieldaten oben zeigen alle Datensätze für eine einzelne Minute (Beginn um 11:00 Uhr). Die Anzahl der QueryEntities-Anforderungen zuzüglich der Anzahl der QueryEntity-Anforderungen zuzüglich der Anzahl der UpdateEntity-Anforderungen ergibt daher den Wert 7. Dies ist die Gesamtsumme, die in der Zeile "user:All" angezeigt wird. Analog können Sie die durchschnittliche End-to-End-Latenz 104,4286 für die Zeile "user:All" ableiten, indem Sie die Berechnung ((143,8 * 5) + 3 + 9)/7 ausführen.

Sie sollten in Betracht ziehen, im klassischen Azure-Portal auf der Seite "Überwachen" Benachrichtigungen einzurichten, damit die Speichermetriken Sie automatisch bei wichtigen Änderungen im Verhalten Ihrer Speicherdienste benachrichtigen können. Wenn Sie ein Speicher-Explorer-Tool zum Herunterladen dieser Metrikdaten in einem Format mit Trennzeichen verwenden, kann Microsoft Excel zum Analysieren der Daten verwendet werden. Im Blogbeitrag [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) (Microsoft Azure-Speicher-Explorer) finden Sie eine Liste der verfügbaren Speicher-Explorer-Tools.

## <a name="accessing-metrics-data-programmatically"></a>Programmgesteuertes Zugreifen auf Metrikdaten
Das folgende Listing zeigt C#-Beispielcode, der auf die minütlichen Metriken für einen bestimmten Zeitraum von Minuten zugreift und die Ergebnisse in einem Konsolenfenster anzeigt. Er verwendet die Azure Storage-Bibliothek, Version 4, die die Klasse "CloudAnalyticsClient" enthält, durch die der Zugriff auf Metriktabellen im Speicher vereinfacht wird.

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in the MetricsEntity class.
          // The PartitionKey identifies the DataTime of the metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
        select entity;

        // Filter on "user" transactions after fetching the metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Welche Kosten fallen an, wenn Speichermetriken aktiviert werden?
Für Schreibanforderungen zum Erstellen von Tabellenentitäten für Metriken fallen die Standardraten an, die für alle Azure Storage-Vorgänge gelten.

Für Lese- und Löschanforderungen durch einen Client für Metrikdaten gelten ebenfalls Standardraten. Wenn Sie eine Datenbeibehaltungsrichtlinie konfiguriert haben, fallen keine Kosten an, wenn Azure Storage alte Metrikdaten löscht. Wenn Sie jedoch Analysedaten löschen, wird Ihr Konto für die Löschvorgänge belastet.

Die von den Metriktabellen verwendete Kapazität ist ebenfalls kostenpflichtig: Sie können die folgenden Angaben verwenden, um die erforderliche Kapazität zum Speichern von Metrikdaten einzuschätzen:

* Wenn ein Dienst jede Stunde alle APIs in jedem Dienst nutzt, werden ungefähr 148 KB Daten pro Stunde in den Metriktransaktionstabellen gespeichert, wenn Sie die Zusammenfassung auf Dienst- und API-Ebene aktiviert haben.
* Wenn ein Dienst jede Stunde alle APIs in jedem Dienst nutzt, werden ungefähr 12 KB Daten pro Stunde in den Metriktransaktionstabellen gespeichert, wenn Sie die Zusammenfassung nur auf Dienstebene aktiviert haben.
* In der Kapazitätstabelle für Blobs werden pro Tag zwei Zeilen hinzugefügt (wenn der Benutzer Protokolle abonniert hat): Dies bedeutet, dass die Größe dieser Tabelle pro Tag um ungefähr 300 Bytes zunimmt.

## <a name="next-steps"></a>Nächste Schritte:
[Aktivieren der Protokollierung von Speicheranalysen und Zugreifen auf Protokolldaten](https://msdn.microsoft.com/library/dn782840.aspx)
