# liblhdcv5

Open-source **LHDC V5** A2DP audio encoder used by Project Infinity-X.

The encoder core is a Rust reimplementation of the Savitech LHDC V5 codec,
ported from the upstream open-source project
[DBeidachazi/liblhdcv5](https://github.com/DBeidachazi/liblhdcv5). It is built
from source, so no proprietary `liblhdcv5.so` is required and the codec is
redistributable in the ROM.

## Layout

- `src/` – Rust encoder core (`rust_ffi_static "liblhdcv5_encoder_rust"`).
- `src/lhdcv5BT_enc.c` – thin C wrapper exposing the Savitech `lhdcv5BT_*` API
  that the Bluetooth A2DP stack `dlopen()`s as `liblhdcv5BT_enc.so`.
- `include/` – headers for the wrapper build.
- `savitech_api/` – Savitech `lhdcv5_api.h` / `lhdcv5BT.h` that the A2DP stack
  includes. The stack loads the wrapper at runtime and only links against it by
  ABI, so these headers are a separate compilation unit from `include/`.

## Build

Soong module (see `Android.bp`): `liblhdcv5BT_enc` (shared, in the
`com.android.bt` apex) wrapping `liblhdcv5_encoder_rust`.

License: Apache-2.0 (see `Android.bp` `default_applicable_licenses`).
