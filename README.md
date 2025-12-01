# 🎵 Spotify Playlist Sync - n8n Workflow Guide

> **[English Version Below](#-spotify-playlist-sync---n8n-workflow-guide-english)**

Eine umfassende Anleitung zum Erstellen eines **bidirektionalen Spotify Playlist Sync Workflows** mit n8n. Synchronisiere zwei Playlisten automatisch, ohne Duplikate und mit intelligenter Deduplication!

## 📖 Dokumentation

Die vollständige, interaktive Dokumentation findest du hier:

👉 **[Spotify Playlist Sync Guide](https://assassins88.github.io/spotify-playlist-sync/)**

## 🚀 One-Click Import (n8n)

Importiere den vorbereiteten Workflow direkt in deine n8n Instanz:

### Option 1: Import URL
Kopiere diese URL in n8n (Workflows → Import from URL):
```
https://raw.githubusercontent.com/Assassins88/spotify-playlist-sync/main/spotify-playlist-sync.workflow.json
```

### Option 2: Direkter Import
[![Import to n8n](https://img.shields.io/badge/Import%20to-n8n-red?style=for-the-badge)](https://app.n8n.io/workflows?import=https://raw.githubusercontent.com/Assassins88/spotify-playlist-sync/main/spotify-playlist-sync.workflow.json)

oder öffne diese URL direkt:
```
https://app.n8n.io/workflows?import=https://raw.githubusercontent.com/Assassins88/spotify-playlist-sync/main/spotify-playlist-sync.workflow.json
```

**Nach dem Import:**
1. Ersetze `YOUR_SPOTIFY_CREDENTIAL_ID` mit deinen echten Spotify Credentials
2. Ersetze `YOUR_PLAYLIST_1_ID` und `YOUR_PLAYLIST_2_ID` mit deinen Playlist IDs
3. Passe die Track-Anzahl im Code Node an (split1 & split2)
4. Teste und deploye! 🎉

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

## 🔧 Setup in 5 Minuten

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

### 4. Workflow importieren & konfigurieren
Siehe oben "One-Click Import" - danach nur noch IDs anpassen!

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

## 📂 Repository Struktur

```
spotify-playlist-sync/
├── index.html                          # Interaktive Dokumentation
├── spotify-playlist-sync.workflow.json  # n8n Workflow (importierbar)
├── README.md                           # Diese Datei
└── LICENSE                             # MIT License
```

## 🔗 Nützliche Links

- [Spotify API Docs](https://developer.spotify.com/documentation/web-api)
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Spotify Integration](https://docs.n8n.io/integrations/builtInNodes/app-nodes/n8n-nodes-base.spotify/)
- [OAuth2 Guide](https://developer.spotify.com/documentation/general/guides/authorization/)
- [n8n Community](https://community.n8n.io/)

## 📝 Lizenz & Hinweise

Dieses Projekt ist ein Tutorial für n8n und Spotify API. 

**Wichtig:**
- Respektiere die Spotify API Nutzungsbedingungen
- Nutze deine Credentials sicher und teile sie nicht
- Beachte Rate Limits um Blacklisting zu vermeiden
- Der exportierte Workflow enthält keine echten Credentials (nur Platzhalter)

## 🤝 Beiträge

Fehler gefunden? Verbesserungsvorschläge?
- [GitHub Issues öffnen](https://github.com/Assassins88/spotify-playlist-sync/issues)
- [Pull Requests sind willkommen](https://github.com/Assassins88/spotify-playlist-sync/pulls)
- Feedback erwünscht!

---

**Made with ❤️ für Spotify + n8n Enthusiasten** 🎵

Viel Erfolg mit deinem Playlist Sync Workflow!

---

# 🎵 Spotify Playlist Sync - n8n Workflow Guide (English)

A comprehensive guide to creating a **bidirectional Spotify Playlist Sync workflow** with n8n. Synchronize two playlists automatically, without duplicates and with intelligent deduplication!

## 📖 Documentation

Find the complete, interactive documentation here:

👉 **[Spotify Playlist Sync Guide](https://assassins88.github.io/spotify-playlist-sync/)**

## 🚀 One-Click Import (n8n)

Import the prepared workflow directly into your n8n instance:

### Option 1: Import URL
Copy this URL in n8n (Workflows → Import from URL):
```
https://raw.githubusercontent.com/Assassins88/spotify-playlist-sync/main/spotify-playlist-sync.workflow.json
```

### Option 2: Direct Import
[![Import to n8n](https://img.shields.io/badge/Import%20to-n8n-red?style=for-the-badge)](https://app.n8n.io/workflows?import=https://raw.githubusercontent.com/Assassins88/spotify-playlist-sync/main/spotify-playlist-sync.workflow.json)

or open this URL directly:
```
https://app.n8n.io/workflows?import=https://raw.githubusercontent.com/Assassins88/spotify-playlist-sync/main/spotify-playlist-sync.workflow.json
```

**After Import:**
1. Replace `YOUR_SPOTIFY_CREDENTIAL_ID` with your actual Spotify Credentials
2. Replace `YOUR_PLAYLIST_1_ID` and `YOUR_PLAYLIST_2_ID` with your playlist IDs
3. Adjust the track count in the Code Node (split1 & split2)
4. Test and deploy! 🎉

## ⚡ Quick Start

### What does this workflow do?

- ✅ Synchronizes two Spotify playlists **bidirectionally**
- ✅ Prevents **duplicates** automatically
- ✅ Filters **local tracks** (cannot be uploaded)
- ✅ Can be triggered **manually** or via **schedule**
- ✅ Works with large playlists (>100 tracks)

### Requirements

- **n8n** (self-hosted or n8n.cloud)
- **Spotify Developer Account** (free)
- Spotify API Credentials (Client ID & Secret)
- Two Spotify playlists to synchronize

## 🔧 Setup in 5 Minutes

### 1. Get Spotify API Credentials
1. Go to [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard)
2. Sign in or create an account
3. Create a new app
4. Copy **Client ID** and **Client Secret**
5. Register Redirect URI: `https://n8n.io/oauth2-credential/callback` (or your n8n domain)

### 2. n8n Setup
1. Go to **Credentials** in n8n
2. Create new **Spotify OAuth2** Credentials
3. Enter Client ID and Secret
4. Authenticate with Spotify (using your account)
5. Make sure these scopes are active:
   - `playlist-modify-public`
   - `playlist-modify-private`
   - `playlist-read-private`

### 3. Find Playlist IDs
1. Open your playlist in Spotify
2. Click "Share" → "Copy Link"
3. The link looks like: `https://open.spotify.com/playlist/3cEYpjA9oz9GiPac4AsH4n`
4. The ID after "playlist/" is your playlist ID

### 4. Import & Configure Workflow
See "One-Click Import" above - then just adjust the IDs!

## 📋 Workflow Nodes

```
Trigger (Manual/Cron)
    ↓
├─→ Get Playlist 1 Tracks
├─→ Get Playlist 2 Tracks
    ↓
Merge Node (combines both)
    ↓
Code Node (compares & filters)
    ↓
├─→ Add Tracks to Playlist 1
└─→ Add Tracks to Playlist 2
```

## 💻 Code Node - Sync Logic

The Code Node does the heavy lifting:

```javascript
// Extracts URIs from both playlists
// Compares them and finds differences
// Filters out local tracks (spotify:local:...)
// Returns arrays with tracks to add

const tracksToAddToP1 = [...];  // Missing in P1
const tracksToAddToP2 = [...];  // Missing in P2
```

**Important:** Local tracks CANNOT be uploaded via API. They are automatically filtered.

## ⚙️ Configurable Parameters

In the **Code Node**, adjust these values for your playlists:

```javascript
const split1 = 100;  // Number of tracks in Playlist 1
const split2 = 150;  // Number of tracks in Playlist 2
```

These values tell the node where the first playlist ends and the second begins (after merge).

## 🎯 Best Practices

✅ **Do's:**
- Test the workflow first with small test playlists
- Use the n8n Inspector sidebar for debugging
- Enable logging with `console.log()` in Code Nodes
- Respect Spotify API Rate Limits (add Wait Nodes between requests)
- Use Token Refresh (n8n handles this automatically)

❌ **Don'ts:**
- Don't upload local tracks (automatically filtered)
- Don't synchronize too frequently (respect rate limits)
- Don't hardcode credentials
- Don't remove all scopes

## 🐛 Troubleshooting

| Error | Solution |
|-------|----------|
| **"Invalid Spotify ID"** | Check playlist ID (no query parameters) |
| **"Insufficient permissions"** | Recreate credentials with all scopes |
| **"Token expired"** | Re-authenticate credentials |
| **"Rate limit exceeded"** | Add Wait Node, reduce batch size |
| **"Empty playlist returned"** | Check "Get all" checkbox in Spotify Node |

See the full documentation for more details: [Troubleshooting Guide](https://assassins88.github.io/spotify-playlist-sync/#troubleshooting)

## 📊 API Limits (Important!)

| Limit | Value |
|-------|-------|
| Max Tracks per Request | 100 URIs |
| Max Items per Retrieval | 50 (Standard) |
| Rate Limit | 429 Too Many Requests |
| Token Validity | 1 Hour |

## 📂 Repository Structure

```
spotify-playlist-sync/
├── index.html                          # Interactive Documentation
├── spotify-playlist-sync.workflow.json  # n8n Workflow (importable)
├── README.md                           # This File
└── LICENSE                             # MIT License
```

## 🔗 Useful Links

- [Spotify API Docs](https://developer.spotify.com/documentation/web-api)
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Spotify Integration](https://docs.n8n.io/integrations/builtInNodes/app-nodes/n8n-nodes-base.spotify/)
- [OAuth2 Guide](https://developer.spotify.com/documentation/general/guides/authorization/)
- [n8n Community](https://community.n8n.io/)

## 📝 License & Notes

This project is a tutorial for n8n and Spotify API.

**Important:**
- Respect Spotify API Terms of Service
- Keep your credentials safe and don't share them
- Respect Rate Limits to avoid blacklisting
- The exported workflow contains no real credentials (only placeholders)

## 🤝 Contributions

Found a bug? Have suggestions?
- [Open GitHub Issues](https://github.com/Assassins88/spotify-playlist-sync/issues)
- [Pull Requests Welcome](https://github.com/Assassins88/spotify-playlist-sync/pulls)
- Feedback appreciated!

---

**Made with ❤️ for Spotify + n8n Enthusiasts** 🎵

Good luck with your Playlist Sync Workflow!