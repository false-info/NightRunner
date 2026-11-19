# NightRunner

Realistisk, krävande extraction shooter i en apokalyptisk värld för 1–4 spelare (squad), förstapersons bodycam-vy, väldigt stor vapenarsenal med djupt mod-system. Byggt i **Unreal Engine 5** med **C++ + SIMD** och konst i **Blender**.

---

## 1. Snabbfakta

| | |
|---|---|
| Motor | Unreal Engine 5.x (**lås EN exakt version, alla använder samma**) |
| Språk | C++ (C++20 i UE5), SIMD för tunga beräkningar |
| Konst | Blender (modellering, rigg, animation) |
| Genre | Extraction shooter, PvPvE, apokalyps |
| Spelare | 1–4 i squad |
| Kamera | Förstaperson, bodycam-look |
| Nätverk | Dedicated server, server-auktoritativt |
| Versionshantering | Git + Git LFS |

---

## 1b. Realism som designmål

NightRunner ska kännas **verkligt, tungt och straffande**, inte arkadigt:

- **Ballistik:** kulfall, hastighet, penetration och ricochet per kaliber/ammo-typ (data-drivet, simulerat i batch via SIMD-modulen).
- **Vapenhantering:** vikt och ergonomi påverkar sikt, svaj och ADS-tid. Rekyl med mönster, riktiga laddnings- och felsituationer (hylsstopp, magasinbyte, kammare).
- **Skador:** kroppsdelar, blödning, frakturer, smärta. Inga regenererande hälsobarer.
- **Bodycam:** diegetisk HUD (så lite UI som möjligt), kamerans begränsningar syns i bilden.
- **Ljud:** verkligt ljud är halva realismen (avstånd, rum, dämpning, tryckvåg).
- **Visuellt:** fotogrammetri/PBR-material, Lumen/Nanite, konsekvent skala i meter.
- **Referensmaterial:** samla verkliga foton och manualer i `Art/Reference/`. Bygg egna vapendesigner som är inspirerade av verkligheten.

---

## 2. Teamroller (exempel, anpassa)

| Person | Ansvar |
|---|---|
| Programmerare | All C++, gameplay, nätverk, SIMD, build/CI |
| Artist 1 | Vapen, attachments, props (Blender → UE) |
| Artist 2 | Karaktärer, miljöer, animationer, VFX |

Alla får röra `Content/` och `Art/`, men **bara en person i taget per binär fil** (se Git-regler).

---

## 3. Filträd

```
NightRunner/
├── .gitignore
├── .gitattributes                  # Git LFS
├── README.md
├── NightRunner.uproject
│
├── Config/
│   ├── DefaultEngine.ini
│   ├── DefaultGame.ini
│   ├── DefaultInput.ini
│   └── DefaultGameplayTags.ini
│
├── Source/
│   ├── NightRunner.Target.cs
│   ├── NightRunnerEditor.Target.cs
│   ├── NightRunnerServer.Target.cs          # dedicated server
│   │
│   ├── NightRunner/                         # huvudmodul (spelet)
│   │   ├── NightRunner.Build.cs
│   │   ├── NightRunner.cpp / .h
│   │   ├── Core/                            # GameMode, GameState, PlayerState, GameInstance
│   │   ├── Player/                          # Character, Controller, Movement, Camera
│   │   ├── Bodycam/                         # Bodycam-kamera, fisheye, timestamp/HUD
│   │   ├── Weapons/
│   │   │   ├── Base/                        # WeaponBase, Firing, Recoil, Ballistics
│   │   │   ├── Attachments/                 # Mod-sockets, stat-modifiers, kompatibilitet
│   │   │   ├── Ammo/
│   │   │   └── Data/                        # DataAssets/DataTables (data-drivet)
│   │   ├── Inventory/                       # Grid-inventory, loot, containers, stash
│   │   ├── Extraction/                      # Raid-state, extract-zoner, spawn, timer
│   │   ├── AI/                              # Fiender, perception, behavior
│   │   ├── Damage/                          # Kroppsdelar, penetration, status effects
│   │   ├── Squad/                           # Party, revive, ping
│   │   ├── Net/                             # Replikering, prediction, anti-cheat hooks
│   │   ├── UI/                              # HUD, inventory-UI
│   │   └── Audio/
│   │
│   ├── NightRunnerSimd/                            # SIMD-modul (egen modul, lätt att benchmarka)
│   │   ├── NightRunnerSimd.Build.cs
│   │   ├── Public/
│   │   │   ├── SimdMath.h
│   │   │   ├── BatchedRaycast.h
│   │   │   ├── ProjectileSim.h              # SoA-batch av kulor
│   │   │   └── SimdAllocators.h
│   │   ├── Private/
│   │   └── Tests/
│   │
│   └── NightRunnerEditor/                   # Editor-verktyg (weapon builder, validators)
│
├── Plugins/
│   ├── GameFeatures/                        # Valfria features/events/DLC
│   └── ThirdParty/
│
├── Content/                                 # .uasset/.umap (LFS)
│   ├── NightRunner/
│   │   ├── Core/
│   │   ├── Characters/
│   │   ├── Weapons/                         # en mapp per vapen, med Attachments/ inuti
│   │   ├── Items/
│   │   ├── Maps/                            # Hideout/, Raid_01/ ...
│   │   ├── Materials/                       # Master materials, bodycam post-process
│   │   ├── VFX/
│   │   ├── Audio/
│   │   ├── UI/
│   │   └── Data/                            # DataTables, DataAssets
│   └── Developers/                          # personliga sandboxar
│
├── Art/                                     # UTANFÖR Content, Blender-källor
│   ├── Blender/
│   │   ├── Weapons/
│   │   ├── Characters/
│   │   ├── Props/
│   │   └── Environment/
│   ├── Exports/                             # FBX/glTF innan import till UE
│   ├── Textures_Source/                     # PSD/Substance
│   └── Reference/
│
├── Docs/
│   ├── GDD.md
│   ├── Architecture.md
│   ├── Weapon_Modding_System.md
│   ├── Coding_Standards.md
│   └── Art_Pipeline.md
│
├── Tools/
│   ├── Scripts/                             # setup, generate_project, build
│   ├── BlenderAddons/                       # Export-script mot UE
│   └── Benchmarks/                          # SIMD-benchmarks
│
└── .github/                                 # (eller .gitlab/) CI: bygg + kompilera
    └── workflows/
```

---

## 4. Komma igång (för alla)

### Krav
- **Unreal Engine 5.x**, exakt samma version för alla (skriv versionen här: `5.__`)
- **Visual Studio 2022** med: *Game development with C++*, *.NET desktop development*, Windows SDK
- **Git** + **Git LFS** (`git lfs install`)
- **Blender** (artisterna), samma huvudversion för alla (skriv här: `4.__`)
- Gärna en snabb NVMe-disk. UE5-projekt blir stora.

### Första gången
```bash
git clone <repo-url>
cd NightRunner
git lfs pull
```
1. Högerklicka `NightRunner.uproject` → **Generate Visual Studio project files**
2. Öppna `NightRunner.sln`, välj **Development Editor** + **Win64**, bygg
3. Starta från Visual Studio (F5) eller öppna `.uproject`

### Artister
Ni behöver inte bygga C++ själva. Be programmeraren om en färdigbyggd editor-binär eller kör bygget en gång lokalt och öppna sedan `.uproject` direkt.

---

## 5. Git-regler (viktigast för att inte förstöra varandras arbete)

**Binära filer (.uasset, .umap, .blend, .fbx) går INTE att merga.** Två personer som ändrar samma fil = en av er förlorar sitt arbete.

- **Prata innan ni rör en gemensam fil.** Skriv i chatten vem som jobbar på vilken map/blueprint.
- **Använd LFS-lås:** `git lfs lock Content/NightRunner/Maps/Raid_01.umap` och `git lfs unlock ...` när ni är klara. Se låsta filer med `git lfs locks`.
- **Branches:** `main` ska alltid gå att öppna och bygga. Jobba på `feature/<namn>-<sak>`, t.ex. `feature/anna-ak47`.
- **Pusha ofta, små commits.** Dagligen minst.
- **Pulla innan ni börjar jobba.**
- **Committa aldrig** `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/`, `.vs/`.
- **Commit-meddelanden:** korta och tydliga, t.ex. `Weapons: add recoil pattern system`, `Art: AK47 low poly + UVs`.
- **Eget sandbox-område** i `Content/Developers/<namn>/` för experiment.

### LFS-lagring
GitHubs gratis LFS-kvot är liten och tar snabbt slut med UE + Blender. Alternativ: Azure DevOps, självhostad Gitea, GitLab, Perforce eller Plastic SCM (Unity Version Control). Bestäm detta **innan** ni har mycket data i repot.

### .gitignore (minimum)
```
Binaries/
Intermediate/
Saved/
DerivedDataCache/
.vs/
.idea/
*.sln
*.VC.db
*.opensdf
*.sdf
*.suo
Build/
*.blend1
*.blend2
```

### .gitattributes (LFS)
```
*.uasset filter=lfs diff=lfs merge=lfs -text
*.umap   filter=lfs diff=lfs merge=lfs -text
*.blend  filter=lfs diff=lfs merge=lfs -text
*.fbx    filter=lfs diff=lfs merge=lfs -text
*.png    filter=lfs diff=lfs merge=lfs -text
*.tga    filter=lfs diff=lfs merge=lfs -text
*.psd    filter=lfs diff=lfs merge=lfs -text
*.wav    filter=lfs diff=lfs merge=lfs -text
*.mp4    filter=lfs diff=lfs merge=lfs -text
```

---

## 6. Namnkonvention (UE-standard)

| Typ | Prefix | Exempel |
|---|---|---|
| Blueprint | `BP_` | `BP_Weapon_AK74` |
| Static Mesh | `SM_` | `SM_AK74_Receiver` |
| Skeletal Mesh | `SK_` | `SK_Player_Arms` |
| Material | `M_` | `M_Metal_Worn` |
| Material Instance | `MI_` | `MI_AK74_Body` |
| Texture | `T_` + suffix | `T_AK74_BC` (BaseColor), `_N` (Normal), `_ORM` |
| Animation | `A_` / `AM_` (montage) | `AM_AK74_Reload` |
| Anim Blueprint | `ABP_` | `ABP_Player_FPS` |
| Niagara | `NS_` | `NS_MuzzleFlash_Rifle` |
| Data Asset | `DA_` | `DA_Weapon_AK74` |
| Data Table | `DT_` | `DT_Ammo` |
| Widget | `WBP_` | `WBP_Inventory` |
| Sound | `S_` / `SC_` (cue) | `SC_AK74_Shot` |

**Filer och mappar:** inga mellanslag, inga å/ä/ö, engelska namn.

**C++-klasser:** `A` (Actor), `U` (UObject), `F` (struct), `E` (enum), `I` (interface), `T` (template). Projektprefix valfritt, t.ex. `ANRWeaponBase`.

---

## 7. Konstpipeline (Blender → UE5)

1. **Skala:** Blender-scenen i **meter**, Unit Scale 1.0. UE använder cm, FBX-exporten hanterar det om ni exporterar med rätt inställningar. Testa med en kub först och lås inställningarna.
2. **Axlar:** exportera med **Forward: -Y**, **Up: Z** (eller använd ett export-addon i `Tools/BlenderAddons/`).
3. **Applicera transforms** (Ctrl+A → All Transforms) innan export.
4. **Pivot:** vapen = pivot vid handtaget/markerad socket-punkt. Props = pivot i botten.
5. **Sockets:** namnge tydligt i rigg/mesh (`socket_muzzle`, `socket_scope`, `socket_grip`, `socket_mag`).
6. **UV:** unika UV:er i kanal 0, lightmap-UV i kanal 1 om det behövs (Nanite/Lumen minskar behovet).
7. **LODs/Nanite:** statiska meshes kan använda Nanite. Skeletal meshes (vapen med animerade delar, karaktärer) gör det **inte** på samma sätt, så budgetera LOD:er.
8. **Texturer:** **packad ORM** (Occlusion/Roughness/Metallic), BaseColor, Normal. Konsekvent texel density (t.ex. 10,24 px/cm för first-person-vapen, lägre för miljö).
9. **Spara källfil** i `Art/Blender/...`, exportera till `Art/Exports/`, importera till `Content/`.
10. **Vapendelar som separata meshes** (receiver, barrel, stock, grip, mag...) så att mods kan bytas, även om basvapnet är ett.

Skriv ner de exakta exportinställningarna i `Docs/Art_Pipeline.md` när ni har testat dem.

---

## 8. Kodstandard

- UE:s kodstandard (Epic Coding Standard) som bas. Använd `.clang-format` i repo-roten så formateringen är lika för alla.
- **Allt gameplay-data ska vara datadrivet.** Vapen, ammo, attachments, items = `UDataAsset` / `UPrimaryDataAsset` + **Gameplay Tags**. Inga hårdkodade vapenvärden i C++.
- **Tunga system i C++**, designer-justerbara saker exponeras med `UPROPERTY(EditAnywhere)` / `UFUNCTION(BlueprintCallable)`.
- **Moduler:** håll beroenden enkla. `NightRunnerSimd` ska **inte** bero på `NightRunner`.
- **Ingen tung logik i Tick** om det går att undvika. Använd timers/events.
- **Nätverk från dag ett:** tänk server-auktoritativt. `UFUNCTION(Server, Reliable)` / `Client` / `NetMulticast`, `GetLifetimeReplicatedProps`. Predicera klientsidan där det känns.
- **Kommentarer** på *varför*, inte *vad*.
- **Inga magiska nummer.** Konstanter eller data.

---

## 9. SIMD-riktlinjer

- **Mät först, optimera sen.** Använd Unreal Insights / egna benchmarks i `Tools/Benchmarks/`. SIMD ska bara användas där profilering visar att det behövs.
- **Börja med UE:s `VectorRegister`-abstraktion** (`VectorRegister4Float`, `VectorLoad`, `VectorMultiplyAdd` osv.). Den är portabel mellan plattformar.
- **Rå intrinsics (SSE/AVX2) endast efter behov**, i `NightRunnerSimd`, med scalar-fallback och `#if` per plattform.
- **Datalayout:** **SoA** (Structure of Arrays) för batcher, t.ex. alla kulors position/hastighet i separata, alignade arrayer. 16/32-byte alignment.
- **Bra kandidater i det här spelet:** batchad kulsimulering, ballistik/penetration, AI-perception/synlighet, partikel/debris-logik utanför Niagara, frustum/avståndscullning av egna objekt.
- **Dåliga kandidater:** enskilda skott från spelaren, UI, sällsynt logik.
- **Testa att scalar och SIMD ger samma resultat** (unit tests i `NightRunnerSimd/Tests/`). Floating point-skillnader kan ge desync i nätverk, så var försiktig med determinism.
- **Kompilatorflaggor** (AVX2 etc.) styrs i `Build.cs`. Kräv inte AVX2 om ni vill att spelet ska köra på äldre CPU:er utan fallback.

---

## 10. Vapen- och moddsystem (arkitektur)

Mål: **hundratals vapen och tusentals möjliga mod-kombinationer** utan att koden växer.

- **`DA_Weapon_*`** (Data Asset): basstats (skadevärde, eldhastighet, rekyl, vikt, kaliber, fire modes, tillåtna slots).
- **Slots:** varje vapen definierar sockets/slots (`Muzzle`, `Barrel`, `Optic`, `Grip`, `Stock`, `Magazine`, `Rail_Side`...) via Gameplay Tags.
- **`DA_Attachment_*`:** vilken slot den passar i, mesh, **stat-modifiers** (additiva/multiplikativa), tillåtna/förbjudna kombinationer (tags).
- **Stat-beräkning:** `BaseStats + Σ attachment-modifiers` → `FinalStats`. Cacha resultatet och räkna om bara vid ändring.
- **Kompatibilitet:** regelbaserat med tags (`Requires`, `Blocks`), inte en enorm manuell tabell.
- **Visuellt:** attachment-meshes attachas på vapnets sockets vid runtime.
- **Replikering:** replikera **konfigurationen** (lista av attachment-ID:n), inte hela statobjekt. Klienter räknar fram stats själva från samma data.
- **Editor-verktyg** (i `NightRunnerEditor/`): en "Weapon Builder"/validator som varnar för saknade sockets, trasiga regler och dubbletter.

Dokumentera detaljerna i `Docs/Weapon_Modding_System.md`.

---

## 11. Bodycam-looken

Görs främst i ett **post-process-material** (och kameraparametrar), inte med dyr geometri:

- Fisheye/lens-distorsion (vinkel + vinjettering)
- Filmkorn, lätt kromatisk aberration, komprimeringsartefakter/blockighet
- Timestamp/REC-indikator, batteri, datum (UI-lager)
- Hög FOV, lätt kameraskaksimulering kopplad till rörelse
- Sensor-aktig exponering/dynamiskt omfång, lite blommande ljus
- Tips: håll effekten **justerbar** (slider i inställningar) av komfort- och tillgänglighetsskäl

---

## 12. Prestandabudget (fyll i tillsammans)

Bestäm tidigt, mät ofta:

| | Mål |
|---|---|
| Målplattform | PC (ange min-spec och rek-spec) |
| FPS-mål | t.ex. 60 fps @ rek-spec |
| Frame budget | 16,6 ms (60 fps) |
| Max spelare per raid | 4 per squad × antal squads: **bestäm** |
| Tris/vapen (first person, LOD0) | **bestäm** |
| Texturstorlek vapen | t.ex. 2K–4K, **bestäm** |

Använd `stat unit`, `stat gpu`, **Unreal Insights** och Nanite/Lumen-visualiseringar. "Very good graphics" är dyrt: bestäm tidigt vilka UE5-funktioner (Lumen, Nanite, VSM, Niagara) ni verkligen kör.

---

## 13. Rekommenderad utvecklingsordning

1. **Setup:** repo, LFS, projekt som bygger för alla, CI som kompilerar.
2. **Spelare:** förstapersonsrörelse + bodycam-kamera + grundläggande skjutande (ett vapen).
3. **Nätverk:** dedicated server + 2 klienter som kan springa runt och skjuta.
4. **Datadrivet vapensystem** + attachments (3–5 vapen, 10 mods).
5. **Inventory + loot.**
6. **Raid-loop:** spawn → loot → extract → stash.
7. **AI-fiender.**
8. **Squad (1–4):** party, revive, ping.
9. **Polish:** grafik, ljud, UI, optimering, SIMD där det mäts behövas.
10. **Innehåll:** fler vapen, kartor, fiender.

Gör **en spelbar vertical slice** så tidigt som möjligt (en karta, ett par vapen, en extract). Det är lättare att hålla motivationen uppe och hitta designproblem.

---

## 14. Tips för ett litet team

- **Scope är fienden.** Extraction shooter + stort vapenmoddande + bra grafik + nätverk är ett enormt projekt. Skär hårt, bygg inkrementellt.
- **Ha ett veckomöte** (även 15 min): vem gör vad, vad blockerar.
- **Använd en task-board** (GitHub Projects, Trello, Notion, Linear).
- **Backup utanför Git** av `Art/` och `Textures_Source/`.
- **Testa bygget på allas datorer** regelbundet ("works on my machine"-problem).
- **Kolla licenser** på allt ni köper/laddar ner (Fab/Marketplace, ljud, typsnitt, texturer).
- **Spara ofta i Blender**, och inkrementella kopior (`_v01`, `_v02`) eller Git-historik för `.blend`-filer.
- **Dokumentera beslut** i `Docs/` medan ni minns varför.

---

## 15. Nyttiga länkar

- Unreal Engine-dokumentation: https://dev.epicgames.com/documentation/unreal-engine
- Epic Coding Standard: sök "Unreal Engine Coding Standard"
- Gameplay Ability System (GAS) kan vara värt att titta på för status effects/skador
- Git LFS: https://git-lfs.com
- Unreal Insights (profilering): i UE-dokumentationen

---

## Att fylla i

- [ ] UE-version: `______`
- [ ] Blender-version: `______`
- [ ] Hosting för repo/LFS: `______`
- [ ] Min-spec / rek-spec: `______`
- [ ] Ägare per system/område: `______`
