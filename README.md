# Podcasts

Eine schlichte, werbefreie Podcast-App als PWA. Eine einzige `index.html`, kein Build, keine Abhängigkeiten, kein Account.

## Funktionen

- Podcasts über die iTunes-Suche finden oder direkt eine RSS-Feed-URL einfügen
- Abos, „Neu“-Liste über alle Abos, Folgenbeschreibung per Antippen
- Fortschritt pro Folge wird gespeichert, Wiedergabe setzt dort fort
- Warteschlange: **läuft nie etwas automatisch weiter, das du nicht selbst eingereiht hast**
  - zeigt das Erscheinungsdatum jeder Folge
  - Umsortieren jederzeit per Ziehen am ≡-Griff, „⤒“ setzt eine Folge direkt an die erste Stelle, „Älteste zuerst“ sortiert nach Datum
- Pro Folge (Titel antippen): als gehört/ungehört markieren, „Zurücksetzen & neu laden“ (setzt den Fortschritt zurück und lädt die Datei beim nächsten Abspielen frisch vom Server), Länge laut Feed vs. tatsächliche Länge
- −15 s / +30 s, Geschwindigkeit, Sleep-Timer
- Sperrbildschirm-Steuerung (Media Session API)
- Backup als JSON (Export/Import)

## Online stellen (GitHub Pages)

1. Repo auf GitHub → **Settings → Pages**
2. Source: **Deploy from a branch**, Branch `main`, Ordner `/ (root)`
3. Nach ca. einer Minute erreichbar unter `https://<benutzer>.github.io/<repo>/`
4. Auf dem Handy öffnen und „Zum Home-Bildschirm hinzufügen“ wählen

Lokal testen: `python3 -m http.server 8000` im Ordner und `http://localhost:8000` öffnen.

## CORS und Proxy

Manche Feeds darf der Browser nicht direkt laden (fehlende CORS-Header). Dann fragt die App einen Proxy an.
Standard ist ein öffentlicher Dienst (`allorigins.win`). Der sieht, welche Feeds du abonnierst.
Wer das nicht möchte, trägt unter **Mehr → CORS-Proxy** einen eigenen Proxy ein (z. B. auf dem Heimserver)
oder leert das Feld. Audiodateien brauchen keinen Proxy, die lädt der Browser direkt.

Ein eigener Proxy ist nur ein Endpunkt, der `?url=<feed>` entgegennimmt, die Seite abruft und mit
`Access-Control-Allow-Origin: *` zurückgibt.

## Bekannte Grenzen

- Daten liegen in `localStorage` (nur dieser Browser/dieses Gerät, regelmäßig Backup exportieren)
- Hintergrundwiedergabe auf iOS ist bei Web-Apps nicht immer zuverlässig
- Keine Offline-Downloads von Folgen
- Feeds werden beim Öffnen der App aktualisiert (höchstens alle 30 Minuten)
