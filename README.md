# LoomaXRViewer

UE 5.6 viewer for the LoomaXR asset demo: mirrors the web app's scene in real
time through the `LoomaSceneSync` plugin (spawn/move/delete flow both ways;
meshes stream at runtime from the demo backend's datalake).

Companion repo: `looma-xr-asset-demo` (backend + web app + sync protocol docs
in `docs/unreal-sync.md` and `unreal/README.md`).

## Clone & build

```
git clone --recurse-submodules <this-repo>
```

Right-click `LoomaXRViewer.uproject` → *Generate Visual Studio project files*,
build **Development Editor**, or just open the `.uproject` and let it compile.

Backend address lives in `Config/DefaultGame.ini`
(`[/Script/LoomaSceneSync.LoomaSceneSyncSubsystem] BackendHost=...`). Use an
explicit IP, not `localhost` (UE may resolve it to IPv6 while uvicorn listens
on IPv4).

## Run

1. Start the demo backend: `uv run uvicorn app.main:app --port 8000`
   (in `looma-xr-asset-demo/backend`).
2. Press **Play (PIE)**. The sync subsystem connects automatically and
   mirrors the shared scene. Press F8 to eject and move synced actors with
   the editor gizmo — they move live in the web app.

## Plugins

- `Plugins/glTFRuntime`, `Plugins/glTFRuntimeWebP` — git submodules
  (runtime GLB loading; WebP is required — the datalake textures are WebP).
- `Plugins/LoomaSceneSync` — committed copy. The source of truth is
  `looma-xr-asset-demo/unreal/LoomaSceneSync`; sync changes back and forth
  with robocopy when editing.
