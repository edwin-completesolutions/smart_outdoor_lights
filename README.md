# Smart Outdoor Lights

Smart Outdoor Lights is a Home Assistant custom integration that keeps your outdoor light **dim and cozy during the evening** and **bright and safe when something happens** – motion, a door opening, or a button press.

It replaces complex automations/blueprints with one lightweight integration per light, fully configurable from the UI.

Current version: `0.1.11`

---

## How it works (logic overview)

The integration has a simple timeline.

### 1. Night starts when the sun is low enough

You choose a **Sun Elevation Threshold** (e.g. `0°`, `-4°`, etc.).

When `sun_elevation <= elevation_threshold`, the integration enters **night mode**.

### 2. Night mode continues until your Evening End Time

You configure an **Evening End Time**, for example `22:30:00`, `23:00:00`, etc.

Before that time, while in night mode:

#### • Idle → Light at *Dimmed Brightness*  
(e.g. 20%)

#### • Triggered → Light at *Full Brightness*  
(motion, door opening, or button press)

The boost lasts for your configured **Boost Duration**, then returns to **Dimmed Brightness**.

### 3. After the Evening End Time

Once the clock passes the Evening End Time:

- If a boost is active → it finishes first  
- When done → the light turns **off**
- It stays off for the rest of the night  
  (until the sun rises above the threshold again and the next evening comes)

### 4. Daytime

If the sun is **above** the elevation threshold, the integration does nothing:

- No dim mode
- No boosts
- The light is left entirely to other automations/manual control

---

## Requirements

- Home Assistant 2023.0+
- A **dimmable light** (supports brightness)
- A **motion/occupancy/presence** `binary_sensor`
- A **door/opening** `binary_sensor`
- A **sun elevation sensor**, for example:
  - `sensor.sun_solar_elevation` (preferred), or
  - `sensor.sun_elevation`
- Optional: a `button.*` entity for manual brightness boosts

---

## Installation

### 1. Copy the custom component

Place the files like this:

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
