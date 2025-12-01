# 🎵 Spotify Playlist Sync - n8n Workflow Guide

Eine umfassende Anleitung zum Erstellen eines **bidirektionalen Spotify Playlist Sync Workflows** mit n8n. Synchronisiere zwei Playlisten automatisch, ohne Duplikate und mit intelligenter Deduplication!

## 📖 Dokumentation

Die vollständige, interaktive Dokumentation findest du hier:

👉 **[Spotify Playlist Sync Guide](https://assassins88.github.io/spotify-playlist-sync/)**

## ⚡ Quick Start

### Was macht dieser Workflow?

- ✅ Synchronisiert zwei Spotify Playlisten **bidirektional**
- ✅ Verhindert **Duplikate** automatisch
- ✅ Filtert **lokale Tracks** (können nicht hochgeladen werden)
- ✅ Kann **manuell** oder per **Schedule** ausgelöst werden
- ✅ Funktioniert mit großen Playlisten (>100 Tracks)

### Anforderungen

- **n8n** (selbst-gehostet oder n8n.cloud)
- **Spotify Developer Account** (kostenlos)
- Spotify API Credentials (Client ID & Secret)
- Zwei Spotify Playlisten zum Synchronisieren

## 🚀 Setup in 5 Minuten

### 1. Spotify API Credentials besorgen
1. Gehe zu [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard)
2. Melde dich an oder registriere dich
3. Erstelle eine neue App
4. Kopiere **Client ID** und **Client Secret**
5. Registriere Redirect URI: `https://n8n.io/oauth2-credential/callback` (oder deine n8n Domain)

### 2. n8n Setup
1. Gehe zu **Credentials** in n8n
2. Erstelle neue **Spotify OAuth2** Credentials
3. Gib Client ID und Secret ein
4. Authentifiziere dich mit Spotify (mit deinem Account)
5. Stelle sicher, dass diese Scopes aktiv sind:
   - `playlist-modify-public`
   - `playlist-modify-private`
   - `playlist-read-private`

### 3. Playlist IDs finden
1. Öffne deine Playlist in Spotify
2. Klick auf "Teilen" → "Link kopieren"
3. Der Link sieht so aus: `https://open.spotify.com/playlist/3cEYpjA9oz9GiPac4AsH4n`
4. Die ID nach "playlist/" ist deine Playlist ID

### 4. Workflow bauen
Siehe die vollständige Anleitung mit Node-Konfiguration und Code-Templates: [Dokumentation öffnen](https://assassins88.github.io/spotify-playlist-sync/)

## 📋 Workflow Nodes

```
Trigger (Manual/Cron)
    ↓
├─→ Get Playlist 1 Tracks
├─→ Get Playlist 2 Tracks
    ↓
Merge Node (kombiniert beide)
    ↓
Code Node (vergleicht & filtert)
    ↓
├─→ Add Tracks to Playlist 1
└─→ Add Tracks to Playlist 2
```

## 💻 Code Node - Sync Logik

Der Code Node macht das Heavy Lifting:

```javascript
// Extrahiert URIs aus beiden Playlisten
// Vergleicht sie und findet Unterschiede
// Filtert lokale Tracks (spotify:local:...)
// Gibt Arrays mit hinzuzufügenden Tracks zurück

const tracksToAddToP1 = [...];  // Fehlend in P1
const tracksToAddToP2 = [...];  // Fehlend in P2
```

**Wichtig:** Lokale Tracks können NICHT via API hochgeladen werden. Sie werden automatisch gefiltert.

## ⚙️ Konfigurierbare Parameter

Im **Code Node**, passe diese Werte an deine Playlisten an:

```javascript
const split1 = 100;  // Anzahl Tracks in Playlist 1
const split2 = 150;  // Anzahl Tracks in Playlist 2
```

Diese Werte sagen dem Node, wo die erste Playlist endet und die zweite beginnt (nach Merge).

## 🎯 Best Practices

✅ **Do's:**
- Teste den Workflow zuerst mit kleinen Test-Playlisten
- Nutze die n8n Inspector-Seiteleiste zum Debuggen
- Aktiviere Logging mit `console.log()` in Code Nodes
- Beachte Spotify API Rate Limits (füge Wait Nodes zwischen Requests ein)
- Verwende Token Refresh (n8n macht das automatisch)

❌ **Don'ts:**
- Keine lokalen Tracks hochladen (werden automatisch gefiltert)
- Nicht zu häufig synchronisieren (beachte Rate Limits)
- Keine Credentials hardcoden
- Nicht alle Scopes entfernen

## 🐛 Troubleshooting

| Fehler | Lösung |
|--------|--------|
| **"Invalid Spotify ID"** | Playlist ID überprüfen (keine Query-Parameter) |
| **"Insufficient permissions"** | Credentials neu erstellen mit allen Scopes |
| **"Token expired"** | Credentials neu authentifizieren |
| **"Rate limit exceeded"** | Wait Node einfügen, Batch Size reduzieren |
| **"Empty playlist returned"** | "Get all" Checkbox im Spotify Node aktivieren |

Siehe die vollständige Dokumentation für mehr Details: [Troubleshooting Guide](https://assassins88.github.io/spotify-playlist-sync/#troubleshooting)

## 📊 API Limits (wichtig!)

| Limit | Wert |
|-------|------|
| Max Tracks pro Request | 100 URIs |
| Max Items pro Abruf | 50 (Standard) |
| Rate Limit | 429 Too Many Requests |
| Token Gültigkeit | 1 Stunde |

## 🔗 Nützliche Links

- [Spotify API Docs](https://developer.spotify.com/documentation/web-api)
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Spotify Integration](https://docs.n8n.io/integrations/builtInNodes/app-nodes/n8n-nodes-base.spotify/)
- [OAuth2 Guide](https://developer.spotify.com/documentation/general/guides/authorization/)

## 📝 Lizenz & Hinweise

Dieses Projekt ist ein Tutorial für n8n und Spotify API. 

**Wichtig:**
- Respektiere die Spotify API Nutzungsbedingungen
- Nutze deine Credentials sicher und teile sie nicht
- Beachte Rate Limits um Blacklisting zu vermeiden

## 🤝 Beiträge

Fehler gefunden? Verbesserungsvorschläge?
- GitHub Issues öffnen
- Pull Requests sind willkommen
- Feedback erwünscht!

---

**Made with ❤️ für Spotify + n8n Enthusiasten** 🎵

Viel Erfolg mit deinem Playlist Sync Workflow!