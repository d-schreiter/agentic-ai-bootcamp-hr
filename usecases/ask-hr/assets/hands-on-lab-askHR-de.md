
## Inhaltsverzeichnis

- [Anwendungsfall-Beschreibung](#anwendungsfall-beschreibung)
- [Architektur](#architektur)
- [Voraussetzungen](#voraussetzungen)
- [Anleitung](#anleitung)
  - [Agent Builder öffnen](#agent-builder-öffnen)
  - [HR-Agent erstellen](#hr-agent-erstellen)
  - [HR-Agent in der Vorschau testen](#hr-agent-in-der-vorschau-testen)
  - [HR-Agent im AI Chat testen](#hr-agent-im-ai-chat-testen)

## Anwendungsfall-Beschreibung

Dieser Anwendungsfall zielt darauf ab, einen AskHR-Agenten unter Verwendung von IBM watsonx Orchestrate zu entwickeln und bereitzustellen, wie im bereitgestellten Architekturdiagramm dargestellt. Dieser Agent ermöglicht es Mitarbeitern, effizient über Conversational AI mit HR-Systemen zu interagieren und auf Informationen zuzugreifen.

In diesem Lab werden wir einen HR-Agenten in watsonx Orchestrate erstellen, der Tools und externes Wissen nutzt, um sich mit einem simulierten Human Capital Management System zu verbinden. Dieser Agent ruft relevante Informationen aus Dokumenten ab, um Benutzeranfragen zu beantworten, und ermöglicht es Benutzern, ihre Profile anzuzeigen und zu verwalten.

Zusätzlich bietet IBM watsonx Orchestrate einen einfachen Zugang zu vorgefertigten Domain-Agenten und Tools, einschließlich solcher im HR-Bereich, zum Beispiel SAP SuccessFactors, Oracle HCM und Workday. Agenten können mit einem Klick aus einer Vorlage erstellt und dann nach Bedarf modifiziert und erweitert werden. Dieser optionale Teil des Labs gibt Ihnen Zugang zum SAP Employee Support Manager Agent, den Sie durch das Testen mit einigen Beispielabfragen in Aktion sehen können.

In einem realen Unternehmensszenario wird empfohlen, mit vorgefertigten Domain-Agenten und Tools zu beginnen. Sie können einfach Ihren eigenen Agenten aus einer vorhandenen Vorlage erstellen und ihn dann nach Bedarf modifizieren und erweitern, zum Beispiel durch Hinzufügen zusätzlicher Tools, Agenten und Verbindungen.

## Architektur

<img width="1000" alt="image" src="arch_diagm.png">


## Voraussetzungen

**Teilnehmer**:
- Überprüfen Sie, dass Sie Zugriff auf die richtige TechZone-Umgebung für dieses Lab haben
- Überprüfen Sie, dass Sie Zugriff auf den gemeinsamen wxO-Mandanten haben (vom Instruktor bereitgestellt) für den Domain-Agenten-Teil (optional)
- Schließen Sie die [Umgebungseinrichtung](../../../environment-setup) ab für Schritte zur API-Schlüsselerstellung und Projekteinrichtung.
- Überprüfen Sie, dass Sie Zugriff auf eine Anmeldedatei haben, die Ihr Instruktor vor Beginn der Labs mit Ihnen teilen wird
- Vertrautheit mit AI-Agenten-Konzepten (z.B. Anweisungen, Tools, Mitarbeiter...)
- Stellen Sie sicher, dass Ihr Instruktor Folgendes bereitgestellt hat:
  - aktualisierte **hr.yaml OpenAPI-Spezifikation**

## Anleitung

### Agent Builder öffnen

- Melden Sie sich bei IBM Cloud (cloud.ibm.com) an. Navigieren Sie zum Hamburger-Menü oben links und dann zur Ressourcenliste. Öffnen Sie den Abschnitt AI/Machine Learning. Sie sollten einen **watsonx Orchestrate**-Service sehen, klicken Sie zum Öffnen.

  <img width="1000" alt="image" src="../../../environment-setup/assets/cloud-resource-list.png">

- Klicken Sie auf die Schaltfläche "Launch watsonx Orchestrate".

   <img width="1000" alt="image" src="../../../environment-setup/assets/cloud-wxo.png">

- Willkommen bei watsonx Orchestrate. Öffnen Sie das Hamburger-Menü, klicken Sie auf den Pfeil nach unten neben **Build**. Klicken Sie dann auf **Agent Builder**:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_1_v2.png">

### HR-Agent erstellen
1. Klicken Sie auf **Create agent +**:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_2_v2.png">

2. Wählen Sie **Create from scratch**, geben Sie Ihrem Agenten einen Namen, z.B. `HR Agent`, und füllen Sie die **Description** wie unten gezeigt aus: 

   ```
   This Agent handles employee HR queries including profile lookups, time-off balance checks, title and address updates, time-off requests, and general questions about company benefits.
   ```  
   Klicken Sie auf **Create**:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_3_v2.png">

3. Wählen Sie **Default** im Abschnitt **Agent style**.

   <img width="1000" alt="image" src="hands-on-lab-assets/step_5_v3.png">
  
4. Scrollen Sie nach unten zum Abschnitt **Knowledge**.
   Klicken Sie auf **Add Source**.
   
   <img width="1000" alt="image" src="add_source.png">
  
5. Wählen Sie **New Knowledge**
   
   <img width="1000" alt="image" src="new_knowledge.png"> 
   
6. Wählen Sie **Upload files**.
   Klicken Sie auf **Next**.
   
   <img width="1000" alt="image" src="hands-on-lab-assets/step_7_v3.png">
     
7. Laden Sie die [Employee Benefits.pdf](/usecases/ask-hr/assets/Employee-Benefits.pdf) auf Ihr System herunter und laden Sie die Datei dann hier hoch. Sie können die PDF herunterladen, indem Sie auf [Employee Benefits.pdf](/usecases/ask-hr/assets/Employee-Benefits.pdf) klicken und dann auf das Download-Symbol auf der geöffneten Seite klicken, wie im Bild unten gezeigt.
      <img width="1000" alt="image" src="hands-on-lab-assets/step_7.1_v3.png">

      
   Sobald Sie die Datei hochgeladen haben, klicken Sie auf **Next**.

   <img width="1000" alt="image" src="hands-on-lab-assets/step_8_v3.png">

8. Kopieren Sie den folgenden Namen und die Beschreibung in die Abschnitte **Name** und **Description** und klicken Sie dann auf **Save**:

   ```
   Employee Benefits
   ```

   ```
   This knowledge base addresses the company's employee benefits, including parental leaves, pet policy, flexible work arrangements, and student loan repayment.
   ```
   
   <img width="1000" alt="image" src="knowledge_details.png">

9. Scrollen Sie nach unten zum Abschnitt **Toolset**. Klicken Sie auf **Add tool +**:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_9_v3.png">

10. Wählen Sie **OpenAPI**:

   <img width="1000" alt="image" src="add_tool.png">


11. Ziehen Sie die **hr.yaml**-Datei (vom Instruktor bereitgestellt) per Drag & Drop oder klicken Sie zum Hochladen, dann klicken Sie auf **Next**:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_12_v3.png">    

12. Wählen Sie alle Operationen aus und klicken Sie auf **Done**:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_13_v3.png">

13. Scrollen Sie nach unten zum Abschnitt **Behavior**. Fügen Sie die folgenden Anweisungen in das Feld **Instructions** ein:

   ```
   Use your knowledge base to answer general questions about employee benefits. 

   Use the tools to get or update user specific information.

   When user asks to show profile data or check time off balance or update title/address or request time off for the very first time,  first ask the user for their name,  then invoke the tool and then use the same name in the whole session without asking for the name again.

   When the user requests time off, convert the dates to YYYY-MM-DD format, e.g. 5/22/2025 should be converted to 2025-05-22 before passing the date to the post_request_time_off tool.
   ```
14. Belassen Sie alle anderen Einstellungen bei den Standardwerten und klicken Sie auf **Deploy** in der oberen rechten Ecke, um Ihren Agenten bereitzustellen:

   <img width="1000" alt="image" src="hands-on-lab-assets/step_14_v4.jpg">

### HR-Agent in der Vorschau testen

Testen Sie den Agenten im AI Chat-Fenster. Klicken Sie auf das Hamburger-Menü in der oberen linken Ecke und dann auf **Chat**:

<img width="1000" alt="image" src="hands-on-lab-assets/step_16_v2.png">

Stellen Sie sicher, dass **HR Agent** ausgewählt ist. Sie können jetzt Ihren Agenten testen:

<img width="1000" alt="image" src="hands-on-lab-assets/hr_step16.png">

Wählen Sie für den nächsten Teil zunächst einen Mitarbeiternamen aus der von Ihrem Instruktor bereitgestellten Liste aus und verwenden Sie ihn für Ihre gesamte Sitzung.

Testen Sie Ihren Agenten im Vorschau-Chat auf der rechten Seite, indem Sie die folgenden Fragen stellen und die Antworten validieren. Sie sollten ähnlich aussehen wie in den Screenshots unten gezeigt:

```
What is the pet policy? 
```
<img width="1000" alt="image" src="hands-on-lab-assets/hr_step13.png">

Probieren Sie als Nächstes die folgenden Prompts aus und beziehen Sie sich auf das Bild unten für weitere Interaktionen mit dem Agenten. 
Erinnerung: Stellen Sie sicher, dass Sie einen vorhandenen Mitarbeiternamen aus der von Ihrem Instruktor bereitgestellten Liste auswählen und denselben Mitarbeiter für die gesamte Sitzung verwenden.


```
Show me my profile data.
```
<img width="1000" alt="image" src="hands-on-lab-assets/show_profile.png">

```
I'd like to update my title. 
```

<img width="1000" alt="image" src="hands-on-lab-assets/update_title.png">

```
Update my address
```
<img width="1000" alt="image" src="hands-on-lab-assets/update_address.png">

```
What is my time off balance?
```
<img width="1000" alt="image" src="hands-on-lab-assets/show_vacation_balance.png">

```
Request time off
```
<img width="1000" alt="image" src="hands-on-lab-assets/request_vacation.png">

```
Show my profile data.
```
<img width="1000" alt="image" src="hands-on-lab-assets/show_profile_after.png">