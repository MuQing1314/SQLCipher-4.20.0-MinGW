**简体中文** | [English](README.en.md)

# SQLCipher 4.20.0 MinGW-w64 构建（非官方）

面向 MinGW-w64 工具链用户的 SQLCipher 4.20.0 community 版非官方预编译产物。

- **工具链**：MinGW-w64 GCC 13.1.0（Qt 自带，posix / SEH / MSVCRT）
- **OpenSSL**：3.5.9，提供静态链接和动态链接两种版本
- **构建类型**：Debug（`-g -O0 -DSQLITE_DEBUG`）与 Release（`-O2`）
- **验证**：`PRAGMA cipher_version;` 输出 `4.20.0 community`

> ⚠️ **不适用于 MSVC**：本仓库提供的是 MinGW 格式的 `.a` / `.dll.a` / `.dll`，MSVC 项目无法直接链接。MSVC 用户请使用 MSVC 工具链 + MSVC 版 OpenSSL 自行编译。
>
> 本仓库为社区非官方构建，与 Zetetic LLC 无关。

## 目录说明

| 目录 | 构建 | OpenSSL 链接 | 运行时额外 DLL |
|---|---|---|---|
| `debug/` | Debug | 静态 | 不需要 |
| `release/` | Release | 静态 | 不需要 |
| `debug-dyn/` | Debug | 动态 | 需要，已随包附带 |
| `release-dyn/` | Release | 动态 | 需要，已随包附带 |

## 文件说明

| 文件 | 用途 |
|---|---|
| `sqlite3.exe` | SQLCipher 命令行工具 |
| `libsqlite3.a` | 静态库（MinGW） |
| `libsqlite3.dll.a` | DLL 导入库（MinGW） |
| `libsqlite3-0.dll` | 动态库 |
| `sqlite3.h` / `sqlite3ext.h` | C 头文件 |
| `libcrypto-3-x64.dll` | OpenSSL 加密库（仅动态版附带） |
| `libssl-3-x64.dll` | OpenSSL SSL 库（仅动态版附带） |
| `libwinpthread-1.dll` | MinGW 线程库（仅动态版附带） |

## 静态链接版说明

`debug/` 和 `release/` 中的 `sqlite3.exe` 和 `libsqlite3-0.dll` 已静态链接 OpenSSL 3.5.9，运行时**不需要**任何 OpenSSL DLL，适合单文件部署或避免 DLL 版本冲突的场景。

## 动态链接版说明

`debug-dyn/` 和 `release-dyn/` 中的 `sqlite3.exe` 和 `libsqlite3-0.dll` 在运行时依赖：

- `libcrypto-3-x64.dll`
- `libssl-3-x64.dll`
- `libwinpthread-1.dll`

这些 DLL 已随包附带。Windows 加载 DLL 时优先搜索 exe 所在目录，将 exe 和这些 DLL 放在同一目录即可直接运行，无需修改 PATH。

**注意**：`debug-dyn/` 附带的是 Debug 版 OpenSSL DLL，`release-dyn/` 附带的是 Release 版 OpenSSL DLL，两者不要混用。

## 验证

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
