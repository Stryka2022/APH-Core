# APH Havoc Virtual Garage

A virtual garage for DayZ 1.30:

- Players store vehicles at **parking meters**, or anywhere with the **mobile garage** steering wheel.
- Vehicles come back exactly as they were left.
- Damaged vehicles can be repaired for a fee from the bank.
- Staff can impound vehicles.
- Every vehicle mod on the server is detected automatically.

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Havoc Banking | Optional, but needed for any paid action. Without it, paid actions are refused and free ones still work. |
| APH Havoc Advanced Clans | Optional. Shows meters on the map. |

---

## Installation

1. Put `APH_Havoc_Virtual_Garage.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\Garage\Settings.json` and `Data\Garage\` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff the permissions below in APH Core's `Config\Permissions.json`. Owner has them all.
5. Place your parking meters (see below).

---

## Parking meters

- **Placing a meter** (staff): open **Garage > Parking meters** in the APH shell and use the APH placer (live preview, rotate, height, Enter places). Or hold the staff-only `APH_ParkingMeter_Kit` and use **Install parking meter**. Meters are saved, come back after every restart, and get a map marker.
- **Parking bay:** right after placing a meter, staff place its bay. A car and 4 hologram cones at the bay's corners show the spot and the way vehicles face (green when clear, red when something is in the way). You can change it later with **Set parking bay (staff)** on the meter, or **Bay** on the Parking meters page.
- **Nearby radius:** after the bay, a green hologram ring round the meter shows how far vehicles count as nearby for storing there. The mouse wheel changes it 1 m a notch (Shift 0.5, Ctrl 5), Up / Down while held, **R** goes back to the server's `StoreRadius`, **Enter** or left click saves it, **Backspace** keeps it as it was. It is saved on that meter and measured from the meter. Change it later with **Radius** on the Parking meters page.
  - At that meter, the vehicle in the bay is listed first.
  - A stored vehicle can come back **to the bay** or **nearby**.
  - `BayOnly` means only the bay's vehicle can be stored there.
- **Using a meter** (players): choose **Open garage** at the meter. The bay and **Nearby vehicles** are listed with their condition and fluids. Store, restore and repair from there.

---

## Mobile garage

The **CosworthMK3 steering wheel** (`APH_CosworthMK3`) works as a garage you carry. Hold it and choose **Open mobile garage** to:

- store a nearby vehicle, anywhere
- restore one in front of you, anywhere

It has its own radius, a cooldown between uses, and an extra fee.

---

## What is kept

A restored vehicle comes back exactly as it was stored:

- every part, in its slot
- all cargo, including items inside items
- the health of the vehicle, its damage zones and every item
- fuel, oil, coolant and brake fluid
- quantities, magazine ammo and liquids

---

## Ownership, repair and impound

- **Ownership:**
  - The player who restores a vehicle owns it.
  - A vehicle with no owner belongs to its last driver.
  - With `OwnerMode` 1, only the owner can store it.
- **Repair:** a stored vehicle can be repaired for `RepairBaseCost + RepairCostPerPercent × missing %`, paid from the bank. `RepairRefillsFluids` also tops up the fluids.
- **Impound** (staff, `garage.impound`):
  1. Look at a vehicle and use **Check owner (staff)** or **Impound vehicle (staff)**, with a buy-back price and a reason. Or use **Garage > Impound > Impound nearest**.
  2. The vehicle goes to its owner's garage as **IMPOUNDED**.
  3. The owner pays the buy-back price from the bank at a meter.
  4. Staff can also **Release**, **Set fee**, **Return here** or **Delete** it.

---

## Vehicle detection

On start, every spawnable vehicle class that inherits one of `VehicleBaseClasses` counts as a vehicle (Car, Boat, Motorbike, Helicopter, the Expansion bases ...). So modded cars, boats and helicopters work without a list.

- `ExtraVehicleClasses` adds classes.
- `BlockedVehicleClasses` removes them.
- **Garage > Vehicle classes** shows the full list, with search and **Rescan**.

---

## Settings (`Config\Garage\Settings.json`)

| Setting | What it does |
|---|---|
| `GarageName`, `MaxStoredVehicles` | Name, and how many vehicles each player can store |
| `MeterUseDistance`, `StoreRadius`, `SpawnDistance`, `SpawnClearRadius` | Distances at a meter |
| `BayRadius`, `BayOnly`, `ShowBays` | Parking bays |
| `OwnerMode`, `StoreCargo`, `AllowRuined` | Who can store, whether cargo is kept, and whether ruined vehicles can be stored |
| `MobileEnabled`, `MobileAllowStore`, `MobileAllowRestore`, `MobileStoreRadius`, `MobileCooldownSeconds`, `MobileExtraCost` | The mobile garage |
| `StoreCost`, `RetrieveCost` | Fees |
| `RepairEnabled`, `RepairBaseCost`, `RepairCostPerPercent`, `RepairRefillsFluids` | Repair |
| `ImpoundDefaultFee`, `ImpoundNearestRadius` | Impound |
| `ShowMetersOnMap`, `MarkerName`, `MarkerIcon`, `MarkerColor` | The meter map marker |
| `VehicleBaseClasses`, `ExtraVehicleClasses`, `BlockedVehicleClasses` | Vehicle detection |

The file's `Help` list explains every field. **Live reload:** **System > Reload configs > Garage**.

| File | What it holds |
|---|---|
| `Data\Garage\Meters.json` | Placed meters and bays |
| `Data\Garage\Players\<steam64>.json` | Each player's stored vehicles (`unowned.json` for impounds with no known owner) |
| `Data\Garage\Owners.json` | Vehicle owners |
| `Data\Garage\Impound.json` | The impound index |

---

## Permissions

| Feature | Allows |
|---|---|
| `garage.admin` | Meters, bays, settings, vehicle classes and the meter kit |
| `garage.impound` | Check owner, impound, release and set buy-back prices |

---

## Discord

`Garage_IMPOUND` posts through APH Core's shared webhooks (**System > Discord webhooks**) when a vehicle is impounded, bought back or released.

---

## types.xml (optional, for traders or staff)

```xml
<type name="APH_CosworthMK3"><nominal>0</nominal><lifetime>14400</lifetime><restock>0</restock><min>0</min><quantmin>-1</quantmin><quantmax>-1</quantmax><cost>100</cost><flags count_in_cargo="0" count_in_hoarder="0" count_in_map="1" count_in_player="0" crafted="0" deloot="0"/><category name="tools"/></type>
<type name="APH_ParkingMeter_Kit"><nominal>0</nominal><lifetime>3600</lifetime><restock>0</restock><min>0</min><quantmin>-1</quantmin><quantmax>-1</quantmax><cost>100</cost><flags count_in_cargo="0" count_in_hoarder="0" count_in_map="1" count_in_player="0" crafted="0" deloot="0"/></type>
```

`APH_ParkingMeter` itself is placed by staff and shouldn't spawn as loot.