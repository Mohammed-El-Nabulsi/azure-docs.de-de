---
title: "Verfügbarkeitsgruppen für Linux-VMs in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung von Verfügbarkeitsgruppen in Azure-Infrastrukturdiensten."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 24f1d91c-8cc0-4251-bb67-ac4c4c37e8cd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: b67f13d279d68712beea78c93d40881d92ba8896
ms.lasthandoff: 03/18/2017


---
# <a name="azure-availability-sets-guidelines-for-linux-vms"></a>Richtlinien für Azure-Verfügbarkeitsgruppen für Linux-VMs

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

In diesem Artikel werden die erforderlichen Planungsschritte für Verfügbarkeitsgruppen erläutert, sodass sichergestellt ist, dass auf Ihre Anwendung bei geplanten oder ungeplanten Ereignissen zugegriffen werden kann.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementierungsrichtlinien für Verfügbarkeitsgruppen
Entscheidungen:

* Wie viele Verfügbarkeitsgruppen benötigen Sie für die verschiedenen Rollen und Ebenen in Ihrer Anwendungsinfrastruktur?

Aufgaben:

* Definieren Sie die Anzahl der virtuellen Computer in jeder Anwendungsebene, die Sie benötigen.
* Bestimmen Sie, ob Sie die Anzahl der Fehler- oder Updatedomäne für Ihre Anwendung anpassen müssen.
* Definieren Sie die erforderlichen Verfügbarkeitsgruppen mithilfe Ihrer Namenskonvention und die virtuellen Computer in den Verfügbarkeitsgruppen. Ein virtueller Computer kann sich nur in einer Verfügbarkeitsgruppe befinden. 

## <a name="availability-sets"></a>Verfügbarkeitsgruppen
In Azure können virtuelle Computer (VMs) in einer logischen Gruppierung, die als Verfügbarkeitsgruppe bezeichnet wird, platziert werden. Wenn Sie virtuelle Computer innerhalb einer Verfügbarkeitsgruppe erstellen, verteilt die Azure-Plattform die Platzierung der virtuellen Computer über die zugrunde liegende Infrastruktur. Bei einer geplanten Wartung der Azure-Plattform oder einem Fehler der zugrunde liegenden Hardware/Infrastruktur wird mithilfe von Verfügbarkeitsgruppen sichergestellt, dass mindestens ein virtueller Computer weiterhin ausgeführt wird.

Anwendungen sollten sich nicht auf einem einzelnen virtuellen Computer befinden. Eine Verfügbarkeitsgruppe, die einen einzelnen virtuellen Computer enthält, bietet keinen Schutz vor geplanten und ungeplanten Ereignissen innerhalb der Azure Platform. Die [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) erfordert mindestens zwei virtuelle Computer in einer Verfügbarkeitsgruppe, um die Verteilung der virtuellen Computer in der zugrunde liegenden Infrastruktur zu ermöglichen. Bei Verwendung von [Azure Storage Premium](../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) gilt die Azure-SLA für einen einzelnen virtuellen Computer.

Die zugrunde liegende Infrastruktur in Azure ist in mehrere Hardwarecluster unterteilt. Jeder Hardwarecluster kann einen Bereich von VM-Größen unterstützen. Eine Verfügbarkeitsgruppe kann zu einem Zeitpunkt nur auf einem einzelnen Hardwarecluster gehostet werden. Der Bereich der VM-Größen, die in einer einzelnen Verfügbarkeitsgruppe vorhanden sein können, ist deshalb auf den Bereich der VM-Größen beschränkt, die vom Hardwarecluster unterstützt werden. Der Hardwarecluster für die Verfügbarkeitsgruppe wird ausgewählt, wenn der erste virtuelle Computer in der Verfügbarkeitsgruppe bereitgestellt wird, oder wenn der erste virtuelle Computer in einer Verfügbarkeitsgruppe gestartet wird, bei der sich derzeit alle virtuellen Computer im Zustand „Beendet (Zuordnung aufgehoben)“ befinden. Der folgende CLI-Befehl kann verwendet werden, um den Bereich der VM-Größen zu bestimmen, die für eine Verfügbarkeitsgruppe verfügbar sind: „azure vm sizes --location \<string\>“.

Jeder Hardwarecluster ist in mehrere Updatedomänen und Fehlerdomänen unterteilt. Diese Domänen werden durch die Hosts definiert, die einen gemeinsamen Aktualisierungszyklus oder eine ähnliche physische Infrastruktur wie Stromversorgung und Netzwerk aufweisen. Azure verteilt Ihre virtuellen Computer automatisch in einer Verfügbarkeitsgruppe über Domänen hinweg, um Verfügbarkeit und Fehlertoleranz zu gewährleisten. Abhängig von der Größe der Anwendung und der Anzahl der virtuellen Computer in einer Verfügbarkeitsgruppe können Sie die Anzahl der Domänen anpassen, die Sie verwenden möchten. Informieren Sie sich über das [Verwalten von Verfügbarkeit und die Verwendung von Update- und Fehlerdomänen](virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Beim Entwerfen der Anwendungsinfrastruktur sollten Sie auch die Anwendungsebenen planen, die Sie verwenden möchten. Gruppieren Sie virtuelle Computer, die demselben Zweck dienen, in einer Verfügbarkeitsgruppe, z.B. in einer Verfügbarkeitsgruppe für virtuelle Front-End-Computer mit Nginx oder Apache. Erstellen Sie eine separate Verfügbarkeitsgruppe für Ihre virtuellen Back-End-Computer mit MongoDB oder MySQL. Dadurch soll gewährleistet werden, dass jede Komponente der Anwendung durch eine Verfügbarkeitsgruppe geschützt ist und mindestens eine Instanz immer ausgeführt wird.

Lastenausgleichsmodule können vor jeder Anwendungsebene zusammen mit einer Verfügbarkeitsgruppe genutzt werden und sicherstellen, dass der Datenverkehr immer an eine aktive Instanz weitergeleitet werden kann. Ohne einen Load Balancer werden Ihre virtuellen Computer möglicherweise während einer geplanten und ungeplanten Wartung weiter ausgeführt, Ihre Endbenutzer können aber möglicherweise die Probleme nicht beheben, wenn der primäre virtuelle Computer nicht verfügbar ist.

Wenn Sie nicht verwaltete Datenträger verwenden, entwerfen Sie Ihre Anwendung für hohe Verfügbarkeit auf Speicherebene. Verwenden Sie als bewährte Methode mehrere Speicherkonten für jeden virtuellen Computer in einer Verfügbarkeitsgruppe. Alle Datenträger (Betriebssystem und Daten) sollten einem virtuellen Computer im selben Speicherkonto zugeordnet sein. Beachten Sie die [Grenzwerte](../storage/storage-scalability-targets.md) des Speicherkontos, wenn Sie einem Speicherkonto weitere VHDs hinzufügen. Bei [Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) wird die Verteilung der zugrunde liegenden Datenträger für Sie übernommen.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]


