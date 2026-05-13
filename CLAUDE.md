# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About

BardMusicPlayer is an automated MIDI music player for FFXIV that uses the in-game bard performance mode. This fork (`-TC`) adds support for the Taiwan Chinese (TC) regional client.

## Build Commands

Target platform is **x64 only**, framework **net481** (Windows). Build via the `BMP.sln` solution:

```bash
# Debug build
dotnet build BMP.sln -c Debug /p:Platform=x64

# Release build
dotnet build BMP.sln -c Release /p:Platform=x64
```

There are no test projects in this solution.

## Architecture

The solution is structured as a set of singleton-based library projects that the main WPF app (`BardMusicPlayer`) composes together.

### Library Dependency Order (bottom to top)

| Project | Role | Key Dependencies |
|---|---|---|
| **Quotidian** | Foundation – enums, structs, utilities | (none) |
| **Pigeonhole** | Auto-saving JSON config (`BmpPigeonhole`) | Quotidian |
| **Transmogrify** | MIDI/song import & processing (`BmpSong`) | Pigeonhole, Quotidian |
| **Coffer** | Song catalog/database via LiteDB | Transmogrify, Pigeonhole |
| **Seer** | Game process monitor (`BmpSeer`) | Pigeonhole, Quotidian + Machina |
| **Siren** | Audio preview/synthesis via NAudio | Transmogrify, Pigeonhole |
| **DalamudBridge** | Named-pipe IPC with DalamudDoot plugin | Seer, Pigeonhole |
| **Maestro** | Playback orchestration (`BmpMaestro`) | Seer, Transmogrify, DalamudBridge, Sanford.Multimedia.Midi |
| **Script** | Scripting engine | Maestro, Seer, Pigeonhole |
| **BardMusicPlayer** | WPF UI entry point | All of the above |

### Startup / Shutdown Order

The order in `App.xaml.cs` matters — deviate from it carefully:

```
Initialize: Pigeonhole → Coffer → Seer firewall → Maestro.Start() → Seer.Start() → DalamudBridge.Start() → Script.Start() → Siren.Setup()
Shutdown:   Siren → Maestro → Script → DalamudBridge → Seer → Coffer → Pigeonhole
```

Maestro **must** start before Seer so all game player instances are captured.

### Seer – Game Reading Backends

`BmpSeer` monitors FFXIV processes via three backends that run per `Game` instance:

- **Sharlayan** – Direct memory reading using ASM signatures. Signatures and structure offsets are stored as JSON per game region in `BardMusicPlayer.Seer/Reader/Backend/Sharlayan/Files/Signatures/` and `.../Structures/`. Files: `Global.json`, `China.json`, `Korea.json`, `TC.json`.
- **Machina** – Network packet capture. Packet parsers live in `Reader/Backend/Machina/Packet.{size}.cs`.
- **DatFile** – Reads FFXIV keybind/hotbar `.dat` config files directly.
- **Dalamud** – Optional enhanced reading via the DalamudDoot companion plugin.

### TC Region Support

This fork adds a `TC` (Taiwan Chinese) FFXIV client region. The Sharlayan JSON files for TC are:
- `BardMusicPlayer.Seer/Reader/Backend/Sharlayan/Files/Signatures/TC.json`
- `BardMusicPlayer.Seer/Reader/Backend/Sharlayan/Files/Structures/TC.json`

Note: `GameRegion` enum in `BardMusicPlayer.Quotidian/Enums/GameRegion.cs` currently lists `Global`, `China`, `Korea` — TC support requires this enum and the loading logic in Sharlayan to be updated accordingly.

### Transmogrify – Song Import

`BmpSong` is the central song object. Importers support: `.mid`, Guitar Pro (`.gp3`–`.gp7`), MML, MidiBard, LRC (lyrics). Processors (`ClassicProcessor`, `LyricProcessor`, `VSTProcessor`) transform raw MIDI into FFXIV-playable note events.

### Maestro – Sequencer Backends

`BmpMaestro` runs a `SequencerHandler` that wraps an `ISequencerBackend`. Currently only `MogAmpSequencerBackend` is wired up (`SequencerType.MogAmp`). A `SanfordSequencerBackend` stub also exists. Players are either `LocalPlayer` (same machine) or `RemotePlayer` (multibox).

### Main App Packaging

The WPF app uses **Costura.Fody** to embed all DLL dependencies into a single executable. The `FodyWeavers.xml` file controls embedding behaviour.
