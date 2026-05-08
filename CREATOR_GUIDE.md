# Custom Cosmetics — Creator Guide

> A step-by-step walkthrough for adding custom 3D models and textures to the mod, and configuring them via `server_cosmetics.json`.

---

## Table of Contents

- [Step 1: Adding Your Assets](#-step-1-adding-your-assets)
- [Step 2: Configuring server_cosmetics.json](#-step-2-configuring-server_cosmeticsjson)
- [Step 3: Understanding the Variables](#-step-3-understanding-the-variables)
- [Step 4: Hiding Player Body Parts](#-step-4-hiding-player-body-parts-optional)
- [Step 5: Adding Trims](#-step-5-adding-trims-optional)
- [Tips & Live Reloading](#-tips--live-reloading)

---

## 🛠️ Step 1: Adding Your Assets

Before writing any configuration, place your assets into a resource pack or directly into the mod's `.jar`.

- **Textures** — Place `.png` files inside:
  ```
  assets/customcosmetics/textures/
  ```
  We recommend organizing by type, for example:
  ```
  textures/hats/
  textures/capes/
  textures/belts/
  ```

- **Models** — If using custom Java models exported from Blockbench, ensure they are compiled into the mod. If you are using base models (standard capes, simple hats), you only need the textures.

---

## 📝 Step 2: Configuring server_cosmetics.json

The `server_cosmetics.json` file is the bridge between your textures, models, and how they appear in the player's GUI.

Here is the base template for adding a new cosmetic:

```json
"CategoryName": {
  "my-custom-item-id": {
    "isFree": "true",
    "modelType": "humanoid",
    "GuiAlignX": 0,
    "GuiAlignY": 15,
    "GuiScale": 1,
    "GuiRotation": 1.4,
    "GuiPitch": -1.0,
    "disablePlayerPart": [0],
    "Models": [
      {
        "location": "customcosmetics:textures/hats/my_cool_hat.png",
        "model_location": "net.mcreator.customcosmetics.client.model.Modelhat"
      }
    ]
  }
}
```

---

## 🧩 Step 3: Understanding the Variables

| Key | Description |
|---|---|
| `CategoryName` | The top-level group — e.g. `"Hats"`, `"Backpacks"`, `"Belts"` |
| `my-custom-item-id` | The unique internal ID for your cosmetic |
| `isFree` | Set to `"true"` to allow all players to use it without unlocking |
| `modelType` | Usually `"humanoid"` |
| `GuiAlignX` / `GuiAlignY` | Position of the preview in the customization menu |
| `GuiScale` | Size of the preview in the GUI |
| `GuiRotation` / `GuiPitch` | Rotation and pitch of the preview model |
| `Models[].location` | Path to your texture file |
| `Models[].model_location` | Fully qualified Java class path to your Blockbench model |

> **Tip:** GUI values (`GuiAlignX`, `GuiAlignY`, `GuiScale`, etc.) can be tweaked live. Save the file and changes apply instantly in-game — no restart needed.

---

## 🫥 Step 4: Hiding Player Body Parts *(Optional)*

When your cosmetic fully covers a vanilla body part, hide that part to prevent Z-fighting (clipping artifacts).

Add the `disablePlayerPart` array to your cosmetic's configuration:

```json
"disablePlayerPart": [0, 8]
```

### Body Part Index Reference

| Index | Body Part | Index | Body Part |
|---|---|---|---|
| `0` | Head | `6` | Right Leg |
| `1` | Body | `7` | Left Leg |
| `2` | Right Arm | `8` | Head Wear (Layer 2) |
| `3` | Left Arm | `9` | Body Wear (Layer 2) |
| `4` | Right Arm Wear | `10` | Right Leg Wear |
| `5` | Left Arm Wear | `11` | Left Leg Wear |

**Example:** A large helmet that covers the full head should use `[0, 8]` to hide both the Head and the Head Wear skin layer.

---

## ✨ Step 5: Adding Trims *(Optional)*

Trims let you define color variants of a cosmetic without creating entirely new items. Each trim references a replacement texture.

```json
"my-trimmed-wings": {
  "isFree": "true",
  "Models": [
    {
      "location": "customcosmetics:textures/wings/base_wings.png",
      "model_location": "net.mcreator.customcosmetics.client.model.ModelWings"
    }
  ],
  "trims": [
    {
      "internalWings": {
        "blue_trim": {
          "isFree": true,
          "location": "customcosmetics:textures/wings/trims/blue.png"
        },
        "red_trim": {
          "isFree": true,
          "location": "customcosmetics:textures/wings/trims/red.png"
        }
      }
    }
  ]
}
```

Each entry in `trims` maps a trim ID to a replacement texture path. The base model is reused; only the texture changes.

---

## ⚡ Tips & Live Reloading

Thanks to the mod's built-in live cache-reloading system, you **do not need to restart your game** when tweaking `server_cosmetics.json` values.

Just save the file in your text editor — changes apply instantly in-game.

This makes iterating on GUI alignment, scale, and rotation very fast. A recommended workflow:

1. Add your cosmetic entry with placeholder GUI values.
2. Open the customization menu in-game.
3. Adjust `GuiAlignX`, `GuiAlignY`, `GuiScale`, `GuiRotation`, and `GuiPitch` in the file.
4. Save → check in-game → repeat until it looks right.

---

*For questions or contributions, open an issue or pull request in this repository.*
