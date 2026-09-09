# Mystievous AMP Templates

Custom [CubeCoders AMP](https://cubecoders.com/AMP) Generic Module templates.

## Templates

### Knockout City

Linux support added on top of [Greelan's](https://github.com/Greelan/AMPTemplates/tree/dev) Windows-only template: 
`Meta.OS` gains Linux, `XDG_RUNTIME_DIR` is set for the Wine prefix, and command line parameters are quoted.

### Pumpkin

Fork of the CubeCoders template with two changes.

**Config schema updated.** The stock template still writes Pumpkin's old flat
config layout. Pumpkin moved those keys into sections and silently drops
anything it doesn't recognise when it re-serialises `pumpkin.toml` on first
start, so the affected settings never took effect:

| Old key                                | Current location                      |
| -------------------------------------- | ------------------------------------- |
| `java_edition`, `java_edition_address`  | `[networking.java]`                   |
| `bedrock_edition`                       | `[networking.bedrock]`                |
| `bedrock_edition_address`               | `[networking.bedrock.nethernet]`      |
| `max_players`, `view_distance`, `simulation_distance`, `online_mode`, `encryption`, `motd` | `[networking.java]` |
| `[networking.authentication]`           | `[networking.java.authentication]`    |
| `[networking.java_compression]`         | `[networking.java.compression]`       |
| `[networking.bedrock_compression]`      | `[networking.bedrock.compression]`    |

Settings for keys Pumpkin removed outright were dropped: `world.chunk`
linear/compression options, `logging.env`, and the two
`networking.authentication` auth URL fields. Bedrock now has its own player
limit, MOTD, online mode and distances, since Pumpkin keeps those separately
from the Java side. Telemetry is exposed as a setting.

The practical symptom of the old template was the Bedrock listener binding
19132 regardless of the port AMP assigned, which collides with any other
Bedrock server on the same host.

**Version pinning.** The stock template hardcodes the rolling `nightly`
release, so there is no way to choose a build. I added a `PumpkinVersion`
setting which feeds the download URL. `nightly` tracks the rolling build,
anything else is treated as an exact release tag, like `0.1.0-dev+26.2-26.45`.

`pumpkinupdates.json` fetches `pumpkin.toml` from this repository rather than
the CubeCoders CDN, so the shipped config matches the settings definitions.

## Upstream

The pumpkin schema relates to [CubeCoders/AMPTemplates](https://github.com/CubeCoders/AMPTemplates).
The Knockout City Linux fix relates to [Greelan/AMPTemplates](https://github.com/Greelan/AMPTemplates) on its `dev` branch. 
