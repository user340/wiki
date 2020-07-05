# FreeRDP

## CtrlとCapsを入れ替える

```
--- libfreerdp/locale/keyboard_xkbfile.c.orig     2020-07-05 10:36:47.441630000 +0900
+++ libfreerdp/locale/keyboard_xkbfile.c  2020-07-05 10:37:28.279571000 +0900
@@ -105,7 +105,7 @@
        { "BKSL", RDP_SCANCODE_OEM_5 },     // evdev 51
        { "BKSP", RDP_SCANCODE_BACKSPACE }, // evdev 22
        // { "BRK",  RDP_SCANCODE_ },   // evdev 419
-       { "CAPS", RDP_SCANCODE_CAPSLOCK }, // evdev 66
+       { "CAPS", RDP_SCANCODE_LCONTROL }, // evdev 66
        { "COMP", RDP_SCANCODE_APPS },     // evdev 135
        // { "COPY", RDP_SCANCODE_ },   // evdev 141
        // { "CUT",  RDP_SCANCODE_ },   // evdev 145
```

portsでは `net/freerdp` で `make extract` したあと、 `work` ディレクトリでパッチをあて `make package` する。パッケージは `work/pkg` に生成される。
