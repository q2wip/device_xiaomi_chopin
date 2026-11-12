# Out-of-tree patches

These two changes live in repositories that are **not** managed under the
q2wip account, so they cannot be pushed. They are kept here as patches and
must be applied after a fresh `repo sync` (the current build tree already
has the commits applied locally).

## 1. hardware/mediatek

Remote: `Eron-Evan/android_hardware_mediatek` (no q2wip fork, no write access)

```
git -C hardware/mediatek am \
    device/xiaomi/chopin/patches/0001-wifi-Build-wifi_hal_fn-from-vendor-symbols-via-dlsym.patch
```

Fixes the WiFi HAL function table: the MIUI 14 (Android 12) vendor
`libwifi-hal-mtk.so` lacks the newer slots of AOSP `wifi_hal_legacy_fn`, so
the table was shifted and callbacks crashed. The patch assembles the table
by resolving every symbol with dlsym.

## 2. vendor/apn

Remote: `LineageOS/android_vendor_apn` (upstream, no q2wip fork)

```
git -C vendor/apn am \
    device/xiaomi/chopin/patches/0001-apn-Prefer-3gnet-over-3gwap-for-China-Unicom-46001.patch
```

Makes `3gnet` the default APN for China Unicom (46001) instead of the
proxy-only `3gwap` APN, which made mobile data unusable (no DNS, only the
WAP proxy worked).

## Check

```
git -C hardware/mediatek am --abort 2>/dev/null || true   # if a previous am is pending
git -C hardware/mediatek log --oneline -1
git -C vendor/apn log --oneline -1
```

After applying, both repos should have one extra commit compared to their
remote branches.
