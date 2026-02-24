# AcousticMaterial Reference

`AcousticMaterial` is a `Resource` that defines how a surface interacts with sound — how much it absorbs, how diffusely it scatters, and how much passes through it.

**Class:** `AcousticMaterial` 
**Inherits:** `Resource` 
**Script:** `acoustic_material.gd`

---

## Concepts

### Absorption

Fraction of sound energy absorbed when sound **reflects** off the surface. Used by `SpatialAudioPlayer3D` to modulate reverb wetness and damping.

- **High absorption** (carpet, gravel) → less reverb energy, shorter tail.
- **Low absorption** (concrete, glass) → more reverb energy, longer tail.

Specified for three frequency bands:
- **Low** (≤ 400 Hz)
- **Mid** (400–2 500 Hz)
- **High** (≥ 2 500 Hz)

### Scattering

How diffusely the surface reflects sound.

- `0.0` = perfect specular reflection (like a mirror for sound).
- `1.0` = fully diffuse/scattered.

> **Note:** Scattering is stored on the resource but is not yet directly used in the current reverb model. It is reserved for future diffuse reflection improvements.

### Transmission

Fraction of sound energy that **passes through** the surface. Used by the occlusion system to determine how muffled a sound becomes behind a wall.

- `0.0` = fully blocks (soundproof).
- `1.0` = fully transparent.

`transmission_high` drives the **lowpass cutoff** — low values heavily muffle high frequencies. 
`transmission_low` drives the **volume reduction** — low values remove bass energy, reducing overall level.

---

## Exported Properties

### Absorption

| Property | Range | Default | Description |
|---|---|---|---|
| `absorption_low` | 0.0–1.0 | `0.10` | Fraction of low-frequency energy absorbed on reflection (≤ 400 Hz). |
| `absorption_mid` | 0.0–1.0 | `0.20` | Fraction of mid-frequency energy absorbed on reflection (400–2 500 Hz). |
| `absorption_high` | 0.0–1.0 | `0.30` | Fraction of high-frequency energy absorbed on reflection (≥ 2 500 Hz). |

### Scattering

| Property | Range | Default | Description |
|---|---|---|---|
| `scattering` | 0.0–1.0 | `0.05` | How diffusely the surface reflects sound. `0` = specular, `1` = fully diffuse. |

### Transmission

| Property | Range | Default | Description |
|---|---|---|---|
| `transmission_low` | 0.0–1.0 | `0.100` | Fraction of low-frequency energy passing through (≤ 400 Hz). Affects volume reduction. |
| `transmission_mid` | 0.0–1.0 | `0.050` | Fraction of mid-frequency energy passing through (400–2 500 Hz). |
| `transmission_high` | 0.0–1.0 | `0.030` | Fraction of high-frequency energy passing through (≥ 2 500 Hz). Drives the lowpass cutoff. |

---

## Creating a Custom Material

1. In the **FileSystem**, right-click and choose **New Resource**.
2. Search for and select `AcousticMaterial`.
3. Save the file (e.g. `res://audio/materials/thin_wood_panel.tres`).
4. Open the resource in the Inspector and adjust the values.
5. Assign it to an `AcousticBody`.

Or create one in code:

```gdscript
var mat := AcousticMaterial.new()
mat.absorption_low = 0.15
mat.absorption_mid = 0.10
mat.absorption_high = 0.08
mat.scattering = 0.20
mat.transmission_low = 0.08
mat.transmission_mid = 0.04
mat.transmission_high = 0.02
my_acoustic_body.acoustic_material = mat
```

---

## Static Preset Constructors

The `AcousticMaterial` class includes static helper methods that return pre-built instances of common real-world materials. These match the `.tres` presets bundled with the plugin.

| Method | Returns |
|---|---|
| `AcousticMaterial.preset_generic()` | A balanced generic surface |
| `AcousticMaterial.preset_brick()` | Brick wall |
| `AcousticMaterial.preset_concrete()` | Concrete wall |
| `AcousticMaterial.preset_ceramic()` | Ceramic tile |
| `AcousticMaterial.preset_gravel()` | Gravel / loose stone |
| `AcousticMaterial.preset_carpet()` | Carpet |
| `AcousticMaterial.preset_glass()` | Glass |
| `AcousticMaterial.preset_plaster()` | Plaster / drywall |
| `AcousticMaterial.preset_wood()` | Wood panel |
| `AcousticMaterial.preset_metal()` | Metal sheet |
| `AcousticMaterial.preset_rock()` | Natural rock |

```gdscript
# Use a preset material at runtime
my_acoustic_body.acoustic_material = AcousticMaterial.preset_glass()
```

See the **[[Material Presets]]** page for full numeric values for all presets.
