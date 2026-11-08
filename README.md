# FS-3 Ternion FCS (Nuclear Option)

A BepInEx plugin that replaces the fly-by-wire of the **FS-3 Ternion** (Blueprinter blueprint, prefab `P_Trisurface1`) with the control laws of the F-22E Strike Raptor FCS, KR-67 Ifrit FCS, FS-12 Revoker FCS and T/A-30 Compass FCS. It is a separate DLL, retargeted to the Ternion's airframe: canards, aft elevators, twin rudders, two trailing-edge pairs, two Turbofans, no thrust vectoring.

## Requirements
- BepInEx 5 for Nuclear Option.
- Blueprinter and the FS-3 Ternion blueprint (`blueprinter.ternion`).

## What it does
- **Pitch:** G + pitch-rate command.
  - Limits: +55° / −40° AoA, +9 / −3.5 g.
  - Limits above 45° fly up to the canards' nose-down authority (swept authority cap). 55° holds down to about 165–180 km/h; below that the jet can't hold the nose up on the way over the top.
  - MPO (Stability Assist off): plain pitch-rate command up to 48 °/s at every AoA, no AoA limit, +12.5 / −3.5 g.
  - Released corkscrews come out without a nose-down dip (`HighAoAReleaseTau`).
- **Control allocation:** every surface is its own effector with modelled pitch, roll and yaw moments.
  - Canards: pitch, roll and yaw; 60° leading edge down / 45° up.
  - Aft elevators: +35 / −30°, stall at 30° local AoA.
  - Rudders ±35°, flaperons and rollervons ±25°, with camber coupling.
  - Every surface at 100 °/s.
- **Lateral law:**
  - Velocity-vector roll with sideslip hold.
  - Corkscrew and high-AoA roll handling.
  - In MPO: velocity-vector roll on the stick (default) or separate axes (`MpoRollMode`).
- **Brakes:**
  - The airbrake on the brake key, independent of throttle (`ThrottleIndependentAirbrake`).
  - **In the air** (`TrailingEdgeBrakeConfig`): the F-22E trailing-edge set (flaperons down, rollervons up, solved for zero added lift), closing between 15° and 35° AoA. The stock split brake stays closed while the FCS flies, so the surfaces keep their full control travel.
  - **On the runway:** every trailing edge up, elevators fully trailing edge up, canards leading edge down.
- **Ground:** runway rate law and INDI nose-wheel steering.
- **Damage tolerance:** asymmetric-load AoA caps, fin-loss limits, engine-out (VMC) protection, fuel balancing and lateral fuel trim.
- **Performance profiles:** Stock (default) or Buffed.
  - Buffed: thrust ×1.08, parasitic drag ×0.90, fuel burn ×0.75.
  - Editable flare, gun-ammo and EW multipliers (Buffed: flares ×10, gun ×2, EW ×1.5 / ×1.2).
- **Multiplayer:** host-controlled.
  - The FCS only flies on a server whose host runs it with `HostAllowFcs` on. The host's own jet is bound by the same switches.
  - `HostLockVariant` sends the host's variant to every client.
  - The server is flagged as modded while either switch is on.
- **FcsApi:** the Flight Data Readout HUD, the FCS Visualizer and Atmospheric Gusts find it automatically.

## Install
1. Install BepInEx 5 and Blueprinter, and the FS-3 Ternion blueprint.
2. Unzip `FS3-Ternion-FCS-<version>.zip` into `BepInEx/plugins/`. You get `BepInEx/plugins/Ternion_FS3.FCS/Ternion_FS3_FCS.dll` and `meta.json`.
3. Optional: put `Ternion_FS3_FCS_Dev.dll` (from the `Optional…DevUnlock` zip) in `BepInEx/plugins/`. It exposes every tuning key in `BepInEx/config/Ternion_FS3.FCS.Dev.cfg` and in the ConfigurationManager (F1).
4. Start the game. `BepInEx/LogOutput.log` should show `FS-3 Ternion FCS <version> … ready`.

## Configuration
- **Player settings:** `BepInEx/config/Ternion_FS3.FCS.cfg`. Includes `Enabled`, `PerformanceProfile`, `ThrottleIndependentAirbrake`, `TrailingEdgeBrakeConfig`, `MpoRollMode`, the pitch stick curve, countermeasure multipliers, AI and multiplayer host switches.
- **Developer and tuning keys:** `BepInEx/config/Ternion_FS3.FCS.Dev.cfg`, only with the dev DLL. The AoA limits (`AoALimitPositive` / `AoALimitNegative`) are here.
- **Upgrades:** changed defaults are migrated once (`TernionDevRevision`). Values you set by hand are kept.

## Compatibility
- Runs alongside the F-22E, Ifrit, Revoker and Compass FCS plugins. Each jet is flown only by its own plugin.
- Works with the Aryx Weaponry Expansion's external-tanks-first fuel hook: the fuel burn is scaled once and drawn once (0.1.7+).
- Player aircraft only. AI Ternions fly it only with `AIFlightControl` on (off by default).

## Building
`build.sh <srcdir> <out.dll>` (in the source zip) compiles the plugin with the .NET SDK's Roslyn `csc` against the game's `Managed` folder and BepInEx `core`. Set the paths at the top of the script.

The dev DLL (`DevUnlock/`) is built separately against BepInEx and `UnityEngine.CoreModule`.

## Status
Developed on an offline flight bench driven by the Ternion's airframe data, extracted from the blueprint (not included here; `bench/extract/extract_ternion.py` rebuilds it). The bench isn't calibrated against flight logs for this jet. Each version's changelog entry says what was benched.

See `CHANGELOG.md` for the version history.
