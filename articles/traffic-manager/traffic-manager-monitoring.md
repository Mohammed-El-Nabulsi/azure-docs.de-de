---
title: "Azure Traffic Manager-Endpunktüberwachung | Microsoft Docs"
description: "In diesem Artikel wird beschrieben, wie Traffic Manager die Endpunktüberwachung und das automatische Endpunktfailover verwendet, um Azure-Kunden bei der Bereitstellung von Anwendungen mit hoher Verfügbarkeit zu unterstützen."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: cec4f541ebac6202a3880ec7338a9f0a0ac645b5
ms.lasthandoff: 03/18/2017

---

# <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager-Endpunktüberwachung

Azure Traffic Manager umfasst eine integrierte Endpunktüberwachung und ein automatisches Endpunktfailover. Dieses Feature unterstützt Sie bei der Bereitstellung von Anwendungen mit hoher Verfügbarkeit, die auch Endpunktausfälle überstehen, z.B. auch Ausfälle von Azure-Regionen.

## <a name="configure-endpoint-monitoring"></a>Konfigurieren der Endpunktüberwachung

Zum Konfigurieren der Endpunktüberwachung müssen Sie die folgenden Einstellungen in Ihrem Traffic Manager-Profil angeben:

* **Protokoll**. Wählen Sie HTTP oder HTTPS aus. Beachten Sie, dass bei der HTTPS-Überwachung nicht überprüft wird, ob Ihr SSL-Zertifikat gültig ist. Es wird nur überprüft, ob es vorhanden ist.
* **Port**. Wählen Sie den Port aus, der für die Anforderung verwendet wird.
* **Pfad**. Geben Sie den relativen Pfad und den Namen der Webseite oder Datei an, auf die die Überwachung zugreift. Ein Schrägstrich (/) ist ein gültiger Eintrag für den relativen Pfad. Dieser Wert bedeutet, dass sich die Datei im Stammverzeichnis befindet (Standard).

Um die Integrität der einzelnen Endpunkte zu überprüfen, sendet Traffic Manager eine GET-Anforderung mit Port, Protokoll und angegebenem relativem Pfad an jeden Endpunkt.

Üblicherweise wird hierfür eine benutzerdefinierte Seite innerhalb der Anwendung implementiert, z.B. „/health.aspx“. Wenn Sie diesen Pfad für die Überwachung verwenden, können Sie anwendungsspezifische Überprüfungen durchführen, wie z.B. die Überprüfung von Leistungsindikatoren oder der Datenbankverfügbarkeit. Die Seite gibt basierend auf diesen benutzerdefinierten Überprüfungen einen entsprechenden HTTP-Statuscode zurück.

Alle Endpunkte in einem Traffic Manager-Profil teilen sich Überwachungseinstellungen. Falls Sie für verschiedene Endpunkte unterschiedliche Überwachungseinstellungen verwenden müssen, können Sie [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings) erstellen.

## <a name="endpoint-and-profile-status"></a>Endpunkt- und Profilstatus

Sie können Traffic Manager-Profile und -Endpunkte aktivieren und deaktivieren. Der Endpunktstatus kann sich auch aufgrund von automatisierten Traffic Manager-Einstellungen und -Prozessen ändern.

### <a name="endpoint-status"></a>Endpunktstatus

Sie können einen spezifischen Endpunkt aktivieren oder deaktivieren. Der zugrunde liegende Dienst, der weiterhin fehlerfrei sein kann, ist nicht betroffen. Das Ändern des Endpunktstatus steuert die Verfügbarkeit des Endpunkts im Traffic Manager-Profil. Wenn der Status eines Endpunkts deaktiviert ist, überprüft Traffic Manager die Integrität des Endpunkts nicht, und der Endpunkt wird nicht in eine DNS-Antwort einbezogen.

### <a name="profile-status"></a>Profilstatus

Indem Sie die Profilstatuseinstellung verwenden, können Sie ein spezifisches Profil aktivieren oder deaktivieren. Während der Endpunktstatus einen einzelnen Endpunkt betrifft, wirkt sich der Profilstatus auf das gesamte Profil aus, einschließlich aller Endpunkte. Wenn Sie ein Profil deaktivieren, werden die Endpunkte nicht auf Integrität überprüft, und keine Endpunkte werden in eine DNS-Antwort eingeschlossen. Ein [NXDOMAIN](https://tools.ietf.org/html/rfc2308)-Antwortcode wird für die DNS-Abfrage zurückgegeben.

### <a name="endpoint-monitor-status"></a>Überwachungsstatus von Endpunkten

Der Überwachungsstatus von Endpunkten ist ein Wert, der von Traffic Manager generiert wird und den Status eines Endpunkts anzeigt. Sie können diese Einstellung nicht manuell ändern. Der Überwachungsstatus eines Endpunkts ist eine Kombination aus den Ergebnissen der Endpunktüberwachung und dem konfigurierten Endpunktstatus. In der folgenden Tabelle sind die möglichen Werte des Überwachungsstatus von Endpunkten aufgeführt:

| Profilstatus | Endpunktstatus | Überwachungsstatus von Endpunkten | Hinweise |
| --- | --- | --- | --- |
| Deaktiviert |Aktiviert |Inaktiv |Das Profil wurde deaktiviert. Obwohl der Endpunktstatus immer noch „Aktiviert“ lautet, hat der Profilstatus („Deaktiviert“) Vorrang. Endpunkte in deaktivierten Profilen werden nicht überwacht. Ein „NXDOMAIN“-Antwortcode wird für die DNS-Abfrage zurückgegeben. |
| &lt;beliebig&gt; |Deaktiviert |Deaktiviert |Der Endpunkt wurde deaktiviert. Deaktivierte Endpunkte werden nicht überwacht. Der Endpunkt wird nicht in DNS-Antworten einbezogen und kann daher auch keinen Datenverkehr empfangen. |
| Aktiviert |Aktiviert |Online- |Der Endpunkt wird überwacht und ist fehlerfrei. Er wird in DNS-Antworten einbezogen und kann Datenverkehr empfangen. |
| Aktiviert |Aktiviert |Heruntergestuft |Bei Integritätsprüfungen im Rahmen der Endpunktüberwachung werden Fehler erkannt. Der Endpunkt wird nicht in DNS-Antworten einbezogen und empfängt keinen Datenverkehr. |
| Aktiviert |Aktiviert |CheckingEndpoint |Der Endpunkt wird überwacht, die Ergebnisse der ersten Überprüfung wurden jedoch noch nicht empfangen. CheckingEndpoint ist ein temporärer Status, der in der Regel unmittelbar nach dem Hinzufügen oder Aktivieren eines Endpunkts im Profil auftritt. Ein Endpunkt in diesem Status kann in DNS-Antworten einbezogen werden und Datenverkehr empfangen. |
| Aktiviert |Aktiviert |Beendet |Der Clouddienst oder die Web-App, auf den bzw. die der Endpunkt zeigt, wird nicht ausgeführt. Überprüfen Sie die Einstellungen des Clouddiensts oder der Web-App. Ein Endpunkt mit dem Status „Beendet“ wird nicht überwacht. Er wird nicht in DNS-Antworten einbezogen und empfängt keinen Datenverkehr. |

Ausführlichere Informationen zur Berechnung des Überwachungsstatus bei geschachtelten Endpunkten finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Überwachungsstatus von Profilen

Der Überwachungsstatus von Profilen ist das Ergebnis einer Kombination aus dem konfigurierten Profilstatus und den Überwachungsstatuswerten von Endpunkten für alle Endpunkte. Die möglichen Werte sind in der folgenden Tabelle beschrieben:

| Profilstatus (wie konfiguriert) | Überwachungsstatus von Endpunkten | Überwachungsstatus von Profilen | Hinweise |
| --- | --- | --- | --- |
| Deaktiviert |&lt;beliebig&gt; oder ein Profil ohne definierte Endpunkte. |Deaktiviert |Das Profil wurde deaktiviert. |
| Aktiviert |Der Status mindestens eines Endpunkts lautet „Heruntergestuft“. |Heruntergestuft |Überprüfen Sie die einzelnen Endpunkt-Statuswerte, um zu ermitteln, für welche Endpunkte weitere Aufmerksamkeit erforderlich ist. |
| Aktiviert |Der Status mindestens eines Endpunkts lautet „Online“. Kein Endpunkt weist den Status „Heruntergestuft“ auf. |Online- |Der Dienst akzeptiert Datenverkehr. Es ist keine weitere Aktion erforderlich. |
| Aktiviert |Der Status mindestens eines Endpunkts lautet „CheckingEndpoint“. Es befinden sich keine Endpunkte in den Status „Online“ oder „Heruntergestuft“. |CheckingEndpoints |Dieser Übergangsstatus tritt auf, wenn ein Profil erstellt oder aktiviert wird. Die Endpunktintegrität wird zum ersten Mal überprüft. |
| Aktiviert |Der Status aller Endpunkte im Profil ist „Deaktiviert“ oder „Beendet“, oder im Profil wurden keine Endpunkte definiert. |Inaktiv |Es sind keine Endpunkte aktiv, das Profil ist jedoch weiterhin aktiviert. |

## <a name="endpoint-failover-and-recovery"></a>Endpunktfailover und -wiederherstellung

Traffic Manager überprüft regelmäßig die Integrität jedes Endpunkts, einschließlich fehlerhafter Endpunkte. Traffic Manager erkennt, wenn ein Endpunkt fehlerfrei wird, und nimmt ihn wieder in den Kreislauf auf.

> [!NOTE]
> Traffic Manager betrachtet einen Endpunkt nur dann als online, wenn die Antwortnachricht „200 OK“ lautet. Ein Endpunkt ist fehlerhaft, wenn eines der folgenden Ereignisse eintritt:
>
> * Eine andere Antwort als „200“ wird empfangen (z.B. ein anderer 2xx-Code oder eine 301/302-Umleitung)
> * Anforderung der Clientauthentifizierung
> * Timeout (Timeout-Schwellenwert beträgt 10 Sekunden)
> * Es kann keine Verbindung hergestellt werden
>
> Weitere Informationen zur Behandlung von Fehlern bei Integritätsprüfungen finden Sie unter [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).

Die folgenden Zeitachse ist eine ausführliche Beschreibung des Überwachungsprozesses.

![Sequenz für Traffic Manager-Endpunktfailover und -failback](./media/traffic-manager-monitoring/timeline.png)

1. **GET**. Für jeden Endpunkt führt das Traffic Manager-Überwachungssystem eine GET-Anforderung für den Pfad und die Datei aus, die in den Überwachungseinstellungen angegeben wurden.
2. **200 OK**. Das Überwachungssystem erwartet, dass innerhalb von zehn Sekunden eine HTTP 200 OK-Nachricht zurückgegeben wird. Wenn diese Nachricht empfangen wird, wird der Dienst als verfügbar betrachtet.
3. **30 Sekunden zwischen Überprüfungen**. Die Endpunktintegritätsprüfung wird alle 30 Sekunden wiederholt.
4. **Dienst nicht verfügbar**. Der Dienst ist nicht verfügbar. Traffic Manager kann den Status erst bei der nächsten Integritätsprüfung ermitteln.
5. **Zugriffsversuche auf die Überwachungsdatei (vier Versuche)**. Das Überwachungssystem führt eine GET-Anforderung aus, erhält aber nicht innerhalb des Zeitlimits von zehn Sekunden eine Antwort (alternativ hierzu wird ggf. eine andere Antwort als 200 empfangen). Es werden dann in Abständen von jeweils 30 Sekunden drei weitere Versuche durchgeführt. Wenn einer der Versuche erfolgreich ist, wird die Anzahl der Versuche zurückgesetzt.
6. **Status auf „Heruntergestuft“ festgelegt**. Nach dem vierten erfolglosen Versuch in Folge markiert das Überwachungssystem den Status des nicht verfügbaren Endpunkts als „Heruntergestuft“.
7. **Datenverkehr wird an andere Endpunkte umgeleitet**. Die DNS-Namenserver in Traffic Manager werden aktualisiert, und Traffic Manager gibt den Endpunkt in Antworten auf DNS-Abfragen nicht mehr zurück. Neue Verbindungen werden an andere, verfügbare Endpunkte umgeleitet. Jedoch werden vorherige DNS-Antworten, die diesen Endpunkt enthalten, von rekursiven DNS-Servern und DNS-Clients möglicherweise weiterhin zwischengespeichert. Clients verwenden den Endpunkt weiterhin, bis der DNS-Cache abläuft. Wenn dieser Cache abläuft, führen Clients neue DNS-Abfragen durch und werden an andere Endpunkte umgeleitet. Die Cachedauer wird über die TTL-Einstellung im Traffic Manager-Profil gesteuert, z.B. 30 Sekunden.
8. **Integritätsprüfungen werden fortgesetzt**. Traffic Manager fährt mit der Integritätsprüfung für den Endpunkt fort, während dieser sich im Status „Heruntergestuft“ befindet. Traffic Manager erkennt, wenn der Endpunkt wieder fehlerfrei wird.
9. **Service wird wieder online geschaltet**. Der Dienst wird verfügbar. Der Endpunkt hat in Traffic Manager weiterhin den Status „Heruntergestuft“, bis das Überwachungssystem die nächste Integritätsprüfung durchführt.
10. **Datenverkehr an den Dienst wird wieder aufgenommen**. Traffic Manager sendet eine GET-Anforderung und empfängt eine 200 OK-Statusantwort. Der Dienst befindet sich wieder in einem fehlerfreien Zustand. Die Traffic Manager-Namensserver werden aktualisiert und beginnen damit, den DNS-Namen des Diensts in DNS-Antworten wieder anzugeben. Der Datenverkehr wird wieder an den Endpunkt geleitet, wenn zwischengespeicherte DNS-Antworten mit Angabe anderer Endpunkte ablaufen und vorhandene Verbindungen mit anderen Endpunkten beendet werden.

> [!NOTE]
> Da Traffic Manager auf DNS-Ebene arbeitet, kann er vorhandene Verbindungen mit Endpunkten nicht beeinflussen. Beim Weiterleiten von Datenverkehr zwischen Endpunkten (entweder durch geänderte Profileinstellungen oder beim Failover oder Failback) leitet Traffic Manager neue Verbindungen an verfügbare Endpunkte weiter. Andere Endpunkte erhalten jedoch möglicherweise über vorhandene Verbindungen weiterhin Datenverkehr, bis diese Sitzungen beendet werden. Um diesen Datenverkehr aus den vorhandenen Verbindungen zu beseitigen, sollte die Sitzungsdauer für Anwendungen verringert werden, die für einen Endpunkt jeweils verwendet wird.

## <a name="traffic-routing-methods"></a>Methoden für das Datenverkehrrouting

Wenn ein Endpunkt den Status „Heruntergestuft“ aufweist, wird er nicht mehr als Antwort auf DNS-Abfragen zurückgegeben. Stattdessen wird ein alternativer Endpunkt ausgewählt und zurückgegeben. Die im Profil konfigurierte Methode für das Datenverkehrrouting bestimmt, wie der alternative Endpunkt ausgewählt wird.

* **Priorität**. Endpunkte bilden eine mit Prioritäten versehene Liste. Der erste verfügbare Endpunkt der Liste wird immer zurückgegeben. Wenn ein Endpunkt sich im Status „Heruntergestuft“ befindet, wird der nächste verfügbare Endpunkt zurückgegeben.
* **Gewichtet**. Alle verfügbaren Endpunkte werden basierend auf der ihnen zugewiesenen Gewichtung und den Gewichtungen der anderen verfügbaren Endpunkte zufällig ausgewählt.
* **Leistung**. Der Endpunkt, der dem Endbenutzer am nächsten liegt, wird zurückgegeben. Wenn dieser Endpunkt nicht verfügbar ist, wird aus allen anderen verfügbaren Endpunkten zufällig ein Endpunkt ausgewählt. Durch das zufällige Auswählen eines Endpunkts wird ein überlappender Fehler verhindert, der auftreten kann, wenn der nächstgelegene Endpunkt überlastet wird. Sie können alternative Failoverpläne für das leistungsorientierte Datenverkehrsrouting mithilfe von [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region) konfigurieren.

Weitere Informationen finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

> [!NOTE]
> Eine Ausnahme des normalen Datenverkehrsrouting-Verhaltens tritt auf, wenn alle auswählbaren Endpunkte den Status „Heruntergestuft“ aufweisen. Traffic Manager wählt die beste Lösung und *antwortet so, als befänden sich alle heruntergestuften Endpunkte im Status „Online“*. Dieses Verhalten ist besser als die Alternative, bei der für die DNS-Antwort keinerlei Endpunkte zurückgegeben würden. Deaktivierte oder beendete Endpunkte werden nicht überwacht und daher für den Datenverkehr als nicht auswählbar betrachtet.
>
> Diese Bedingung wird häufig durch eine falsche Konfiguration des Dienst verursacht, wie z.B.:
>
> * Eine Zugriffssteuerungsliste (Access Control List, ACL) blockiert die Traffic Manager-Integritätsprüfungen
> * Eine falsche Konfiguration des Überwachungspfads im Traffic Manager-Profil
>
> Wenn die Traffic Manager-Integritätsprüfungen nicht richtig konfiguriert sind, führt dieses Verhalten dazu, dass es aufgrund des Datenverkehrsroutings so aussieht, *als ob* Traffic Manager richtig funktioniert. Allerdings kann in diesem Fall kein Endpunktfailover durchgeführt werden. Dies wirkt sich auf die Gesamtverfügbarkeit der Anwendung aus. Es ist wichtig, sicherzustellen, dass das Profil den Status „Online“ aufweist, und nicht den Status „Heruntergestuft“. Der Status „Online“ weist darauf hin, dass die Traffic Manager-Integritätsprüfungen wie gewünscht durchgeführt werden.

Weitere Informationen zur Behandlung von Fehlern bei Integritätsprüfungen finden Sie unter [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)

Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .

Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](traffic-manager-manage-profiles.md)

[Problembehandlung beim Status „Heruntergestuft“](traffic-manager-troubleshooting-degraded.md) auf einem Traffic Manager-Endpunkt.

