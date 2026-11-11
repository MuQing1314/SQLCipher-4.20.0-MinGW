[简体中文](README.md) | **English**

# SQLCipher 4.20.0 MinGW-w64 Builds (Unofficial)

Unofficial prebuilt binaries of SQLCipher 4.20.0 community for MinGW-w64 toolchains.

- **Toolchain**: MinGW-w64 GCC 13.1.0 (bundled with Qt, posix / SEH / MSVCRT)
- **OpenSSL**: 3.5.9, both static and dynamic linking available
- **Build types**: Debug (`-g -O0 -DSQLITE_DEBUG`) and Release (`-O2`)
- **Verification**: `PRAGMA cipher_version;` returns `4.20.0 community`

> ⚠️ **Not for MSVC**: This repository provides MinGW-format `.a` / `.dll.a` / `.dll` files. MSVC projects cannot link them directly. MSVC users should build with the MSVC toolchain and MSVC-built OpenSSL.
>
> This is a community unofficial build, not affiliated with Zetetic LLC.

## Directory Overview

| Directory | Build | OpenSSL Linking | Runtime DLLs |
|---|---|---|---|
| `debug/` | Debug | Static | Not required |
| `release/` | Release | Static | Not required |
| `debug-dyn/` | Debug | Dynamic | Required, bundled |
| `release-dyn/` | Release | Dynamic | Required, bundled |

## File Overview

| File | Purpose |
|---|---|
| `sqlite3.exe` | SQLCipher command-line tool |
| `libsqlite3.a` | Static library (MinGW) |
| `libsqlite3.dll.a` | DLL import library (MinGW) |
| `libsqlite3-0.dll` | Dynamic library |
| `sqlite3.h` / `sqlite3ext.h` | C headers |
| `libcrypto-3-x64.dll` | OpenSSL crypto library (dynamic builds only) |
| `libssl-3-x64.dll` | OpenSSL SSL library (dynamic builds only) |
| `libwinpthread-1.dll` | MinGW thread library (dynamic builds only) |

## Static Linking

Binaries in `debug/` and `release/` have OpenSSL 3.5.9 statically linked. No OpenSSL DLLs are required at runtime — suitable for single-file deployment or avoiding DLL version conflicts.

## Dynamic Linking

Binaries in `debug-dyn/` and `release-dyn/` depend at runtime on:

- `libcrypto-3-x64.dll`
- `libssl-3-x64.dll`
- `libwinpthread-1.dll`

These DLLs are bundled. Windows searches the executable's directory first, so placing the exe and these DLLs in the same folder is sufficient — no PATH changes needed.

**Note**: `debug-dyn/` ships Debug OpenSSL DLLs, `release-dyn/` ships Release OpenSSL DLLs. Do not mix them.

## Verification

```bash
./sqlite3.exe
PRAGMA cipher_version;
```
```bash
-- 4.20.0 community
```
```bash
PRAGMA key='test123';
CREATE TABLE t(x);
INSERT INTO t VALUES(1);
SELECT * FROM t;
```
```bash
-- 1
```
