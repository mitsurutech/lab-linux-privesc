# Lab zur Rechteausweitung unter Linux: ein setuid-Programm, eine sudo-Regel und ein beschreibbarer cron-Job

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [日本語](README.ja.md) | [Deutsch](README.de.md)

[![check](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml)

Ein fertiges **Lab zur Rechteausweitung (Privilege Escalation) unter Linux** für den Unterricht, von der [MitsuruTech Learning Platform](https://mitsurutech.co.uk/de/learning), auf der Schulen und Colleges allen Lernenden eigene Maschinen und ein Terminal im Browser geben. Alle Lernenden erhalten eine eigene Ubuntu-VM, auf der bereits ein zweites Konto mit wenigen Rechten liegt, `intern`, und in der drei gewöhnliche Fehler von Administratoren zum Finden versteckt sind: ein Hilfsprogramm mit einem setuid-Bit, das es nie haben sollte, eine sudo-Regel, mit der `intern` einen Editor ohne Passwort als root ausführen darf, und ein Wartungsskript, das root jede Minute über den eigenen cron ausführt und das jeder beschreiben kann. Die Aufgabe ist, über eine der drei Schwachstellen root zu werden, `/root/flag.txt` zu lesen und aufzuschreiben, was dabei genutzt wurde.

Dies ist eine Sicherheitsübung, daher kommt die eine Regel zuerst: Greifen Sie nur Ihre eigene VM an. Sie hat keinen Weg zu den Maschinen anderer Lernender und keinen Weg ins Internet außer zum eigenen Paketarchiv von Ubuntu, zu PyPI, npm und GitHub. Richten Sie nie etwas, das Sie hier lernen, auf eine Maschine, die nicht Ihre eigene ist: Nach dem Computer Misuse Act 1990 ist das ohne Erlaubnis eine Straftat.

## Das Lab

| Datei | Was die Lernenden tun | Hilfeseite |
| --- | --- | --- |
| [`linux-privesc.lab.yaml`](linux-privesc.lab.yaml) | Zum Konto `intern` mit wenigen Rechten wechseln, eine der drei versteckten Schwachstellen finden, über sie root werden und es beweisen | [Lab zur Rechteausweitung unter Linux](https://mitsurutech.co.uk/de/help/lab-linux-privesc) |

Das Setup legt eine README im Home-Ordner der Lernenden ab, die die Regel nennt, das zweite Konto und sein Passwort angibt und sagt, dass root einen Weg hinein offen gelassen hat. Sie enthält keine Schritte zur Ausnutzung.

**Es hat absichtlich kein Bewertungsskript.** Das eigene Konto der Lernenden auf der VM hat Administratorrechte. Jede Datei, die ein Skript als Beweis lesen könnte, könnten die Lernenden also mit einer Zeile selbst schreiben, ohne eine der drei Schwachstellen zu nutzen: Wir haben es ausprobiert, und eine Prüfung hat bestanden. Eine Bewertung, die sich leicht fälschen lässt, ist schlechter als keine. Die Arbeit ist daher ein kurzer schriftlicher Bericht, welche Schwachstelle, welche Befehle und was in der Flag stand, den die Lehrkraft liest. Das anschließende Beheben der Schwachstellen wird nicht verlangt, eine Lehrkraft kann es aber als weiterführende Aufgabe stellen.

## Was in der Datei steht

`linux-privesc.lab.yaml` ist eine vollständige **Definition eines VM-Labs**, genau das, was die Plattform ausführt:

| Schlüssel | Was er ist |
| --- | --- |
| `cpus`, `memory`, `disk` | die Größe der VM der lernenden Person: 1 CPU, 1024 MB und 6 GB |
| `open` | die Ports, die im Browser geöffnet werden: keine, das Lab ist das Terminal |
| `setup` | ein Skript, das einmal als root beim ersten Start der VM läuft: Es legt das Konto `intern` an, schreibt die Flag, versteckt die drei Schwachstellen und schreibt die README |

## Auf der Plattform verwenden

**Um es einer Klasse als Aufgabe zu stellen:**

1. Fügen Sie in Ihrem Modul eine Aufgabe hinzu und wählen Sie die Karte **Ubuntu-VM-Lab**.
2. Klicken Sie auf das Beispiel, um davon auszugehen, oder fügen Sie den Inhalt von `linux-privesc.lab.yaml` ein. Ein Beispiel ersetzt den Text, und „Rückgängig“ bringt Ihren zurück.
3. Veröffentlichen Sie und probieren Sie zuerst Ihre eigene Kopie aus. Alle Lernenden erhalten eine eigene VM, die zwischen den Sitzungen erhalten bleibt.
4. Weisen Sie sie zu wie jede Aufgabe, und wählen Sie in ihren Abgabeeinstellungen „Ein Dokument oder eine PDF-Datei“, damit die Lernenden einen schriftlichen Bericht abgeben können. Lesen Sie ihn wie jede Abgabe: Nichts auf der Maschine bewertet ihn.

Der [Leitfaden zu Ubuntu-VM-Labs](https://mitsurutech.co.uk/de/help/labs) im Hilfehandbuch und seine [Seite mit Beispiel-Labs](https://mitsurutech.co.uk/de/help/sample-labs) erklären den Rest. Noch kein Konto für Ihr College? [Ihr erstes Jahr ist kostenlos](https://mitsurutech.co.uk/de/pricing) für bis zu 50 Lernende, und für die [Registrierung](https://mitsurutech.co.uk/de/signup) genügt eine E-Mail-Adresse.

## Die Datei prüfen

Bei jedem Push prüft `.github/workflows/check.yml`, dass die Datei gültiges YAML mit den Schlüsseln oben ist und dass `setup` innerhalb der Grenze der Plattform von 16 KB liegt. Es startet keine VM: Das geschieht auf den eigenen Maschinen der Plattform.

## Was in einem Lab nicht funktioniert

Die VM erreicht das Paketarchiv von Ubuntu, PyPI, npm und GitHub und sonst nichts. Auf der anderen Seite dieses Labs gibt es also nichts anzugreifen und keine zweite Maschine, von der aus angegriffen werden könnte. Es ist eine einzige VM mit einem zusätzlichen Konto darauf.

## Über uns

Erstellt von [Mitsuru Technologies](https://mitsurutech.co.uk/de) für die [MitsuruTech Learning Platform](https://mitsurutech.co.uk/de/learning): Python, Java, Go, Node, SQL und Linux im Browser für Schulen und Colleges, mit Container-Labs, VM-Labs, aus GitHub bereitgestellten Web-Apps und Quizzen, die sich selbst bewerten. Probieren Sie Code ohne Konto im [Playground](https://mitsurutech.co.uk/de/playground) aus.

Weitere Labs vom selben Ort: [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-juice-shop](https://github.com/mitsurutech/lab-juice-shop), [lab-dvwa](https://github.com/mitsurutech/lab-dvwa), [lab-webgoat](https://github.com/mitsurutech/lab-webgoat), [lab-mysql-phpmyadmin](https://github.com/mitsurutech/lab-mysql-phpmyadmin) und [lab-sample](https://github.com/mitsurutech/lab-sample).

## Lizenz

Dieses Werk steht unter der Lizenz [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Sie dürfen es zum Lernen und Lehren weitergeben und bearbeiten, wenn Sie Mitsuru Technologies Ltd nennen, es nicht kommerziell nutzen und alles, was Sie daraus machen, unter derselben Lizenz weitergeben. Der vollständige Text steht in der Datei `LICENSE`.

Alles, was das Setup installiert, `cron` und `vim`, kommt beim Start der VM aus dem eigenen Archiv von Ubuntu und behält die Lizenz seines eigenen Projekts; nichts von beiden wird hier verteilt.
