# Smart Outdoor Lights

Smart Outdoor Lights is a Home Assistant custom integration that keeps your outdoor light **dim and cozy in the evening** and **bright and safe when something happens** – motion, a door opening, or a button press.

It replaces complex automations/blueprints with one small integration per light that:

- Uses a configurable **sun elevation sensor** to detect “night”
- Keeps the light at a **low brightness in the evening**
- Temporarily boosts to **full brightness** on motion, door open, or button press
- Turns the light **fully off after your configured evening end time**
- Lets you fine-tune everything via the **UI (config flow + options flow)**

Current version: `0.1.11`

---

## How it works (logic overview)

For each configured entry:

- **Daytime (sun elevation above threshold)**  
  - The integration leaves your light alone.
  - Motion/door/button triggers are ignored until it becomes “night”.

- **Nighttime before evening end**  
  - When sun elevation ≤ your **Sun Elevation Threshold**, “night logic” becomes active.
  - Between **12:00** and your **Evening End Time**, the light behaves as:
    - **Idle**: on at `Dimmed Brightness`
    - **Triggered (motion/door/button)**: on at `Full Brightness` for `Boost Duration` seconds

- **After evening end time**  
  - The evening window ends; the light will:
    - Finish any active boost
    - Then turn **off completely**

- **Button behavior**
  - Button is optional.
  - If configured, pressing it during the evening/night window behaves like a boost trigger.
  - Boost duration and brightness are the same as for motion/door.

The integration does not create entities of its own; it simply **controls your existing light** using `light.turn_on` / `light.turn_off` with brightness.

---

## Requirements

- Home Assistant 2023.0+ (async config flow, options flow)
- One dimmable **light** entity (any `light.*` that supports brightness)
- One **motion** (or occupancy/presence) `binary_sensor`  
  Device class: `motion`, `occupancy` or `presence`
- One **door/opening** `binary_sensor`  
  Device class: `door` or `opening`
- A **sun elevation sensor**  
  - Preferably `sensor.sun_solar_elevation`  
  - Fallback: `sensor.sun_elevation`  
  - Or any numeric `sensor` that reports degrees
- Optional: a **button** entity (`button.*`) to manually trigger boosts

---

## Installation

### 1. Copy the custom component

Clone or download this repository and copy the `smart_outdoor_lights` folder to:

```text
<config>/
  custom_components/
    smart_outdoor_lights/
      __init__.py
      manifest.json
      config_flow.py
      light_manager.py
      const.py
      translations/
        en.json
