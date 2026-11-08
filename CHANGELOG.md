# Changelog

## 0.1.8 (7 Nov 2026)
MPO has no AoA ceiling any more. On the Ternion (no thrust vectoring) the MPO pitch-rate budget fell to 5 + 0.15 x IAS (m/s) at
high AoA, because the 40 x T/W term that keeps it near the cap on the vectoring jets is 0 here: 24 deg/s at 450 km/h, 14 deg/s at
215 km/h, and full aft stopped at 55-58 deg. MPO now gets the full pitch-rate cap (MaxPitchRate 48) at every AoA on a jet without
nozzle pitch (code switch MpoNoVecFullRate). Normal modes unchanged. Bench, MPO full aft from 350-650 km/h: 47-48 deg/s through
every AoA (held full aft goes over the top); released at 75-80 deg it recovers below 40 deg within 4 s; sideslip <= 1.1 deg.

## 0.1.7 (7 Nov 2026)
Fuel: the buffed fuel burn is applied once again when the Aryx Weaponry Expansion is loaded. Its external-tanks-first prefix on
Aircraft.UseFuel drew every engine fuel call a second time, unscaled (1.75x stock burn on Buffed, 2x on Stock with
FuelFromAttachedTanksOnly on). The fuel hook is now two prefixes: UseFuelScalePrefix (Priority.First) scales the amount,
UseFuelPrefix (Priority.Last) draws only if no other prefix already drew (__runOriginal).

## 0.1.6 (7 Nov 2026)
Canards and elevators no longer left in opposing roll splits after a manoeuvre: StabilatorRollCost 0.0005 -> 0.01 (two pitch
pairs made a cancelling split nearly free for the allocator). One-time move (TernionDevRevision 4). Corkscrew entries reach 30 deg
~0.15-0.2 s later.

## 0.1.5 (7 Nov 2026)
AoALimitPositive 50 -> 55 (TernionDevRevision 3). Holds 55 down to ~165-180 km/h on the bench.

## 0.1.4 (7 Nov 2026)
Elevator stall curve: the 30 deg stall keeps the airfoil's own drag (ElevatorStallDragShift false); 0.1.2-0.1.3 moved the drag rise
in with the peak, which raised the elevators' normal force past the stall. ElevatorPlateauCL (dev, 1 = off) for plateau tests.

## 0.1.3 (7 Nov 2026)
Stock split airbrake and split yaw off while the FCS flies (AirbrakeSplitInFlight / SplitFlapYaw false): they took the elevators',
rudders' and flaperons' travel; the in-flight brake is the F-22E trailing-edge set. HighAoAReleaseTau 0.3 s: no nose-down dip when
a corkscrew is let go. LowThrustCapMargin 8 -> 4: limits of 50-55 hold to ~150-185 km/h. AoALimitPositive 40 -> 50.
TernionDevRevision 2.

## 0.1.2 (7 Nov 2026)
Elevators stall at 30 deg local (ElevatorStallAoA) and x1.2 effective (ElevatorEffectiveness); canards x1.1 (CanardEffectiveness);
shoulder LERX x1.13 (LerxEffectiveness, with its HLD areas) so the static margin stays at 0.1.1's -0.29 m. Rollout aero-brake adds
the elevators fully trailing edge up (GroundBrakeElevator 35, the F-22E's stabs). LateralStabReserve 0.7 -> 0: corkscrew entries
built pitch on 30 % of the canard/elevator authority (no nozzle pitch on this jet); now within ~0.1-0.3 s of a plain pull.
One-time move of the three changed dev defaults (TernionDevRevision 1).

## 0.1.1 (7 Nov 2026)
AoA limits above 45 now fly: the inherited fixed no-TVC caps (NoTvcAoACapPositive / Negative 45) were always on (vectorable T/W
is 0 on this jet) and capped every limit above 45, MPO included. On a jet with no vectorable nozzle (noVecJet) they are off;
AoALimitPositive / Negative and the swept authority cap rule. Defaults unchanged (40 / -40). New dev key ElevatorStallAoA
(0 = stock curve) for the earlier-elevator-stall test; bench: no high-AoA roll gain (roll is yaw-capacity-limited).

## 0.1.0 (7 Nov 2026)
New mod (`Ternion_FS3.FCS`), forked from T/A-30 Compass FCS 0.1.11. FS-3 Ternion (Blueprinter, prefab P_Trisurface1). Canards and
aft elevators are both Stabilator-kind pairs (`isCanard` flag): canard travel 60 LE down / 45 LE up (Revoker), elevators +35/-30,
rudders +-35, flaperon_L/R and rollervon_L/R +-25 (F-22E), all surfaces 100 deg/s. Revoker staggered trailing-edge stall (inboard
flaperons 26, outboard rollervons 45, knee-preserving) with camber coupling (0.62 / 0.66) and rudder camber 0.54. No downwash.
Canard attached-flow window, settled window and rollout brake on the canards only; per-pair trim costs (ElevatorTrimCost);
FlightPathRateFloor; trailing-edge brake set and its lift balance handle control-surface flaperons. Turbojet -> Turbofan.
AoA limits +40 / -40. Buffed: thrust x1.08, fuel x0.75, parasitic drag x0.90, flares x10, gun x2, EW x1.5 / x1.2.
