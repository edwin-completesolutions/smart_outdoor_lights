
the integration enters **night mode**.

### 2. **Night mode continues until your Evening End Time**
You configure an **Evening End Time**, for example `22:30:00`, `23:00:00`, etc.

Before that time:

#### • Idle → Light at *Dimmed Brightness*  
(e.g. 20%)

#### • Triggered → Light at *Full Brightness*  
(motion, door opening, or button press)

The boost lasts for your configured **Boost Duration**, then returns to **Dimmed Brightness**.

### 3. **After the Evening End Time**
Once the clock passes Evening End Time:

- If a boost is active → it finishes first  
- When done → the light turns **off**
- It stays off for the rest of the night  
  (until the sun rises above the threshold again)

### 4. **Daytime**
If the sun is **above** the elevation threshold, the integration does nothing.

No dim mode, no boosts.

---

## Requirements

- Home Assistant 2023.0+
- A **dimmable light** (supports brightness)
- A **motion/occupancy/presence** `binary_sensor`
- A **door/opening** `binary_sensor`
- A **sun elevation sensor**  
  - e.g. `sensor.sun_solar_elevation` or `sensor.sun_elevation`
- Optional: a `button.*` entity for manual brightness boosts

---

## Installation

### 1. Copy the custom component

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
