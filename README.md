# Modified Section

Added Docker Compose; start it as follows. It has only been tested locally on localhost so far. Will test it on a server later. Before starting, check your nginx.conf file.

```bash
docker compose up -d
```

# Original Section

# Live2D API

Backend API used for the Live2D mascot plugin (https://www.fghrsh.net/post/123.html).

### Features

- Developed in native PHP, no need for pseudo-static, ready to use out of the box
- Supports sequential and random switching of models and skins
- Supports single model single skin switching, multi-group skin exhaustive combination
- Supports loading and switching multiple models or multiple paths within the same group

## Usage

### Requirements

- PHP version >= 5.2
- Required PHP extension: json

### Directory Structure

```shell
│  model_list.json              // List of models
│
├─model                         // Model path
│  └─GroupName                  // Model group
│      └─ModelName              // Model name
│
├─add                           // Update skin list
├─get                           // Get model configuration
├─rand                          // Randomly switch model
├─rand_textures                 // Randomly switch skin
├─switch                        // Sequentially switch model
├─switch_textures               // Sequentially switch skin
└─tools
        modelList.php           // List of models
        modelTextures.php       // List of skins
        name-to-lower.php       // Filename formatting
```

### Adding a Model

- Single model, single skin switching
  - Loads only one skin at a time
  - Place skins in the `textures` folder, automatically recognized

```shell
│  index.json
│  model.moc
│  textures.cache       // Skin list cache, auto-generated
│
├─motions
│      idle_01.mtn
│      idle_02.mtn
│      idle_03.mtn
│
└─textures
        default-costume.png
        school-costume.png
        winter-costume.png
```

- Single model, multiple skin combinations
  - Combines multiple groups of skins, exhaustive combinations
  - Name skin folders as `texture_XX`
  - Add `textures_order.json` to list combinations

```shell
│  index.json
│  model.moc
│  textures.cache
│  textures_order.json
│
├─motions
│      idle_01.mtn
│      idle_02.mtn
│      idle_03.mtn
│
├─texture_00
│      00.png
│
├─texture_01
│      00.png
│      01.png
│      02.png
│
├─texture_02
│      00.png
│      01.png
│      02.png
│
└─texture_03
       00.png
       01.png
```

textures_order.json

```json
[["texture_00"], ["texture_01", "texture_02"], ["texture_03"]]
```

textures.cache

```json
[
  [
    "texture_00/00.png",
    "texture_01/00.png",
    "texture_02/00.png",
    "texture_03/00.png"
  ],
  [
    "texture_00/00.png",
    "texture_01/00.png",
    "texture_02/00.png",
    "texture_03/01.png"
  ],
  [
    "texture_00/00.png",
    "texture_01/01.png",
    "texture_02/01.png",
    "texture_03/00.png"
  ],
  [
    "texture_00/00.png",
    "texture_01/01.png",
    "texture_02/01.png",
    "texture_03/01.png"
  ],
  [
    "texture_00/00.png",
    "texture_01/02.png",
    "texture_02/02.png",
    "texture_03/00.png"
  ],
  [
    "texture_00/00.png",
    "texture_01/02.png",
    "texture_02/02.png",
    "texture_03/01.png"
  ]
]
```

- Multiple models or paths within the same group switching
  - Modify `model_list.json` to add multiple models

```shell
│
├─model
│  ├─Group1
│  │  ├─Model1
│  │  │      index.json
│  │  │
│  │  └─Model2
│  │          index.json
│  │
│  ├─Group2
│  │  └─Model1
│  │          index.json
│  │
│  └─GroupName
│     └─ModelName
│          │  index.json
│          │  model.moc
│          │
│          ├─motions
│          └─textures
│
```

model_list.json

```json
{
  "models": [
    "GroupName/ModelName",
    ["Group1/Model1", "Group1/Model2", "Group2/Model1"]
  ],
  "messages": ["Example 1", "Example 2"]
}
```

### API Usage

- `/add/` - Detects new skins and updates cache list
- `/get/?id=1-23` - Gets the 23rd skin in group 1
- `/rand/?id=1` - Randomly switches based on the previous group
- `/switch/?id=1` - Sequentially switches based on the previous group
- `/rand_textures/?id=1-23` - Randomly switches other skins in the same group based on the previous skin
- `/switch_textures/?id=1-23` - Sequentially switches other skins in the same group based on the previous skin

## Copyright Notice

> (>▽<) You've read this far, give it a Star ~

**All models within the API are copyrighted by their original authors, for research and learning only, not for commercial use**

MIT © FGHRSH
