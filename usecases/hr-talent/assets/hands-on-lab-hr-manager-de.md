
# 🧑‍💼 Agentischer HR-Manager

## Inhaltsverzeichnis

- [Anwendungsfall-Beschreibung](#anwendungsfall-beschreibung)
- [Talentakquise-Agent](#-talentakquise-agent)
- [Talentakquise-Agent mit agentischen Workflows automatisieren](#-talentakquise-agent-mit-agentischen-workflows-automatisieren)
- [HR-Fallprüfungs-Agent](#-hr-fallprüfungs-agent)
    
## Anwendungsfall-Beschreibung

Dies ist die Geschichte von **Luisa**. **Luisa** ist HR-Managerin für ein großes Unternehmen, das 5.000 Mitarbeiter für seine neue Abteilung einstellt. Ihre Herausforderung ist zweifach:

1. **Kandidaten rekrutieren** für ihre offenen Stellen
2. **Berichte bearbeiten** von Mitarbeitern über potenzielle Verstöße gegen die Verhaltensrichtlinien.

Für die Rekrutierung erhält Luisa viele PDFs mit Kandidaten-Lebensläufen. Sie muss:

- Prüfen, ob Kandidaten die **Anforderungen** einer bestimmten Position **erfüllen**
- Eine **Tabelle** mit den Fähigkeiten/Erfahrungen jedes Kandidaten ausfüllen
- **Kandidaten** für Vorstellungsgespräche auswählen
- **Interviewer** aus dem Team zuweisen
- **Vorstellungsgespräche** mit Kandidaten und Interviewern per E-Mail koordinieren
- **Vorstellungsgespräche** planen
- **Feedback** von verschiedenen Prüfern zusammenstellen
- Die Ergebnisse an den Einstellungsmanager **zurückmelden**

Luisa möchte ihren Einstellungsprozess effizienter gestalten.

## 🥇 Talentakquise-Agent

Dieser erste Agent wird beim Rekrutierungsprozess helfen. Befolgen Sie diese Schritte, um Ihren Talentakquise-KI-Agenten zu erstellen:

1. Öffnen Sie watsonx Orchestrate. Sie sehen den unten stehenden Bildschirm. Klicken Sie dann unten links auf **Create an Agent** und wählen Sie **Create from scratch**.

<img width="1681" alt="welcome" src="hands-on-lab-assets/6a7b9866-09ae-4c89-8902-20a8930f0e7a.png">
<br>
<br>

2. Geben Sie ihm einen Namen und eine Beschreibung. Beschreibungen werden verwendet, um eine bestimmte Anfrage bei Bedarf an diesen Agenten weiterzuleiten. Sie können die folgende Beschreibung verwenden oder mit Ihrer eigenen experimentieren:
```
This agent helps figure out whether a set of candidates match the skills given in a job description
```

<img width="600" alt="create-agent" src="hands-on-lab-assets/8e821db1-99f1-43ba-a796-cc46ecaae0e1.png">
<br>
<br>

3. Nach dem Klicken auf **Create** werden Sie zu diesem Bildschirm weitergeleitet. Beachten Sie, dass das Modell standardmäßig auf **GPT-OSS 120B** eingestellt sein sollte. Falls nicht, verwenden Sie das Dropdown-Menü, um es auszuwählen.
<img width="1723" alt="profiile" src="hands-on-lab-assets/58af3961-7584-4d89-9ae3-b158ff90f159.png" />
<br>
<br>

> **Wichtig:** Sie sollten auch die Agentenbeschreibung zum Abschnitt **Behavior** hinzufügen. Scrollen Sie nach unten zum Abschnitt **Behavior** und fügen Sie dort dieselbe Beschreibung hinzu, um die Antworten und Routing-Logik des Agenten zu steuern.

4. Scrollen Sie nach unten und aktivieren Sie den **Chat with Documents**-Schalter:

<img width="713" alt="chat-with-documents" src="hands-on-lab-assets/ca258ba3-149b-462f-a28c-8e3574707fbf.png">
<br>
<br>

5. Lassen Sie uns nun den Agenten bereitstellen, indem wir auf die blaue Schaltfläche **Deploy** klicken. So einfach können Sie einen Agenten in watsonx Orchestrate bereitstellen.

<img width="600" alt="deploy" src="hands-on-lab-assets/3d079a57-5969-4889-bd04-90a06e28d960.png">
<br>
<br>


6. Lassen Sie uns nun simulieren, was die HR-Managerin tun würde, um Lebensläufe automatisch zu verarbeiten. Laden Sie zunächst die Lebensläufe und die Stellenbeschreibungsdateien unten herunter. Sobald Sie sie auf Ihrem lokalen Computer haben, laden Sie sie alle auf einmal hoch, indem Sie auf die Schaltfläche **Upload** unter dem Chat klicken. Sie können die Dateien auch alternativ per Drag & Drop in den Chat ziehen.


- [Lebenslauf von Kandidat 1](../data/Candidate%201.pdf)
- [Lebenslauf von Kandidat 2](../data/Candidate%202.pdf)
- [Lebenslauf von Kandidat 3](../data/Candidate%203.pdf)
- [Lebenslauf von Kandidat 4](../data/Candidate%204.pdf)
- [Lebenslauf von Kandidat 5](../data/Candidate%205.pdf)
- [Stellenbeschreibung](../data/Job%20Description.pdf)

<img width="600" alt="live" src="hands-on-lab-assets/e4e4480c-4629-430f-aef2-7ebb64c25b26.png">
<br>
<br>


7. Sie sehen eine Bestätigung der hochgeladenen Dateien wie folgt:

<img width="685" alt="upload" src="hands-on-lab-assets/4849445e-8936-44f4-9915-76850bd0841c.png">
<br>
<br>

8. Lassen Sie uns nun einige verschiedene Prompts ausprobieren, um die Lebensläufe zu verarbeiten und sie mit der Stellenbeschreibung abzugleichen. Lassen Sie uns zunächst die Fähigkeiten und Anforderungen in der Stellenbeschreibung zusammenfassen:

```
Above, I have uploaded 5 documents with candidate resumes and one document with job description. Can you give me a short one-paragraph summary of the job description?
```

9. Lassen Sie uns nun überprüfen, ob die Lebensläufe korrekt hochgeladen wurden, indem wir die Namen der Kandidaten abfragen:

```
give me the names of all the candidates
```

<img width="687" alt="Screenshot 2025-09-25 at 10 44 18 AM" src="hands-on-lab-assets/52594697-ccdc-4835-939e-6d380c7683aa.png">
<br>
<br>


10. Lassen Sie uns nun eine Tabelle erstellen, die die erforderlichen Fähigkeiten mit jedem Kandidaten abgleicht:
```
make a table where each row is a candidate and each column is a skill in the job description. Have the check emoji if the candidate does have the corresponding skill.
```

<img width="685" alt="Screenshot 2025-09-25 at 10 26 30 AM" src="hands-on-lab-assets/8b88bf74-7671-437d-8275-1a63901390e3.png">
<br>
<br>

Sie können sehen, dass Emma die Person ist, die die beste Übereinstimmung der Fähigkeiten hat. Die HR-Managerin muss jedoch noch Emmas Profil und Lebenslauf überprüfen, bevor sie fortfährt. Es ist wichtig, einen Menschen in der Schleife zu halten, insbesondere bei Entscheidungen, die Menschen betreffen. Das Ziel von Agentic AI ist es, die mühsamen Aufgaben zu automatisieren, anstatt die Arbeit der HR-Managerin zu ersetzen.

<!--11. Now let's ask for drafting an email to schedule an interview:
```
Draft an email asking Emma for three potential times for next week to interview.
```

<img width="685" alt="Screenshot 2025-09-25 at 10 26 53 AM" src="hands-on-lab-assets/47a3ef11-20ce-4e15-82a2-13ca81ef4362.png">

-->

11. Lassen Sie uns nun an der Planung der Vorstellungsgespräche arbeiten. Fügen Sie zunächst Interviewerdaten hinzu. Im wirklichen Leben stammen diese aus einer Datenbank oder einem Data Lakehouse, das mehrere Systeme in der Organisation abfragt. Der Einfachheit halber nehmen wir an, dass wir eine PDF-Datei mit der Verfügbarkeit von Interviewern und ihren Fähigkeiten haben. Wir können watsonx Orchestrate verwenden, um dem Agenten **Knowledge** über Interviewer hinzuzufügen. Scrollen Sie nach unten zum Abschnitt **Knowledge** und klicken Sie auf **Choose Knowledge**:

<img width="733" alt="Screenshot 2025-09-25 at 10 58 53 AM" src="hands-on-lab-assets/88c73733-5121-4f27-96d6-cb892c7cb84a.png">.
<br>
<br>


12. Wählen Sie unten **Upload Files**, klicken Sie auf **Next**:

<img width="1588" alt="Screenshot 2025-09-29 at 2 24 57 PM" src="hands-on-lab-assets/788f7870-aee1-4a9a-9799-afb0932e4c2c.png">
<br>
<br>

13. Ziehen Sie die Datei [Interviewer availability dataset](../data/Interviewer%20availability.docx) per Drag & Drop oder laden Sie sie hoch. Klicken Sie auf **Next**:

<img width="604" alt="Screenshot 2025-09-29 at 2 25 06 PM" src="hands-on-lab-assets/8c6bc433-b4ff-442b-8204-7164dd94bdaa.png">
<br>
<br>

Jetzt müssen Sie eine Beschreibung festlegen. Diese wird verwendet, um zu bestimmen, wann das Wissen in der Datei aufgerufen werden soll. Fügen Sie Folgendes unter **Description** hinzu und klicken Sie auf **Save**:

```
This document has the availability and skills of different interviewers
```

<img width="991" alt="Screenshot 2025-09-29 at 2 27 32 PM" src="hands-on-lab-assets/7c334577-58f8-43f1-ba8f-56c9f5b4f8bd.png">
<br>
<br>


14. Lassen Sie uns nun einige zusätzliche Abfragen für die Vorstellungsgespräche durchführen. Überprüfen Sie zunächst, ob die Interviewerdaten korrekt geladen wurden:

```
show me the availability of interviewers
```

<img width="667" alt="Screenshot 2025-09-29 at 11 51 36 AM" src="hands-on-lab-assets/86d72ba9-945b-4d5b-8c46-9ae724936c48.png">
<br>
<br>

15. Lassen Sie uns nun Luisa helfen, die am besten geeigneten Interviewer für die gegebene Stellenbeschreibung auszuwählen:

```
who's the most proficient interviewer for the job description? Show me the skills they have
```
<img width="682" alt="Screenshot 2026-02-18 at 2 57 13 PM" src="hands-on-lab-assets/e742ab81-07d2-492a-ae51-d31bf806bc72.png" />


16. Wählen Sie schließlich einen Interviewer aus und entwerfen Sie eine E-Mail an einen der Kandidaten mit der Verfügbarkeit des Interviewers:
 
```
draft an email to Emma to invite her for an interview with Aisha. Use Aisha's availability in the email draft
```
<img width="686" alt="email-draft" src="hands-on-lab-assets/6d2357bd-6c24-4329-843f-0cc7b16398a8.png" />
<br>
<br>

## 🤖 Talentakquise-Agent mit agentischen Workflows automatisieren

Bisher haben Sie einen Agenten erstellt, der die Funktion **Chat with documents** von watsonx Orchestrate nutzt, um Lebensläufe, Stellenbeschreibungen und Interviewerpläne hochzuladen und zu verarbeiten. In diesem Fall übernimmt das LLM des Agenten die gesamte schwere Arbeit, während es Luisas Aufgabe ist, den richtigen Prompt/die richtige Abfrage bereitzustellen.

Es ist jedoch oft nicht offensichtlich, was der richtige Prompt sein sollte, insbesondere für eine HR-Managerin ohne Prompt-Engineering-Hintergrund. Darüber hinaus können zusätzliche Schritte erforderlich sein, wie z.B. das automatische Kontaktieren des ausgewählten Kandidaten oder das automatische Planen von Vorstellungsgesprächen. In diesem Fall könnten wir **Agentische Workflows** nutzen.

Der nächste Teil des Labs ist fortgeschrittener und erfordert einige Low-Coding-Fähigkeiten und Vertrautheit mit grundlegenden Programmierkonzepten wie Variablen und For-Each-Schleifen. Wenn Sie lernen möchten, wie man mit **Agentischen Workflows** arbeitet, [folgen Sie diesen Schritten](./hands-on-lab-hr-manager-flows-de.md)

**🎉🎉 Herzlichen Glückwunsch!! Sie haben das Talentakquise-Modul abgeschlossen. Sie sind bereit für das nächste!**

## 🧑‍💼📝 HR-Fallprüfungs-Agent

1. Erstellen Sie einen weiteren Agenten wie zuvor. Fügen Sie diesmal Folgendes zur Beschreibung hinzu:
```
This agent reviews HR cases from employee complaints of potential business conduct guidelines violations
```

<img width="723" alt="Screenshot 2025-09-25 at 10 59 02 AM" src="hands-on-lab-assets/6a49ad39-b869-4846-be4a-43216386fdd7.png">
<br>
<br>

> **Wichtig:** Denken Sie daran, diese Beschreibung auch zum Abschnitt **Behavior** des Agenten hinzuzufügen, um ein ordnungsgemäßes Routing und die Antwortbehandlung sicherzustellen.

2. Fügen Sie ihm Wissen hinzu. Scrollen Sie nach unten zum Abschnitt **Knowledge** und klicken Sie auf **Choose Knowledge**

<img width="733" alt="Screenshot 2025-09-25 at 10 58 53 AM" src="hands-on-lab-assets/88c73733-5121-4f27-96d6-cb892c7cb84a.png">
<br>
<br>

3. Jetzt laden Sie das [IBM Business Conduct Guidelines Dokument](../data/ibm_business_conduct_guidelines.pdf) hoch. Sie können auch mit den BCG Ihres Unternehmens experimentieren, falls verfügbar. Geben Sie eine Beschreibung ein. Es könnte etwa so aussehen:

```
This is the IBM Business Conduct Guideliness
```

Nach dem Speichern sehen Sie etwa Folgendes:

<img width="704" alt="Screenshot 2025-09-25 at 11 01 08 AM" src="hands-on-lab-assets/ed0ff06f-3243-4b28-a8af-d82cfdf6c2d6.png">
<br>
<br>

4. Sie sind jetzt bereit, einige Abfragen zu testen:

```
Help me understand if the following complaint from an employee infringes the IBM Business Conduct Guidelines: "my manager raised his voice and called me names and made fun of me and told me really nasty things every day for the past month"
```

<img width="683" alt="Screenshot 2025-09-25 at 11 03 56 AM" src="hands-on-lab-assets/2c88e831-a267-4cc2-9c5c-9ddc03b75d19.png">
<br>
<br>

```
How about this one: my manager gave me a chocolate from Hawaii after her trip to Maui. Is this a BCG violation?
```

<img width="680" alt="Screenshot 2025-09-25 at 11 07 56 AM" src="hands-on-lab-assets/e2326cb3-7a42-456c-aa47-c4bc6ee6d981.png">
<br>
<br>

5. Sie können feststellen, dass das oben Genannte in der Praxis möglicherweise keine echte Verletzung der Verhaltensrichtlinien darstellt. Wir können den Agenten anpassen, um bestimmte Situationen anders zu behandeln. Dafür können wir die Funktion **Guidelines** verwenden. Scrollen Sie nach unten zum Abschnitt **Guidelines** und klicken Sie auf **New Guideline**:

<img width="706" alt="Screenshot 2025-09-25 at 3 52 41 PM" src="hands-on-lab-assets/12cf07ec-efea-4e2a-8c15-a3e60455e782.png">
<br>
<br>

6. Speichern Sie sie und versuchen Sie dieselbe Abfrage noch einmal im Chat. Sie sollten etwa Folgendes sehen:

<img width="623" alt="Screenshot 2025-09-25 at 3 53 09 PM" src="hands-on-lab-assets/1d13bc4a-5844-4f00-8137-25b5c1f7b859.png">
<br>
<br>

7. Das Ergebnis nach erneutem Versuch derselben Abfrage würde so aussehen:

<img width="678" alt="Screenshot 2025-09-25 at 11 11 26 AM" src="hands-on-lab-assets/0bf1d4ae-c5ba-4b52-8d81-23bfc8d464ee.png">
<br>
<br>

## 🛠️ Alles zusammenfügen

Wir haben gesehen, wie Sie zwei separate Agenten erstellen können, um unterschiedliche Geschäftsanforderungen zu erfüllen, nämlich (1) Talentakquise und (2) HR-Fallprüfungen. Aber wäre es nicht cool, eine einzige Schnittstelle zu haben, um beide Arten von Abfragen vom Benutzer zu bearbeiten? Dazu erstellen wir einen HR-Manager-Agenten, der Abfragen entsprechend weiterleiten kann.

1. Erstellen Sie einen neuen Agenten. Verwenden Sie das gleiche Verfahren wie oben. Geben Sie in der Beschreibung einige grundlegende Routing-Anweisungen an, wie z.B.:

```
This agent manages different HR requests:

1. Talent acquisition: processing resumes, job descriptions, ad interviewers, routing to the talent acquisition agent

2. HR Case Reviewer: processing HR complaints or cases submitted by employees as potential violations to the Business Conduct Guideliness
```

2. Scrollen Sie nach unten zum Abschnitt Agents.
3. Wählen Sie Add from Local Instance
4. Suchen Sie nach den beiden Agenten, die Sie gerade erstellt haben, und fügen Sie beide hinzu.
5. Probieren Sie jetzt verschiedene Abfragen am HR-Manager-Agenten aus

