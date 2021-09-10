# Gnoll: Gnomoria Mod SDK and Mod Loader

Fork and repackage of "gnomodkit" from: https://github.com/minexew/gnomodkit

## Prerequisites:

  - Gnomoria v1.0
  - Python 3.2+
  - .NET SDK 4.0 or newer

## Steam Workshop Mods

Gnoll works with steam workshop mods. 
Gnoll does not currently have any content mods, so game data and save games are untouched.

To get Gnoll working with Steam, first build the modloader and mods, as shown in the Usage section. After successfully building the loader and mods, you should have "GnoMod.exe" in your Gnomoria directory. Next:
* Backup your Gnomoria.exe (rename Gnomoria.exe -> Gnomoria.orig.exe. if something ever happens to your original .exe, don't worry, ask Steam to verify game files and it will re-download it). 
* Rename Gnoll modified exe (rename GnoMod.exe -> Gnomoria.exe)
* Run Gnomoria via Steam as usually

## Usage examples

Building SDK
```
./gnomodkit.py sdk
```
Building ModLoader
```
./gnomodkit.py modloader
```
Building a mod contained in this project (mod name = directory name under "Gnoll Mods")
```
./gnomodkit.py mod:ExampleHelloWorld
```
Build a mod and run the modded game
```
./gnomodkit.py sdk mod:ExampleHelloWorld run
```

# Included mods

Check out the existing mods under "Gnoll Mods" directory

# Screenshots

For pictures visit this Steam discussion:
https://steamcommunity.com/app/224500/discussions/0/3042732434759234670/

# How this thing works

Note: This here explains how things work. The actual work should be done via gnomodkit.py script.

## 1. Building SDK

- check retail Gnomoria.exe
- deobfuscate to make all symbol names valid identifiers; Gnomoria.exe -> GnomoriaGame.exe
- disassemble GnomoriaGame.exe -> GnomoriaGame.il
- make all fields & methods public; GnomoriaGame.il -> GnomoriaSDK.il
- assemble GnomoriaSDK.il -> GnomoriaSDK.dll
- copy retail gnomorialib.dll into working directory

GnomoriaSDK.dll + gnomorialib.dll can be now used to target Gnomoria internal APIs.

## 2. Building ModLoader

- compile GnollModLoader.dll, copy to game dir
- patch Gnomoria with ModLoader hooks; GnomoriaSDK.il + GnollModLoader.patch -> GnoMod.il
- assemble GnoMod.il -> GnoMod.exe

## 3. Building Mods manually
- compile mod .dll
- copy the resulting .dll into "/Gnoll Mods/enabled" under Gnomoria directory 
- all .dlls in "Gnoll Mods/enabled" directory will be loaded by the Mod Loader