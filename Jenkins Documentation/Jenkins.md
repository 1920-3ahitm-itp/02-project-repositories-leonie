# [Jenkins](https://jenkins.vm81.htl-leonding.ac.at)

## Was macht Jenkins?

Codeänderungen werden sofort automatisch getestet und auf die Systeme deployed.

![pipeline](./images/jenkins_pipeline.png)



## Aufsetzen

Eine neue Pipeline wird mit "Element anlegen" angelegt.

![screenshot](./images/aufsetzen_1.png)

Danach muss man einen Namen angeben und "Pipeline" auswählen.

![screenshot](./images/aufsetzen_2.png)

Unter "General" wird "GitHub-Projekt" angehakt und als Project url wird die URL zum GitHub Repository angegeben.

![screenshot](./images/aufsetzen_3.png)

Weiters muss "GitHub hook trigger for GITScm polling" angehakt werden.

![screenshot](./images/aufsetzen_4.png)

Unter "Pipeline" wird "Pipeline script from SCM" als Definition ausgewählt. Als SCM wird Git ausgewählt.

![screenshot](./images/aufsetzen_5.png)

Die Repository URL ist die Remote URL des GitHub Repositories und die Credentials sind `htl-leonding-jenkins/******`.

![screenshot](./images/aufsetzen_6.png)

Als Branch Specifier wird der Branch ausgewählt, der den Code enthält, der gebuildet werden soll.

![screenshot](./images/aufsetzen_7.png)

Mit "Speichern" wird die Pipeline gespeichert.

![screenshot](./images/aufsetzen_8.png)

Auf GitHub wird unter Settings > Webhooks der Webhook hinzugefügt.

![screenshot](./images/aufsetzen_9.png)

Die Payload URL ist `[Jenkins URL]/github-webhook/`. Danach wird "Send me **everything**"  ausgewählt und auf "Add webhook" geklickt.

![screenshot](./images/aufsetzen_10.png)



## Konfiguration

Unter "Konfigurieren" können die aktuellen Einstellungen geändert werden.

![screenshot](./images/konfiguration.png)



## [Jenkinsfile](https://github.com/htblaleonie/leonie-web/blob/dev/Jenkinsfile)

Am Anfang des Jenkinsfiles wird der `agent` definiert. Dieser ist eine Maschine oder ein Container, der die Pipeline ausführt. `any`, bedeutet, dass sie auf jeder verfügbaren Maschine ausgeführt werden kann.

Danach werden die Environment-Variablen definiert. Diese Variablen können im gesamten Jenkinsfile verwendet werden.

In der Variable `GPG_PASSWORD` werden von Jenkins gespeicherte Credentials gespeichert.

Danach werden die Stages definiert. Es gibt die Stages `Build`, `Test`, und `Deploy`. Jede Stage besteht aus Steps. In diesen können z.B. Shell-Befehle ausgeführt werden.

Stages bei Leonie:

- **Build**: Dateien decrypten
- **Test**: Noch nicht implementiert
- **Deploy**: Über `ssh` deployen

Unter `post` kann man definieren, was nach den Stages ausgeführt wird. Bei Leonie wird mit `cleanWs()` der Workspace aufgeräumt.