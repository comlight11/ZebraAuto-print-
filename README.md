# ZebraAuto-print-
Zebra Auto print


# Zebra AutoPrint v2.7.0

## C# (.NET 8 WinForms) - Productie Release

Automatisch QR Code labels printen voor productie CSV-bestanden.

---

## 🚀 Quick Start

### Optie 1: Alleen EXE (aanbevolen voor testen)
```batch
BUILD.bat
```
Output: `publish\ZebraAutoPrint.exe` (72MB single-file)

### Optie 2: Met MSI Installer
```batch
BUILD_INSTALLER.bat
```
Vereist: WiX Toolset v4 (`dotnet tool install -g wix`)

### Optie 3: Inno Setup Installer
1. Installeer [Inno Setup](https://jrsoftware.org/isinfo.php)
2. Run eerst `BUILD.bat`
3. Open `ZebraAutoPrint.iss` in Inno Setup
4. Compileer (Ctrl+F9)
5. Output: `Output\ZebraAutoPrint_Setup_v2.7.0.exe`

---

## 📁 Project Structuur

```
ZebraAutoPrint_WinForms/
├── src/
│   ├── ZebraAutoPrint/               # Main applicatie
│   │   ├── Forms/                    # UI Forms
│   │   │   ├── MainForm.cs
│   │   │   ├── AdminLoginForm.cs     # Beheerder login
│   │   │   ├── AboutForm.cs
│   │   │   ├── SettingsForm.cs
│   │   │   ├── PrintPreviewForm.cs
│   │   │   ├── ReprintForm.cs
│   │   │   ├── ScanBufferForm.cs
│   │   │   ├── LabelSizeForm.cs      # Label formaten + test print
│   │   │   ├── DataMatrixSizeForm.cs
│   │   │   ├── ZplViewerForm.cs      # ZPL code viewer
│   │   │   ├── NetworkPrinterForm.cs
│   │   │   └── DuplicateWarningForm.cs
│   │   ├── Services/                 # Business logic
│   │   │   ├── ConfigService.cs
│   │   │   ├── DatabaseService.cs
│   │   │   ├── PrintService.cs
│   │   │   ├── FolderWatcherService.cs
│   │   │   ├── ScannerInputService.cs
│   │   │   ├── ArchiveService.cs
│   │   │   ├── LanguageService.cs    # UI vertalingen
│   │   │   └── DebugService.cs
│   │   ├── Helpers/
│   │   │   ├── FilenameParser.cs     # OFPNC + Mantel parsing
│   │   │   └── ZplGenerator.cs       # ZPL generatie
│   │   └── Resources/
│   │       ├── Icons/app.ico
│   │       └── logo.png              # Photonis Defense logo
│   │
│   └── ZebraAutoPrint.Installer/     # WiX MSI Installer
│       ├── Package.wxs
│       └── Localization/
│           ├── nl-NL.wxl             # Nederlands
│           └── en-US.wxl             # English
│
├── BUILD.bat                         # Build standalone EXE
├── BUILD_INSTALLER.bat               # Build EXE + MSI
├── ZebraAutoPrint.iss                # Inno Setup script
└── ZebraAutoPrint.sln
```

---

## ✨ Features

### Label Formaten (Automatisch)
| Formaat | Afmeting | Bestandspatroon | ZPL |
|---------|----------|-----------------|-----|
| **OFPNC** | 102×25mm | `OFPNC..._Oven..._Plank..._timestamp.csv` | ^PW1200 ^LL300 |
| **Mantel** | 50×25mm | `mantel_..._dd-mm-yyThh-mm.csv` | ^PW598 ^LL598 |

- ✅ Automatische detectie op basis van bestandsnaam
- ✅ Test print per formaat (via Label Formaten menu)
- ✅ ZPL Code Viewer (bekijk exacte ZPL code)

### QR-Code
- ✅ **Bevat volledig bestandspad** (bijv. `C:\InProcess\OFPNC...csv`)
- ✅ Traceerbaarheid: scan QR → direct naar bestand
- ✅ Tekst onder QR: korte naam zonder timestamp
- ✅ Preview toont exact 1-op-1 wat naar printer gaat

### Visuele Status Indicatie
| Status | Header | Knop | Statusbalk |
|--------|--------|------|------------|
| **Gestopt** | 🔴 Rode rand | [▶ Start] groen | 🔴 GESTOPT |
| **Actief** | 🟢 Groene rand | [🟢 Actief] / [⏹ Stop] | 🟢 ACTIEF |

- ✅ Header rand verandert van kleur (rood/groen)
- ✅ Start knop transformeert naar Stop (alleen na admin login)
- ✅ Niet ingelogd: knop toont "🟢 Actief" (alleen status, geen stop)

### Start Geminimaliseerd
- ✅ Instelling: "Start geminimaliseerd naar systeemvak"
- ✅ App start direct naar system tray (naast klok)
- ✅ Monitoring start automatisch (indien AutoStart aan)
- ✅ Dubbelklik op tray icon → venster openen

### Beveiliging
- ✅ Beheerder login systeem
- ✅ Instellingen alleen na inloggen
- ✅ Stop functie alleen voor beheerders
- ✅ Wachtwoord reset via `reset.txt`
- ✅ Debug mode alleen voor beheerders

### Taal
- ✅ Nederlands en Engels
- ✅ Taal selecteerbaar in Instellingen
- ✅ Backend/logs altijd in Engels

### Data Folder
- ✅ Instelbaar bij installatie
- ✅ Via `datapath.txt` naast exe
- ✅ Via environment variable `ZEBRAAUTOPRINT_DATA`
- ✅ Standaard: `%LOCALAPPDATA%\ZebraAutoPrint`

### Workflow
- ✅ CSV folder monitoring (FileSystemWatcher)
- ✅ File stability check (wacht tot schrijven klaar)
- ✅ OFPNC + Mantel bestandsnaam parsing
- ✅ QR Code label generatie (ZPL)
- ✅ Zebra printer support (USB + TCP/IP 9100)
- ✅ 3-map workflow: IMES → In Process → Archief
- ✅ 2 minuten archief delay

### Scanner
- ✅ DS2208 / USB HID keyboard hook
- ✅ Duplicaat detectie (SQLite buffer)
- ✅ Popup waarschuwing bij duplicaat
- ✅ Herkent streepjes (-) EN underscores (_)
- ✅ Case-insensitive matching

### Database
- ✅ SQLite met WAL mode
- ✅ Thread-safe met lock
- ✅ Laatste 50 OFPNC codes opslaan
- ✅ Laatste 50 print jobs opslaan
- ✅ Herprint functie

### UI
- ✅ Photonis Defense branding (logo + paarse kleur)
- ✅ System tray icon met status
- ✅ Minimize naar tray
- ✅ Print preview (1-op-1 met ZPL)
- ✅ ZPL Code Viewer
- ✅ Label formaten dialog (met test print)
- ✅ QR Code grootte dialog
- ✅ Netwerk printer scanner
- ✅ About/Help met contactinfo

### File Operations
- ✅ 5 COPY methodes (File.Copy, ReadWrite, cmd, PowerShell, FileStream)
- ✅ 5 DELETE methodes (direct, attributes, cmd, PowerShell, rename)
- ✅ Retry met backoff
- ✅ Crash-proof cleanup
- ✅ Access Denied handling

### Installer
- ✅ Meertalig (NL/EN)
- ✅ Per-user install (geen admin)
- ✅ Desktop/Startmenu shortcuts
- ✅ Auto-start optie
- ✅ Custom data folder optie
- ✅ Clean uninstall

---

## 🖥️ Hoofdvenster

### Niet ingelogd:
```
┌─────────────────────────────────────────────────────────────────┐
│  Zebra AutoPrint                        [PHOTONIS Defense]      │
│  🔴 Gestopt                                                     │
├═════════════════════════════════════════════════════════════════┤ ← RODE rand
│  [▶ Start]  [🔍 Preview]  [🔄 Herdruk]                          │
├─────────────────────────────────────────────────────────────────┤
│  ... log berichten ...                                          │
├─────────────────────────────────────────────────────────────────┤
│ 🔴 GESTOPT │ 📁 C:\IMES │ 📷 Scanner: Uit │ v2.7.0 | RL Software│
└─────────────────────────────────────────────────────────────────┘
```

### Ingelogd + Actief:
```
┌─────────────────────────────────────────────────────────────────┐
│  Zebra AutoPrint                        [PHOTONIS Defense]      │
│  🟢 Bewaking actief                                             │
├═════════════════════════════════════════════════════════════════┤ ← GROENE rand
│  [⏹ Stop]   [🔍 Preview]  [🔄 Herdruk]                          │
├─────────────────────────────────────────────────────────────────┤
│  ... log berichten ...                                          │
├─────────────────────────────────────────────────────────────────┤
│ 🟢 ACTIEF │ 📁 C:\IMES │ 📷 Scanner: Aan │ v2.7.0 | RL Software │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Menu Structuur

### Niet ingelogd:
```
Bestand | Weergave | Help | 🔐 Inloggen
```

### Ingelogd:
```
Bestand | Weergave | ⚙ Instellingen | Help | 🔓 Uitloggen
```

### Instellingen menu (na login):
- Algemene instellingen
- Label formaten (met test print)
- QR Code grootte
- Printers zoeken
- Scan buffer
- ZPL Code bekijken
- 🗑 Log wissen
- Debug modus

---

## 🔐 Beheerder Login

### Wachtwoord reset:
1. Maak bestand: `reset.txt` in de data folder
2. Start app opnieuw
3. Wachtwoord is gereset → stel nieuw wachtwoord in

---

## 📂 Data Folder Configuratie

### Prioriteit (hoogste eerst):
1. `datapath.txt` naast `ZebraAutoPrint.exe`
2. Environment variable `ZEBRAAUTOPRINT_DATA`
3. `%LOCALAPPDATA%\ZebraAutoPrint` (standaard)

### Bestanden in data folder:
| Bestand | Beschrijving |
|---------|--------------|
| `config.json` | Alle instellingen |
| `zebrautoprint.db` | SQLite database |
| `debug.log` | Debug logging (indien ingeschakeld) |
| `scanner_debug.log` | Scanner debug logging |
| `reset.txt` | Voor wachtwoord reset |

---

## 🏷️ Label Voorbeelden

### OFPNC 102×25mm:
```
┌──────────────────────────────────────────────────────────────────┐
│  OFPNC12345678901                              ┌────────────┐    │
│  Oven: 12                                      │   QR CODE  │    │
│  Plank: 34_A                           5x      │ (full path)│    │
│  OFPNC12345678901_Oven12_Plank34_A             └────────────┘    │
└──────────────────────────────────────────────────────────────────┘
```

### Mantel 50×25mm:
```
┌────────────────────────────────────────┐
│  OFPNC2500000          ┌────────┐      │
│  Batch: OFPNC2500000   │ QR     │      │
│  Plank: -       3x     │(path)  │      │
│  mantel_OFPNC2500000   └────────┘      │
└────────────────────────────────────────┘
```

### QR-code inhoud:
- **Bevat:** Volledig bestandspad (bijv. `C:\InProcess\OFPNC12345678901_Oven12_Plank34_A_20250116_1430.csv`)
- **Tekst onder:** Korte naam zonder timestamp (bijv. `OFPNC12345678901_Oven12_Plank34_A`)

---

## 🔧 Vereisten

### Development
- .NET 8 SDK
- Visual Studio 2022 of VS Code
- (Optioneel) WiX v4 voor MSI
- (Optioneel) Inno Setup voor EXE installer

### Runtime
- Windows 10/11 x64
- Geen .NET installatie nodig (self-contained)

---

## 📝 Changelog

### v2.7.0 - QR-code & Status Update
- **QR-code bevat nu volledig bestandspad** (traceerbaarheid)
- Tekst onder QR: korte naam zonder timestamp
- Preview toont exact 1-op-1 wat naar printer gaat
- **Visuele status indicatie:**
  - Header rand kleurt groen/rood
  - Statusbalk toont 🟢 ACTIEF / 🔴 GESTOPT
  - Start knop transformeert naar Stop (admin)
- **Start geminimaliseerd naar systeemvak:**
  - Nieuwe instelling in Algemene instellingen
  - App start direct naar tray
  - Automatische monitoring indien AutoStart aan

### v2.5.0 - Dual Label Formats & Branding
- **Twee label formaten:** OFPNC (102×25mm) + Mantel (50×25mm)
- Automatische detectie op basis van bestandsnaam
- Photonis Defense branding (logo + paarse kleur)
- ZPL Code Viewer (bekijk exacte ZPL)
- Test print per label formaat
- Wis Log verplaatst naar Instellingen menu (admin)
- UI cleanup en vereenvoudiging

### v2.4.5 - Language & Stop Security
- Taal selectie direct werkend (geen herstart)
- Stop knop alleen voor beheerders
- UI teksten volledig vertaald

### v2.4.2 - Custom Data Folder
- Data map instelbaar bij installatie
- Via datapath.txt of environment variable
- Installer optie: Aangepaste data map

### v2.4.0 - Beheerder Login & Beveiliging
- Beheerder login systeem toegevoegd
- Instellingen alleen toegankelijk na inloggen
- Wachtwoord reset via reset.txt bestand

### v2.3.1 - Scanner Format Fix
- Herkent streepjes EN underscores in OFPNC codes
- Case-insensitive matching voor database lookup

### v2.2.6 - Wachttijd & Startup Fix
- 2 minuten wachttijd na scan (voor laser verwerking)
- Bestaande bestanden bij opstart verwerkt

### v2.0.0 - C# Herschrijving
- Complete herschrijving van Python naar .NET 8
- Native WinForms (geen externe UI dependencies)
- Single-file EXE deployment
- Professionele MSI/Inno installer

---

## 👤 Contact

**RL Software**  
Rachid Lahmar  
r.lahmar@exosens.com  

Gemaakt voor **Photonis Netherlands B.V.**, Roden

---

## 📄 Licentie

Copyright (c) 2025 RL Software - Rachid Lahmar  
Alle rechten voorbehouden.
