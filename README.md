# gpui-patched

> **Branch `windows-shaders-only`.** Uncommon Rares is moving off this fork. On this branch only
> change 2 below (embedded HLSL, `gpui-pre-windows`) is still applied; changes 1 and 3 are reverted
> and changes 4 and 5 are no longer used. The game patches `gpui-pre-windows` alone from here and
> takes every other crate from crates.io. Change 3 landed upstream in Zed PR #66799.

The few `gpui-pre` and `gpui-kit` crates Uncommon Rares builds with local
changes. Each directory is the unmodified crates.io archive of that crate plus
the commits listed below. Everything else resolves from crates.io.

| Directory          | Upstream            |
| ------------------ | ------------------- |
| `gpui-pre`         | `gpui-pre` 0.3.12   |
| `gpui-pre-macos`   | `gpui-pre-macos` 0.3.12 |
| `gpui-pre-linux`   | `gpui-pre-linux` 0.3.12 |
| `gpui-pre-windows` | `gpui-pre-windows` 0.3.12 |
| `gpui-pre-wgpu`    | `gpui-pre-wgpu` 0.3.12 |
| `gpui-base`        | `gpui-base` 0.9.1   |
| `gpui-component`   | `gpui-component` 0.9.1 |

## Changes

`git log` holds one commit per change on top of the two import commits.

1. **Cancellable termination** (`gpui-pre`, `-macos`, `-linux`, `-windows`).
   `Platform::on_should_quit` installs a guard that is asked before native
   termination commits: programmatic quit and restart everywhere, AppKit
   terminate requests on macOS, session-end queries on Windows. Upstream only
   offers `on_app_quit`, which cannot cancel and waits a fixed 200 ms.
2. **Embedded HLSL** (`gpui-pre-windows`). A Windows release target built on
   another host compiles its shaders at startup, because FXC only runs on a
   Windows build host.
3. **Shaping offsets** (`gpui-pre-wgpu`). Glyph positions include cosmic-text's
   shaping offsets, so combining marks keep their anchors.
4. **Accessible names** (`gpui-base`, `gpui-component`). `AlertDialog`,
   `Popover` and the component `Dialog` take `accessibility_label`, as
   gpui-base's `Dialog` already does.
5. **Disabled buttons** (`gpui-component`). A disabled button keeps its variant
   colours and fades to half.

## Use

```toml
[patch.crates-io]
gpui-pre = { git = "https://github.com/Tavernpunk/gpui-patched", rev = "<commit>" }
# ...one entry per directory, all at the same commit
```

## Moving to a new upstream release

Replace each directory with the new crates.io archive in one import commit,
then cherry-pick the change commits onto it. Drop a change once upstream
provides it.

The sources are Apache-2.0, as upstream; see each crate's `LICENSE-APACHE`.
