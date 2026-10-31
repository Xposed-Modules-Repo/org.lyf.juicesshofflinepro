# JuiceSSH Offline Pro Fix

Legacy Xposed/LSPosed API 82-compatible module for keeping a legitimate JuiceSSH Pro entitlement usable offline after the original Google/Sonelli verification path disappeared.

## Source / 引用来源

This Xposed-Modules-Repo entry republishes the upstream project by `li-yifei`:

- Upstream repository: <https://github.com/li-yifei/JuiceSSHOfflineProFix>
- Upstream author: `li-yifei`
- Package name: `org.lyf.juicesshofflinepro`
- Canonical source: <https://github.com/li-yifei/JuiceSSHOfflineProFix>

This mirror keeps the original license and source history context through explicit attribution.

## 中文说明

这是一个面向 LSPosed / Xposed API 82 的旧版模块，用于在 JuiceSSH 原有 Google / Sonelli 联网校验路径失效后，让**已经合法购买 JuiceSSH Pro / Pro Pack 的用户**继续离线使用对应权益。

适用场景：

- 你过去已经购买过 JuiceSSH Pro 或 Pro Pack
- 现在原有校验接口已经失效
- 你希望在本地继续恢复自己原本拥有的专业版能力

使用边界：

- 该项目面向归档、兼容性恢复、个人自救使用
- 适用于合法拥有权益的用户
- 请自行确认所在地法律、软件许可与平台条款

## Disclaimer

This project is intended for archival, interoperability, and personal recovery use by people who legitimately own JuiceSSH Pro / Pro Pack and need continued access after the original app, backend, and Google Play purchase flow became unavailable. Do not use it to obtain paid functionality you did not purchase or are not entitled to. You are responsible for complying with all applicable laws, licenses, and terms in your jurisdiction.

This project is not affiliated with, endorsed by, or supported by Sonelli Ltd., JuiceSSH, Google, LSPosed, or Xposed.

## License

This project is licensed under the GNU Affero General Public License v3.0 or later. See [LICENSE](LICENSE).

## Release APK

Download the signed APK from GitHub Releases:

```text
JuiceSSHOfflineProFix-v1.0-xposed82-release.apk
```

Package:

```text
org.lyf.juicesshofflinepro
```

Compatibility:

- Xposed API 82 / LSPosed legacy module API
- `minSdk 26` to match JuiceSSH 3.2.2
- `targetSdk 36`

## Hooks

Recommended LSPosed scope is declared by the module as:

```text
com.sonelli.juicessh
```

The module hooks:

- `com.sonelli.oi0.d(Object)` -> `true` (central Pro check)
- `com.sonelli.juicessh.models.User.H()` -> `true` (local user signature gate)
- `User.w()` -> configurable future session time (prevents dead API refresh)
- `com.sonelli.pi0.m(Context)` -> `true`
- `com.sonelli.pi0.j(Context, pi0.p)` -> calls the callback with the local user instead of forcing the dead network refresh path

## Settings

The module includes a minimal native Android settings page with no extra UI libraries. Defaults are suitable for most cases.

After changing settings, force-stop and reopen JuiceSSH. Some LSPosed versions may require a reboot.

## Install

```bash
adb install -r JuiceSSHOfflineProFix-v1.0-xposed82-release.apk
```

Then in LSPosed:

1. Enable `JuiceSSH Offline Pro Fix`.
2. Confirm the scope includes `JuiceSSH` / `com.sonelli.juicessh`.
3. Force-stop and reopen JuiceSSH, or reboot.

## Build

```bash
cd lsposed-juicessh-offline-pro
gradle assembleDebug --no-daemon
```

This branch uses legacy metadata:

- `assets/xposed_init`
- AndroidManifest `xposedmodule`, `xposeddescription`, `xposedminversion=82`, `xposedscope`

The dependency is a local compile-only stub jar:

```gradle
compileOnly files('libs/xposed-api-82-stub.jar')
```

The stub is for compilation only and is not packaged into the APK; the actual Xposed classes are provided by LSPosed/Xposed at runtime.
