---
title: ubuntu smb mount
date: 2022-10-10T00:53:55+09:00
last_modified_at: 2022-10-26T01:11:29+09:00
---

```bash
sudo mount -t cifs -o user=SMB유저이름,vers=2.0,noperm,password=비밀번호 FTP주소 마운트할로컬경로
```

[chmod - how do I mount a CIFS share so I can fully control the mounted volume on the client - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/98707/how-do-i-mount-a-cifs-share-so-i-can-fully-control-the-mounted-volume-on-the-cli)

자동 시작을 위해 서비스로 만들었음.

초기엔 와이파이가 연결되지 않아 unreachable 오류가 발생하여 busy wait을 해줌.