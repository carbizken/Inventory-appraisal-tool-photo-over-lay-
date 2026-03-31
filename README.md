# GhostCar

Guided vehicle photo capture overlay component. Built for the HarteCash vehicle acquisition platform.

GhostCar displays a translucent vehicle silhouette inside the camera viewfinder so customers can line up each shot consistently — no guesswork, no unusable angles, no damage missed.

---

## How it works

The customer opens the photo capture flow. GhostCar receives the vehicle type from the Black Book VIN decode and renders the matching silhouette overlay on screen. The customer aligns the vehicle to the ghost, taps capture, and moves to the next shot. Every photo comes back framed the same way — which means your damage detection AI has a clean, consistent baseline to work from.

---

## Shot types (13 total)

| ID | Label | Notes |
|----|-------|-------|
| driver_side | Driver Side | Landscape recommended |
| passenger_side | Passenger Side | Landscape recommended |
| front | Front | Landscape recommended |
| rear | Rear | Landscape recommended |
| driver_rocker | Driver Rocker Panel | Crouch low between wheels |
| pass_rocker | Passenger Rocker Panel | Crouch low between wheels |
| wheel | Wheel / Tire | Tread depth check |
| windshield | Windshield | Chip and crack scan |
| hood | Engine Bay | Open hood required |
| trunk | Trunk / Cargo Area | Open trunk/liftgate required |
| driver_door | Driver Door Interior | Open door, shoot across seat |
| dashboard | Dashboard & Odometer | Odometer must be visible |
| undercarriage | Wheel Well | Rust and damage check |

---

## Vehicle archetypes

GhostCar maps Black Book VIN decode output to one of six silhouette templates.

| vehicleType prop | Covers |
|-----------------|--------|
| sedan | Compact, midsize, luxury sedans |
| compact_suv | CX-30, CR-V, RAV4, Kona, etc. |
| midsize_suv | Pilot, Pathfinder, Explorer, etc. |
| large_suv | Tahoe, Expedition, Suburban, etc. |
| truck | F-150, Silverado, Ram, Tundra, etc. |
| van | Transit, Sprinter, Sienna, Odyssey, etc. |

---

## Props

```jsx
<GhostCar
  vehicleType="sedan"
  enabledShots={["driver_side", "passenger_side", "front", "rear", "wheel", "dashboard"]}
  onComplete={(capturedPhotos) => uploadToSupabase(capturedPhotos)}
/>
```

| Prop | Type | Source | Description |
|------|------|--------|-------------|
| vehicleType | string | Black Book VIN decode | Controls which silhouette renders |
| enabledShots | string[] | Dealer admin config | Which shots are shown per rooftop |
| onComplete | function | Your upload handler | Fires with captured photo map when all shots are done |

---

## Dealer configuration

Each rooftop controls its own shot checklist via the enabledShots array. Pass the dealer's saved config from your admin panel. Defaults to all 13 shots if not provided. Dealers can add or remove shots without any code changes.

---

## Mobile camera integration

In Lovable, replace the capture button with a native camera input to open the rear camera directly on mobile:

```jsx
<input
  type="file"
  accept="image/*"
  capture="environment"
  onChange={(e) => handleCapture(e.target.files[0])}
  style={{ display: 'none' }}
  ref={cameraInputRef}
/>
```

Trigger it programmatically: `cameraInputRef.current.click()`

---

## Overlay colors

Three overlay color options are available in-component — green, red, and white. The customer can switch between them if the vehicle color makes one hard to see.

---

## Tech stack

| Layer | Tool |
|-------|------|
| Frontend | React / Lovable |
| VIN decode | Black Book API |
| Vehicle type mapping | Parent component passes vehicleType prop |
| Photo storage | Supabase Storage |
| Damage detection | Runs against stored images post-upload |

---

## File structure

```
GhostCar.jsx    — Self-contained React component, no external dependencies
README.md       — This file
```

---

## Updating this component

1. Open Claude and describe the changes needed
2. Copy the updated component code
3. Commit to this repo (GitHub)
4. Lovable auto-syncs via GitHub integration — no manual steps needed

---

## Part of the HarteCash platform

GhostCar is a component within HarteCash, a white-label vehicle acquisition SaaS built for automotive dealers. HarteCash powers off-street, service drive, and in-store vehicle acquisition channels across multiple rooftops.
