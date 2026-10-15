# DXMT

A Metal-based translation layer for Direct3D 11 and 10 which allows running 3D applications on macOS using Wine.

For the current status of the project, please refer to the [project wiki](https://github.com/3Shain/dxmt/wiki).

The most recent development builds can be found [here](https://github.com/3Shain/dxmt/actions).


## This fork

Zagrios/dxmt builds the native `libdxmt-native.dylib` that [Voile](https://github.com/Zagrios/voile) ships for Obelisk's Linux engine, where [Neptune](https://github.com/Zagrios/neptune)'s render server loads it. It is upstream DXMT, merged up to 3Shain/dxmt's main of 2026-10-07, plus:

- the commits of [UTM](https://github.com/utmapp/dxmt)'s native backend this use needs: the single native dylib, shared textures and events with their version 2 handle, the Metal pipeline and error-retain crash fixes, typed integer render-target outputs, an occlusion query that ends in a draw-less chunk, and `UpdateSubresource` into STAGING resources;
- a 16-bit `WCHAR` in native builds, so the structures an embedder hands in keep Windows' layout.

The `upstream` branch follows 3Shain/dxmt; `main` takes its new commits by merging it.

## Build

See [DEVELOPMENT.md](docs/DEVELOPMENT.md)

## Native backend

A native (non-Wine) macOS build links every module into a single
`libdxmt-native.dylib` that exports the standard D3D11/DXGI/D3D10 entry
points plus a small embedder API (`dxmt_native.h`): Win32-style events
(`dxmt_event_*`) and cross-process shared textures over POSIX shm. An
embedder `dlopen()`s the one dylib; `pkg-config --cflags --libs
dxmt-native` provides the include and link flags. Build it by configuring
without a cross file (see [DEVELOPMENT.md](docs/DEVELOPMENT.md)).
