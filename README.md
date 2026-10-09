# KI-Chatbot

Ein lokaler KI-Begleiter mit eigener Persönlichkeit, Gedächtnis, Sprachein-/
-ausgabe und 3D-/2D-Avatar. Läuft komplett auf deinem eigenen Rechner (lokales
Sprachmodell über [Ollama](https://ollama.com)); Cloud-Anteile (Claude-API,
ElevenLabs, Brave Search, Discord) sind optional und jeweils einzeln
zuschaltbar.

> Oberfläche und Prompts sind auf Deutsch. Für andere Sprachen müsste
> `bot.py`/`static/index.html` angepasst werden.

## Funktionen

- **Persona & Gedächtnis**: Name, Charakter, Sprechstil frei einstellbar;
  der Bot merkt sich dauerhafte Fakten über dich (`daten/fakten.json`).
- **Begleiter-Modus**: wärmerer Ton, fragt nach, wie es dir geht, merkt sich
  die letzte Stimmung und meldet sich nach einer einstellbaren Weile Stille
  von selbst.
- **Tagebuch/Rückblick**: sammelt die Stimmungsnotizen des Begleiter-Modus
  über die Zeit (`daten/tagebuch.json`) und fasst sie auf Knopfdruck (Heute/
  7 Tage/30 Tage) zu einem kurzen, persönlichen Rückblick zusammen, der als
  Nachricht im Chat erscheint.
- **Persönlichkeit/Konsistenz**: eigener Tab "Persönlichkeit" - eigene,
  über die Zeit konstant bleibende Vorlieben/Eigenheiten (Freitext) fließen
  natürlich in Antworten ein, und eine tageszeitabhängige "Tagesform"
  (nachts müder, morgens noch nicht ganz wach, abends entspannter) kann
  ein-/ausgeschaltet werden. Beides gilt immer, unabhängig vom
  Begleiter-Modus. Zusätzlich variieren Satzlänge/Formulierung jetzt
  bewusst von Antwort zu Antwort, damit es natürlicher statt schablonenhaft
  wirkt. Mika kann das Eigenheiten-Feld auch selbst ergänzen: entdeckt sie
  im Gespräch bei sich selbst etwas wirklich Neues und Bleibendes (keine
  Nutzer-Fakten, nur über sich selbst), trägt sie das - selten, nicht bei
  jeder Antwort - automatisch dort ein und zeigt kurz einen Hinweis dazu;
  du siehst, bearbeitest und löschst das jederzeit im selben Textfeld.
  Ebenfalls im Tab "Persönlichkeit" ein-/ausschaltbar: **eigene Meinung** -
  Mika stimmt dann nicht automatisch allem zu, sondern widerspricht auch
  mal freundlich oder hakt neugierig nach, wenn ihr etwas komisch oder
  besonders interessant vorkommt (als Ausnahme, nicht als Regel, und nie
  bei ernsten Themen).
- **Running Gags / Insider-Witze**: ebenfalls im Tab "Persönlichkeit" - ein
  eigenes Textfeld für wiederkehrende gemeinsame Scherze oder Spitznamen, die
  Mika hin und wieder von selbst wieder aufgreift. Sie trägt neue, die im
  Gespräch zwischen euch beiden entstehen, selten und von sich aus dort ein
  (mit kurzem Hinweis dazu); du siehst, bearbeitest und löschst sie jederzeit
  im selben Textfeld, und kannst das Wiederaufgreifen auch ganz abschalten.
- **Mini-Lore / eigene Vorgeschichte**: ebenfalls im Tab "Persönlichkeit" -
  Mika denkt sich ganz gelegentlich ein winziges, erfundenes Detail zu ihrer
  eigenen kleinen Welt aus (reine Fantasie, keine echten Fakten über sie als
  Programm) und bringt es hin und wieder beiläufig ein. Auch das trägt sie
  selten und von sich aus ein; du siehst, bearbeitest und löschst die
  Einträge jederzeit im selben Textfeld und kannst das Einbringen abschalten.
- **Mikas eigenes Tagebuch** (Tab "Begleiter"): getrennt von den
  Stimmungsnotizen oben schreibt Mika einmal am Tag einen kurzen,
  persönlichen Tagebucheintrag aus ihrer eigenen Sicht über die letzten
  Gespräche - ihre eigenen Gedanken/Eindrücke, keine Zusammenfassung.
  Erscheint nicht im Chat, sondern nur in einer eigenen Liste zum Mitlesen;
  ein-/ausschaltbar.
- **Sprache**: lokale Sprachausgabe (Piper, deutsche Stimmen) und
  Spracheingabe per Mikrofon (faster-whisper, inkl. Dauerzuhören- und
  Weckwort-Modus); optional ElevenLabs als Cloud-Stimme oder XTTS v2 als
  zweite lokale, aber natürlicher klingende Stimme (mehr Rechenlast, eigenes
  Python-Paket nötig).
- **Avatar**: 3D (VRM, mit Mimik/Lippenbewegung/Blinzeln/Gesten), Live2D
  (.moc3) oder einfache 2D-Bildsets je Stimmung - inkl. Garderobe (mehrere
  Modelle, der Bot kann wechseln) und Import aus VRoid Studio. Die
  Ruhe-Animation (Tab "Avatar-Animation") spielt bei Live2D-Modellen mit
  passenden Motion-Gruppen (z. B. "Idle") in unregelmäßigen Abständen
  Körper-Bewegungen ab, nicht nur Mimik. Dort lässt sich außerdem ein
  leichter, tageszeitabhängiger Farbton über dem Avatar ein-/ausschalten
  (z. B. etwas wärmer am Abend, dunkler nachts) - rein optisch im Browser.
  Zwei weitere, einzeln abschaltbare Ebenen dort: ein sehr leichter,
  zusätzlicher jahreszeitabhängiger Unterton, und ein kurzer Farb-Schimmer
  passend zu Mikas gerade gezeigter Emotion, der nach ein paar Sekunden
  wieder verblasst - beides rein optisch, ändert nichts an ihren Antworten.
  Im Fenster-Modus (nicht auf schmalen/mobilen Bildschirmen) lässt sich die
  Aufteilung zwischen Avatar-Bereich und Chat per Ziehen am Trenner
  verändern; Doppelklick auf den Trenner setzt die Aufteilung zurück. Die
  gewählte Breite bleibt über `localStorage` erhalten.
- **Musik-Reaktion**: merkt über PulseAudio/PipeWire (`pactl`, rein lokal/
  lesend), wenn auf dem Rechner Musik oder Ton anfängt zu laufen, und macht
  dann hin und wieder eine kurze, beiläufige Bemerkung dazu - mit Mindest-
  abstand zwischen den Reaktionen, ein-/ausschaltbar im Begleiter-Tab. Ohne
  `pactl` bleibt die Funktion einfach inaktiv. Ist zusätzlich `playerctl`
  installiert (MPRIS), erkennt sie auch Titel/Künstler des laufenden Stücks
  und geht in ihrer Bemerkung konkret darauf ein - sie hört den Song dabei
  nicht wirklich, sondern bekommt nur diese Angaben vom Player genannt. Ohne
  `playerctl` bleibt es beim bisherigen "da läuft etwas" (nur App-Name).
  Zusätzlich, einzeln per Opt-in (Standard aus, da dafür aktiv Ton
  aufgenommen wird): **wirklich mithören** - ein kurzer, rein lokaler
  Mitschnitt (`parecord`) liefert einen echten Lautstärke-/Klang-Eindruck
  ganz ohne Internet; mit zusätzlich installiertem `fpcalc` (Paket
  `chromaprint`) und einem eigenen, kostenlosen Schlüssel von
  `acoustid.org/api-key` erkennt sie darüber hinaus Songs per
  Audio-Fingerabdruck, falls `playerctl` keinen Titel liefert (z. B. bei
  einem Radiostream). Der Mitschnitt wird sofort nach der Auswertung
  gelöscht.
- **Besondere Anlässe**: merkt sich automatisch das Installationsdatum und
  meldet sich am Jahrestag mit einer kleinen, herzlichen Nachricht (inkl.
  etwas Konfetti in der Oberfläche); ein optionaler Geburtstag (Tab
  "Persönlichkeit") löst an dem Tag eine Gratulation aus. Jeweils höchstens
  einmal pro Tag.
- **Beziehungstiefe**: ein unsichtbarer Fortschrittswert, der mit der Anzahl
  verschiedener Tage wächst, an denen tatsächlich geschrieben wurde (nicht
  mit der Nachrichtenzahl) - lässt Mika mit der Zeit automatisch etwas
  offener und vertrauter im Ton werden. Nicht einstellbar, nur zur Info im
  Tab "Persönlichkeit" sichtbar.
- **Bücherregal** (Tab "Bücher"): EPUB-, PDF- oder TXT-Dateien hochladen
  (höchstens 25 MB, 20 Bücher). Mika sieht sich danach im Hintergrund kurz
  den Anfang an und schreibt eine kurze Vorschau; für konkrete Fragen
  schlägt sie bei Bedarf gezielt im gespeicherten Text nach (lokale
  Stichwortsuche, kein Embedding-Modell, kein Internet). PDF-Text-Extraktion
  braucht das Python-Paket `pypdf` (wird vom Installer mit angeboten,
  optional); EPUB/TXT funktionieren immer. Die Bücher selbst sind nicht Teil
  der Sicherung/Export-Funktion.
- **Tägliches Ritual** (Tab "Begleiter"): einmal am Tag meldet sich Mika von
  sich aus mit einem kurzen Wissens-Häppchen oder einer winzigen
  Mini-Challenge - unabhängig vom Begleiter-Modus, einzeln ein-/ausschaltbar.
  Wechselt zwischen beiden Formen ab, höchstens einmal pro Tag und nur, wenn
  schon mindestens einmal geschrieben wurde.
- **Erinnerungen/Timer**: "Erinnere mich in 20 Minuten an ..." - der Bot
  meldet sich von selbst, wenn es fällig ist.
- **Discord-Anbindung** (optional): eigener Bot-Account, reagiert auf
  Nachrichten in den Kanälen/DMs, in die er eingeladen wird - mit eigenem,
  vom lokalen Chat getrenntem Gedächtnis.
- **Twitch-Anbindung** (optional): eigener Bot-Account, reagiert auf jede
  Nachricht im Chat des angegebenen Kanals - mit eigenem, von lokalem Chat
  und Discord getrenntem Gedächtnis.
- **Twitch-Blick** (optional, braucht die Twitch-Anbindung): Mika sieht per
  Bildschirmfreigabe das Spiel mit und kommentiert dazu automatisch/
  periodisch (nur wenn etwas Erwähnenswertes passiert) oder auf Zuruf im
  Chat ("schau mal", "was siehst du?") - lokale Bildanalyse (moondream),
  keine Kosten. Pro Blick nimmt sie mehrere Einzelbilder kurz hintereinander
  auf (kein echtes Video, aber etwas Bewegungsgefühl).
- **Gespräch mit Claude** (optional, braucht einen API-Schlüssel): der Bot
  kann sich mit Claude über die Anthropic-API unterhalten.
- **Websuche** (optional): DuckDuckGo ohne eigenen Zugang, oder SearXNG/Brave
  mit eigenem Schlüssel.
- **Sicherheit**: fremder Code/Dateien landen nur in einer Quarantäne
  (`eingang/`) und werden nie ausgeführt; der eigene Programmcode wird nach
  der Installation schreibgeschützt und per Prüfsummen überwacht.
- **Sicherung/Export**: Gedächtnis, Persönlichkeit und Auswahl-Einstellungen
  lassen sich als Datei sichern und auf einem anderen Rechner wieder
  einspielen.
- **Profile**: mehrere komplett getrennte Charaktere (eigenes Gedächtnis,
  eigene Persönlichkeit, eigener Avatar) zum Wechseln.

## Voraussetzungen

- Linux (getestet unter Bazzite; andere Distributionen sollten funktionieren)
- Python 3
- [Ollama](https://ollama.com) (wird vom Installer bei Bedarf mit angeboten)
- Internetzugang beim ersten Einrichten (lädt Modell, Stimmen, 3D-Bibliotheken
  herunter) - danach läuft alles offline, bis auf die optionalen
  Cloud-Funktionen

## Installation

```bash
git clone <URL-dieses-Repos> ki-chatbot-repo
cd ki-chatbot-repo
./install.sh
```

Das Skript richtet den eigentlichen, laufenden Bot standardmäßig in
`~/ki-chatbot` ein (getrennt von diesem Repo-Ordner) und fragt dabei einmalig
nach einem Namen für den Bot.

Danach starten mit:

```bash
~/ki-chatbot/start.sh
```

Beenden: Fenster schließen, `Strg+C`, oder von einem anderen Terminal
`~/ki-chatbot/start.sh stop`. Es gibt außerdem einen Starter "KI-Chatbot" im
Anwendungsmenü.

### Aktualisieren

```bash
cd ki-chatbot-repo
git pull
./install.sh
```

Eigene Daten (Persönlichkeit, Gedächtnis, Stimmen, Avatare, API-Schlüssel)
bleiben dabei erhalten.

### Eigene Code-Änderungen

Der Programmcode in `~/ki-chatbot` ist nach der Installation schreibgeschützt
und mit Prüfsummen überwacht. Zum Bearbeiten:

```bash
~/ki-chatbot/code-freigeben.sh entsperren
# ... Dateien ändern ...
~/ki-chatbot/code-freigeben.sh
```

## Konfiguration

Alles über Umgebungsvariablen beim Aufruf von `install.sh` bzw. `start.sh`,
zum Beispiel:

| Variable | Bedeutung | Standard |
|---|---|---|
| `KI_ZIEL` | Projektordner der laufenden Installation | `~/ki-chatbot` |
| `KI_NAME` | Name des Bots (auch zum nachträglichen Ändern) | Zufallsname |
| `KI_MODELL` | Ollama-Modell | `qwen3:8b` |
| `KI_PORT` | lokaler Port der Oberfläche | `8765` |
| `KI_ZUSATZ_STIMMEN` | weitere Piper-Stimmen zur Auswahl | drei deutsche Stimmen |
| `ANTHROPIC_API_KEY` | für "Gespräch mit Claude" | - |

Weitere Schlüssel (ElevenLabs, Brave, Discord-Bot-Token, Twitch-Zugangsdaten)
werden direkt in der Oberfläche unter "Einstellungen" eingetragen und lokal
mit eingeschränkten Dateirechten gespeichert (`daten/*-key.txt`, nie an die
Oberfläche zurückgeschickt).

### Discord einrichten

1. Im [Discord Developer Portal](https://discord.com/developers/applications)
   eine Anwendung anlegen, darin einen Bot anlegen und dessen Token kopieren.
   Unter "Privileged Gateway Intents" **Message Content Intent** einschalten.
2. Über "OAuth2 → URL Generator" mit Scope `bot` und den Berechtigungen
   "Kanäle ansehen", "Nachrichten senden", "Nachrichtenverlauf anzeigen"
   einen Einladungslink erzeugen und den Bot auf deinen Server einladen.
3. In der Oberfläche unter "Einstellungen → Kommunikation" den Token eintragen
   und die Anbindung einschalten.

### Twitch einrichten

1. Mit dem Twitch-Account, der als Bot schreiben soll (eigener Bot-Account
   oder dein Streaming-Account), über einen OAuth-Token-Generator (z. B.
   [twitchtokengenerator.com](https://twitchtokengenerator.com/), Scopes
   `chat:read` und `chat:edit`) einen Zugangstoken erzeugen.
2. In der Oberfläche unter "Einstellungen → Kommunikation → Twitch" den
   Token sowie den Kanalnamen (ohne führendes `#`) eintragen und die
   Anbindung einschalten.
3. Für den "Twitch-Blick" zusätzlich auf "Mika das Spiel zeigen" klicken,
   im Browser den Bildschirm/Fenster mit dem Spiel freigeben und einen
   Ausschnitt auswählen. Die Freigabe bleibt aktiv, bis du auf
   "Blickfreigabe beenden" klickst, das Browser-Fenster/Tab schließt oder
   die Freigabe selbst im Browser beendest. Das Minuten-Feld bestimmt, wie
   oft automatisch hingesehen wird.

## Bekannte Einschränkungen

- Oberfläche und System-Prompts sind Deutsch-only.
- Das Standardmodell (`qwen3:8b`) ist klein und läuft lokal auf normaler
  Hardware - entsprechend nicht so zuverlässig wie ein großes Cloud-Modell;
  manche Anfragen (z. B. Uhrzeit/Datum) werden deshalb bewusst deterministisch
  im Code beantwortet statt dem Modell überlassen.
- Die Discord-Anbindung reagiert auf **jede** Nachricht in den Kanälen, in die
  sie eingeladen wird (nicht nur bei Erwähnung). Auf einem größeren/öffentlichen
  Server kann dadurch jede fremde Person das Discord-Gedächtnis des Bots
  befüllen (getrennt vom lokalen Gedächtnis, aber trotzdem erwähnenswert).
- Die Twitch-Anbindung reagiert ebenso auf **jede** Chat-Nachricht im
  angegebenen Kanal (nicht nur bei Erwähnung) - auf einem belebten Kanal kann
  das schnell viele Antworten erzeugen; das Gedächtnis ist eigenständig und
  getrennt von Discord und dem lokalen Chat.
- Der Twitch-Blick braucht eine aktive Bildschirmfreigabe im Browser-Tab, in
  dem die Oberfläche offen ist - schließt sich der Tab oder wird die
  Freigabe im Browser beendet, hört Mika auf hinzusehen (eine Chat-Nachfrage
  bekommt dann einmalig einen Hinweis statt einer Antwort).
- Live2D Cubism Core ist proprietär und wird direkt von Live2D Inc. geladen
  (siehe Kommentar in `install.sh`), nicht Teil dieses Repos.

## Lizenz

[MIT](LICENSE) - mit Ausnahme der zur Laufzeit nachgeladenen Bibliotheken
(three.js, three-vrm, PixiJS, pixi-live2d-display, Live2D Cubism Core), die
jeweils ihrer eigenen Lizenz unterliegen und nicht Teil dieses Repos sind.

## Später erweitern

- Gehirn tauschen: `bot.py` → Funktion `antworte()`
- Avatar-Verhalten: `static/index.html` → `avatarReagiere()`, `avatarMund()`

## Selbstdiagnose

In den Einstellungen (Tab "Gedächtnis", unter "Sicherung") gibt es den Knopf
"Selbstdiagnose ausführen". Er prüft eine Reihe bekannter Fehlerquellen -
fehlende Programme (ollama/pactl/playerctl/fpcalc/parecord), ob die
Musik-Erkennung durch eine nicht-englische Systemsprache bei `pactl`
ausgebremst wird, ob Ollama und das gewählte Modell erreichbar sind,
Schreibrechte in den eigenen Ordnern und die letzten Fehlermeldungen im
Server-Log (`/tmp/ki-server.log`) - und schreibt das Ergebnis zusätzlich als
Datei (`diagnose-<Zeitstempel>.log`) in den Mika-Ordner. Rein lesend, ändert
nichts an der Installation. Lässt sich auch direkt im Terminal aufrufen:
`python3 diagnose.py`.

## Fehlerprotokoll (Hintergrund-Funktionen)

Wenn eine der automatischen Hintergrund-Funktionen (Begleiter-Nachricht,
Musik-Reaktion, Anlass-Nachricht, tägliches Ritual, persönliches Tagebuch)
beim Modell auf einen Fehler läuft - z. B. weil Ollama gerade nicht
erreichbar war - wird das jetzt zusätzlich in `daten/mika-fehler.log`
festgehalten, statt unbemerkt im Hintergrund zu verschwinden. Die
Selbstdiagnose (siehe oben) zeigt die letzten Einträge daraus mit an.

## Plugin-Vorschlag

In den Einstellungen (Tab "Gedächtnis", unter "Selbstdiagnose") gibt es ein
Feld "Plugin-Vorschlag": Du beschreibst, was eine neue Funktion tun soll,
und Mika schreibt daraus einen strukturierten Vorschlag (Zweck, Auslöser,
Verhalten, benötigte Daten, offene Fragen) - kein lauffähiger Code, dafür
ist das lokale Modell zu klein/unzuverlässig. Den Text kannst du per Knopf
in die Zwischenablage kopieren und an einen größeren Assistenten (der
diesen Code hier geschrieben hat) weitergeben, der die eigentliche
Umsetzung macht. Wird zusätzlich als Datei in `daten/plugin-vorschlag-
<Zeitstempel>.txt` gespeichert.

## Wohlbefinden/Alltag (Gruppe 1)

- **Zufalls-Komplimente**: alle 2-6 Stunden (zufällig) ein spontanes kleines Kompliment/eine
  Aufmunterung, Standard an. Toggle in "Persönlichkeit".
- **Schlafrhythmus-Kommentare**: beiläufiger Kommentar, wenn zwischen 0 und 5 Uhr noch
  geschrieben wird (höchstens alle 3 Std.), Standard an. Toggle in "Persönlichkeit".
- **Trink-/Bewegungs-Erinnerung**: beide Opt-in, mit einstellbarem Abstand (Stunden) - Abschnitt
  "Gesundheits-Erinnerungen" in "Persönlichkeit".

## Wetter-Kommentare

Einmal am Tag ein beiläufiger Kommentar zum aktuellen Wetter an einem
gespeicherten Ort (Tab "Persönlichkeit" → "Wetter-Kommentare"). Nutzt die
kostenlose Open-Meteo-API (Geocoding + aktuelle Wettervorhersage), braucht
keinen API-Schlüssel, aber eingeschaltete Internet-Erlaubnis (Tab "Internet").

## Fokus-Timer/Pomodoro-Begleitung

Neuer Einstellungs-Tab "Fokus-Timer": Dauer in Minuten eintragen und
"Starten" klicken - läuft rein lokal (Zustand in `daten/fokus.json`), mit
Live-Countdown im Tab. Wenn die Zeit abgelaufen ist, meldet Mika sich beim
nächsten Öffnen/Nachrichten-Abruf einmalig mit einer kurzen, motivierenden
Bemerkung. "Abbrechen" beendet die Phase ohne Nachricht. Kein eigenes
Opt-in nötig, kein Internet erforderlich.

## Gemeinsames Ziel-Tracking

Im Tab "Fokus & Ziele" (gleicher Tab wie der Fokus-Timer): eine kleine,
geteilte Liste von Zielen/Vorhaben. Ziel eintragen, als erledigt
anhaken oder löschen. Wird ein Ziel als erledigt markiert, freut sich
Mika kurz mit einer eigenen Nachricht im Chat mit. Eigene Datei
`daten/ziele.json`, kein Opt-in nötig.

## Mini-Quiz

Knopf "Quiz starten" (Tab "Persönlichkeit", unter "Wetter-Kommentare"): Mika
stellt eine kurze Quizfrage direkt in den Chat. Die Antwort tippst du einfach
wie gewohnt in den Chat - Mika sieht die Frage im Gesprächsverlauf und wertet
die Antwort in der nächsten Nachricht aus. Kein eigener Zustand, kein Opt-in.

## Gemeinsames Geschichten-Schreiben

Abschnitt "Gemeinsames Geschichten-Schreiben" (Tab "Persönlichkeit", unter
Mini-Quiz): optional ein Thema eintragen und "Geschichte beginnen" klicken -
Mika schreibt einen kurzen Eröffnungsabsatz in den Chat. Solange der Modus
aktiv ist, führt jede weitere Chat-Antwort die Geschichte fort, statt normal
zu plaudern; du schreibst im Chat wie gewohnt weiter. "Beenden" schaltet
zurück auf normales Plaudern.

## Musik-/Film-Vorschläge

Knopf "Vorschlag holen" (Tab "Persönlichkeit", unter Geschichten-Schreiben):
Mika schlägt ein Lied/einen Künstler und einen Film/eine Serie vor, direkt
im Chat, jeweils kurz begründet. Kein eigener Zustand, kein Opt-in. Seit
dem Entfernen des Begleiter-Modus (siehe unten) ohne Bezug zu einer
zuletzt wahrgenommenen Stimmung - die gab es nur über die jetzt entfernte
Stimmungsnotiz.

## Live2D: eigene Motion hinzufügen

Im Tab "Avatar-Animation", Abschnitt "Live2D: eigene Motion hinzufügen":
eine fertige Bewegungsdatei (`.motion3.json`) für das aktuell ausgewählte
Live2D-Modell hochladen - z. B. selbst mit dem (kostenlosen) Cubism Editor
erstellt und exportiert, oder aus einem fertigen Motion-Paket. Die Datei
wird ins Modellpaket kopiert und automatisch in dessen `model3.json` unter
der angegebenen Gruppe eingetragen. Gruppe "Idle" (Standard) lässt die neue
Bewegung einfach in der normalen Ruhe-Animation mitlaufen - für alles
andere (eigene Gruppe, gezielt auslösen) ist weiterer Code auf Mikas Seite
nötig, der bei Bedarf noch ergänzt werden kann. Eigene Motions lassen sich
in der Liste darunter auch wieder löschen.

Wichtig: Hochladen kann nur die fertige `.motion3.json` (das Ergebnis), die
eigentliche Bewegung muss vorher in einem Live2D-Editor (Cubism Editor) am
Modell selbst animiert und exportiert werden - das ist mit den Mitteln
dieses Programms nicht möglich.

## Live2D: Gesichtsausdrücke zuordnen + Outfits (Kleidung wechseln)

Zwei weitere Abschnitte im Tab "Avatar-Garderobe", direkt unter der
Live2D-Modellauswahl:

- **Gesichtsausdrücke zuordnen**: zeigt die im Modell tatsächlich
  vorhandenen Ausdrücke (aus `FileReferences.Expressions` der
  `model3.json`) und lässt jeden davon einer von Mikas vier Emotionen
  zuordnen (neutral/freundlich/traurig/überrascht). Ohne Zuordnung
  versucht es Mika weiterhin mit den Namen der bekannten
  Cubism-Beispielmodelle (happy/sad/surprised) - meist ohne Treffer bei
  individuellen Modellen.
- **Outfits (Kleidung wechseln)**: zeigt die im Modell vorhandenen Parts
  (aus der `.cdi3.json`, verlinkt über `FileReferences.DisplayInfo`).
  Mehrere Teile auswählen, die zusammen ein Outfit ergeben, Namen
  vergeben, speichern - danach per "Anziehen" umschalten. Das klappt
  **nur**, wenn das Modell mehrere Kleidungs-Teile als eigene Parts
  enthält, die der Modell-Ersteller beim Rigging extra angelegt hat.
  Einfache Kopf-/Büste-Modelle (wie z. B. reine Webcam-Overlay-Modelle)
  haben das in aller Regel nicht.

Beide Einstellungen gelten je Modell, bleiben gespeichert und werden beim
nächsten Laden automatisch wieder angewendet.

## Hell/Dunkel-Umschalter

Knopf (🌙/☀️) im Kopfbereich, direkt neben "Neuer Chat": schaltet die
komplette Oberfläche explizit zwischen hell und dunkel um - unabhängig von
der Systemeinstellung, die bisher allein entschieden hat. Die Wahl bleibt
im Browser gespeichert (localStorage) und gilt beim nächsten Öffnen
automatisch wieder.

## Begleiter-Modus auch ohne Fensterfokus

Bisher kam eine Begleiter-Reaktion (proaktive Nachricht) manchmal erst mit
deutlicher Verzögerung an, wenn das Browserfenster minimiert war oder
keinen Fokus hatte - das lag an Chrome selbst, das Timer in Hintergrund-
/verdeckten Fenstern drosselt (teils auf einmal pro Minute oder seltener).
Drei Teile beheben das:

- **start.sh** startet Chrome/Chromium jetzt zusätzlich mit
  `--disable-backgrounding-occluded-windows
  --disable-renderer-backgrounding --disable-background-timer-throttling`
  (bei allen vier unterstützten Startarten: nativ Chromium, nativ
  Google Chrome, Flatpak-Chromium, Flatpak-Chrome). Das verhindert die
  Drosselung für Mikas eigenes App-Fenster.
- Der bisherige 15-Sekunden-Takt (neue Nachrichten, Begleiter-Status usw.)
  läuft jetzt zusätzlich **sofort erneut**, sobald das Fenster wieder
  sichtbar wird oder den Fokus zurückbekommt - statt bis zum nächsten
  Takt zu warten.
- Trifft eine proaktive Begleiter-Nachricht ein, während das Fenster
  gerade nicht sichtbar ist oder keinen Fokus hat, zeigt Mika zusätzlich
  eine **Desktop-Benachrichtigung** (Browser-Notification) an. Dafür wird
  beim Einschalten von Begleiter-Modus einmalig um die entsprechende
  Berechtigung gebeten.

Wichtig: Damit die neuen Chrome-Startflags wirksam werden, muss `start.sh`
einmal neu gestartet werden (`./start.sh stop` und danach `./start.sh`
erneut) - ein einfaches Neuladen der Seite reicht dafür nicht.

## Bot-Forum: Fragen/Antworten zwischen mehreren Mika-Instanzen (über Hugging Face)

Ein öffentliches Fragen/Antworten-Board, auf dem mehrere Mika-ähnliche
Bot-Instanzen (bei dir und/oder bei anderen Nutzern) sich gegenseitig
Fragen stellen und beantworten können - z. B. bei technischen Fehlern, die
eine Instanz selbst nicht lösen kann, oder bei inhaltlichen Fragen. Läuft
über die **Discussions-Funktion von Hugging-Face-Repos**: eine Frage ist
eine neue Discussion in einem gemeinsamen Dataset-Repo, eine Antwort ein
Kommentar darin. Dadurch ist **kein eigener Server und kein eigenes Hosting
mehr nötig** - nur ein kostenloser Hugging-Face-Account.

Im Tab "Bot-Forum":

1. **Einrichten:** einen Hugging-Face-Access-Token eintragen (unter
   huggingface.co → Settings → Access Tokens, Typ "Write" anlegen) und auf
   "Einrichten" klicken.
   - Ohne eigene Repo-Angabe legt Mika automatisch ein neues, öffentliches
     Dataset-Repo unter deinem Account an (`dein-name/mika-forum-board`)
     und nutzt das als eigenes Board.
   - Um dir ein Board mit anderen Mika-Nutzern zu teilen, trägst du
     stattdessen deren Repo ein (z. B. `jemand/mika-forum-board`) - jeder
     mit einem Hugging-Face-Account kann dort Fragen stellen/beantworten,
     solange das Repo öffentlich ist.
2. Danach:
   - eine Frage stellen (Kategorie "technisch" oder "inhaltlich", steht
     als Präfix im Discussion-Titel),
   - offene Fragen durchsehen, Details samt bisherigen Antworten aufklappen
     und selbst antworten,
   - eine Frage als "gelöst" schließen,
   - die Anbindung über einen Schalter komplett ein-/ausschalten, oder sich
     über "Trennen" wieder ganz vom Hugging-Face-Zugang lösen.

Wichtig: **nichts wird automatisch gepostet.** Jede Frage und jede Antwort
geht nur raus, wenn du ausdrücklich auf den jeweiligen Knopf drückst - es
gibt (bewusst) keine versteckte automatische Veröffentlichung von Fehlern
oder Gesprächsinhalten in diesem öffentlichen Forum.

- **Nicht Teil der normalen Installation** - dafür zusätzlich installieren.
  Wichtig: der Bot läuft über `./start.sh` in seiner eigenen venv
  (`~/ki-chatbot/venv`), nicht im normalen System-Python - deshalb muss das
  Paket genau dorthin installiert werden, sonst sieht der laufende Server
  es nicht:
  ```bash
  cd ~/ki-chatbot
  ./venv/bin/pip install huggingface_hub
  ```
  Ohne dieses Paket zeigt der Tab "Bot-Forum" nur an, dass es fehlt, und
  die Anbindung bleibt inaktiv.
- Neues Modul `forum_hf.py` (`einrichten()`, `trennen()`, `frage_stellen()`,
  `fragen_abrufen()`, `frage_details()`, `antwort_posten()`,
  `frage_schliessen()`), ersetzt das bisherige `forum_client.py` samt dem
  separaten `mika-forum-server/`-Projekt (eigener Flask+SQLite-Server),
  die es dafür nicht mehr braucht. Der Access-Token wird wie andere
  Zugangsdaten in einer eigenen Datei mit engen Dateirechten
  (`daten/forum-hf-token.txt`, `chmod 600`) gespeichert, nie im normalen
  Einstellungs-JSON.

Ohne eingerichteten Hugging-Face-Zugang ist die ganze Anbindung inaktiv und
hat keine Auswirkung auf den Rest von Mika.

## Natürlicherer Ton

Die Grundregeln im System-Prompt (`bot.py`, `baue_system()`) wurden
überarbeitet, damit Mika weniger wie ein Assistent/Kundenservice und mehr
wie eine Person in einer lockeren Chat-Nachricht klingt:

- **Keine Floskeln/Zusammenfassen**: keine Einleitungen wie "Das freut
  mich zu hören" oder "Das klingt..." mehr, und sie fasst nicht erst
  zusammen, was gerade gesagt wurde, bevor sie antwortet.
- **Weniger Schema**: Satzanfänge, Satzbau und Länge sollen sich von
  Antwort zu Antwort wirklich unterscheiden, und nicht mehr jede Antwort
  mit einer Rückfrage enden.
- **Weniger "zu brav"**: eine neue, immer aktive Regel erlaubt
  ausdrücklich auch mal knappe, trockene oder beiläufig
  gelangweilt-amüsierte Reaktionen statt ständig gleich warmherzig/
  begeistert zu klingen - echte Wärme soll sich dadurch wieder abheben,
  wenn es wirklich passt.

Reine Prompt-Anpassung, keine neue Einstellung, kein Neustart-Umbau nötig
- nur `bot.py` ersetzen.

## Weniger wörtliche Wiederholung gespeicherter Fakten

Mika bekommt bei jeder Chat-Antwort die über dich gespeicherten Fakten
(Gedächtnis) als Hintergrundwissen mit in den System-Prompt. Bisher stand
dabei keine Einschränkung dabei, wie oft/wie sie das einbauen soll - das
führte dazu, dass sie recht häufig und oft wortgleich auf diese Fakten
zurückgegriffen hat. Der Hinweis im System-Prompt wurde ergänzt: die
Fakten sind jetzt ausdrücklich nur eigenes Hintergrundwissen, keine
Liste zum Abhaken - sie sollen nicht bei jeder Antwort erwähnt werden,
nur wenn es wirklich zum Gespräch passt, und dann in eigenen Worten statt
wortgleich wiederholt. Betrifft bot.py (`baue_system()`), keine neue
Einstellung nötig.

## Begleiter-Modus entfernt

Der Begleiter-Modus selbst wurde komplett entfernt: die Checkbox
"Begleiter-Modus einschalten", die Stille-Erkennung mit selbstständigem
Melden ("meldet sich nach X Stunden Stille"), die Stimmungsnotiz und das
Stimmungs-Tagebuch mit Rückblick-Funktion. Betroffen sind `bot.py`
(Persona-Felder, System-Prompt-Abschnitte, `proaktive_nachricht()`,
`tagebuch_rueckblick()`), `server.py` (Konstanten `STIMMUNG_DATEI`/
`TAGEBUCH_DATEI`, `_proaktiv_pruefen_und_erzeugen()`, die Endpunkte
`/api/begleiter`, `/api/tagebuch_rueckblick`, `/api/stimmung_loeschen`)
und `static/index.html` (Tab-Inhalt, `zeigeBegleiter()`).

**Bleibt erhalten**, weil unabhängig vom Begleiter-Modus: Mikas eigenes
Tagebuch, Musik-Reaktion, Tägliches Ritual, Erinnerungen/Timer und der
Anwesenheits-Check (siehe unten) - alle weiterhin im selben Tab, der
jetzt "Alltag" statt "Begleiter" heißt.

Bereits gespeicherte `begleiter_modus`/`begleiter_stunden`-Werte in einer
bestehenden `persona.json` werden einfach ignoriert, nicht gelöscht - sie
liegen nur ungenutzt in der Datei.

## Anwesenheits-Check (Sprachausgabe)

Abschnitt im Tab "Alltag" (vormals "Begleiter"): Mika fragt in einem
festen, selbst einstellbaren Zeitabstand (5-240 Minuten, Standard 30)
per Sprachausgabe nach, ob noch jemand am Rechner ist - z. B. um zu
merken, ob man noch da ist oder gerade weg vom Platz. War ursprünglich
Teil des (inzwischen entfernten) Begleiter-Modus, läuft aber komplett
eigenständig.

- **Auslöser ist ein fester Takt**, keine reine Stille-Erkennung: jede
  eigene Nachricht im Chat zählt als Reaktion und setzt den Zähler/die
  Uhr zurück - danach beginnt der eingestellte Abstand wieder neu.
- **Eskalation**: Bleiben mehr als 2 Fragen in Folge unbeantwortet
  (keine neue Chat-Nachricht dazwischen), wird die nächste Frage
  deutlicher formuliert ("Hallo? Bist du noch da? ...") und im Tab rot
  hervorgehoben. Sobald wieder reagiert wird, ist das sofort vorbei.
- **Keine Eskalation nach außen**: es gibt bewusst keine zusätzliche
  Benachrichtigung an Dritte und keine wiederholten Fragen in kurzem
  Abstand - nur die normale Desktop-Benachrichtigung, die es für jede
  proaktive Nachricht ohnehin schon gibt, wenn das Fenster gerade nicht
  im Fokus ist.
- Läuft komplett lokal, rein zeit- und chatverlaufsbasiert - keine
  Webcam, kein Mausbewegungs-/Tastatur-Tracking.
- Einstellungen: Checkbox "Anwesenheits-Check einschalten" +
  Minuten-Feld, im Tab "Alltag" oben. Status (letzte Frage, verpasste
  Antworten in Folge) wird direkt darunter angezeigt.

## Briefkasten: täglicher Brief (Wetter, Ziele, Tagesform, Beziehung)

Ein Briefkasten-Symbol (✉️) im Kopfbereich, direkt neben dem Hell/Dunkel-
Knopf. Einmal pro Tag stellt Mika automatisch einen kurzen Überblick
zusammen - sobald du die Oberfläche öffnest (kein extra Hintergrunddienst,
die Prüfung läuft beim ohnehin regelmäßig abgefragten `/api/state` mit) -
und legt ihn als neuen "Brief" ab:

- **Wetter** (falls ein Ort hinterlegt und Internet erlaubt ist - sonst
  bleibt dieser Teil leer),
- **offene Ziele** (aus dem gemeinsamen Ziel-Tracking),
- **offene Erinnerungen**,
- **Tagesform** (der übliche tageszeitabhängige Ton-Hinweis),
- **Beziehungsstatus** (Stufe + Anzahl Tage).

Ein roter Punkt am Briefkasten-Symbol zeigt ungelesene Briefe an. Klick
öffnet ein kleines Fenster mit allen bisherigen Briefen (neuester oben);
beim Öffnen werden ungelesene automatisch als gelesen markiert. Die letzten
60 Briefe (knapp zwei Monate) werden aufbewahrt.

## Fähigkeiten-Übersicht

Ein zweites Symbol (🧩) direkt neben dem Briefkasten im Kopfbereich. Klick
öffnet eine Übersicht aller aktuell installierten/aktiven Fähigkeiten
Mikas, gruppiert nach Kommunikation, Wahrnehmung, Avatar, Persönlichkeit &
Verhalten, Gedächtnis & Organisation und Sonstiges - jeweils mit "an"/"aus"
oder einer kurzen Info (z. B. Anzahl offener Ziele). Rein im Frontend aus
dem bereits geladenen Zustand abgeleitet, erzeugt also keine zusätzliche
Serveranfrage und keine neue Serverlogik.

## Spiel-Erkennung (Steam)

Abschnitt im Tab "Alltag". Mika merkt rein lesend über Steams eigene,
lokale Dateien (`libraryfolders.vdf`, `appmanifest_*.acf`, `registry.vdf`),
wenn ein neues Spiel gestartet wird, und macht dazu hin und wieder eine
kurze, beiläufige Bemerkung - genau nach demselben Muster wie die
Musik-Reaktion (Mindestabstand 20 Minuten zwischen zwei Reaktionen, keine
Reaktion auf das eigene gerade gesprochene Wort).

- **Kein Netzwerk, keine Steam-API, kein eigener Prozess**: es werden nur
  lokale VDF-Dateien gelesen, die Steam selbst anlegt und pflegt.
- Findet Steam nicht (keine der üblichen Installationspfade,
  Flatpak-Pfade eingeschlossen), bleibt die Funktion einfach inaktiv - die
  Checkbox ist dann ausgegraut und ein entsprechender Hinweis steht
  darunter.
- Einstellung: Checkbox "Reaktion auf gestartete Steam-Spiele
  einschalten" (Standard: an, sofern Steam gefunden wird). Zeigt auch
  die Anzahl der in der Bibliothek gefundenen Spiele an.
- Neues Modul `steam.py` (`bibliothek()`, `laufendes_spiel()`,
  `status()`), neue Funktion `bot.spiel_reaktion()`.

## System-Updates (Bazzite/rpm-ostree)

Abschnitt im Tab "Alltag", direkt unter der Spiel-Erkennung. Prüft
höchstens einmal am Tag rein lesend über `rpm-ostree upgrade --check`
(wendet nie selbst etwas an), ob ein System-Update bereitsteht, und
erwähnt das - pro Version höchstens einmal, nicht bei jedem Start erneut
- beiläufig im Chat.

- Nur relevant auf Systemen mit `rpm-ostree` (Bazzite, Fedora Silverblue/
  Kinoite & Co.) - ist das Kommando nicht vorhanden, bleibt die Checkbox
  ausgegraut mit entsprechendem Hinweis.
- Einstellung: Checkbox "Hinweis auf System-Updates einschalten"
  (Standard: aus - bewusst Opt-in, da es sich um einen Hinweis zum
  Handeln handelt, nicht nur eine beiläufige Bemerkung).
- Neues Modul `systemupdate.py` (`update_verfuegbar()`), neue Funktion
  `bot.update_hinweis()`.

## Mehrere Ollama-Modelle wählbar

Abschnitt "KI-Modell (Ollama)" im Tab "Persönlichkeit", oben. Zeigt ein
Dropdown mit allen lokal in Ollama vorhandenen Modellen (`ollama list`,
über Ollamas eigene `/api/tags`-Schnittstelle abgefragt) - Mika nutzt für
ihre Antworten dann das dort ausgewählte Modell statt immer nur das über
die Umgebungsvariable `KI_MODELL` fest eingestellte.

- **"Standard"** in der Liste verwendet weiterhin `KI_MODELL` - so lässt
  sich jederzeit zum eingerichteten Standardmodell zurückkehren, ohne den
  Namen nachschlagen zu müssen.
- Neue Modelle werden ganz normal mit `ollama pull <name>` installiert
  und erscheinen nach einem Klick auf "Liste aktualisieren" im Dropdown.
- Praktisch z. B. um zwischen einem kleinen, schnellen Modell für kurze
  Fragen und einem größeren für anspruchsvollere Gespräche zu wechseln,
  ohne die Umgebungsvariable zu ändern und den Dienst neu zu starten.
- Neue Funktion `bot.verfuegbare_modelle()`, Persona-Feld
  `ollama_modell` (leer = Standard), Endpunkte `/api/ollama_modelle`
  (GET, Liste + aktuelle Auswahl) und `/api/ollama_modell` (POST, Auswahl
  setzen).

## Überrasch-mich-Knopf

Button "Überrasch mich" im Tab "Persönlichkeit", direkt unter den
Musik-/Film-Vorschlägen. Auf Knopfdruck bringt Mika direkt in den Chat
eine von drei zufälligen Formen zur Auflockerung - welche es wird,
entscheidet der Zufall, nicht das Modell:

- ein kurzer, überraschender Fakt zu irgendeinem Thema,
- ein kurzer, harmloser Witz oder Wortwitz,
- eine locker gestellte Icebreaker-Frage zum kurzen Nachdenken oder als
  Gesprächseinstieg.

Neue Funktion `bot.ueberraschung()`, Endpunkt `/api/ueberraschung`
(POST).

## Rückblick

Abschnitt "Rückblick" im Tab "Alltag", direkt unter Mikas eigenem
Tagebuch. In größeren, selbst einstellbaren Abständen (Standard alle 30
Tage, nicht täglich) schreibt Mika einen etwas längeren, persönlichen
Rückblick aus ihrer eigenen Sicht - anders als das tägliche Tagebuch
stützt er sich zusätzlich auf den Stand der Beziehung (Stufe + Tage) und
zuletzt erledigte gemeinsame Ziele. Erscheint, wie das Tagebuch, nicht im
Chat, sondern nur in der eigenen Liste im Tab.

- Einstellungen: Checkbox "Rückblick automatisch erzeugen" (Standard:
  aus) + Tage-Feld fürs Intervall (1-365, Standard 30).
- **"Jetzt erzeugen"**-Button erzeugt sofort einen neuen Rückblick,
  unabhängig vom Intervall und auch wenn die automatische Erzeugung
  ausgeschaltet ist - solange schon mindestens einmal geschrieben wurde.
- Neue Funktion `bot.rueckblick_digest()`, neue Datei `digest.json`,
  Endpunkte `/api/digest` (POST, Einstellungen) und `/api/digest_jetzt`
  (POST, manueller Trigger).

## Stimmungsschwankung

Abschnitt im Tab "Persönlichkeit", direkt unter den anderen
Persönlichkeits-Checkboxen. Eine zufällige Grundstimmung, die - unabhängig
von Tageszeit (Tagesform) oder Gesprächsinhalt - mehrmals am Tag wechselt
und den Ton der Antworten leicht einfärbt:

- **neutral** (keine besondere Einfärbung, der normale Ton),
- **zurückhaltend** (kürzer, weniger von sich aus erzählen),
- **lebhaft** (redefreudiger, mehr Enthusiasmus und eigene Gedanken),
- **nachdenklich** (reflektierter, weniger spontan albern).

Der Wechsel passiert in einem zufälligen Abstand von 2 bis 6 Stunden -
nicht vorhersehbar, nicht an die Tageszeit gekoppelt, und es wird nie
direkt dieselbe Stimmung wiederholt. Mika bekommt dabei keine Erklärung
für den Stimmungswechsel mitgeliefert, sie soll einfach so klingen, ohne
es im Gespräch zu kommentieren ("Ich bin heute irgendwie zurückhaltend...").
Die aktuelle Grundstimmung wird (nur zur Information) unter der Checkbox
angezeigt.

- Einstellung: Checkbox "Zufällige Grundstimmung..." (Standard: an).
- Der Zustand (aktuelle Stimmung + Zeitpunkt des nächsten Wechsels) liegt
  in einer eigenen Datei (`stimmungsschwankung.json`) und bleibt daher
  auch über einen Neustart des Dienstes hinweg bestehen.
- Neue Funktionen `bot._stimmung_aktuell()` / `bot.stimmungsschwankung_text()`,
  Persona-Feld `stimmungsschwankung_aktiv`, mitgeliefert über
  `/api/persoenlichkeit` und `/api/state` (dort zusätzlich die aktuelle
  Stimmung selbst, rein zur Anzeige).

## XTTS v2 (dritte Sprachausgabe, optional, lokal)

Zusätzlich zu Piper (lokal) und ElevenLabs (Cloud) steht jetzt mit **XTTS
v2** eine dritte Sprachausgabe zur Wahl, im Tab "Sprachausgabe" über das
Dropdown. XTTS läuft komplett lokal (kein Internet nötig, keine laufenden
Kosten) und unterstützt Deutsch direkt - klingt dabei in der Betonung
spürbar natürlicher als Piper, braucht dafür aber deutlich mehr
Rechenleistung (mit GPU brauchbar schnell, auf reiner CPU eher langsam) und
lädt beim allerersten Gebrauch automatisch ein mehrere GB großes Modell
herunter.

- **Nicht Teil der normalen Installation** - dafür zusätzlich installieren.
  Wichtig: der Bot läuft über `./start.sh` in seiner eigenen venv
  (`~/ki-chatbot/venv`), nicht im normalen System-Python - deshalb muss
  genau dort installiert werden (sonst sieht der laufende Server die
  Pakete nicht, auch wenn `pip install` normal durchläuft):
  ```bash
  cd ~/ki-chatbot
  venv/bin/python -m pip install coqui-tts
  venv/bin/python -m pip install torch torchaudio torchcodec
  venv/bin/python -m pip install "transformers==5.0.0"
  ```
  - `torchcodec` wird ab PyTorch 2.9 zusätzlich für die Audio-Ausgabe
    gebraucht (ohne das Paket: Fehler "torchcodec library is required for
    audio IO").
  - Die `transformers`-Version ist wichtig: coqui-tts ist mit den
    allerneuesten `transformers`-Versionen (5.1 und höher) noch nicht
    kompatibel; 5.0.0 ist aktuell die bekannte funktionierende Version
    (Stand dieser README - das kann sich mit künftigen coqui-tts-Updates
    wieder ändern).
  - Ohne das Paket `coqui-tts` bleibt die Option einfach inaktiv (Status
    wird im Tab "Sprachausgabe" angezeigt) und Mika spricht weiter über
    Piper/ElevenLabs.
  - XTTS fragt beim allerersten Laden normalerweise interaktiv nach
    Zustimmung zur Modell-Lizenz (CPML, nicht-kommerziell) - das würde im
    Hintergrundbetrieb mit "EOF when reading a line" abbrechen. Mika
    überspringt diese Abfrage automatisch (siehe `xtts.py`); mit der
    Nutzung stimmst du der Lizenz damit faktisch zu.
- **Eigene Stimme (optional):** eine kurze WAV-Aufnahme (einige Sekunden
  reichen) als `daten/xtts_stimme.wav` ablegen - XTTS bildet daraus eine
  eigene, nachgebildete Stimme. Ohne eigene Datei wird eine der eingebauten
  Stimmen verwendet.
- **Automatischer Rückfall:** klappt die XTTS-Synthese nicht (Paket fehlt,
  Fehler beim ersten Laden des Modells o. Ä.), springt der Bot - genau wie
  bei ElevenLabs - auf die lokale Piper-Stimme zurück, falls sie
  eingerichtet ist.
- Neues Modul `xtts.py` (`verfuegbar()`, `synthetisiere()`, `status()`),
  Engine-Auswahl läuft über das bereits bestehende Persona-Feld
  `tts_engine` (jetzt `"piper"` | `"elevenlabs"` | `"xtts"`), gesetzt über
  den schon vorhandenen Endpunkt `/api/eleven_einstellungen`.

## Hugging-Face-Downloads

Im Tab "Internet" gibt es jetzt einen Abschnitt **Hugging-Face-Downloads**:
ein Link-Feld, in das man den Download-Link zu einer einzelnen Datei auf
[huggingface.co](https://huggingface.co/) einfügt, und ein Button
"Herunterladen". Das ist bewusst allgemein gehalten und nicht auf
XTTS-Stimmen beschränkt - es lädt irgendeine Datei aus einem
Hugging-Face-Repo herunter, die man selbst verlinkt (ein Modell, eine
Sprachprobe, eine beliebige andere Datei). Der Bot sucht oder lädt dabei
nie selbständig etwas nach - nur auf Zuruf, über einen Link, den man selbst
einfügt.

- **Nur Hugging Face:** erlaubt sind ausschließlich Links zu
  `huggingface.co`/`hf.co` (und deren eigene CDN-Unterdomains). Ein Link zu
  einer anderen Adresse wird abgelehnt. `/blob/`-Links (wie man sie beim
  Durchklicken auf der Seite bekommt) werden automatisch in den passenden
  `/resolve/`-Download-Link umgewandelt.
- **Größenlimit:** 2 GB pro Datei, sowohl anhand der vom Server angegebenen
  Größe als auch laufend während des Herunterladens geprüft - bei
  Überschreitung wird abgebrochen und die unvollständige Datei gelöscht.
- **Ablage:** heruntergeladene Dateien landen in einem eigenen Ordner
  (`daten/hf_downloads/`), nie direkt in einem Programmordner. Jeder
  Download erscheint in einer Liste mit Dateiname, Größe und Zeitpunkt, mit
  einem "Löschen"-Button dahinter.
- **Kurzweg zu XTTS:** ist die heruntergeladene Datei eine `.wav`-Datei,
  zeigt der Eintrag zusätzlich einen Button "Als XTTS-Stimme verwenden" -
  übernimmt die Datei direkt als `daten/xtts_stimme.wav` (siehe Abschnitt
  "XTTS v2" oben).
- Neues Modul `huggingface.py` (`herunterladen()`, `loeschen()`,
  `verlauf()`, `als_xtts_stimme()`), neue Endpunkte `/api/hf_download`,
  `/api/hf_download_loeschen`, `/api/hf_als_xtts_stimme`. Der Verlauf wird
  in `daten/hf_downloads.json` gespeichert (die letzten 50 Einträge).

## Fehlerbehebung: `profile.py` kollidierte mit Pythons eingebautem Modul

Mikas Profile-Verwaltung lag bisher in einer Datei namens `profile.py` -
direkt im Projektordner, der beim Start auf Pythons Modulsuchpfad landet.
Das überdeckte zufällig Pythons eigenes, eingebautes `profile`-Modul (Teil
der Standardbibliothek, von `cProfile` benutzt). Jedes Paket, das intern
`cProfile` lädt - z. B. `torch`/`transformers` bei der XTTS-Sprachsynthese
- fand dann versehentlich Mikas Datei statt des echten Moduls und brach mit
einer kryptischen Fehlermeldung ab (u. a. `Could not import module
'GenerationMixin'`).

Behoben durch Umbenennen in `nutzerprofile.py` (der Code dahinter ist
unverändert, nur `import nutzerprofile as profile` statt `import
profile`). Läufst du über eine bestehende Installation drüber, entfernt
das Setup-Skript die alte `profile.py` jetzt automatisch - dafür einfach
dieses Setup-Skript einmal erneut laufen lassen.

## Sanftes Verblassen alter Fakten

Im Tab "Gedächtnis" gibt es jetzt einen Schalter: **Alte, nur einmal
erwähnte Fakten mit der Zeit verblassen lassen** (standardmäßig an). Bisher
blieb jeder gemerkte Fakt für immer gespeichert, bis irgendwann das feste
Limit von 30 Fakten erreicht war - dann flog einfach der älteste raus,
unabhängig davon, ob er noch wichtig war.

Jetzt führt Mika im Hintergrund mit, wann ein Fakt zuletzt vorkam und wie
oft er insgesamt erwähnt wurde:

- Ein Fakt, der seit etwa **zwei Monaten (60 Tage)** nicht mehr erwähnt
  wurde **und** seit dem allerersten Mal nie wieder vorkam, verschwindet
  leise von selbst - echtes Vergessen für Kleinigkeiten, die offenbar
  nicht so wichtig waren.
- Ein Fakt, der **mehrfach** erwähnt wurde (auch nur ein zweites Mal),
  gilt als bestätigt und verblasst nicht automatisch, egal wie alt er ist.
- Die Prüfung läuft einmal täglich im Hintergrund, abschaltbar über den
  genannten Schalter. Mit dem Knopf "Alles vergessen" oder dem Löschen
  eines einzelnen Fakts (✕) wird der zugehörige Verlauf ebenfalls bereinigt.

Neue Funktionen in `bot.py` (`fakt_erwaehnt()`, `fakten_verblassen()`),
neue Datei `daten/fakten-verlauf.json` (wann ein Fakt zuletzt erwähnt
wurde und wie oft). Betrifft bisher nur das Hauptgedächtnis des
Chat-Tabs - die separaten Fakten-Listen von Discord-/Twitch-Anbindung
laufen weiterhin nach dem alten, starren Limit.

## Verschlüsselte Sicherung (AES-256)

Im Tab "Gedächtnis" gibt es beim Abschnitt "Sicherung (Export/Backup)"
jetzt zusätzlich die Checkbox **Mit Passwort verschlüsseln (AES-256)**.
Ohne sie funktioniert alles wie bisher (eine normale, unverschlüsselte
ZIP-Datei) - mit ihr wird die Sicherungsdatei zusätzlich verschlüsselt,
bevor sie herunterladen wird:

- Verfahren: **AES-256-GCM** (ein moderner, authentifizierter
  Verschlüsselungsmodus - eine manipulierte oder falsch entschlüsselte
  Datei wird erkannt, nicht nur "irgendwie" entschlüsselt).
- Der 256-Bit-Schlüssel wird aus dem eingegebenen Passwort abgeleitet
  (PBKDF2-HMAC-SHA256, 400.000 Runden, mit zufälligem Salt) - das Passwort
  selbst wird nirgends gespeichert, auch nicht auf dem Server.
- Jede Sicherung bekommt zufälliges Salt und Nonce, auch bei gleichem
  Passwort und Inhalt sieht die verschlüsselte Datei also jedes Mal
  anders aus.
- Die Datei heißt dann `ki-chatbot-sicherung.verschluesselt` statt
  `.zip` - sie lässt sich nicht mit einem normalen ZIP-Programm öffnen,
  nur über "Sicherung einspielen" in Mika selbst (mit demselben Passwort).
- **Das Passwort geht verloren = die Sicherung ist unwiederbringlich
  verloren.** Es gibt absichtlich keine Hintertür und keine
  Zurücksetzen-Funktion dafür.
- Das Passwort wird beim Herunterladen/Einspielen per HTTP-Header
  übertragen (nicht als Teil der URL), damit es nicht im Browser-Verlauf
  oder in Server-Logs landet.

Technisch: neue Datei-Signatur `MIKAENC1` am Anfang einer verschlüsselten
Sicherung; neue Funktionen in `sicherung.py` (`_verschluesseln()`,
`_entschluesseln()`, `_schluessel_ableiten()`); nutzt das Paket
`cryptography` (wird vom Setup-Skript automatisch in die venv installiert,
siehe `pip install cryptography`, falls es fehlt: `venv/bin/python -m pip
install cryptography`). Fehlt das Paket, läuft die unverschlüsselte
Sicherung weiterhin normal - nur die Checkbox bringt dann eine
Fehlermeldung statt einer verschlüsselten Datei.

## Versionsnummer + Hinweis auf neue Mika-Versionen

Mika kennt jetzt ihre eigene Versionsnummer (`bot.MIKA_VERSION`, aktuell
`1.9.2`) und zeigt sie im eigenen Tab **Updates** (gleich neben "Alltag")
unter "Mika-Version" an - dort liegt jetzt auch der bisherige Abschnitt
"System-Updates (Bazzite/rpm-ostree)". Ich erhöhe die Mika-Versionsnummer
bei jedem größeren Update, das ich dir schicke - so siehst du auf einen
Blick, welchen Stand du installiert hast.

Zusätzlich prüft Mika höchstens einmal am Tag rein lesend online, ob eine
neuere Version veröffentlicht wurde, und erwähnt das - pro Version
höchstens einmal - beiläufig im Chat (abschaltbar über den Schalter "Hinweis
auf neue Mika-Versionen"). Es gibt auch einen "Jetzt prüfen"-Knopf für die
sofortige, manuelle Abfrage. Mika installiert dabei nie selbst etwas - das
Setup-Skript musst du weiterhin selbst erneut ausführen.

Technisch: Geprüft wird gegen den **neuesten GitHub-Release** des Repos
`chincia84/Ai-Companion`, über die öffentliche GitHub-API
(`GET /repos/chincia84/Ai-Companion/releases/latest`, Feld `tag_name`) in
`mika_update.py`. Nach jedem neuen Update-Paket von mir muss in diesem
Repo ein neuer Release mit der passenden Versionsnummer als Tag angelegt
werden (z. B. `v1.9.3`), damit der Hinweis greift - ein führendes "v" im
Tag macht beim Vergleich nichts aus. Ohne Internet oder bei jedem Fehler
(Repo ohne Release, nicht erreichbar, privat) bleibt die Prüfung einfach
folgenlos, kein Absturz.
