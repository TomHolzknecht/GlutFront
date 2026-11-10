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
- Schadenszähler pro Turm und Rangliste der stärksten Türme am Rundenende.
- 4 Regionen mit eigener Optik und eigener Musik: Kohleflöz, Pilzgrund, Kristallgrotte, Glutschlund.
- Bestenlisten je Karte, gespeichert im Browser (localStorage). Optional eine weltweite Liste, siehe unten.

## Steuerung

- **Touch:** Turmkarte antippen, Finger aufs Spielfeld legen, auf das Feld ziehen, loslassen. Die Bombe funktioniert genauso.
- **Maus und Tastatur:** Tasten 1–6 wählen einen Turm, B macht die Bombe scharf, Leertaste startet die Welle, P pausiert, M schaltet den Ton, Esc bricht ab.
- Die Steuerung und der Ton lassen sich im Menü (Zahnrad) umstellen.

## Online-Bestenliste (optional)

Ohne Einrichtung bleibt die Bestenliste lokal. Neue Einträge landen trotzdem in einer Warteschlange und werden später gesendet.

Die weltweite Liste nutzt eine Supabase-Tabelle. Das Spiel braucht dafür keinen eigenen Server.

1. Lege auf supabase.com ein Projekt an.
2. Führe im SQL-Editor dieses Skript aus:

```sql
create table scores (
  id uuid primary key,
  map text not null check (char_length(map) <= 20),
  name text not null check (char_length(name) between 1 and 16),
  wave int not null check (wave between 0 and 300),
  kohle int not null check (kohle >= 0),
  kills int,
  time int,
  version text,
  created_at timestamptz default now()
);
alter table scores enable row level security;
create policy "lesen" on scores for select using (true);
create policy "eintragen" on scores for insert with check (true);
```

3. Trage in `index.html` bei `const ONLINE` die Projekt-URL (`url`) und den anon-Key (`key`) ein.

Hinweis: Die Einträge kommen ungeprüft aus dem Browser. Wer will, kann falsche Werte senden. Für eine Liste unter Freunden reicht das. Löschen und Ändern erlaubt die Tabelle niemandem.

## Neuigkeiten und Kommentare

Das Menü hat eine Seite „Neuigkeiten“ mit dem Änderungsprotokoll. Neuer Eintrag = neues Objekt ganz oben in `CHANGELOG` in `index.html`. Die `id` muss eindeutig und stabil bleiben, an ihr hängen die Kommentare.

Kommentare nutzen dasselbe Supabase-Projekt wie die Bestenliste. Ohne die Tabelle bleibt die Seite lesbar, das Kommentarfeld meldet dann „noch nicht eingerichtet“. Skript für den SQL-Editor:

```sql
create table comments (
  id uuid primary key default gen_random_uuid(),
  entry text not null check (char_length(entry) between 1 and 40),
  name text not null check (char_length(name) between 1 and 16),
  text text not null check (char_length(text) between 1 and 300),
  created_at timestamptz default now()
);
alter table comments enable row level security;
create policy "lesen" on comments for select using (true);
create policy "eintragen" on comments for insert with check (true);
```

Moderation: Löschen geht nur im Supabase Table Editor. Im Spiel gilt eine Wartezeit von 30 Sekunden pro Browser. Sie hält Spam nicht auf, wenn jemand die Schnittstelle direkt anspricht. Texte erscheinen als reiner Text, nie als HTML.

## Für Entwickler

- Kommentare im Code sind auf Deutsch. „Feedback N" verweist auf frühere Playtest-Runden, „Overhaul v3" auf den Umbau von Optik, Steuerung und Wellen.
- `glutfront-v1-vor-balance-update.html` ist ein altes Backup und bleibt unverändert.
- `stepGame(dt)` rechnet einen Simulationsschritt ohne Zeichnen. Damit lassen sich Testläufe im Headless-Browser schneller als in Echtzeit fahren.
