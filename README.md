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
  Weckwort-Modus); optional ElevenLabs als Cloud-Stimme.
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
- **Musik-Reaktion**: merkt über PulseAudio/PipeWire (`pactl`, rein lokal/
  lesend), wenn auf dem Rechner Musik oder Ton anfängt zu laufen, und macht
  dann hin und wieder eine kurze, beiläufige Bemerkung dazu - mit Mindest-
  abstand zwischen den Reaktionen, ein-/ausschaltbar im Begleiter-Tab. Ohne
  `pactl` bleibt die Funktion einfach inaktiv.
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
| `KI_MODELL` | Ollama-Modell | `qwen2.5:7b` |
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
- Das Standardmodell (`qwen2.5:7b`) ist klein und läuft lokal auf normaler
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



- Gehirn tauschen: `bot.py` → Funktion `antworte()`
- Avatar-Verhalten: `static/index.html` → `avatarReagiere()`, `avatarMund()`
