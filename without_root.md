# Dockerをroot権限なしで動かす

セキュリティ上のリスクがあるが、占有しているマシン上なら問題はないかと思われる。

```
# chown root.dockerroot /var/run/docker.sock
# usermod -aG dockerroot USERNAME
# systemctl restart docker
```
