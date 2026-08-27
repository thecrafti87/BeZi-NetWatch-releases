# NetWatch

**Netzwerküberwachung für Live-Events.**

NetWatch läuft auf einem Laptop im Veranstaltungsnetz und zeigt jederzeit,
ob die Technik erreichbar ist — und wo es klemmt, wenn nicht. Es gibt zwei
Ansichten: eine für Kundschaft und Helfende, die nur Ampelfarben und
Klartext zeigt, und eine für die Technik mit allen Rohdaten.

Gebaut für Wrestling-Shows, Messen und Firmenveranstaltungen, also für
Situationen, in denen niemand Zeit hat, in einem Switch-Menü zu suchen,
während der Stream läuft.

Aktuelle Fassung: **1.12.1**

> Dies ist die Download-Seite. Der Quellcode ist nicht öffentlich.

---

## Was es kann

**Überwachung**
- Erreichbarkeit, Latenz, Jitter und Paketverlust je Gerät
- Ampelstatus in Grün, Gelb und Rot mit einstellbaren Schwellen —
  wahlweise global oder je Gerät, denn eine WLAN-Kamera darf langsamer
  sein als der Encoder
- Prüfabstand je Gerät: Nebensächliches muss nicht im selben Takt laufen
- Sofortige Aktualisierung ohne Neuladen der Seite
- Ton- und Browser-Benachrichtigung, wenn ein Gerät ausfällt
- Abhängigkeiten zwischen Geräten: fällt der Switch aus, wird nicht alles
  dahinter als Einzelstörung gemeldet
- Protokoll aller Statusänderungen mit Zeitstempel für die Nachbereitung

**Vor der Veranstaltung**
- **Startprüfung:** Ein Knopf prüft in einem Durchgang Erreichbarkeit,
  Leitungsgeschwindigkeit zum Router, Upload, Dienste, Switch-Auslastung,
  ob Alarme tatsächlich ankommen und ob die Technik-Ansicht geschützt ist —
  mit klarem Urteil und einem Hinweis, was zu tun ist

**Fehlersuche**
- **Engpass-Analyse** — findet die langsamste Stelle im Weg zu einem Gerät.
  Ein einzelner 100-Mbit-Port bremst die ganze Verbindung aus und fällt
  sonst nur als „das Internet ist langsam" auf. Verbraucht praktisch keine
  Bandbreite und ist im laufenden Betrieb unbedenklich.
- **Routenverfolgung** mit Latenz und Paketverlust je Zwischenstation
- **Internet-Geschwindigkeitstest** für Download, Upload und Ping
- **Bandbreitenanzeige** je Netzwerkschnittstelle in Echtzeit
- **Netzwerk-Suchlauf** findet Geräte im Netz und erkennt ihren Typ

**Erfahrung aus früheren Veranstaltungen**
- **Über Sitzungen hinweg:** NetWatch merkt sich, was für jedes Gerät in
  jedem Netz normal ist, und zeigt, wie die heutigen Werte dazu stehen.
  Aus „40 ms“ wird so „40 ms, wo sonst 8 stehen“
- Getrennt nach Netz, weil dasselbe Gerät in Halle A am Kabel und in
  Halle B im WLAN hängen kann

**Nachbereitung**
- **Verlauf** zeigt für jedes Gerät als Zeitbalken, wann es erreichbar war
  und wann nicht — zoombar bis auf die Sekunde
- **Bericht als PDF** mit Zeitbalken je Gerät: zeigt nicht nur *dass*,
  sondern *wann* etwas ausgefallen ist
- **Bericht zum Herunterladen** in Klartext, direkt an Auftraggeber
  weiterzugeben: Verfügbarkeit je Gerät, Unterbrechungen mit Dauer. Kurze
  Aussetzer werden getrennt ausgewiesen, damit ein gesundes Netz nicht wie
  ein kaputtes aussieht

**Benachrichtigung nach außen**
- **Telegram**, **E-Mail** (eigener SMTP-Server) und **Webhook**
  (Slack, Discord, Microsoft Teams oder
  allgemeines JSON) melden Störungen dorthin, wo Sie ohnehin hinschauen
- **Lebenszeichen:** NetWatch meldet in Abständen, dass es noch läuft.
  Bleibt die Meldung aus, weiß man, dass die Überwachung selbst steht —
  sonst zeigt jede offene Seite weiter den letzten Stand
- **Quittieren:** Ein bekanntes Problem lässt sich für eine Weile stumm
  schalten, ohne die Überwachung abzuschalten
- Gemeldet wird erst, wenn eine Störung eine einstellbare Zeit anhält —
  kurze Aussetzer lösen nichts aus, damit die Meldungen ernst genommen werden
- Sperrzeit gegen wiederholte Meldungen, auf Wunsch Entwarnung bei Erholung

**Veranstaltungstechnik**
- Switches über SNMP: Portstatus und **tatsächliche Auslastung je Port**
- ArtNet und sACN: aktive DMX-Universen und deren Sender
- Dante und NDI: Geräte- und Quellenerkennung
- PTP: Grandmaster und Synchronisierung
- Port- und Dienstprüfungen für HTTP, HTTPS, RTMP, SRT, RTSP und NDI

**Zugangsschutz**
- Die Techniker-Ansicht lässt sich mit einer PIN sichern — wichtig in
  fremden Veranstaltungsnetzen, wo jeder im WLAN auch den Laptop erreicht
- Die Kundenansicht bleibt ohne Anmeldung offen, zeigt dann aber nur
  Ampelfarben und Klartext: keine Adressen, keine Netzstruktur
- Die PIN wird nur als Prüfsumme gespeichert und landet nicht in Profilen
- Auf Wunsch läuft die Techniker-Ansicht verschlüsselt über HTTPS, während
  die Kundenansicht ohne Zertifikatswarnung erreichbar bleibt

**Handhabung**
- **Gerätegruppen** wie „Bühne", „Regie" oder „Publikum-WLAN": Die
  Kundenansicht zeigt je Gruppe eine eigene Ampel, Störungen stehen oben.
  In der Technik lässt sich die Geräteliste nach Gruppe filtern
- Event-Profile: Gerätelisten je Veranstaltungsort speichern und laden
- Mehrere Geräte auf einmal auswählen und entfernen — praktisch, wenn nach
  einem Ortswechsel die alte Geräteliste im Weg steht
- Heller und dunkler Anzeigemodus
- Läuft im Browser, also auch auf Tablet und Telefon im selben Netz

---

## Installation

1. Die ZIP-Datei unten unter *Assets* herunterladen.
2. In einen Ordner eigener Wahl entpacken, etwa `C:\NetWatch`.
3. `NetWatch.exe` starten.
4. Der Browser öffnet sich auf `http://localhost:5000`.

Im selben Netz erreichen andere Geräte die Ansichten über die IP-Adresse
des Laptops, also etwa `http://192.168.1.50:5000` für die Kundenansicht und
`http://192.168.1.50:5000/tech` für die Technik.

### Hinweis zu SmartScreen

Beim ersten Start meldet Windows möglicherweise „Der Computer wurde durch
Windows geschützt". Das liegt daran, dass das Programm nicht mit einem
kostenpflichtigen Zertifikat signiert ist, und ist kein Hinweis auf
Schadsoftware. Über *Weitere Informationen* und *Trotzdem ausführen* lässt
sich der Start fortsetzen.

### Voraussetzungen

- Windows 10 oder 11, 64 Bit
- Rund 250 MB Platz auf der Festplatte
- Keine Administratorrechte nötig
- Keine Installation von Python erforderlich

---

## Aktualisierung

NetWatch sieht beim Start im Hintergrund nach, ob eine neuere Fassung
vorliegt. Ist das der Fall, erscheint in der Techniker-Ansicht ein Hinweis
mit der Möglichkeit, sie zu laden und einzuspielen. Für die Installation
startet NetWatch einmal neu und fragt vorher nach — mitten in einer
Veranstaltung soll nichts unangekündigt stoppen.

**Ihre Daten bleiben erhalten.** Konfiguration, Event-Profile und
Protokolle werden bei einer Aktualisierung nicht angefasst.

### Datenschutz

Nach außen geht ausschließlich die Frage, welche Fassung die neueste ist.
Diese Abfrage geht an GitHub und überträgt keine Angaben über Sie, Ihr Netz
oder Ihre Geräte. NetWatch sendet keine Nutzungsdaten, keine Messwerte und
keine Gerätelisten. Alles, was NetWatch misst, bleibt auf Ihrem Rechner.

Wer die Abfrage nicht möchte, kann den Rechner ohne Internetzugang
betreiben — NetWatch arbeitet vollständig weiter und meldet den
fehlgeschlagenen Abruf nicht als Fehler.

---

## Erwerb und Nutzung

NetWatch lässt sich 14 Tage lang vollständig testen. Danach ist eine Lizenz
erforderlich.

Anfragen an **BeZi-Film — Benjamin Ziemann**.

Die Software wird ohne Gewähr bereitgestellt. Sie ist ein Hilfsmittel zur
Überwachung und ersetzt weder eine fachgerechte Netzplanung noch die
Kontrolle durch die verantwortliche Technik.

Die mitgelieferten Fremdkomponenten und ihre Lizenzen sind in
`THIRD-PARTY-LICENSES.md` im Programmordner aufgeführt.

---

## Was noch fehlt

Ehrlichkeitshalber, damit niemand vergeblich sucht:

- **Keine Bandbreite je Gerät.** NetWatch kann nicht sagen, welches Gerät
  gerade wie viel Datenverkehr erzeugt. Dafür wäre eine Auswertung am
  Router oder ein gespiegelter Switch-Port nötig. Die Port-Auslastung über
  SNMP kommt dem am nächsten und zeigt, welche Strecke dichtmacht.
- **Kein Windows-Installer.** Die Auslieferung erfolgt als ZIP zum
  Entpacken.
- **Keine Signatur.** Daher die SmartScreen-Meldung oben.
- **Nur Windows.** Der Code läuft grundsätzlich auch anderswo, es gibt aber
  keine fertigen Pakete für macOS oder Linux.
- **Keine Weiterleitung per SMS.** E-Mail, Telegram und Webhook sind
  enthalten.
