---
title: 'Beispiel: Analysieren von Daten, die aus Azure Application Insights exportiert wurden | Microsoft-Dokumentation'
description: "Codieren Sie Ihre eigene Telemetrieanalyse in Application Insights mithilfe des Features für den fortlaufenden Export. Speichern Sie Daten in SQL."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: douge
ms.assetid: 3ffb62b6-3fe9-455d-a260-b2a0201b5ecd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 08ce387dd37ef2fec8f4dded23c20217a36e9966
ms.openlocfilehash: fedd078402bbd220bce9b71cd035508d46f92f82


---
# <a name="code-sample-parse-data-exported-from-application-insights"></a>Codebeispiel: Analysieren von Daten, die aus Application Insights exportiert wurden
In diesem Artikel wird erläutert, wie Sie Code zum Verarbeiten von Daten schreiben, die mit [fortlaufendem Export][export] aus [Azure Application Insights][start] exportiert wurden. Beim fortlaufenden Export werden Ihre Telemetrie im JSON-Format in Azure Storage verschoben. Daher schreiben wir Code zum Analysieren der JSON-Objekte und zum Erstellen von Zeilen in einer Datenbanktabelle.

Als Beispiel schreiben wir Code, um Ihre Telemetriedaten aus Application Insights in eine SQL-Datenbank zu verschieben.

Beachten Sie Folgendes, bevor Sie beginnen:

* [Mithilfe von Stream Analytics](app-insights-code-sample-export-sql-stream-analytics.md) können exportierte Daten effizienter in eine Datenbank übertragen werden, Ziel dieses Artikels ist es jedoch, Code zum Verarbeiten von exportierten Daten zu veranschaulichen. Sie können dieses Codebeispiel für andere Aufgaben mit den exportierten Telemetriedaten anpassen.
* In diesem Beispiel verschieben wir die Daten durch Ausführen des Codes in einer Azure-Workerrolle in eine Azure-Datenbank. Sie können diesen Code jedoch so anpassen, dass er auf einem lokalen Server ausgeführt wird und die Daten per Pull auf einen lokalen SQL-Server übertragen werden.
* Sie können [Code schreiben, um direkt in Application Insights auf Ihre Telemetriedaten zuzugreifen](http://dev.applicationinsights.io/), ohne diese zu exportieren.

Wenn Sie noch nicht begonnen haben, Ihre Webanwendung mit Application Insights zu überwachen, [tun Sie dies jetzt][start].



## <a name="create-storage-in-azure"></a>Erstellen von Speicher in Azure
Daten von Application Insights werden immer in ein Azure-Speicherkonto  im JSON-Format exportiert. Aus diesem Speicher liest Ihr Code dann die Daten.

1. Erstellen Sie ein „klassisches“ Speicherkonto in Ihrem Abonnement im [Azure-Portal][portal].
   
    ![Wählen Sie im Azure-Portal "Neu", "Daten" und "Speicher".](./media/app-insights-code-sample-export-telemetry-sql-database/040-store.png)
2. Erstellen eines Containers
   
    ![Wählen Sie im neuen Speicher "Container" aus, klicken Sie auf die Kachel "Container" und anschließend auf "Hinzufügen".](./media/app-insights-code-sample-export-telemetry-sql-database/050-container.png)

## <a name="start-continuous-export-to-azure-storage"></a>Starten des fortlaufenden Exports in den Azure-Speicher
1. Navigieren Sie im Azure-Portal zu der Application Insights-Ressource, die Sie für Ihre Anwendung erstellt haben.
   
    ![Wählen Sie „Durchsuchen“, „Application Insights“ und Ihre Anwendung aus.](./media/app-insights-code-sample-export-telemetry-sql-database/060-browse.png)
2. Erstellen Sie einen fortlaufenden Export.
   
    ![Wählen Sie „Einstellungen“ > „Fortlaufender Export“ > „Hinzufügen“.](./media/app-insights-code-sample-export-telemetry-sql-database/070-export.png)

    Wählen Sie das zuvor erstellte Speicherkonto aus:

    ![Legen Sie das Exportziel fest.](./media/app-insights-code-sample-export-telemetry-sql-database/080-add.png)

    Legen Sie die Ereignistypen fest, die Sie anzeigen möchten:

    ![Wählen Sie Ereignistypen aus.](./media/app-insights-code-sample-export-telemetry-sql-database/085-types.png)

1. Warten Sie, bis sich einige Daten angesammelt haben. Lehnen Sie sich zurück, und lassen Sie Ihre Benutzer die Anwendung eine Weile lang verwenden. Telemetriedaten gehen ein, und Sie sehen statistische Diagramme im [Metrik-Explorer](app-insights-metrics-explorer.md) sowie einzelne Ereignisse in der [Diagnosesuche](app-insights-diagnostic-search.md). 
   
    Darüber hinaus werden die Daten in Ihren Speicher exportiert. 
2. Überprüfen Sie die exportierten Daten. Wählen Sie in Visual Studio **Anzeigen/Cloud Explorer**, und öffnen Sie „Azure/Storage“. (Wenn diese Menüoption nicht verfügbar ist, müssen Sie das Azure SDK installieren: Öffnen Sie das Dialogfeld "Neues Projekt" und anschließend "Visual C# / Cloud / Microsoft Azure SDK für .NET abrufen".)
   
    ![Öffnen Sie in Visual Studio den "Server-Browser", "Azure" und "Storage".](./media/app-insights-code-sample-export-telemetry-sql-database/087-explorer.png)
   
    Notieren Sie sich den gemeinsamen Teil des Pfadnamens, der sich vom Anwendungsnamen und vom Instrumentierungsschlüssel ableitet. 

Die Ereignisse werden in Blobdateien im JSON-Format geschrieben. Jede Datei kann ein oder mehrere Ereignisse enthalten. Daher möchten wir die Ereignisdaten lesen und die gewünschten Felder herausfiltern. Es gibt viele verschiedene Möglichkeiten zur Nutzung der Daten, aber unser Plan besteht darin, Code zum Verschieben der Daten in eine SQL-Datenbank zu verfassen. Auf diese Weise können wir ganz einfach viele interessante Abfragen ausführen.

## <a name="create-an-azure-sql-database"></a>Erstellen einer Azure-SQL-Datenbank
In diesem Beispiel schreiben wir Code, um die Daten in eine Datenbank zu übertragen.

Beginnen Sie erneut bei Ihrem Abonnement im [Azure-Portal][portal], und erstellen Sie die Datenbank (und einen neuen Server, sofern noch keiner verfügbar ist), in die die Daten geschrieben werden sollen.

![Neu, Daten, SQL](./media/app-insights-code-sample-export-telemetry-sql-database/090-sql.png)

Stellen Sie sicher, dass der Datenbankserver den Zugriff auf Azure-Dienste ermöglicht:

![Durchsuchen, Server, Ihr Server, Einstellungen, Firewall, Zugriff auf Azure erlauben](./media/app-insights-code-sample-export-telemetry-sql-database/100-sqlaccess.png)

## <a name="create-a-worker-role"></a>Erstellen einer Workerrolle
Zu guter Letzt können wir nun [Code](https://sesitai.codeplex.com/) zum Analysieren der JSON-Daten in den exportierten Blobs und zum Erstellen von Datensätzen in der Datenbank schreiben. Da sich der Exportspeicher und die Datenbank in Azure befinden, führen wir den Code in einer Azure-Workerrolle aus.

Dieser Code extrahiert automatisch alle Eigenschaften, die in den JSON-Daten vorhanden sind. Beschreibungen der Eigenschaften finden Sie unter [Exportdatenmodell](app-insights-export-data-model.md).

#### <a name="create-worker-role-project"></a>Erstellen eines Workerrollenprojekts
Erstellen Sie in Visual Studio ein neues Projekt für die Workerrolle.

!["Neues Projekt", "Visual C#", "Cloud", "Azure Cloud Service"](./media/app-insights-code-sample-export-telemetry-sql-database/110-cloud.png)

![Wählen Sie im Dialogfeld für den neuen Clouddienst "Visual C#", "Workerrolle"](./media/app-insights-code-sample-export-telemetry-sql-database/120-worker.png)

#### <a name="connect-to-the-storage-account"></a>Verbinden mit dem Speicherkonto
Rufen Sie in Azure die Verbindungszeichenfolge aus Ihrem Storage-Konto ab:

![Wählen Sie im Storage-Konto "Schlüssel" aus, und kopieren Sie die primäre Verbindungszeichenfolge](./media/app-insights-code-sample-export-telemetry-sql-database/055-get-connection.png)

Konfigurieren Sie die Workerrolleneinstellungen in Visual Studio mit der Verbindungszeichenfolge für das Storage-Konto:

![Erweitern Sie im Projektmappen-Explorer unter dem Cloud Service-Projekt die Option "Rollen", und öffnen Sie die" Workerrolle". Öffnen Sie die Registerkarte "Einstellungen", wählen Sie "Einstellung hinzufügen", und legen Sie "name=StorageConnectionString" und "type=Verbindungszeichenfolge" fest. Klicken Sie, um den Wert festzulegen. Legen Sie ihn manuell fest, und fügen Sie die Verbindungszeichenfolge ein.](./media/app-insights-code-sample-export-telemetry-sql-database/130-connection-string.png)

#### <a name="packages"></a>Pakete
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt "Workerrolle", und wählen Sie dann "NuGet-Pakete verwalten" aus.
Suchen und installieren Sie diese Pakete: 

* EntityFramework 6.1.2 oder höher – Zum dynamischen Generieren des DB-Tabellenschemas basierend auf dem Inhalt der JSON-Daten im Blob.
* JsonFx – zum Reduzieren der JSON-Daten auf C#-Klasseneigenschaften.

Verwenden Sie dieses Tool zum Generieren von C#-Klassendaten aus dem einzelnen JSON-Dokument. Dazu sind einige kleinere Änderungen erforderlich, etwa das Reduzieren von JSON-Arrays in einzelne C#-Eigenschaften in einer einzigen Spalte in der Datenbanktabelle (z.B. urlData_port). 

* [C#-Klassen-Generator aus JSON](http://jsonclassgenerator.codeplex.com/)

## <a name="code"></a>Code
Sie können diesen Code in `WorkerRole.cs`ablegen.

#### <a name="imports"></a>Importe
    using Microsoft.WindowsAzure.Storage;

    using Microsoft.WindowsAzure.Storage.Blob;

#### <a name="retrieve-the-storage-connection-string"></a>Abrufen der Speicherverbindungszeichenfolge
    private static string GetConnectionString()
    {
      return Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    }

#### <a name="run-the-worker-at-regular-intervals"></a>Ausführen des Workers in regelmäßigen Abständen
Ersetzen Sie die vorhandene run-Methode, und wählen Sie das gewünschte Intervall. Es sollte mindestens eine Stunde umfassen, da das Exportfeature ein JSON-Objekt pro Stunde abschließt.

    public override void Run()
    {
      Trace.TraceInformation("WorkerRole1 is running");

      while (true)
      {
        Trace.WriteLine("Sleeping", "Information");

        Thread.Sleep(86400000); //86400000=24 hours //1 hour=3600000

        Trace.WriteLine("Awake", "Information");

        ImportBlobtoDB();
      }
    }

#### <a name="insert-each-json-object-as-a-table-row"></a>Einfügen der einzelnen JSON-Objekte als Tabellenzeilen
    public void ImportBlobtoDB()
    {
      try
      {
        CloudStorageAccount account = CloudStorageAccount.Parse(GetConnectionString());

        var blobClient = account.CreateCloudBlobClient();
        var container = blobClient.GetContainerReference(FilterContainer);

        foreach (CloudBlobDirectory directory in container.ListBlobs())//Parent directory
        {
          foreach (CloudBlobDirectory subDirectory in directory.ListBlobs())//PageViewPerformance
             {
            foreach (CloudBlobDirectory dir in subDirectory.ListBlobs())//2015-01-31
            {
              foreach (CloudBlobDirectory subdir in dir.ListBlobs())//22
              {
                foreach (IListBlobItem item in subdir.ListBlobs())//3IAwm6u3-0.blob
                {
                  itemname = item.Uri.ToString();
                  ParseEachBlob(container, item);
                  AuditBlob(container, directory, subDirectory, dir, subdir, item);
                } //item loop
              } //subdir loop
            } //dir loop
          } //subDirectory loop
        } //directory loop
      }
      catch (Exception ex)
      {
        //handle exception
      }
    }

#### <a name="parse-each-blob"></a>Analysieren der einzelnen Blobs
    private void ParseEachBlob(CloudBlobContainer container, IListBlobItem item)
    {
      try
      {
        var blob = container.GetBlockBlobReference(item.Parent.Prefix + item.Uri.Segments.Last());

        string json;

        using (var memoryStream = new MemoryStream())
        {
          blob.DownloadToStream(memoryStream);
          json = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());

          IEnumerable<string> entities = json.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));

          recCount = entities.Count();
          failureCount = 0; //resetting failure count

          foreach (var entity in entities)
          {
            var reader = new JsonFx.Json.JsonReader();
            dynamic output = reader.Read(entity);

            Dictionary<string, object> dict = new Dictionary<string, object>();

            GenerateDictionary((System.Dynamic.ExpandoObject)output, dict, "");

            switch (FilterType)
            {
              case "PageViewPerformance":

              if (dict.ContainsKey("clientPerformance"))
                {
                  GenerateDictionary(((System.Dynamic.ExpandoObject[])dict["clientPerformance"])[0], dict, "");
                }

              if (dict.ContainsKey("context_custom_dimensions"))
              {
                if (dict["context_custom_dimensions"].GetType() == typeof(System.Dynamic.ExpandoObject[]))
                {
                  GenerateDictionary(((System.Dynamic.ExpandoObject[])dict["context_custom_dimensions"])[0], dict, "");
                }
              }

            PageViewPerformance objPageViewPerformance = (PageViewPerformance)GetObject(dict);

            try
            {
              using (var db = new TelemetryContext())
              {
                db.PageViewPerformanceContext.Add(objPageViewPerformance);
                db.SaveChanges();
              }
            }
            catch (Exception ex)
            {
              failureCount++;
            }
            break;

            default:
            break;
          }
        }
      }
    }
    catch (Exception ex)
    {
      //handle exception 
    }
    }

#### <a name="prepare-a-dictionary-for-each-json-document"></a>Vorbereiten eines Wörterbuchs für jedes JSON-Dokument
    private void GenerateDictionary(System.Dynamic.ExpandoObject output, Dictionary<string, object> dict, string parent)
        {
            try
            {
                foreach (var v in output)
                {
                    string key = parent + v.Key;
                    object o = v.Value;

                    if (o.GetType() == typeof(System.Dynamic.ExpandoObject))
                    {
                        GenerateDictionary((System.Dynamic.ExpandoObject)o, dict, key + "_");
                    }
                    else
                    {
                        if (!dict.ContainsKey(key))
                        {
                            dict.Add(key, o);
                        }
                    }
                }
            }
            catch (Exception ex)
            {
              //handle exception 
            }
        }

#### <a name="cast-the-json-document-into-c-class-telemetry-object-properties"></a>Umwandeln des JSON-Dokuments in C#-Klassen-Telemetrieobjekteigenschaften
     public object GetObject(IDictionary<string, object> d)
        {
            PropertyInfo[] props = null;
            object res = null;

            try
            {
                switch (FilterType)
                {
                    case "PageViewPerformance":

                        props = typeof(PageViewPerformance).GetProperties();
                        res = Activator.CreateInstance<PageViewPerformance>();
                        break;

                    default:
                        break;
                }

                for (int i = 0; i < props.Length; i++)
                {
                    if (props[i].CanWrite && d.ContainsKey(props[i].Name))
                    {
                        props[i].SetValue(res, d[props[i].Name], null);
                    }
                }
            }
            catch (Exception ex)
            {
              //handle exception 
            }

            return res;
        }

#### <a name="pageviewperformance-class-file-generated-out-of-json-document"></a>Aus JSON-Dokument generierte PageViewPerformance-Klassendatei
    public class PageViewPerformance
    {
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public Guid Id { get; set; }

        public string url { get; set; }

        public int urlData_port { get; set; }

        public string urlData_protocol { get; set; }

        public string urlData_host { get; set; }

        public string urlData_base { get; set; }

        public string urlData_hashTag { get; set; }

        public double total_value { get; set; }

        public double networkConnection_value { get; set; }

        public double sendRequest_value { get; set; }

        public double receiveRequest_value { get; set; }

        public double clientProcess_value { get; set; }

        public string name { get; set; }

        public string internal_data_id { get; set; }

        public string internal_data_documentVersion { get; set; }

        public DateTime? context_data_eventTime { get; set; }

        public string context_device_id { get; set; }

        public string context_device_type { get; set; }

        public string context_device_os { get; set; }

        public string context_device_osVersion { get; set; }

        public string context_device_locale { get; set; }

        public string context_device_userAgent { get; set; }

        public string context_device_browser { get; set; }

        public string context_device_browserVersion { get; set; }

        public string context_device_screenResolution_value { get; set; }

        public string context_user_anonId { get; set; }

        public string context_user_anonAcquisitionDate { get; set; }

        public string context_user_authAcquisitionDate { get; set; }

        public string context_user_accountAcquisitionDate { get; set; }

        public string context_session_id { get; set; }

        public bool context_session_isFirst { get; set; }

        public string context_operation_id { get; set; }

        public double context_location_point_lat { get; set; }

        public double context_location_point_lon { get; set; }

        public string context_location_clientip { get; set; }

        public string context_location_continent { get; set; }

        public string context_location_country { get; set; }

        public string context_location_province { get; set; }

        public string context_location_city { get; set; }
    }


#### <a name="dbcontext-for-sql-interaction-by-entity-framework"></a>DBcontext für SQL-Interaktion nach Entity Framework
    public class TelemetryContext : DbContext
    {
        public DbSet<PageViewPerformance> PageViewPerformanceContext { get; set; }
        public TelemetryContext()
            : base("name=TelemetryContext")
        {
        }
    }

Fügen Sie die DB-Verbindungszeichenfolge des Namens `TelemetryContext` in `app.config` hinzu.

## <a name="schema-information-only"></a>Schema (nur zu Informationszwecken)
Dies ist das Schema für die Tabelle, die für PageView generiert wird.

> [!NOTE]
> Sie müssen dieses Skript nicht ausführen. Die Attribute in JSON bestimmen die Spalten in der Tabelle.
> 
> 

    CREATE TABLE [dbo].[PageViewPerformances](
    [Id] [uniqueidentifier] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlData_port] [int] NOT NULL,
    [urlData_protocol] [nvarchar](max) NULL,
    [urlData_host] [nvarchar](max) NULL,
    [urlData_base] [nvarchar](max) NULL,
    [urlData_hashTag] [nvarchar](max) NULL,
    [total_value] [float] NOT NULL,
    [networkConnection_value] [float] NOT NULL,
    [sendRequest_value] [float] NOT NULL,
    [receiveRequest_value] [float] NOT NULL,
    [clientProcess_value] [float] NOT NULL,
    [name] [nvarchar](max) NULL,
    [User] [nvarchar](max) NULL,
    [internal_data_id] [nvarchar](max) NULL,
    [internal_data_documentVersion] [nvarchar](max) NULL,
    [context_data_eventTime] [datetime] NULL,
    [context_device_id] [nvarchar](max) NULL,
    [context_device_type] [nvarchar](max) NULL,
    [context_device_os] [nvarchar](max) NULL,
    [context_device_osVersion] [nvarchar](max) NULL,
    [context_device_locale] [nvarchar](max) NULL,
    [context_device_userAgent] [nvarchar](max) NULL,
    [context_device_browser] [nvarchar](max) NULL,
    [context_device_browserVersion] [nvarchar](max) NULL,
    [context_device_screenResolution_value] [nvarchar](max) NULL,
    [context_user_anonId] [nvarchar](max) NULL,
    [context_user_anonAcquisitionDate] [nvarchar](max) NULL,
    [context_user_authAcquisitionDate] [nvarchar](max) NULL,
    [context_user_accountAcquisitionDate] [nvarchar](max) NULL,
    [context_session_id] [nvarchar](max) NULL,
    [context_session_isFirst] [bit] NOT NULL,
    [context_operation_id] [nvarchar](max) NULL,
    [context_location_point_lat] [float] NOT NULL,
    [context_location_point_lon] [float] NOT NULL,
    [context_location_clientip] [nvarchar](max) NULL,
    [context_location_continent] [nvarchar](max) NULL,
    [context_location_country] [nvarchar](max) NULL,
    [context_location_province] [nvarchar](max) NULL,
    [context_location_city] [nvarchar](max) NULL,
    CONSTRAINT [PK_dbo.PageViewPerformances] PRIMARY KEY CLUSTERED 
    (
     [Id] ASC
    )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

    GO

    ALTER TABLE [dbo].[PageViewPerformances] ADD  DEFAULT (newsequentialid()) FOR [Id]
    GO


Um dieses Beispiel in Aktion zu sehen, [laden Sie den vollständigen Arbeitscode herunter](https://sesitai.codeplex.com/), ändern Sie die `app.config`-Einstellungen, und veröffentlichen Sie die Workerrolle in Azure.

## <a name="next-steps"></a>Nächste Schritte

* [Schreiben von Code für den direkten Zugriff auf Telemetriedaten](http://dev.applicationinsights.io/)
* [Exportieren in SQL über eine Workerrolle](app-insights-code-sample-export-telemetry-sql-database.md)
* [Fortlaufender Export in Application Insights](app-insights-export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)
* [Exportdatenmodell](app-insights-export-data-model.md)
* [Weitere Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)


<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md





<!--HONumber=Jan17_HO4-->


