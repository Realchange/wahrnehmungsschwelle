# SPEC: „Die Wahrnehmungsschwelle" — Perkolations-Simulation mit Schafherde

**Projekttyp:** Interaktive Browser-Simulation (agentenbasiert)
**Zielgruppe:** Interessierte Laien (Blog-Leser Cassirer.de, Begleitmaterial Sachbuch)
**Auftraggeber:** Arne
**Für:** Claude Code

---

## 1. Ziel & Erkenntnisabsicht

Baue eine visuelle Gesellschaftssimulation, die eine einzige Kernfrage erfahrbar macht:

> **Wie kann eine unliebsame Nachricht in einer Gesellschaft „verschwinden", ohne dass sie jemand verbietet — allein dadurch, dass ihr Signal unter einer kritischen Wahrnehmungsschwelle gehalten wird?**

Die Simulation soll den **kritischen Punkt (Perkolationsschwelle)** sichtbar machen: Knapp unterhalb verpufft jede Nachricht lokal und spurlos; knapp oberhalb entsteht eine gesellschaftsweite Informationskaskade. Der Nutzer soll diesen Kipppunkt durch Verschieben von Reglern selbst „ertasten" können.

**Didaktische Kernbotschaft:** Informationskontrolle braucht keine Kontrolle über Individuen — es genügt die Kontrolle über Systemparameter (Signalstärke, Schwellen, Dämpfung).

---

## 2. Modellspezifikation

### 2.1 Agenten („Schafe")

- N ≈ 100–140 Agenten, zufällig auf einer 2D-Weide verteilt (Mindestabstand, kein Gitter — organische Anmutung).
- Jedes Schaf *i* hat eine **individuelle Wahrnehmungsschwelle** θᵢ, gezogen aus einer Normalverteilung: θᵢ ~ N(θ̄, σ), geclampt auf [5, 95]. σ fest (z. B. 12), θ̄ ist Regler-Parameter.
- Zustand: `awareness aᵢ ∈ [0, 1]`. Ein Schaf gilt als **informiert**, wenn aᵢ ≥ 0.5.
- Nachbarschaft: alle Schafe im Radius R (räumliches Netzwerk, R so wählen, dass im Mittel 5–8 Nachbarn entstehen). Nachbarschaftsgraph einmalig bei Initialisierung berechnen.

### 2.2 Nachrichten („Funken")

- Eine Nachricht erscheint als **Funke** an einer Position (Zufall oder Mausklick) mit Anfangs-Signalstärke S₀ (Regler-Parameter).
- Der Funke zerfällt exponentiell: S(t) = S₀ · e^(−λt), wobei λ aus dem Dämpfungs-Regler abgeleitet wird. Funke erlischt bei S < 5.
- **Direkte Wahrnehmung:** Ein Schaf im Wirkradius des Funkens (mit linearem Distanz-Falloff) wird informiert (aᵢ → 1), wenn die effektive Signalstärke **> θᵢ**.

### 2.3 Soziale Weitergabe (Granovetter/Watts-Schwellenlogik)

- In festen Sozial-Ticks (z. B. alle 300 ms): Für jedes nicht-informierte Schaf berechne f = (informierte Nachbarn) / (Nachbarn gesamt).
- Wenn **f ≥ φ** (Weitergabe-Schwelle, Regler-Parameter): Schaf wird mit Wahrscheinlichkeit p ≈ 0.35 pro Tick informiert (probabilistisch, damit die Ausbreitung sichtbar „läuft" statt schlagartig kippt).

### 2.4 Vergessen & Reaktivierung

- Informierte Schafe verlieren awareness kontinuierlich: daᵢ/dt = −δ (Vergessensrate, Regler-Parameter).
- Solange f ≥ φ, wird aᵢ sozial aufgefrischt (aᵢ → 1). Dadurch entstehen zwei qualitativ verschiedene Regime: **selbsttragende Kaskade** vs. **kollabierende Erinnerung**.

### 2.5 Parameter (genau 4 Regler — Minimalismus ist Feature)

| Parameter | Symbol | Bereich | Default | Bedeutung |
|---|---|---|---|---|
| Signalstärke | S₀ | 0–100 | 55 | Lautstärke/Reichweite der Nachricht |
| Wahrnehmungsschwelle (Mittel) | θ̄ | 0–100 | 50 | Ab wann ein Schaf den Funken überhaupt registriert |
| Weitergabe-Schwelle | φ | 0–100 % | 30 % | Anteil informierter Nachbarn, ab dem ein Schaf „mitzieht" |
| Vergessensrate | δ | 0–100 | 15 | Wie schnell Informiertheit ohne Verstärkung zerfällt |

**[ANNAHME]** σ = 12, R kalibriert auf ~6 Nachbarn, p = 0.35 — Feinwerte dürfen bei der Kalibrierung angepasst werden, solange der kritische Übergang bei mittleren Reglerstellungen erreichbar bleibt (siehe Akzeptanzkriterium A3).

---

## 3. Visualisierung

### 3.1 Die Schafe

- **Pummelige Schafe**, Comic-schematisch, Canvas-2D-gezeichnet (keine Bild-Assets): flauschiger ovaler Wollkörper (überlappende Kreise für Wolkenkontur), dunkler Kopf mit Ohren, vier Stummelbeine, Knopfauge. Zufällige Blickrichtung links/rechts, leichte Größenvariation.
- **Wollfarbe = Zustand:** Uninformiert = cremeweiß/hellgrau. Informiert = warmes Orange, Intensität proportional zu aᵢ (Vergessen wird als Verblassen sichtbar). Informierte Schafe erhalten dezenten Glow.
- **Idle-Animation:** leichtes Kopfnicken/Grasen mit individueller Phase — die Herde soll leben, nicht statisch wirken.
- Beim ersten Wahrnehmen: kurzes „!"-Symbol über dem Kopf.

### 3.2 Der Funke

- Leuchtender Punkt mit radialem Gradient + expandierendem Ring beim Erscheinen. Helligkeit/Radius proportional zur aktuellen Signalstärke S(t); verblasst mit dem Zerfall.

### 3.3 Statistik-Panel

- Live-Anzeige: **% informiert** (groß), Peak-Reichweite der letzten Nachricht, Zähler gesendeter Nachrichten.
- **Sparkline** (Zeitreihe des Informiertheitsanteils, letzte ~60 s).
- Nach jeder Nachricht ein Verdikt: „verpufft" (Peak < 10 %), „lokal" (10–50 %), „**Kaskade**" (> 50 %).

---

## 4. Interaktion & UI

- 4 Slider (siehe 2.5) mit Live-Wertanzeige, wirken sofort auf die laufende Simulation.
- Buttons: **„📣 Nachricht senden"** (Funke an Zufallsposition), **Auto-Modus** (Funke alle ~4 s), **Reset** (neue Herde, Statistik nullen).
- Klick/Tap auf die Weide platziert einen Funken an der Klickposition.
- Responsives Layout, Desktop-first; muss aber auf ~380 px Breite bedienbar bleiben.
- Sprache der UI: Deutsch.

---

## 5. Technischer Rahmen

- **Stack:** Vite + Vanilla TypeScript + Canvas 2D. **Keine Frameworks, keine externen Laufzeit-Dependencies.** Build muss als statische Seite deploybar sein (Vercel/beliebiger Static Host, auch einbettbar via iframe auf Cassirer.de/WordPress).
- Simulation entkoppelt vom Rendering: fixer Simulations-Tick (dt), Rendering via requestAnimationFrame.
- Projektstruktur (Vorschlag):

```
wahrnehmungsschwelle/
├── index.html
├── src/
│   ├── main.ts          // Bootstrap, UI-Bindung
│   ├── model.ts         // Agenten, Funken, Dynamik (framework-frei, testbar)
│   ├── render.ts        // Canvas-Zeichnung (Schafe, Funken, Sparkline)
│   └── ui.ts            // Slider, Buttons, Statistik
├── src/model.test.ts    // Unit-Tests für die Kerndynamik (vitest)
└── README.md            // Modellbeschreibung + Parameterdoku
```

---

## 6. Meilensteine (jeweils einzeln lauffähig & abnehmbar)

1. **M1 — Kernmodell headless:** `model.ts` mit Unit-Tests. Nachweis per Test: Bei S₀ = 30/θ̄ = 60 verpufft die Nachricht (< 10 % Peak), bei S₀ = 80/φ = 20 % entsteht eine Kaskade (> 60 % Peak). *(Erst abnehmen lassen, dann weiter.)*
2. **M2 — Rendering:** Weide + animierte Schafe + Funken, Zustandsfärbung, noch ohne UI-Panel (Parameter hart kodiert).
3. **M3 — UI & Statistik:** Slider, Buttons, Klick-Funken, Sparkline, Verdikt-Anzeige.
4. **M4 — Kalibrierung & Politur:** Defaults so justieren, dass der Kipppunkt didaktisch „in Reglermitte" liegt; Performance-Check; README.

---

## 7. Akzeptanzkriterien

- **A1:** 60 fps bei N = 120 Schafen auf gängigem Laptop; keine Speicherlecks im Auto-Modus über 10 min.
- **A2:** Alle 4 Parameter verändern das Verhalten sichtbar und in der modellgemäß erwarteten Richtung.
- **A3 (Kernkriterium):** Es existiert eine Reglerkonstellation, bei der eine Änderung der **Signalstärke um ±10 Punkte** über Kaskade vs. Verpuffen entscheidet — der kritische Punkt ist real erlebbar, nicht nur behauptet.
- **A4:** Zwei aufeinanderfolgende Nachrichten bei identischen Parametern können unterschiedlich ausgehen (Stochastik erhalten, kein deterministisches Theater).
- **A5:** Code läuft ohne Netzwerkzugriff; `npm run build` erzeugt deploybares `dist/`.

---

## 8. Explizit außerhalb des Scopes (nicht bauen, nur notieren)

- Phasendiagramm (Peak-Reichweite über S₀×θ̄-Raster als Heatmap) — starke Erweiterung für V2.
- „Zensor-Agent", der S₀ dynamisch regelt, um die Herde knapp unter der Schwelle zu halten (Regelkreis-Variante, Anschluss an *Die Regelung der Psyche*).
- Mehrere konkurrierende Nachrichten, Netzwerk-Topologien jenseits des Radius-Graphen, Sound.

## 9. Arbeitsregeln für Claude Code

- Bei Lücken oder Widersprüchen in dieser Spec: **Flag, don't fill** — Lücke benennen, 2–3 Optionen vorschlagen, auf Entscheidung warten.
- Jede bei der Umsetzung getroffene Annahme im Abschlussbericht des jeweiligen Meilensteins explizit auflisten.
- Kein Scope über die Meilensteine hinaus, auch wenn Erweiterungen naheliegen (→ Abschnitt 8 ergänzen statt bauen).
