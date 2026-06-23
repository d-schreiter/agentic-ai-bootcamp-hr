
# 🧑‍💼 Talentakquise mit agentischen Workflows automatisieren

## Inhaltsverzeichnis

- [Anwendungsfall-Beschreibung](#anwendungsfall-beschreibung)
- [Voraussetzungen](#voraussetzungen)
- [Talentakquise-Agent mit agentischen Workflows](#-talentakquise-agent-mit-agentischen-workflows)
     - [Einen neuen Agenten erstellen](#einen-neuen-agenten-erstellen)
     - [Schritt 1: Einen agentischen Workflow erstellen und Ein- und Ausgaben konfigurieren](#schritt-1-einen-agentischen-workflow-erstellen-und-ein--und-ausgaben-konfigurieren)
     - [Schritt 2: Benutzeraktivität zum Erfassen der Anzahl der Kandidaten](#schritt-2-benutzeraktivität-zum-erfassen-der-anzahl-der-kandidaten)
     - [Schritt 3: Codeblock zum Speichern der Anzahl der Kandidaten](#schritt-3-codeblock-zum-speichern-der-anzahl-der-kandidaten)
     - [Schritt 4: For-Each-Schleife zum Hochladen von Kandidaten-Lebensläufen](#schritt-4-for-each-schleife-zum-hochladen-von-kandidaten-lebensläufen)
     - [Schritt 5: Nachricht zum Hochladen eines Lebenslaufs anzeigen](#schritt-5-nachricht-zum-hochladen-eines-lebenslaufs-anzeigen)
     - [Schritt 6: Lebenslauf-Datei-Upload](#schritt-6-datei-upload)
     - [Schritt 7: Dokumentenextraktor für Lebensläufe](#schritt-7-dokumentenextraktor-für-lebensläufe)
     - [Schritt 8: Name und Fähigkeiten des Kandidaten speichern](#schritt-8-name-und-fähigkeiten-des-kandidaten-für-später-speichern)
     - [Schritt 9: Nachricht zum Hochladen einer Stellenbeschreibung anzeigen](#schritt-9-nachricht-zum-hochladen-einer-stellenbeschreibung-anzeigen)
     - [Schritt 10: Stellenbeschreibung hochladen](#schritt-10-stellenbeschreibung-hochladen)
     - [Schritt 11: Dokumentenextraktor für Job-Fähigkeiten](#schritt-11-dokumentenextraktor-für-job-fähigkeiten)
     - [Schritt 12: Generativer Prompt - Kandidaten mit Job abgleichen](#schritt-12-generativer-prompt---kandidatenfähigkeiten-mit-job-fähigkeiten-abgleichen)
     - [Schritt 13: Abgleichszusammenfassung anzeigen](#schritt-13-abgleichszusammenfassung-anzeigen---ausgabe-des-generativen-prompts)
     - [Agentenverhalten aktualisieren](#agentenverhalten-aktualisieren)
     - [Agent testen](#agent-testen)
- [Alles zusammenfügen](#alles-zusammenfügen)


## Anwendungsfall-Beschreibung

Im [ersten Teil des HR Talent Labs](./hands-on-lab-hr-manager-de.md) haben Sie die Funktion **Chat with documents** verwendet, um mehrere Lebensläufe und eine Stellenbeschreibung hochzuladen. Sie haben dann den Agenten aufgefordert, eine Tabelle zu erstellen, die die Fähigkeiten der Kandidaten mit den erforderlichen Job-Fähigkeiten vergleicht. In diesem Fall erledigt das interne LLM des Agenten die gesamte Arbeit, alles, was vom Benutzer erforderlich ist, ist die Bereitstellung des richtigen Prompts/der richtigen Abfrage. Manchmal ist es jedoch möglicherweise nicht offensichtlich, was der richtige Prompt ist, da HR-Manager keine Prompt-Engineers sind. Zusätzlich möchten wir möglicherweise den Agenten so programmieren, dass er einige zusätzliche Schritte ausführt, z.B. automatisch den ausgewählten Kandidaten kontaktieren, ihn bitten, eine Interviewzeit auszuwählen, die Antwort automatisch zu verarbeiten und sie dem Kalender hinzuzufügen. In diesem Fall möchten wir möglicherweise einen **agentischen Workflow** erstellen.

Ein agentischer Workflow stellt eine Abfolge von Schritten dar, die bedingte Steuerungen und Aktivitäten nutzt. Agentische Workflows ermöglichen es uns, Aufgabensequenzen sowie Bedingungen, Verzweigungen und Schleifen zu erstellen. Wir können eine Vielzahl von Knoten verwenden, einschließlich kleiner Codeblöcke, Benutzereingaben, Dokumentenverarbeitungsknoten zum Extrahieren von Daten aus Dokumenten und generativen Prompts zum Erstellen und Konfigurieren von LLM-Prompts mit Ein- und Ausgaben.

Anstatt jeden Schritt einzeln zu bearbeiten, können Agenten einen agentischen Workflow starten, um den gesamten Prozess von Anfang bis Ende zu verwalten. Agentische Workflows sind ideal für Aufgaben, die eine Koordination über Systeme oder mehrere Entscheidungspunkte hinweg erfordern.

Beispielsweise kann ein agentischer Workflow erstellt werden, um das Onboarding von Mitarbeitern zu handhaben: Informationen sammeln, Konten erstellen, Willkommens-E-Mails senden und interne Teams benachrichtigen. Einmal erstellt, kann dieser agentische Workflow abteilungsübergreifend wiederverwendet werden, ausgelöst von Agenten, wann immer ein neuer Mitarbeiter eintritt - ohne dass jeder Schritt manuell koordiniert werden muss.

Durch die Verwendung agentischer Workflows gewinnen Geschäftsanwender:

- Vertrauen, dass Aufgaben korrekt und konsistent abgeschlossen werden.
- Geschwindigkeit durch Automatisierung sich wiederholender Schritte.
- Sichtbarkeit, wie Prozesse ablaufen und wo Engpässe auftreten.
- Skalierbarkeit, um dieselbe Logik über Teams, Regionen oder Produkte hinweg anzuwenden.

## Voraussetzungen

Falls Sie dies noch nicht als Teil der früheren Schritte des HR-Manager-Labs getan haben, laden Sie die folgenden Dateien herunter: 

[Candidate 1.pdf](../data/Candidate%201.pdf)

[Candidate 2.pdf](../data/Candidate%202.pdf)

[Candidate 3.pdf](../data/Candidate%203.pdf)

[Job Description.pdf](../data/Job%20Description.pdf)

## 🥇 Talentakquise-Agent mit agentischen Workflows 

In diesem Teil des Labs werden wir den folgenden Workflow implementieren: 

![alt text](./hands-on-lab-assets/flow_to_build.jpeg)

Wir werden Sie nun Schritt für Schritt durch die Erstellung des obigen Workflows führen. Wir werden zunächst einen separaten Agenten zum Experimentieren erstellen.

### Einen neuen Agenten erstellen

Öffnen Sie den Agent Builder in watsonx Orchestrate, falls Sie noch nicht dort sind - klicken Sie im Hauptmenü auf **Build->Agent Builder**. 

![alt text](./hands-on-lab-assets/open_agent_builder.png)

Erstellen Sie einen neuen Agenten:

![alt text](./hands-on-lab-assets/create_new_agent.png)

Wählen Sie **Create from scratch**, nennen Sie ihn **Talent Agent** und geben Sie ihm eine kurze Beschreibung. Beschreibungen werden verwendet, um eine Benutzeranfrage an den richtigen Agenten weiterzuleiten. Sie können die folgende Beschreibung verwenden:
```
This agent helps match candidates to a job based on their skills
```
![alt text](./hands-on-lab-assets/agent_description.png)

Nach dem Klicken auf **Create** werden Sie zu diesem Bildschirm weitergeleitet:

![alt text](./hands-on-lab-assets/talent_agent_intro.png)

Wir belassen vorerst alle Einstellungen bei den Standardwerten. Scrollen Sie nach unten zum Abschnitt **Toolset**. Hier werden wir unseren Flow (agentischen Workflow) hinzufügen. Klicken Sie auf **Add Tool**:

![alt text](./hands-on-lab-assets/add_tool.png)

Wählen Sie **Create an agentic workflow**:

![alt text](./hands-on-lab-assets/create_workflow.png)

### Schritt 1: Einen agentischen Workflow erstellen und Ein- und Ausgaben konfigurieren

Zunächst bearbeiten wir die Flow-Beschreibung, Ein- und Ausgaben. Klicken Sie auf den Stift neben dem Namen des Flows in der oberen linken Ecke: 

![alt text](./hands-on-lab-assets/edit_flow_description.png)

Ändern Sie den Namen in **Match candidates**, ändern Sie die Beschreibung in: 

```
Extracts skills from candidates' resumes, extracts skills from a job description, and generates a summary table showing which candidates have which skills required and preferred for the job.
```
und klicken Sie auf die Schaltfläche **Add output**, um die Ausgabe des Flows anzugeben: 

![alt text](./hands-on-lab-assets/flow_description.png)

Hier konfigurieren wir die Variable, die die Ausgabe des gesamten Flows speichert und nach Abschluss des Flows an den Agenten zurückgegeben wird. Wählen Sie **String** für den Variablentyp: 

![alt text](./hands-on-lab-assets/select_string_output.png)

Geben Sie ihr einen Namen, z.B. **match_summary**, und klicken Sie auf **Add**: 

![alt text](./hands-on-lab-assets/match_summary_var.png)

Nach dem Klicken auf **Save** sollte Ihr Flow ähnlich aussehen wie: 

![alt text](./hands-on-lab-assets/flow_start.png)

Der Flow hat vorerst nur zwei Knoten - den Startknoten mit 0 Eingaben und 0 konfigurierten Variablen und den Endknoten mit 1 konfigurierten Variable. Sie können überprüfen, ob Ihre Ausgabevariable erfolgreich hinzugefügt wurde, indem Sie auf den Endknoten klicken: 

![alt text](./hands-on-lab-assets/output_node.png)


Als Nächstes konfigurieren wir ein paar Flow-Variablen, die wir in unserem gesamten Flow verwenden können. Wir benötigen zwei: 

- *num_candidates* - eine Liste, die einen Bereich von Ganzzahlen *0* bis *n* darstellt, wobei *n* die Anzahl der hochzuladenden und zu verarbeitenden Kandidaten ist. Um mehrere Kandidaten-Lebensläufe hochzuladen und zu verarbeiten, verwenden wir einen **For each**-Knoten. Dazu können wir über *num_candidates* iterieren
- *candidates* - dies ist eine String-Variable, die extrahierte Kandidatennamen und entsprechende Fähigkeiten enthält. Wir benötigen sie, damit wir sie in einem generativen Prompt-Knoten verwenden können

Klicken Sie auf den Startknoten und wählen Sie **Edit** flow variables: 

![alt text](./hands-on-lab-assets/edit_flow_variables.png)

Flow-Variable hinzufügen: 

![alt text](./hands-on-lab-assets/add_flow_var.png)

und wählen Sie **Integer**: 

![alt text](./hands-on-lab-assets/integer_var.png)

Geben Sie den Namen der Variable ein, **num_candidates**, und eine Beschreibung, z.B.:

```
list of candidates, enum
```
Aktivieren Sie die Option **List of Integer**, da wir eine Liste haben werden, und klicken Sie auf **Add**, um die Variable hinzuzufügen.

> **Wichtig:** Stellen Sie sicher, dass Sie diese Variable genau **num_candidates** (als Liste von Ganzzahlen) benennen, da sie später in der For-Each-Schleife referenziert wird.

Fügen Sie eine weitere Variable hinzu:

![alt text](./hands-on-lab-assets/add_another_var.png)

Machen Sie sie diesmal zu einem String. Geben Sie ihr den Namen **candidates** und eine einfache Beschreibung, z.B.:

```
candidate names and skills
```

Geben Sie den Standardwert (Startwert) an: "" und klicken Sie auf **Add**:

![alt text](./hands-on-lab-assets/candidates_var.png)

> **Wichtig:** Stellen Sie sicher, dass Sie diese Variable genau **candidates** (als String) benennen, da sie verwendet wird, um Kandidateninformationen im gesamten Flow zu speichern.

Ihr Flow sollte jetzt so aussehen:

![alt text](./hands-on-lab-assets/start_vars_defined.png)

### Schritt 2: Benutzeraktivität zum Erfassen der Anzahl der Kandidaten

Wir fügen nun unsere erste Benutzeraktivität hinzu. Die erste Aktivität, die wir erstellen werden, besteht darin, den Benutzer zu fragen, wie viele Kandidaten er für die Stelle bewerten möchte. Bewegen Sie den Mauszeiger über den Pfeil, der den Startknoten mit dem Endknoten verbindet, und klicken Sie auf das **+**-Zeichen: 

![alt text](./hands-on-lab-assets/add_user_activity1.png)

Klicken Sie auf **User activity**:

![alt text](./hands-on-lab-assets/select_user_activity.png)

Bewegen Sie den Mauszeiger über den Pfeil von Start zu End **innerhalb von User activity 1**. Klicken Sie auf **+**, dann auf **Collect from user**, dann auf **Number**: 

![alt text](./hands-on-lab-assets/collect_number.png)

Klicken Sie auf **Number 1**, dann auf das Stift-Symbol, um die Frage zu bearbeiten, die dem Benutzer angezeigt werden soll: 

```
How many candidates would you like to evaluate?
```

![alt text](./hands-on-lab-assets/collect_number2.png)

Ihr Flow sollte jetzt so aussehen: 

![alt text](./hands-on-lab-assets/number_collected.png)

### Schritt 3: Codeblock zum Speichern der Anzahl der Kandidaten

Wir definieren nun einen Knoten, um die Variable *num_candidates* zu aktualisieren: 

Bewegen Sie den Mauszeiger über den Pfeil, der die Benutzeraktivität mit dem Endknoten verbindet. Klicken Sie auf das **+**-Zeichen und dann auf **Code block**: 

![alt text](./hands-on-lab-assets/add_code_block.png)

Klicken Sie auf den neuen Codeblock-Knoten und öffnen Sie den Code-Editor: 

![alt text](./hands-on-lab-assets/open_code_editor.png)

Geben Sie den folgenden Code in den Editor ein: 

```
numc = flow["User activity 1"]["How many candidates would you like to evaluate?"].output.value
flow.private.num_candidates = list(range(0, numc))
```

Und klicken Sie auf das **X**, um den Editor zu schließen: 

![alt text](./hands-on-lab-assets/enter_code.png)

Klicken Sie erneut auf den Codeblock und benennen Sie ihn mit der Schaltfläche Edit (Stift) um:

![alt text](./hands-on-lab-assets/edit_code_block_name.png)

Nennen Sie ihn **store candidate list** und klicken Sie auf **V** zum Speichern. Ihr Flow sollte jetzt wie folgt aussehen: 

![alt text](./hands-on-lab-assets/flow_with_code_block.png)

### Schritt 4: For-Each-Schleife zum Hochladen von Kandidaten-Lebensläufen

Als Nächstes erstellen wir eine **For-Each-Schleife**, um jeden Lebenslauf hochzuladen, den Namen des Kandidaten und seine Fähigkeiten zu extrahieren und all diese Informationen in der Variable *candidates* zu speichern.

Bewegen Sie den Mauszeiger über den Pfeil, der den Codeblock mit dem Endknoten verbindet, und klicken Sie auf das **+**-Zeichen, dann wählen Sie **For each**:

![alt text](./hands-on-lab-assets/create_for_each.png)

> **Wichtig:** Nachdem Sie den Knoten **For each 1** erstellt haben, klicken Sie darauf und konfigurieren Sie die Iterationsliste. Anstatt Automap zu verwenden, wählen Sie **Flow variables -> num_candidates** als die Liste, über die iteriert werden soll. Dies stellt sicher, dass die Schleife für die richtige Anzahl von Kandidaten ausgeführt wird.

### Schritt 5: Nachricht zum Hochladen eines Lebenslaufs anzeigen

Erstellen Sie innerhalb des **For each**-Knotens eine **User activity**, indem Sie den Mauszeiger über den Pfeil innerhalb des **For each**-Knotens bewegen und auf das **+**-Zeichen klicken. Bewegen Sie dann den Mauszeiger über das Innere der **User activity**, klicken Sie auf **+**, wählen Sie **Display to user**, dann **Message**:

![alt text](./hands-on-lab-assets/add_display_message.png)

Klicken Sie als Nächstes auf **Message** und bearbeiten Sie die **Output message**:

```
Please upload a candidate's resume
```

Dies wird dem Benutzer angezeigt, um ihn aufzufordern, einen Lebenslauf hochzuladen.

Ändern Sie gleichzeitig den Knotennamen in: 

```
Prompt user to upload resume
```

![alt text](./hands-on-lab-assets/upload_resume_message.png)


### Schritt 6: Datei-Upload

Fügen Sie eine weitere **User Activity** hinzu:

![alt text](./hands-on-lab-assets/add_another_user_activity.png)

Diesmal wird es eine **File Upload**-Aktivität sein: 

![alt text](./hands-on-lab-assets/create_file_upload.png)

Klicken Sie auf den neuen **File upload**-Knoten und benennen Sie ihn in **Upload resume** um: 

![alt text](./hands-on-lab-assets/rename_file_upload.png)

### Schritt 7: Dokumentenextraktor für Lebensläufe

Als Nächstes erstellen wir einen Dokumentenextraktionsknoten, um den Namen des Kandidaten und seine Fähigkeiten aus seinem Lebenslauf zu extrahieren.

Bewegen Sie sich immer noch innerhalb der **For all**-Schleife, bewegen Sie den Mauszeiger über den letzten Pfeil und klicken Sie auf **+**, um einen neuen **Document extractor**-Knoten zu erstellen:

![alt text](./hands-on-lab-assets/create_doc_extractor.png)

Klicken Sie auf den **Document extractor**-Knoten und dann auf **Edit fields**:

![alt text](./hands-on-lab-assets/edit_doc_extractor_fields.png)

> **Wichtig:** Bevor Sie die Felder konfigurieren, müssen Sie die hochgeladene Datei an diesen Dokumentenextraktor binden. Klicken Sie auf **Edit data mapping** für den Document extractor-Knoten. Anstatt Automap zu verwenden, wählen Sie das **Variablen-Symbol** und wählen Sie **Upload resume -> value**, um die hochgeladene Datei direkt der Eingabe des Dokumentenextraktors zuzuordnen.

Wir laden nun einen der Lebensläufe als Beispiel hoch, um den Dokumentenextraktor zu trainieren. Ziehen Sie die Datei [Candidate2.pdf](../data/Candidate%202.pdf), die Sie zuvor im Lab heruntergeladen haben, per Drag & Drop:

Sobald das Dokument hochgeladen ist, sehen Sie den folgenden Bildschirm. Klicken Sie auf **Add field**, um mit dem Hinzufügen von Feldern zu beginnen, die wir extrahieren und den Dokumentenextraktor trainieren möchten:

![alt text](./hands-on-lab-assets/doc_extractor_show.png)

Geben Sie **Name** für den Namen des Feldes ein und drücken Sie Enter. Der Dokumentenextraktor versucht, den Namen aus dem Lebenslauf zu extrahieren und zeigt ihn an, sobald er fertig ist:

![alt text](./hands-on-lab-assets/candidate_name.png)

Als Nächstes müssen wir ein weiteres Feld **Skills** hinzufügen. Fügen Sie ein weiteres Feld hinzu und nennen Sie es **Skills**. Sobald Sie Enter drücken, füllt der Dokumentenextraktor das Feld aus dem Dokument:

![alt text](./hands-on-lab-assets/candidate_skills.png)

> **Wichtig:** Stellen Sie sicher, dass das Feld genau **Skills** (mit großem S) benannt ist, da es später im Codeblock als `output.skills` referenziert wird.

Benennen Sie den Dokumentenextraktor-Knoten in **Resume extractor** um, indem Sie darauf klicken und seinen Namen bearbeiten.

> **Kritisch:** Der Dokumentenextraktor-Knoten MUSS genau **Resume extractor** benannt werden, da dieser Name in den Codeblöcken referenziert wird. Jeder andere Name führt zu Fehlern.

Ihre **For each**-Schleife sollte jetzt so aussehen:

![alt text](./hands-on-lab-assets/for_each_after_extractor.png)

### Schritt 8: Name und Fähigkeiten des Kandidaten für später speichern

Die letzte Aktivität, die wir in der **For each**-Schleife erstellen müssen, ist ein weiterer Codeblock, der den Namen und die Fähigkeiten des Kandidaten nach jeder Iteration speichert:

![alt text](./hands-on-lab-assets/store_candidate_info.png)

Klicken Sie auf den Codeblock und öffnen Sie den Code-Editor. Geben Sie Folgendes in den Code-Editor ein:

```
flow.private.candidates += "Name: " + str(flow["For each 1"]["Resume extractor"].output.name) + "\n\nSkills: " + str(flow["For each 1"]["Resume extractor"].output.skills) + "\n\n"
```

> **Wichtig:** Dieser Code referenziert die in Schritt 7 erstellten Dokumentenextraktor-Variablen. Stellen Sie sicher:
> - Der Dokumentenextraktor ist genau **Resume extractor** benannt
> - Die Felder sind genau **Name** und **Skills** benannt (entsprechend den Referenzen `.output.name` und `.output.skills`)
> - Wenn Sie das Feld anders benannt haben (z.B. "Skill" statt "Skills"), aktualisieren Sie den Code entsprechend: `output.skill` statt `output.skills`

Benennen Sie den Codeblock in **Update candidates** um. Die **For each**-Schleife sollte jetzt so aussehen:

![alt text](./hands-on-lab-assets/for_each_final.png)

### Schritt 9: Nachricht zum Hochladen einer Stellenbeschreibung anzeigen

Als Nächstes bitten wir den Benutzer, eine Stellenbeschreibung hochzuladen. Zunächst zeigen wir dem Benutzer eine Nachricht an, dann fügen wir eine Datei-Upload-Aktivität hinzu.

**Unterhalb** der **For each**-Schleife klicken Sie auf den Pfeil, der zum Endknoten führt, und fügen Sie eine **User activity** hinzu: 

![alt text](./hands-on-lab-assets/add_user_activity_job.png)

Klicken Sie in die Benutzeraktivität und wählen Sie **Display to user**, dann **Message**: 

![alt text](./hands-on-lab-assets/display_to_user_job_upload.png)

Aktualisieren Sie die **Output message** auf: 

```
Please upload the job description
```

Und ändern Sie die **Display message** (Name des Knotens im Flow) in: 

```
Prompt user to upload job description
```

![alt text](./hands-on-lab-assets/configure_display_to_user_job_upload.png)

### Schritt 10: Stellenbeschreibung hochladen

Fügen Sie eine weitere **User activity** direkt vor dem Endknoten hinzu. Machen Sie sie diesmal vom Typ **File upload**: 

![alt text](./hands-on-lab-assets/file_upload_job.png)

Klicken Sie auf den neu erstellten Datei-Upload-Knoten und ändern Sie seinen Namen in: 

```
Upload job description
```

![alt text](./hands-on-lab-assets/change_file_upload_job_node_name.png)

### Schritt 11: Dokumentenextraktor für Job-Fähigkeiten

Als Nächstes erstellen wir einen weiteren Dokumentenextraktor-Knoten, um erforderliche und bevorzugte Fähigkeiten aus der Stellenbeschreibung zu extrahieren.

Fügen Sie einen **Document extractor**-Knoten vor dem Ende des Flows hinzu und benennen Sie ihn in **Extract job skills** um:

![alt text](./hands-on-lab-assets/extract_job_skills.png)

> **Kritisch:** Der Dokumentenextraktor-Knoten MUSS genau **Extract job skills** benannt werden, da dieser Name im generativen Prompt-Datenmapping referenziert wird. Jeder andere Name führt zu Fehlern.

> **Wichtig:** Bevor Sie die Felder konfigurieren, klicken Sie auf **Edit data mapping** für diesen Document extractor-Knoten. Anstatt Automap zu verwenden, wählen Sie das **Variablen-Symbol** und wählen Sie **Upload job description -> value**, um die hochgeladene Datei direkt der Eingabe des Dokumentenextraktors zuzuordnen.

Bearbeiten Sie die Felder dieses Dokumentenextraktor-Knotens und ziehen Sie [die Stellenbeschreibungsdatei](../data/Job%20Description.pdf), die Sie zuvor heruntergeladen haben, per Drag & Drop:

![alt text](./hands-on-lab-assets/job_file_drag_drop.png)

Sobald das Dokument verarbeitet wurde, sehen Sie den folgenden Bildschirm: 

![alt text](./hands-on-lab-assets/job_desc_doc_proc.png)

Fügen Sie zwei Felder hinzu, ähnlich wie Sie es für den Lebenslauf-Extraktor getan haben. Fügen Sie diesmal jedoch die Felder **required** und **preferred** hinzu, um erforderliche und bevorzugte Fähigkeiten zu extrahieren: 

Schließen Sie den Extraktor-Knoten, sobald Sie fertig sind.

### Schritt 12: Generativer Prompt - Kandidatenfähigkeiten mit Job-Fähigkeiten abgleichen

Wir sind endlich fast am Ende des Flows. Wir müssen noch einen generativen Prompt implementieren. Dieser Prompt nimmt als Eingabe den Wert der Variable *candidates*, die eine Zeichenfolge ist, die inzwischen alle Kandidatennamen und ihre Fähigkeiten enthält. Er nimmt auch als Eingabe die erforderlichen und bevorzugten Fähigkeiten, die gerade aus der Stellenbeschreibung extrahiert wurden. Der Prompt vergleicht die Fähigkeiten jedes Kandidaten mit den vom Job geforderten Fähigkeiten und generiert eine Tabelle, die zusammenfasst, wie gut die Kandidatenfähigkeiten zur Stellenbeschreibung passen.

Fügen Sie einen **Generate prompt**-Knoten am Ende des Flows (vor dem Endknoten) hinzu: 

![alt text](./hands-on-lab-assets/generative_prompt.png)

Benennen Sie den Knoten in **Match candidate skills to job skills** um und bearbeiten Sie **Prompt settings**: 

![alt text](./hands-on-lab-assets/rename_prompt_node.png)

Geben Sie für den System-Prompt Folgendes ein: 

```
You are a helpful assistant who can match candidates skills to job requirements.
```

Geben Sie für den Benutzer-Prompt ein: 

```
Make a table where each row is a candidate and each column is a skill in the job description. Do not invent any candidates. Have the check emoji if the candidate does have the corresponding skill. Mark columns for required job skills with *. Include the candidate's name in each row.

Candidate names and skills: 
Required job skills: 
Preferred job skills:
```

Damit unser generativer Prompt funktioniert, muss er **als Eingabe** eine Zeichenfolge nehmen, die Kandidatennamen und Fähigkeiten enthält, die zuvor im Flow extrahiert wurden. Er benötigt auch zwei Zeichenfolgen für Fähigkeiten (erforderlich und bevorzugt) aus der Stellenbeschreibung selbst. Daher müssen wir drei _String_-Eingabevariablen erstellen, die diese Werte enthalten und auf die wir im Benutzer-Prompt als Variablen verweisen können.

Wir können auch einen Beispiel-Testwert für jede Variable bereitstellen, damit wir den Prompt direkt im generativen Prompt-Editor ausführen und überprüfen können, dass die Ausgabe wie erwartet ist, ohne beenden und den gesamten Flow ausführen zu müssen.

Fügen Sie die folgenden _String_-Eingabevariablen hinzu: *candidates*, *job_required* und *job_preferred* und weisen Sie einige Testwerte zu, z.B.: 

![alt text](./hands-on-lab-assets/create_new_var_gp.png)

Füllen Sie den Namen der Variable aus und fügen Sie eine einfache Beschreibung hinzu: 

![alt text](./hands-on-lab-assets/create_candidates_var_gp.png)

Bearbeiten Sie die Variable, um einen Testwert hinzuzufügen: 

![alt text](./hands-on-lab-assets/add_test_value.png)

Fügen Sie den folgenden Text ein, um den Wert hinzuzufügen: 

```
Name: Jane Smith
Skills: Java, Javascript

Name: John Doe
Skills: Java, Python, Javascript, ML
```

Ihre Variable _candidates_ sollte jetzt so aussehen:

![alt text](./hands-on-lab-assets/prompt_candidates_var.png)

Befolgen Sie die obigen Schritte, um zwei weitere _String_-Variablen hinzuzufügen, _job_required_ und _job_preferred_: 

![alt text](./hands-on-lab-assets/job_reqs.png)

Referenzieren Sie schließlich diese Variablen in Ihrem Prompt, indem Sie auf das **X**-Zeichen im Benutzer-Prompt-Bereich klicken: 

![alt text](./hands-on-lab-assets/select_vars.png)

Ihr Prompt sollte jetzt so aussehen: 

![alt text](./hands-on-lab-assets/generative_prompt_finished.png)

Klicken Sie auf **Generate response**, um den Prompt mit den von Ihnen bereitgestellten Testwerten auszuführen und die zurückgegebenen Ergebnisse zu beobachten: 

![alt text](./hands-on-lab-assets/gen_prompt_test.png)

Wie Sie sehen können, ist das Ergebnis eine Tabelle, die die Fähigkeiten jedes Kandidaten mit den Jobanforderungen vergleicht. Das ist genau das, wonach wir gesucht haben, also haben wir validiert, dass unser generativer Prompt funktioniert und können zum nächsten Schritt übergehen.

Schließen Sie jetzt die Prompt-Definition, klicken Sie auf den gerade erstellten Generative prompt-Knoten und **Edit data mapping**: 

![alt text](./hands-on-lab-assets/edit_data_mapping.png)

Wir müssen jetzt die zuvor im Flow gesammelten Daten den Eingaben des generativen Prompts zuordnen.

Klicken Sie auf das **Variablen**-Symbol in der Zeile *candidates*: 

![alt text](./hands-on-lab-assets/prompt_edit_candidates_input.png)

Wählen Sie im Editor **Flow variables -> candidates**: 

![alt text](./hands-on-lab-assets/select_candidates_fv.png)

Wählen Sie für *job_preferred* auch das **Variablen-Symbol** und wählen Sie **Extract job skills -> preferred**

![alt text](./hands-on-lab-assets/select_job_preferred.png)

Wählen Sie ähnlich für *job_required* das Variablen-Symbol und wählen Sie **Extract job skills -> required**

![alt text](./hands-on-lab-assets/select_job_required.png)

### Schritt 13: Abgleichszusammenfassung anzeigen - Ausgabe des generativen Prompts

Erstellen Sie schließlich einen letzten **User activity**-Knoten, um die Ausgabe des generativen Prompts anzuzeigen: 

![alt text](./hands-on-lab-assets/display_output.png)

Aktualisieren Sie den Knotennamen auf **Show summary** und klicken Sie auf **Select variable**, um die Ausgabenachricht auszuwählen: 

![alt text](./hands-on-lab-assets/edit_output_node.png)

Wählen Sie im Editor den Namen des generativen Prompt-Knotens und dann die entsprechende Ausgabevariable *value*:

![alt text](./hands-on-lab-assets/select_prompt_output_var.png)

Wir sind endlich mit der Definition des Flows fertig. Klicken Sie auf **Done**, um den Flow zu schließen.

### Agentenverhalten aktualisieren

Bevor wir den Agenten testen, vervollständigen wir den Abschnitt **Behavior**. Verwenden Sie die folgenden Anweisungen: 

```
When asked to match a candidate to job or to recommend the best candidate for a job, call the 'Match candidates' tool.  All other questions should be answered based on the context in the chat.
```

![alt text](./hands-on-lab-assets/behavior.png)

### Agent testen

Testen Sie Ihren Agenten, indem Sie zwei Kandidaten-Lebensläufe bereitstellen. Geben Sie die folgende Abfrage im Chat ein: 

```
recommend a candidate for a job
```

Der Agent fragt Sie, wie viele Kandidaten Sie bewerten möchten. Antworten Sie: 2

Sie werden dann aufgefordert, den Lebenslauf eines Kandidaten hochzuladen. Sie können den Lebenslauf eines beliebigen Kandidaten hochladen, zum Beispiel [Candidate 3.pdf](../data/Candidate%203.pdf). Beachten Sie, dass Sie möglicherweise aufgefordert werden, die Extraktionsergebnisse zu überprüfen - wenn die Konfidenz des Extraktors unter 95% liegt, ist eine menschliche Validierung erforderlich. Dieses Verhalten kann einfach innerhalb des Dokumentenextraktor-Knotens konfiguriert werden. Dasselbe gilt für alle anderen hochgeladenen Dokumente.

Sie werden dann aufgefordert, den zweiten Lebenslauf hochzuladen. Sie können einen weiteren Kandidaten-Lebenslauf hochladen, zum Beispiel [Candidate 1.pdf](../data/Candidate%201.pdf).

Sie werden schließlich aufgefordert, eine Stellenbeschreibung hochzuladen. Sie können [Job Description.pdf](../data/📄%20Job%20Description.pdf) verwenden.

Die Ergebnisse sollten ähnlich wie folgt aussehen: 

![alt text](./hands-on-lab-assets/table_output.png)

Wie Sie sehen können, sind die mit * markierten Spalten Fähigkeiten, die vom Job gefordert werden. Andere Fähigkeiten sind bevorzugt.
Jede Kandidatenzeile zeigt, welche Fähigkeiten der Kandidat hat.

Der Agent fasst zusammen, indem er uns mitteilt, wer der empfohlene Kandidat ist: 

![alt text](./hands-on-lab-assets/recommended_candidate.png)

## Alles zusammenfügen

In diesem Teil des Labs haben wir den Prozess der Extraktion von Fähigkeiten aus Lebensläufen und der Stellenbeschreibung automatisiert und zusammengefasst, wie gut die Fähigkeiten der Kandidaten mit den für den Job erforderlichen und bevorzugten Fähigkeiten übereinstimmen. Wir haben **Dokumentenverarbeitungs**-Knoten verwendet, um die aus Dokumenten zu extrahierenden Felder zu definieren und den Dokumentenprozessor zu trainieren. Wir haben dann die Ausgabe der Dokumentenverarbeitungsknoten als Eingabe in den **generativen Prompt**-Knoten eingespeist, der den richtigen Prompt für das LLM zusammengestellt hat, um zusammenzufassen, wie gut die Kandidatenfähigkeiten mit den Jobanforderungen übereinstimmen.
Wir könnten diesen Workflow leicht mit zusätzlichen Knoten und Verzweigungen erweitern, zum Beispiel um eine E-Mail an die am höchsten bewerteten Kandidaten zu senden, sie zu bitten, einen Interviewslot auszuwählen, und zu bestätigen, dass ihre Antwort empfangen wurde. Die Ausführung dieser Aufgaben als Workflow ermöglicht eine deterministischere Handhabung sich wiederholender Aufgaben, sodass der Agent den Prozess steuern und die HR-Managerin einbeziehen kann, wann immer ihre Eingabe benötigt wird.

Wie Sie beim Testen des Flows bemerkt haben, kann je nachdem, wie die Konfidenzschwellen in den Dokumentenverarbeitungsknoten eingerichtet sind, eine menschliche Überprüfung angefordert werden, um sicherzustellen, dass Felddaten korrekt extrahiert werden.

Die Kombination von agentischen Workflows mit regulären Tools und einzelnen Aufgaben in einem Agenten bietet die größte Flexibilität. Ein Benutzer kann mit dem Agenten chatten und einzelne Aufgaben nach Bedarf aufrufen. Für komplexere, mehrstufige Prozesse sind agentische Workflows ein leistungsstarkes Tool, das den gesamten Prozess von Anfang bis Ende verwalten kann.
