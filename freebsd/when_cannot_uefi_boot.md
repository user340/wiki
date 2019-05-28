# RHEL系OSでUEFIブートができないとき

UEFIシェルでは

```
\EFI\centos\grubx64.efi
```

と実行する。

ブートしてログイン後、`/boot/efi/EFI/centos/grubx64.efi`を`/boot/efi/EFI/BOOT/BOOTX64.EFI`としてコピーする。

```
# cp /boot/efi/EFI/centos/grubx64.efi /boot/efi/EFI/BOOT/BOOTX64.EFI
```
