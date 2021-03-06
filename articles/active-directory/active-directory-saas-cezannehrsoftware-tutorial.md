---
title: 'Tutorial: Azure Active Directory-Integration mit Cezanne HR Software | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cezanne HR Software konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 9585494ebff68891d374a29e8e3e4b7756914bcc
ms.openlocfilehash: d8a654340df56002e503f2f61e910facb51696c5


---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Tutorial: Azure Active Directory-Integration mit Cezanne HR Software

In diesem Tutorial erfahren Sie, wie Sie Cezanne HR Software in Azure Active Directory (Azure AD) integrieren.

Die Integration von Cezanne HR Software in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Cezanne HR Software Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Cezanne HR Software anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Cezanne HR Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cezanne HR Software-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Cezanne HR Software aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Hinzufügen von Cezanne HR Software aus dem Katalog
Zum Konfigurieren der Integration von Cezanne HR Software in Azure AD müssen Sie Cezanne HR Software aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Cezanne HR Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld den Suchbegriff **Cezanne HR Software**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_00001.png)

5. Wählen Sie im Ergebnisbereich **Cezanne HR Software** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD in Cezanne HR Software konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in Cezanne HR Software zu einem Benutzer in Azure AD ist. Anders ausgedrückt muss zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cezanne HR Software eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in Cezanne HR Software zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Cezanne HR Software müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Cezanne HR Software-Testbenutzers](#creating-a-cezanne-hr-software-test-user)** , um eine Entsprechung von Britta Simon in Cezanne HR Software zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Verwaltungsportal und konfigurieren es in Ihrer Cezanne HR Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Cezanne HR Software die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Cezanne HR Software** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Cezanne HR Software** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_02.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`
    
    b. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `https://w3.cezanneondemand.com/CezanneOnDemand/`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Hier empfehlen wir Ihnen, den eindeutigen Wert der URL im Bezeichner zu verwenden. Wenden Sie sich an das [Supportteam von Cezanne HR Software](mailto:info@cezannehr.com), um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)     

5. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf das Kalendersymbol, und wählen Sie ein **Ablaufdatum** aus. Klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_300.png)

6. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Neues Zertifikat aktivieren**, und klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

7. Klicken Sie im Popupfenster **Rolloverzertifikat** auf **OK**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png) 

9. Klicken Sie im Abschnitt mit der **Cezanne HR Software-Konfiguration** auf **Cezanne HR Software konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_06.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_07.png)

10. Melden Sie sich in einem anderen Webbrowserfenster an Ihrem Cezanne HR Software-Mandanten als Administrator an.

11. Klicken Sie im linken Navigationsbereich auf **System Setup**(Systeminstallation). Wechseln Sie zu **Sicherheitseinstellungen**. Navigieren Sie anschließend zu **Single Sign-On Configuration**(Konfiguration des einmaligen Anmeldens).

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

12. Aktivieren Sie im Bereich **Allow users to log in using the following Single Sign-On (SSO) Service** (Benutzeranmeldung per SSO-Dienst zulassen) das Kontrollkästchen **SAML 2.0**, und wählen Sie die Option für die **Erweiterte Konfiguration** aus.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

13. Klicken Sie auf die Schaltfläche **Neu hinzufügen** .

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

14. Führen Sie im Abschnitt **SAML 2.0 IDENTITY PROVIDERS** (SAML 2.0-IDENTITÄTSANBIETER) die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    a. Geben Sie den Namen Ihres Identitätsanbieters als **Anzeigename**ein.

    b. Geben Sie im Textfeld **Entitätsbezeichner** den Wert für die **SAML-Entitäts-ID** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    c. Ändern Sie **SAML-Bindung** in „POST“.

    d. Geben Sie im Textfeld **Security Token Service Endpoint** (Sicherheitstoken-Dienstendpunkt) den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    e. Geben Sie im Textfeld **Benutzer-ID-Attributname** „http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name“ ein.

    f. Klicken Sie auf das Symbol **Hochladen** , um das heruntergeladene Zertifikat aus Azure AD hochzuladen.

    g. Klicken Sie auf die Schaltfläche **OK** . 

15. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 



### <a name="creating-a-cezanne-hr-software-test-user"></a>Erstellen eines Cezanne HR Software-Testbenutzers

Damit sich Azure AD-Benutzer bei Cezanne HR Software anmelden können, müssen sie in Cezanne HR Software bereitgestellt werden. Bei Cezanne HR Software ist die Bereitstellung ein manueller Vorgang.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1.  Melden Sie sich als Administrator auf Ihrer Cezanne HR Software-Unternehmenswebsite an.

2.  Klicken Sie im linken Navigationsbereich auf **System Setup**(Systeminstallation). Navigieren Sie zu **Benutzer verwalten**. Navigieren Sie anschließend zu **Neuen Benutzer hinzufügen**.

    ![Neuer Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Neuer Benutzer")

3.  Führen Sie im Abschnitt **Person Details** (Angaben zur Person) die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Neuer Benutzer")

    a. Legen Sie **Interner Benutzer** auf „AUS“ fest.

    b. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    c. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    d. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

4.  Führen Sie unter **Kontoinformationen** die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Neuer Benutzer")

    a. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse von Britta Simon ein.

    b. Geben Sie im Textfeld **Kennwort** das Kennwort des Kontos von Britta Simon ein.

    c. Wählen Sie unter **Sicherheitsrolle** die Option **HR Professional** (HR-Mitarbeiter).

    d. Klicken Sie auf **OK**.

5. Navigieren Sie zur Registerkarte **Einmalige Anmeldung**, und wählen Sie im Bereich **SAML 2.0 Identifiers** (SAML 2.0-Bezeichner) die Option **Neu hinzufügen**.

    ![Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Benutzer")

6. Wählen Sie unter **Identitätsanbieter** Ihren Identitätsanbieter aus, und geben Sie im Textfeld von **Benutzer-ID** die E-Mail-Adresse des Kontos von Britta Simon ein.

    ![Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Benutzer")
    
7. Klicken Sie auf die Schaltfläche **Save** .

    ![Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Benutzer")



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Cezanne HR Software gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Cezanne HR Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie dann zur Verzeichnisansicht, wechseln Sie zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Cezanne HR Software**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Cezanne HR Software“ klicken, sollten Sie automatisch an Ihrer Cezanne HR Software-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png


<!--HONumber=Feb17_HO1-->


