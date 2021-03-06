---
title: Beheben von Problemen mit Azure File Storage | Microsoft Docs
description: Beheben von Problemen mit Azure File Storage
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: na
tags: storage
ms.assetid: fbc5f600-131e-4b99-828a-42d0de85fff7
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 7f719fb38709f4bb7083b7f21a5979f7e0588d0f
ms.lasthandoff: 03/22/2017


---
# <a name="troubleshooting-azure-file-storage-problems"></a>Beheben von Problemen mit Azure File Storage
Dieser Artikel beschreibt allgemeine Probleme im Zusammenhang mit Microsoft Azure File Storage, wenn Sie eine Verbindung von Windows- und Linux-Clients herstellen. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme bereitgestellt.

**Allgemeine Probleme (treten sowohl auf Windows- als auch auf Linux-Clients auf)**

* [Kontingentfehler beim Öffnen einer Datei](#quotaerror)
* [Geringe Leistung beim Zugriff auf Azure File Storage über Windows oder Linux](#slowboth)
* [Verfolgen von Lese- und Schreibvorgängen in Azure File Storage](#traceop)

**Windows-Clientprobleme**

* [Geringe Leistung beim Zugriff auf Azure File Storage über Windows 8.1 oder Windows Server 2012 R2](#windowsslow)
* [Fehler 53 beim Versuch, eine Azure-Dateifreigabe einzubinden](#error53)
* [Fehler 87: Falscher Parameter bei dem Versuch, eine Azure-Dateifreigabe bereitzustellen](#error87)
* [„net use“ war erfolgreich, aber die Azure Dateifreigabe wird nicht als in der Windows-Explorer-Benutzeroberfläche eingebunden oder als Laufwerkbuchstabe angezeigt](#netuse)
* [Mein Speicherkonto enthält „/“, und beim Ausführen des Befehls „net use“ tritt ein Fehler auf](#slashfails)
* [Meine Anwendung bzw. mein Dienst kann nicht auf das eingebundene Azure Files-Laufwerk zugreifen.](#accessfiledrive)
* [Zusätzliche Empfehlungen zur Leistungsoptimierung](#additional)
* [Fehler „You are copying a file to a destination that does not support encryption“ (Sie kopieren eine Datei in ein Ziel, das die Verschlüsselung nicht unterstützt) beim Hochladen/Kopieren von Dateien in Azure Files](#encryption)

**Linux-Clientprobleme**

* [Zeitweiliger E/A-Fehler „Host nicht verfügbar“ (Fehler 112) bei vorhandenen Dateifreigaben, oder die Shell hängt beim Ausführen von Listenbefehlen auf dem Einbindungspunkt](#errorhold)
* [Einbindungsfehler 115 beim Versuch, Azure Files auf der Linux-VM einzubinden](#error15)
* [In der Azure-Dateifreigabe auf der Linux-VM treten Probleme mit langsamer Leistung auf](#delayproblem)
* [Einbindungsfehler (11): Ressource beim Einbinden von Ubuntu-Kerneln ab Version 4.8 vorübergehend nicht verfügbar](#ubuntumounterror)

<a id="quotaerror"></a>

## <a name="quota-error-when-trying-to-open-a-file"></a>Kontingentfehler beim Öffnen einer Datei
Unter Windows erhalten Sie Fehlermeldungen, die wie folgt aussehen:

`1816 ERROR_NOT_ENOUGH_QUOTA <--> 0xc0000044`
`STATUS_QUOTA_EXCEEDED`
`Not enough quota is available to process this command`
`Invalid handle value GetLastError: 53`

Unter Linux erhalten Sie Fehlermeldungen, die wie folgt aussehen:

`<filename> [permission denied]`
`Disk quota exceeded`

### <a name="cause"></a>Ursache
Das Problem tritt auf, da Sie die obere Grenze der gleichzeitig geöffneten Handles erreicht haben, die für eine Datei zulässig sind.

### <a name="solution"></a>Lösung
Reduzieren Sie die Anzahl der gleichzeitig geöffneten Handles, indem Sie einige Handles schließen und es anschließend erneut versuchen. Weitere Informationen finden Sie unter [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](storage-performance-checklist.md).

<a id="slowboth"></a>

## <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Geringe Leistung beim Zugriff auf Azure File Storage über Windows oder Linux
* Wenn Sie keine bestimmte Anforderung für die Mindest-E/A-Größe haben, empfehlen wir Ihnen für eine optimale Leistung die Verwendung von 1 MB als E/A-Größe.
* Wenn Sie die endgültige Größe einer Datei kennen, die Sie mit Schreibvorgängen erweitern, und Ihre Software keine Kompatibilitätsprobleme aufweist, wenn das noch nicht geschriebene Fragment in der Datei Nullen enthält, legen Sie die Dateigröße im Voraus fest, anstatt dass jeder Schreibvorgang einen Erweiterungsschreibvorgang darstellt.
* Verwenden Sie die richtige Kopiermethode:
      * Verwenden Sie AZCopy für Übertragungen zwischen zwei Dateifreigaben. Ausführlichere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy#file-copy).
      * Verwenden Sie Robocopy zwischen Dateifreigaben auf einem lokalen Computer. Weitere Details finden Sie unter [Multi-threaded robocopy for faster copies](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) (Multithread-Robocopy für schnellere Kopien).
<a id="windowsslow"></a>

## <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Geringe Leistung beim Zugriff auf Azure File Storage über Windows 8.1 oder Windows Server 2012 R2
Stellen Sie für Clients, auf denen Windows 8.1 oder Windows Server 2012 R2 ausgeführt wird, sicher, dass der Hotfix [KB3114025](https://support.microsoft.com/kb/3114025) installiert ist. Dieser Hotfix verbessert die Leistung beim Erstellen und Schließen eines Handle.

Sie können das folgende Skript ausführen, um zu überprüfen, ob der Hotfix installiert wurde:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Wenn der Hotfix installiert wurde, wird die folgende Ausgabe angezeigt:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`
`{96c345ef-3cac-477b-8fcd-bea1a564241c}    REG_DWORD    0x1`

> [!NOTE]
> Bei Windows Server 2012 R2-Images in Azure Marketplace ist der Hotfix KB3114025 ab Dezember 2015 standardmäßig installiert.
>
>

<a id="traceop"></a>

### <a name="how-to-trace-the-read-and-write-operations-in-azure-file-storage"></a>Verfolgen von Lese- und Schreibvorgängen in Azure File Storage

[Microsoft Message Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226) kann eine Kundenanforderung im Klartext anzeigen. Außerdem gibt es eine deutliche Beziehung zwischen Überweisungsanforderungen und Transaktionen (bei Annahme von SMB anstelle von REST).  Der Nachteil ist, dass Sie dies auf jedem Client ausführen müssen, sodass der Vorgang bei vielen IaaS-VM-Mitarbeitern sehr zeitaufwendig ist.

Wenn Sie Message Analyzer mit ProcMon verwenden, erhalten Sie eine sehr gute Übersicht darüber, welcher App-Code für die Transaktionen verantwortlich ist.

<a id="additional"></a>

## <a name="additional-recommendations-to-optimize-performance"></a>Zusätzliche Empfehlungen zur Leistungsoptimierung
Erstellen oder öffnen Sie niemals eine Datei für zwischengespeichertes E/A, die Schreibzugriff, jedoch keinen Lesezugriff anfordert. D.h., geben Sie beim Aufruf von **CreateFile()** niemals nur **GENERIC_WRITE** an, sondern immer **GENERIC_READ | GENERIC_WRITE**. Ein lesegeschütztes Handle kann kleine Schreibvorgänge nicht lokal zwischenspeichern, selbst wenn es das einzige geöffnete Handle der Datei ist. Dies führt zu schwerwiegenden Leistungseinbußen für kleine Schreibvorgänge. Beachten Sie, dass der „a“-Modus auf CRT **fopen()** ein lesegeschütztes Handle öffnet.

<a id="error53"></a>

## <a name="error-53-or-error-67-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>„Fehler 53“ oder „Fehler 67“ beim Versuch, eine Azure-Dateifreigabe einzubinden oder die Einbindung aufzuheben
Dieses Problem kann folgende Ursachen haben:

### <a name="cause-1"></a>Ursache 1
„Systemfehler 53 ist aufgetreten. Zugriff verweigert.“ Aus Sicherheitsgründen werden Verbindungen mit Azure File-Freigaben blockiert, wenn der Kommunikationskanal nicht verschlüsselt ist und der Verbindungsversuch nicht von dem gleichen Rechenzentrum aus erfolgt, in dem sich die Azure File-Freigaben befinden. Die Verschlüsselung des Kommunikationskanals ist nicht verfügbar, wenn das Clientbetriebssystem des Benutzers die SMB-Verschlüsselung nicht unterstützt. Dies wird durch die Fehlermeldung „Systemfehler 53 ist aufgetreten. Zugriff verweigert“ angezeigt, wenn ein Benutzer versucht, eine Dateifreigabe von einer lokalen Umgebung oder einem anderen Rechenzentrum aus einzubinden. Windows 8, Windows Server 2012 und höhere Versionen jeder Negotiate-Anforderung, die SMB 3.0 umfassen, wodurch die Verschlüsselung unterstützt wird.

### <a name="solution-for-cause-1"></a>Lösung für Ursache 1
Stellen Sie eine Verbindung von einem Client aus her, der die Anforderungen von Windows 8, Windows Server 2012 und höheren Versionen erfüllt, oder stellen Sie eine Verbindung von einem virtuellen Computer aus her, der sich im gleichen Rechenzentrum befindet wie das Azure Storage-Konto, das für die Azure-Dateifreigabe verwendet wird.

### <a name="cause-2"></a>Ursache 2
„Systemfehler 53“ oder „Systemfehler 67“ kann beim Einbinden einer Azure-Dateifreigabe auftreten, wenn die von Port 445 ausgehende Kommunikation zum Azure Files-Rechenzentrum blockiert ist. Klicken Sie [hier](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx), um eine Zusammenfassung der ISPs anzuzeigen, die den Zugang von Port 445 aus zulassen oder verweigern.

Comcast und einige IT-Organisationen blockieren diesen Port. Um zu verstehen, ob dies der Grund für die Meldung „Systemfehler 53“ ist, können Sie Portqry verwenden, um den Endpunkt TCP:445 abzufragen. Wenn der Endpunkt TCP:445 als gefiltert angezeigt wird, wird der TCP-Port blockiert. Dies ist eine Beispielabfrage:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Wenn der TCP 445 durch eine Regel entlang des Netzwerkpfads blockiert wird, wird Ihnen die folgende Ausgabe angezeigt:

**TCP port 445 (microsoft-ds service): FILTERED**

Weitere Informationen zur Verwendung von Portqry finden Sie unter [Beschreibung des Befehlszeilenprogramms Portqry.exe](https://support.microsoft.com/kb/310099).

### <a name="solution-for-cause-2"></a>Lösung für Ursache 2
Arbeiten Sie mit Ihrer IT-Organisation zusammen, um den Port 445-Ausgang zu [Azure-IP-Bereichen](https://www.microsoft.com/download/details.aspx?id=41653) zu öffnen.

<a id="error87"></a>
### <a name="cause-3"></a>Ursache 3
„Systemfehler 53“ oder „Systemfehler 87“ können auch auftreten, wenn die NTLMv1-Kommunikation auf dem Client aktiviert ist. Wenn NTLMv1 aktiviert ist, ist der Client weniger sicher. Aus diesem Grund wird die Kommunikation für Azure-Dateien blockiert. Um zu überprüfen, ob dies die Ursache des Fehlers ist, überprüfen Sie, ob der folgende Registrierungsunterschlüssel auf den Wert 3 festgelegt ist:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Weitere Informationen finden Sie im Thema [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) im TechNet.

### <a name="solution-for-cause-3"></a>Lösung für Ursache 3
Um dieses Problem zu beheben, setzen Sie den LmCompatibilityLevel-Wert im Registrierungsschlüssel „HKLM\SYSTEM\CurrentControlSet\Control\Lsa“ auf den Standardwert „3“ zurück.

Azure Files unterstützt nur die NTLMv2-Authentifizierung. Stellen Sie sicher, dass die Gruppenrichtlinie auf die Clients angewendet wird. Dadurch wird verhindert, dass dieser Fehler auftritt. Dies gilt zudem als bewährte Sicherheitsmethode. Weitere Informationen finden Sie unter [Netzwerksicherheit: LAN Manager-Authentifizierungsebene](https://technet.microsoft.com/library/jj852207\(v=ws.11\).aspx).

Die empfohlene Richtlinieneinstellung ist **Nur NTLMv2-Antworten senden**. Dies entspricht einem Registrierungswert von 3. Clients verwenden nur die NTLMv2-Authentifizierung, und sie verwenden die NTLMv2-Sitzungssicherheit, wenn der Server diese unterstützt. Domänencontroller akzeptieren die NTLMv2-, LM- und NTLM-Authentifizierung.

<a id="netuse"></a>

## <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>„net use“ war erfolgreich, jedoch wird nicht angezeigt, dass die Azure-Dateifreigabe in den Windows-Explorer eingebunden wurde
### <a name="cause"></a>Ursache
Standardmäßig wird der Windows-Explorer nicht als Administrator ausgeführt. Wenn Sie **net use** von einer Administratoreingabeaufforderung aus ausführen, ordnen Sie das Netzlaufwerk „Als Administrator“ zu. Da zugeordnete Laufwerke benutzerorientiert sind, zeigt das angemeldete Benutzerkonto die Laufwerke nicht an, wenn sie unter einem anderen Benutzerkonto eingebunden werden.

### <a name="solution"></a>Lösung
Binden Sie die Freigabe von einer Befehlszeile für Benutzer ohne Administratorrechte aus ein. Alternativ können Sie [diesem TechNet-Thema folgen](https://technet.microsoft.com/library/ee844140.aspx), um den Registrierungswert **EnableLinkedConnections** zu konfigurieren.

<a id="slashfails"></a>

## <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Mein Speicherkonto enthält „/“, und beim Ausführen des Befehls „net use“ tritt ein Fehler auf
### <a name="cause"></a>Ursache
Wenn der Befehl **net use** unter einer Eingabeaufforderung ausgeführt wird (cmd.exe), wird er durch das Hinzufügen von „/“ als Befehlszeilenoption analysiert. Dadurch tritt ein Fehler bei der Zuordnung des Laufwerks auf.

### <a name="solution"></a>Lösung
Sie können eine der folgenden Methoden verwenden, um dieses Problem zu umgehen:

•    Verwenden Sie den folgenden PowerShell-Befehl:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Von einer Batchdatei aus kann dies folgendermaßen aussehen:

`Echo new-smbMapping ... | powershell -command –`

•    Setzen Sie den Schlüssel in doppelte Anführungszeichen, um dieses Problem zu umgehen, sofern das erste Zeichen nicht „/“ ist. Wenn dies der Fall ist, verwenden Sie den interaktiven Modus, und geben Sie Ihr Kennwort separat ein, oder generieren Sie Ihre Schlüssel neu, um einen Schlüssel zu erhalten, der nicht mit einem Schrägstrich (/) beginnt.

<a id="accessfiledrive"></a>

## <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Meine Anwendung bzw. mein Dienst kann nicht auf das eingebundene Azure Files-Laufwerk zugreifen
### <a name="cause"></a>Ursache
Laufwerke werden pro Benutzer eingebunden. Wenn Ihre Anwendung oder Ihr Dienst unter einem anderen Benutzerkonto ausgeführt wird, wird Benutzern das Laufwerk nicht angezeigt.

### <a name="solution"></a>Lösung
Binden Sie das Laufwerk von dem gleichen Benutzerkonto aus ein, unter dem sich die Anwendung befindet. Dies kann durch die Verwendung von Tools wie z.B. psexec erfolgen.

Eine weitere Möglichkeit für **net use** ist, den Speicherkontonamen und -schlüssel an die Parameter des **net use**-Befehls zu übergeben.

Nachdem Sie diese Anleitung befolgt haben, erhalten Sie möglicherweise die folgende Fehlermeldung: „Systemfehler 1312 ist aufgetreten. Eine angegebene Anmeldesitzung ist nicht vorhanden. Sie wurde gegebenenfalls bereits beendet.“, wenn Sie **net use** für das System-/Netzwerkdienstkonto ausführen. Wenn dies auftritt, vergewissern Sie sich, dass der an **net use** übergebene Benutzername Domäneninformationen enthält (z.B. „[Speicherkontoname].file.core.windows.net“).

<a id="encryption"></a>

## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fehler „You are copying a file to a destination that does not support encryption“ (Sie kopieren eine Datei in ein Ziel, das die Verschlüsselung nicht unterstützt)
### <a name="cause"></a>Ursache
Mit BitLocker verschlüsselte Dateien können in Azure Files kopiert werden. File Storage unterstützt NTFS EFS jedoch nicht. Aus diesem Grund werden Sie in diesem Fall wahrscheinlich EFS verwenden. Wenn Sie über Dateien verfügen, die über EFS verschlüsselt sind, kann beim Kopiervorgang in File Storage ein Fehler auftreten, sofern der Kopierbefehl keine kopierte Datei entschlüsselt.

### <a name="workaround"></a>Problemumgehung
Um eine Datei in File Storage zu kopieren, müssen Sie sie zunächst entschlüsseln. Hierfür können Sie eine der folgenden Methoden verwenden:

•    Verwenden Sie **copy /d**.

•    Legen Sie den folgenden Registrierungsschlüssel fest:

* Path=HKLM\Software\Policies\Microsoft\Windows\System
* Value type=DWORD
* Name = CopyFileAllowDecryptedRemoteDestination
* Value = 1

Beachten Sie jedoch, dass sich das Festlegen des Registrierungsschlüssels auf alle Kopiervorgänge von Netzwerkfreigaben auswirkt.

<a id="errorhold"></a>

## <a name="host-is-down-error-112-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Fehler „Host is down“ (Host nicht verfügbar) (Fehler 112) bei vorhandenen Dateifreigaben, oder die Shell hängt beim Ausführen von Listenbefehlen auf dem Einbindungspunkt
### <a name="cause"></a>Ursache
Dieser Fehler tritt auf dem Linux-Client auf, wenn sich der Client für einen längeren Zeitraum im Leerlauf befand. Wenn der Client über einen längeren Zeitraum im Leerlauf ist, trennt der Client die Verbindung, und der Timeout für die Verbindung wird erreicht. 

Die Verbindung kann sich aufgrund verschiedener Ursachen im Leerlauf befinden. Ein Grund dafür sind Netzwerkkommunikationsfehler, die das erneute Wiederherstellen einer TCP-Verbindung mit dem Server verhindern, wenn die Option zur zeitweiligen Einbindung verwendet wird (dies ist die Standardeinstellung).

Ein weiterer Grund dafür ist möglicherweise, dass es auch einige erneute Verbindungen gibt, die nicht in älteren Kernels befinden.

### <a name="solution"></a>Lösung

Durch Festlegen einer ständigen Einbindung wird der Client gezwungen, zu warten, bis eine Verbindung hergestellt oder explizit unterbrochen wurde. Auf diese Weise lassen sich auch Fehler aufgrund von Netzwerktimeouts verhindern. Benutzer sollten sich jedoch darüber im Klaren sein, dass diese Einstellung zu unendlichen Wartevorgängen führen kann, und Verbindungen bei Bedarf anhalten.

Dieses Verbindungsproblem im Linux-Kernel ist nun als Teil der folgenden Changesets

* [Fix reconnect to not defer smb3 session reconnect long after socket reconnect (Stellen Sie die Verbindung wieder her, um die Verbindungswiederherstellung der smb3-Sitzung lange nach der Socket-Verbindungswiederherstellung rückzustellen)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)

* [Call echo service immediately after socket reconnect (Sofortiges Aufrufen des Echo-Diensts nach der Socket-Verbindungswiederherstellung)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)

* [CIFS: Fix a possible memory corruption during reconnect (CIFS: Beheben einer Arbeitsspeicherbeschädigung während der Verbindungswiederherstellung)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)

* [CIFS: Fix a possible double locking of mutex during reconnect - for kernels v4.9 and higher (CIFS: Beheben einer möglichen doppelten Sperre des Mutex während der Verbindungswiederherstellung – für Kernels v4.9 und höher)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183) 

Diese Änderung kann möglicherweise noch nicht an alle Linux-Distributionen portiert werden. Dies ist die Liste häufig verwendeter und bekannter Linux-Kernel, die über diese und andere Korrekturen eine erneute Verbindung verfügen: 4.4.40+, 4.8.16+, 4.9.1+.
Sie können zu den oben empfohlenen Kernelversionen wechseln, um die neueste Fehlerbehebung zu übernehmen.

### <a name="workaround"></a>Problemumgehung
Wenn Sie die neuesten Kernelversionen nicht aufrufen können, können Sie dieses Problem umgehen, indem Sie eine Datei in der Azure-Dateifreigabe speichern, in die Sie alle 30 Sekunden oder früher schreiben. Dabei muss es sich um einen Schreibvorgang handeln, wie z.B. die Umschreibung des Erstellungs-/Änderungsdatums in der Datei. Andernfalls erhalten Sie möglicherweise zwischengespeicherte Ergebnisse, und Ihr Vorgang kann die Verbindungswiederherstellung möglicherweise nicht auslösen. 


<a id="error15"></a>

## <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>„Einbindungsfehler 115“ beim Versuch, Azure Files auf der Linux-VM einzubinden
### <a name="cause"></a>Ursache
Linux-Distributionen unterstützen die Verschlüsselungsfunktion in SMB 3.0 noch nicht. In manchen Distributionen erhalten Benutzer eine „115“-Fehlermeldung, wenn sie aufgrund eines fehlenden Features versuchen, Azure Files mithilfe von SMB 3.0 einzubinden.

### <a name="solution"></a>Lösung
Wenn der verwendete Linux-SMB-Client die Verschlüsselung nicht unterstützt, binden Sie Azure Files mithilfe von SMB 2.1 von einer Linux-VM aus ein, die sich im gleichen Rechenzentrum wie das File Storage-Konto befindet.

<a id="delayproblem"></a>

## <a name="azure-file-share-mounted-on-linux-vm-experiencing-slow-performance"></a>In der Azure-Dateifreigabe auf der Linux-VM treten Probleme mit langsamer Leistung auf

Ein möglicher Grund für langsame Leistung könnte deaktiviertes Caching sein. Um zu überprüfen, ob das Caching aktiviert ist, suchen Sie nach „cache=“.  *cache=none* gibt an, dass das Caching deaktiviert ist. Stellen Sie die Freigabe erneut bereit, entweder mit dem Standardbereitstellungsbefehl oder explizit durch das Hinzufügen der Option **cache=strict** zum Bereitstellungsbefehl, um sicherzustellen, dass das Standardcaching oder der Cachingmodus „strict“ aktiviert ist.

In einigen Szenarios kann die serverino-Bereitstellungsoption einen „Is“-Befehl auslösen, um „stat“ für jeden Verzeichniseintrag auszuführen. Dieses Verhalten für zu Leistungsbeeinträchtigungen bei Auflistung eines großen Verzeichnisses. Sie können die Bereitstellungsoption in Ihrem „/etc/fstab“-Eintrag finden:

`//azureuser.file.core.windows.net/cifs        /cifs   cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Sie können auch überprüfen, ob korrekte Optionen verwendet werden, indem Sie den Befehl **sudo mount | grep cifs** ausführen und die Ausgabe überprüfen:

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs
(rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777,
dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Wenn die Optionen „cache=strict“ oder „serverino“ nicht vorhanden sind, heben Sie die Bereitstellung von Azure Files aus, und stellen Sie sie wieder her, indem Sie den „mount“-Befehl aus der [Dokumentation](https://docs.microsoft.com/en-us/azure/storage/storage-how-to-use-files-linux#mount-the-file-share) ausführen und erneut überprüfen, dass der Eintrag „/etc/fstab“ die korrekten Optionen besitzt.

<a id="ubuntumounterror"></a>
## <a name="mount-error11-resource-temporarily-unavailable-when-mounting-to-ubuntu-48-kernel"></a>Einbindungsfehler (11): Ressource beim Einbinden von Ubuntu-Kerneln ab Version 4.8 vorübergehend nicht verfügbar

### <a name="cause"></a>Ursache
Bekanntes Problem in Ubuntu 16.10 (mit Kernelversion 4.8), bei dem der Client angibt, Verschlüsselung zu unterstützen, obwohl dies nicht zutrifft. 

### <a name="solution"></a>Lösung
Geben Sie bis zur Behebung des Problems in Ubuntu 16.10 die Einbindungsoption „vers=2.1“ an, oder verwenden Sie Ubuntu 16.04.
## <a name="learn-more"></a>Weitere Informationen
* [Erste Schritte mit Azure File Storage unter Windows](storage-dotnet-how-to-use-files.md)
* [Verwenden von Azure File Storage unter Linux](storage-how-to-use-files-linux.md)

