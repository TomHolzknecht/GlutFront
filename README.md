# GLUTFRONT

Tower-Defense im Kohlestollen. Das ganze Spiel ist eine einzige Datei (`index.html`), ohne Build-Schritt und ohne externe Dateien. Es läuft auf GitHub Pages und offline.

Spielen: https://tomholzknechtnetapp.github.io/GlutFront/

## So funktioniert es

1. Die Gegner kommen aus dem Stollen und laufen den Weg entlang zur Schmiede.
2. Du baust Türme auf die Felder neben dem Weg. Jeder Turm kostet Kohle.
3. Besiegte Gegner lassen Kohle fallen. Damit baust du weiter und rüstest Türme bis Stufe 4 auf.
4. Jeder Gegner, der die Schmiede erreicht, kostet Leben. Bei 0 Leben ist die Runde verloren.
5. Nach 20 Wellen hast du gewonnen. Danach kannst du im Endlos-Modus weiterspielen.

Die vollständige Anleitung steht im Spiel unter „Anleitung". Das Tutorial „Schmiedeschule" zeigt alles Schritt für Schritt.

## Inhalt

- 9 Karten in 3 Schwierigkeitsgraden, dazu die Tutorial-Karte „Schmiedeschule".
- 6 Türme: Schleuderer, Frostturm, Ofen, Kanone, Magier, Scharfschütze. Jeder Turm geht bis Stufe 4. Bei Stufe 4 wählst du einen von zwei Wegen.
- Paragon: 3 Türme desselben Typs auf Stufe 4 verschmelzen zu einem Paragon (höchstens 1 pro Typ, höchstens 5 Stufe-4-Türme gleichzeitig).
- 12 Gegner mit Sonderregeln: Flieger, Schwarm, Schild, Panzerung, Feuerimmunität, Heiler, Brutmutter, Boss.
- Bombe: 200 Kohle, 15 pro Spiel, 6 Sekunden Ladezeit, 25 % Kohle für Gegner, die durch sie sterben.
- Wellenvorschau, Zielmodus pro Turm und Fokus-Markierung per Antippen.
- Endlos-Modus nach Welle 20 mit Ären und dem Schmiedewerk als Kohle-Senke.
- Bestenlisten je Karte, gespeichert im Browser (localStorage).

## Steuerung

- **Touch:** Turmkarte antippen, Finger aufs Spielfeld legen, auf das Feld ziehen, loslassen. Die Bombe funktioniert genauso.
- **Maus und Tastatur:** Tasten 1–6 wählen einen Turm, B macht die Bombe scharf, Leertaste startet die Welle, P pausiert, M schaltet den Ton, Esc bricht ab.
- Die Steuerung und der Ton lassen sich im Menü (Zahnrad) umstellen.

## Für Entwickler

- Kommentare im Code sind auf Deutsch. „Feedback N" verweist auf frühere Playtest-Runden, „Overhaul v3" auf den Umbau von Optik, Steuerung und Wellen.
- `glutfront-v1-vor-balance-update.html` ist ein altes Backup und bleibt unverändert.
- `stepGame(dt)` rechnet einen Simulationsschritt ohne Zeichnen. Damit lassen sich Testläufe im Headless-Browser schneller als in Echtzeit fahren.
