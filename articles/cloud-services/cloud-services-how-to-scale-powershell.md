---
title: Skalieren eines Azure-Clouddiensts in Windows PowerShell | Microsoft-Dokumentation
description: (Klassisch) Hier erfahren Sie, wie Sie mithilfe von PowerShell eine Web- oder Workerrolle in Azure horizontal herunter- oder hochskalieren.
services: cloud-services
documentationcenter: 
author: seanmck
manager: timlt
editor: 
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 4acea8e7f4714754e6b3e54e9aaa622219ca9f7f
ms.openlocfilehash: f16d981a0dc33295c1981d9c282beb1d3243d9ef


---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a>Skalieren eines Clouddiensts in PowerShell

Sie können Windows PowerShell verwenden, um eine Web- oder Workerrolle durch Hinzuzufügen oder Entfernen von Instanzen horizontal hoch- oder herunterzuskalieren.  

## <a name="log-in-to-azure"></a>Anmelden an Azure

Damit Sie über PowerShell einen Vorgang für Ihr Abonnement ausführen können, müssen Sie sich anmelden:

```powershell
Add-AzureAccount
```

Falls Ihrem Konto mehrere Abonnements zugeordnet sind, müssen Sie abhängig davon, wo sich Ihr Clouddienst befindet, ggf. das aktuelle Abonnement ändern. Führen Sie den folgenden Befehl aus, um das aktuelle Abonnement zu überprüfen:

```powershell
Get-AzureSubscription -Current
```

Falls Sie das aktuelle Abonnement ändern müssen, führen Sie den folgenden Befehl aus:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a>Überprüfen der aktuellen Instanzenanzahl für Ihre Rolle

Führen Sie den folgenden Befehl aus, um den aktuellen Zustand Ihrer Rolle zu überprüfen:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Daraufhin sollten Sie Informationen zur Rolle erhalten (einschließlich der aktuellen Betriebssystemversion und der Instanzenanzahl). In diesem Fall ist eine einzelne Instanz für die Rolle vorhanden.

![Informationen zur Rolle](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a>Horizontales Hochskalieren der Rolle durch Hinzufügen weiterer Instanzen

Wenn Sie die Rolle horizontal hochskalieren möchten, übergeben Sie die gewünschte Anzahl von Instanzen als **Count**-Parameter an das **Set-AzureRole**-Cmdlet:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

Das Cmdlet blockiert kurzzeitig, während die neuen Instanzen bereitgestellt und gestartet werden. Wenn Sie während dieser Zeit ein neues PowerShell-Fenster öffnen und **Get-AzureRole** wie oben gezeigt aufrufen, erscheint die neue Zielinstanzenanzahl. Und wenn Sie sich den Rollenstatus im Portal ansehen, sollten Sie sehen, dass die neue Instanz gestartet wird:

![Startende VM-Instanz im Portal](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Sobald die neuen Instanzen gestartet wurden, wird die Blockierung des Cmdlets wieder aufgehoben:

![Erfolgreiche Erhöhung der Anzahl von Rolleninstanzen](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a>Horizontales Herunterskalieren der Rolle durch Entfernen von Instanzen

Sie können eine Rolle auf die gleiche Weise horizontal herunterskalieren, indem Sie Instanzen entfernen. Legen Sie den **Count**-Parameter für **Set-AzureRole** auf die Anzahl von Instanzen fest, die nach Abschluss des horizontalen Herunterskalierens vorhanden sein soll.

## <a name="next-steps"></a>Nächste Schritte

Die automatische Skalierung für Clouddienste kann über PowerShell nicht konfiguriert werden. Informationen zum automatischen Skalieren eines Clouddiensts finden Sie [hier](cloud-services-how-to-scale-portal.md).



<!--HONumber=Dec16_HO1-->


