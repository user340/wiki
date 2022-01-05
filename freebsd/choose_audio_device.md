# オーディオデバイスを切り替える

## 参考

https://wiki.freebsd.org/Sound

## Mixerでオーディオデバイスを切り替える

`/dev/sndstat` を見てオーディオデバイスの一覧を表示する。
`pcm[N]` の `[N]` を `hw.snd.default_unit` に設定する。

```
$ cat /dev/sndstat
Installed devices:
pcm0: <NVIDIA (0x0080) (HDMI/DP 8ch)> (play)
pcm1: <NVIDIA (0x0080) (HDMI/DP 8ch)> (play)
pcm2: <NVIDIA (0x0080) (HDMI/DP 8ch)> (play)
pcm3: <Realtek ALCS1200A (Rear Analog)> (play/rec)
pcm4: <Realtek ALCS1200A (Front Analog)> (play/rec)
pcm5: <Realtek ALCS1200A (Internal Digital)> (play)
pcm6: <USB audio> (play/rec) default
No devices installed from userspace.
$ sysctl hw.snd.default_unit=6
hw.snd.default_unit: 3 -> 6
```
