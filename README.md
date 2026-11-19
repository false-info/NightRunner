ApocExtract/
├── .gitignore
├── .gitattributes                  # Git LFS för binärt
├── README.md
├── ApocExtract.uproject
│
├── Config/
│   ├── DefaultEngine.ini
│   ├── DefaultGame.ini
│   ├── DefaultInput.ini
│   └── DefaultGameplayTags.ini
│
├── Source/
│   ├── ApocExtract.Target.cs
│   ├── ApocExtractEditor.Target.cs
│   ├── ApocExtractServer.Target.cs          # dedicated server
│   │
│   ├── ApocExtract/                         # huvudmodul (spel)
│   │   ├── ApocExtract.Build.cs
│   │   ├── ApocExtract.cpp / .h
│   │   ├── Core/                            # GameMode, GameState, PlayerState, Instance
│   │   ├── Player/                          # Character, Controller, Movement, Camera
│   │   ├── Bodycam/                         # Bodycam-kamera, fisheye, timestamp/HUD
│   │   ├── Weapons/
│   │   │   ├── Base/                        # WeaponBase, Firing, Recoil, Ballistics
│   │   │   ├── Attachments/                 # Mod-sockets, stat-modifiers, compat-regler
│   │   │   ├── Ammo/
│   │   │   └── Data/                        # DataAssets/Tables (data-drivet!)
│   │   ├── Inventory/                       # Grid-inventory, loot, containers, stash
│   │   ├── Extraction/                      # Raid-state, extract-zoner, spawn, timer
│   │   ├── AI/                              # Fiender, zombies/raiders, perception
│   │   ├── Damage/                          # Kroppsdelar, penetration, status effects
│   │   ├── Squad/                           # Party, 1–4 spelare, revive, ping
│   │   ├── Net/                             # Replikering, prediction, anti-cheat hooks
│   │   ├── UI/                              # HUD, inventory UI (UMG/CommonUI)
│   │   └── Audio/
│   │
│   ├── ApocSimd/                            # SIMD-modul (egen modul = lätt att benchmarka)
│   │   ├── ApocSimd.Build.cs
│   │   ├── Public/
│   │   │   ├── SimdMath.h                   # wrappers över VectorRegister/intrinsics
│   │   │   ├── BatchedRaycast.h
│   │   │   ├── ProjectileSim.h              # SoA-batch av kulor
│   │   │   └── SimdAllocators.h
│   │   ├── Private/
│   │   └── Tests/
│   │
│   └── ApocExtractEditor/                   # editor-verktyg (weapon builder, validators)
│
├── Plugins/
│   ├── GameFeatures/                        # valfria features/DLC/events
│   │   └── ApocWeaponPack_Example/
│   └── ThirdParty/
│
├── Content/                                 # .uasset/.umap (LFS)
│   ├── ApocExtract/
│   │   ├── Core/
│   │   ├── Characters/
│   │   ├── Weapons/                         # en mapp per vapen + Attachments/
│   │   ├── Items/
│   │   ├── Maps/                            # Hideout/, Raid_01/ ...
│   │   ├── Materials/                       # Master materials, Bodycam PP
│   │   ├── VFX/
│   │   ├── Audio/
│   │   ├── UI/
│   │   └── Data/                            # DataTables, DataAssets
│   └── Developers/                          # personliga sandboxar (ignoreras ev.)
│
├── Art/                                     # UTANFÖR Content — Blender-källor
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
│   └── Art_Pipeline.md                      # namnkonvention, skala, export-inställningar
│
├── Tools/
│   ├── Scripts/                             # setup.bat, generate_project.bat, build.bat
│   ├── BlenderAddons/                       # export-script mot UE
│   └── Benchmarks/                          # SIMD-benchmarks
│
└── .github/ (eller .gitlab/)
    └── workflows/                           # CI: bygg + kompilera
