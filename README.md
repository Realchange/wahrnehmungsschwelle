# Die Wahrnehmungsschwelle

**Eine Schafherde als Gesellschaftsmodell** — interaktive Simulation dazu, wie Nachrichten unterhalb einer kritischen Wahrnehmungsschwelle spurlos verpuffen und oberhalb gesellschaftsweite Informationskaskaden auslösen.

> Wie bringt man eine unliebsame Nachricht zum Verschwinden, ohne sie zu verbieten?
> Man hält ihr Signal ein paar Punkte unter der Schwelle, ab der eine Gesellschaft sie wahrnimmt und weiterträgt.

## Die Simulation

Auf einer Weide grasen rund 110 Schafe. Jedes hat eine individuelle Wahrnehmungsschwelle. Eine Nachricht erscheint als Funke, dessen Signal mit Entfernung und Zeit zerfällt — registriert wird sie nur von Schafen, deren persönliche Schwelle das Signal übersteigt. Wer informiert ist, färbt sich orange und steckt seine Nachbarn an, sobald genug von ihnen bereits informiert sind. Ohne soziale Auffrischung verblasst die Erinnerung wieder.

Aus diesen wenigen Regeln entsteht ein scharfer **Kipppunkt (Perkolationsschwelle)**: Knapp darunter erreicht keine Nachricht mehr als die unmittelbare Umgebung ihres Ursprungs. Knapp darüber erfasst dieselbe Nachricht die halbe Herde. Genau diesen Übergang kann man mit vier Reglern selbst ertasten.

| Regler | Bedeutung |
|---|---|
| **Signalstärke** | Lautstärke und Reichweite der Nachricht — der Hebel des „Zensors" |
| **Wahrnehmungsschwelle** | Ab wann ein Individuum den Reiz überhaupt registriert (Abstumpfung, Reizüberflutung) |
| **Weitergabe-Schwelle** | Anteil informierter Nachbarn, ab dem ein Schaf „mitzieht" (soziale Kosten des Weitersagens) |
| **Vergessensrate** | Wie schnell Informiertheit ohne Verstärkung zerfällt — der Nachrichtenzyklus als Löschmechanismus |

## Ausprobieren

Einfach `wahrnehmungsschwelle-simulation.html` herunterladen und im Browser öffnen — eine einzige Datei, keine Installation, keine Abhängigkeiten, kein Internet nötig.

**Ein Experiment für den Anfang:** Lass alle Regler auf Standard und sende einige Nachrichten (Klick auf die Weide oder „Nachricht senden"). Schiebe dann die Signalstärke schrittweise von 65 auf 40 herunter und beobachte die Peak-Anzeige. Irgendwo dazwischen kippt das Verhalten qualitativ — von „Kaskade" zu „verpufft". Dieselbe Wirkung erzielst du spiegelbildlich, indem du die Wahrnehmungsschwelle anhebst oder die Weitergabe-Schwelle erhöhst. Und bei hoher Vergessensrate kollabieren selbst erfolgreiche Kaskaden wieder, weil die Erinnerung schneller zerfällt, als die Nachbarschaft sie auffrischen kann.

## Warum das interessant ist

Die Simulation macht eine unbequeme Struktureinsicht erfahrbar: **Informationskontrolle braucht keine Kontrolle über Individuen.** Kein Schaf wird gezwungen, keines belogen, nichts wird verboten. Es genügt, an den Systemparametern zu drehen — Signalstärke dämpfen, Schwellen anheben, Aufmerksamkeitszyklen beschleunigen. Die Herde zensiert sich dann von selbst, und zwar ohne dass es irgendjemandem auffallen müsste: Eine Nachricht, die unter der Perkolationsschwelle bleibt, hinterlässt keine Spur, an der man ihr Fehlen bemerken könnte.

Das Modell steht in der Tradition der Schwellenwertmodelle kollektiven Verhaltens (Granovetter 1978) und der Kaskadenmodelle in Netzwerken (Watts 2002), übersetzt in eine räumliche, bewusst anschauliche Form. Es ist ein Denkwerkzeug, kein Abbild der Wirklichkeit — aber wie jedes gute Modell zeigt es, wie wenig nötig ist, damit ein Phänomen entsteht, das wir sonst vorschnell böser Absicht oder großer Verschwörung zuschreiben würden.

## Technisches

Vanilla JavaScript, Canvas 2D, eine einzige HTML-Datei. Agentenbasierte Dynamik mit räumlichem Nachbarschaftsgraphen (Ø 6 Nachbarn), normalverteilten individuellen Schwellen (σ = 12), exponentiellem Signalzerfall und probabilistischer Weitergabe. Heller und dunkler Modus folgen automatisch der Systemeinstellung; `prefers-reduced-motion` wird respektiert. Die Datei `SPEC-wahrnehmungsschwelle-simulation.md` enthält die vollständige Modellspezifikation mit Kalibrierungswerten und Akzeptanzkriterien.

## Ausblick

Dies ist die erste einer geplanten Serie kleiner Gesellschaftssimulationen zu Mechanismen der Meinungsdynamik — darunter pluralistische Ignoranz (das private und das öffentliche Schaf), das Overton-Fenster als Regelkreis und Verdrängung durch Aufmerksamkeitsökonomie. Gemeinsamer Nenner: Manipulation setzt nicht an den Elementen eines Systems an, sondern an seinen Regelparametern.
