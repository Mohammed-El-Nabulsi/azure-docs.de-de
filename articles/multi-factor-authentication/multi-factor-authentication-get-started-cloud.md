---
title: Erste Schritte mit Azure MFA in der Cloud | Microsoft Docs
description: Auf dieser Seite zu Microsoft Azure Multi-Factor Authentication werden die ersten Schritte mit Azure MFA in der Cloud beschrieben.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 79c6ef627d20be52b6067d1f6dfea2a53505c590
ms.lasthandoff: 03/18/2017


---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Erste Schritte mit Azure Multi-Factor Authentication in der Cloud
In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie die ersten Schritte mit Azure Multi-Factor Authentication in der Cloud ausführen.

> [!NOTE]
> Die folgende Dokumentation enthält Informationen zum Aktivieren von Benutzern mithilfe des **klassischen Azure-Portals**. Weitere Informationen zum Einrichten von Azure Multi-Factor Authentication für Office 365-Benutzer finden Sie unter [Einrichten der mehrstufigen Authentifizierung für Office 365-Benutzer](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US).

![MFA in der Cloud](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a>Voraussetzung
[Registrieren Sie sich für ein Azure-Abonnement:](https://azure.microsoft.com/pricing/free-trial/) Falls Sie noch nicht über ein Azure-Abonnement verfügen, müssen Sie sich für ein Abonnement registrieren. Wenn Sie die Verwendung von Azure MFA testen möchten, können Sie ein Testabonnement verwenden.

## <a name="enable-azure-multi-factor-authentication"></a>Aktivieren von Azure Multi-Factor Authentication
Solange Ihre Benutzer über Lizenzen verfügen, die Azure Multi-Factor Authentication enthalten, müssen Sie nichts unternehmen, um Azure MFA zu aktivieren. Sie können die Überprüfung in zwei Schritten auf Benutzerbasis obligatorisch machen. Azure MFA kann mit den folgenden Lizenzen aktiviert werden:
- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

Falls Sie nicht über eine dieser drei Lizenzen verfügen oder nicht genügend Lizenzen für alle Benutzer haben, ist dies auch kein Problem. Sie müssen lediglich einen zusätzlichen Schritt ausführen und in Ihrem Verzeichnis [einen Anbieter für die Multi-Factor Authentication erstellen](multi-factor-authentication-get-started-auth-provider.md).

## <a name="turn-on-two-step-verification-for-users"></a>Aktivieren der Überprüfung in zwei Schritten für Benutzer
Ändern Sie den Status des Benutzers von „Deaktiviert“ in „Aktiviert“, um die Überprüfung in zwei Schritten für einen Benutzer obligatorisch zu machen.  Weitere Informationen zum Status von Benutzern finden Sie unter [Benutzerstatus in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md).

Gehen Sie wie unten angegeben vor, um MFA für die Benutzer zu aktivieren.

### <a name="to-turn-on-multi-factor-authentication"></a>So aktivieren Sie Multi-Factor Authentication
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Klicken Sie im linken Bereich auf **Active Directory**.
3. Wählen Sie unter „Verzeichnis“ das Verzeichnis für den Benutzer, den Sie aktivieren möchten.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Klicken Sie oben auf **Benutzer**.
5. Klicken Sie unten auf der Seite auf **Multi-Factor Auth verwalten**. Im Browser wird eine neue Registerkarte geöffnet.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Suchen Sie nach dem Benutzer, für den Sie die Überprüfung in zwei Schritten aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. Vergewissern Sie sich, dass der Status **Deaktiviert** lautet.
   ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. Aktivieren Sie das Kontrollkästchen **** neben dem gewünschten Namen.
8. Klicken Sie auf der rechten Seite auf **Aktivieren**.
   ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. Klicken Sie auf **Multi-Factor Auth aktivieren**.
   ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. Beachten Sie, dass sich der Zustand des Benutzers von **Deaktiviert** in **Aktiviert** geändert hat.
    ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user.png)

Nachdem Sie Ihre Benutzer aktiviert haben, sollten Sie sie per E-Mail benachrichtigen. Bei der nächsten Anmeldung werden die Benutzer aufgefordert, ihr Konto für die Überprüfung in zwei Schritten zu registrieren. Nach der ersten Verwendung der Überprüfung in zwei Schritten müssen die Benutzer außerdem App-Kennwörter einrichten, um zu verhindern, dass sie für andere Apps als Browser-Apps gesperrt werden.

## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Verwenden von PowerShell zum Automatisieren der Überprüfung in zwei Schritten
Sie können den [Zustand](multi-factor-authentication-whats-next.md) mithilfe von [Azure AD PowerShell](/powershell/azureps-cmdlets-docs) wie folgt ändern.  `$st.State` kann auf einen der folgenden Zustände festgelegt werden:

* Aktiviert
* Erzwungen
* Deaktiviert  

> [!IMPORTANT]
> Wir raten davon ab, Benutzer direkt aus dem Zustand „Deaktiviert“ in den Zustand „Erzwungen“ zu schalten. Andere Apps als Browser-Apps funktionieren nicht mehr, da der Benutzer die MFA-Registrierung durchlaufen und ein [App-Kennwort](multi-factor-authentication-whats-next.md#app-passwords) erhalten hat. Wenn Sie über Apps verfügen, die nicht browserbasiert sind, und App-Kennwörter benötigen, empfehlen wir den Wechsel von „Deaktiviert“ zu „Aktiviert“. So können sich Benutzer registrieren und ihr App-Kennwort beziehen. Danach können Sie die Umschaltung auf „Erzwungen“ durchführen.

Die Verwendung von PowerShell ist eine Option für die Massenaktivierung von Benutzern. Derzeit steht im Azure-Portal keine Option für eine Massenaktivierung zur Verfügung, sodass jeder Benutzer einzeln ausgewählt werden muss. Dies kann bei einer großen Anzahl von Benutzern ziemlich aufwendig sein. Mithilfe eines PowerShell-Skripts mit den folgenden Angaben können Sie eine Liste mit Benutzern durchlaufen und die Benutzer aktivieren.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Beispiel:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Weitere Informationen finden Sie unter [Benutzerstatus in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Azure Multi-Factor Authentication in der Cloud eingerichtet haben, können Sie die Bereitstellung konfigurieren und einrichten. Weitere Informationen finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md).


