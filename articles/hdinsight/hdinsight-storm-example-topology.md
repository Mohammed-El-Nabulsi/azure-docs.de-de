---
title: Apache Storm-Beispieltopologien in HDInsight | Microsoft Docs
description: "Eine Liste von Beispieltopologien, die mit Apache Storm in HDInsight erstellt und getestet wurden, einschließlich der grundlegenden C#- und Java-Topologien, und mit Event Hubs funktionieren."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 46bc5b3b70120cd631523fd2b27ad8b9a47e3c6d
ms.openlocfilehash: 21c25e4579a440b13c3201ee8f6c4e1db7a2aa46
ms.lasthandoff: 11/17/2016


---
# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Beispiel-Storm-Topologien und -Komponenten für Apache Storm auf HDInsight

Es folgt eine Liste von Beispielen zur Verwendung mit Apache Storm in HDInsight, die von Microsoft erstellt und verwaltet wird. Diese Beispiele decken eine Vielzahl von Themen ab – angefangen bei grundlegenden C# und Java-Topologien bis hin zur Arbeit mit Azure-Diensten wie Event Hubs, DocumentDB, Power BI, SQL-Datenbank, HBase in HDInsight und Azure Storage. Einige Beispiele demonstrieren außerdem die Arbeit mit nicht-Azure oder sogar nicht-Microsoft-Technologien, wie SignalR und Socket.IO

| Beschreibung | Zeigt | Sprache/Framework |
|:--- |:--- |:--- |
| [Schreiben von Daten in Azure Data Lake-Speicher aus Apache Storm](hdinsight-storm-write-data-lake-store.md) |Schreiben von Daten in Azure Data Lake-Speicher |Java |
| [Quelle für Event Hub-Spout und -Bolt](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Quelle für Event Hub-Spout und -Bolt |Java |
| [Entwickeln von Java-basierten Topologien für Apache Storm in HDInsight][5797064f] |Maven |Java |
| [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio][16fce2d1] |HDInsight Tools für Visual Studio |C#, Java |
| [Erstellen mehrerer Datenströme in einer C#-Storm-Topologie][ec5a4064] |Mehrere Datenströme |C# |
| [Ermitteln von beliebten Twitter-Themen mit Storm in HDInsight][3c86c7c8] |Trident |Java, Trident |
| [Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)][844d1d81] |Event Hubs |C# und Java |
| [Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) |Event Hubs |Java |
| [Verwenden von Power BI zur Visualisierung von Daten aus einer Storm-Topologie][94d15238] |Power BI |C# |
| [Analysieren von Sensordaten mit Storm und HBase in HDInsight][ab894747] |Event Hubs, HBase, Socket.IO, Web-Dashboard |C#, Java, JavaScript, HTML |
| [Verarbeitung von Fahrzeugsensordaten von Event Hubs mit Storm auf HDInsight][246ee964] |Event Hubs, DocumentDb, Azure-Speicher-Blob (WASB) |C#, Java |
| [Extrahieren, Transformieren und Laden (ETL) von Azure-Event Hubs auf HBase mit Storm in HDInsight][b4b68194] |Event Hubs, HBase |C# |
| [C#-Storm-Topologie-Vorlagenprojekt für die Arbeit mit Azure-Diensten über Storm auf HDInsight][ce0c02a2] |Event Hubs, DocumentDb, SQL-Datenbank, HBase, SignalR |C#, Java |
| [Skalierbarkeits-Benchmarks für das Lesen von Azure-Event Hubs mit Storm auf HDInsight][d6c540e3] |Nachrichtendurchsatz, Event Hubs, SQL-Datenbank |C#, Java |
| [Zuordnen von Ereignissen mithilfe von Storm und HBase auf HDInsight](hdinsight-storm-correlation-topology.md) |HBase |C# |
| [Verwenden von Python mit Storm in HDInsight](hdinsight-storm-develop-python-topology.md) |Python-Komponenten mit Java- und Clojure-basierten Storm-Topologien |Python |

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Apache Storm in HDInsight][2b8c3488]
* [Erfahren Sie, wie Sie Storm-Topologien mit Storm auf HDInsight bereitstellen und verwalten][6eb0d3b8]

[2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Erfahren Sie, wie Sie einen Storm auf HDInsight-Cluster erstellen und das Storm-Dashboard zum Bereitstellen von Beispieltopologien verwenden."
[6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Erfahren Sie, wie Sie Topologien mit dem webbasierten Storm-Dashboard und der Storm-Benutzeroberfläche oder die HDInsight-Tools für Visual Studio bereitstellen und verwalten."
[16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Erfahren Sie, wie Sie C#-Storm-Topologien mithilfe der HDInsight Tools für Visual Studio erstellen."
[5797064f]: hdinsight-storm-develop-java-topology.md "Erfahren Sie, wie Sie Storm-Topologien in Java mit Maven erstellen, indem Sie eine Grundtopologie zur Wortzählung erstellen."
[94d15238]: hdinsight-storm-power-bi-topology.md "Zeigt das Schreiben von Daten in Power BI aus einer C#-Topologie und das Erstellen eines Diagramms und eines Dashboards aus den Daten."
[ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Zeigt eine Storm-Grundtopologie mit Ausführung einer Wortzählung, die in C# implementiert wird. Außerdem wird das Erstellen mehrerer Datenströme in einer C#-Topologie gezeigt."
[844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Enthält Informationen zum Lesen und Schreiben von Daten aus Azure-Event Hubs mit Storm in HDInsight."
[ab894747]: hdinsight-storm-sensor-data-analysis.md "Erfahren Sie, wie Sie Apache Storm auf HDInsight zum Verarbeiten von Sensordaten von Azure Event Hubs, Darstellen mit „D3.js“ und (optional) Speichern unter HBase verwenden."
[3c86c7c8]: hdinsight-storm-twitter-trending.md "Erfahren Sie, wie Sie mit Trident eine Storm-Topologie erstellen, die beliebte Twitter-Themen auf Grundlage von Hashtags ermittelt."
[246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Erfahren Sie, wie Sie eine Storm-Topologie verwenden, um Azure Event Hubs-Nachrichten und Azure DocumentDB-Dokumente zur Datenreferenzierung zu lesen und Daten in Azure Storage zu speichern."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Enthält mehrere Topologien, um den Durchsatz beim Lesen von Azure Event Hubs und das Speichern in einer SQL-Datenbank unter Verwendung von Apache Storm in HDInsight zu veranschaulichen."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Erfahren Sie, wie Sie Daten aus Azure Event Hubs lesen, Daten aggregieren und transformieren und diese dann HBase in HDInsight speichern."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dieses Projekt enthält Vorlagen für Spouts, Bolts und Topologien für die Interaktion mit verschiedenen Azure-Diensten wie Event Hubs, DocumentDB und SQL-Datenbank."


