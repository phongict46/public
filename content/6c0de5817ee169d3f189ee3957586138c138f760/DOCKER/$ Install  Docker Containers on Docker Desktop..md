**Run in Terminal macos**

- Install *libreoffice*
	```bash
	docker run -d \
	--name=libreoffice \
	--security-opt seccomp=unconfined \
	-e PUID=1000 \
	-e PGID=1000 \
	-e TZ=Etc/UTC \
	-p 3005:3000 \
	-p 3006:3001 \
	--restart unless-stopped \
	lscr.io/linuxserver/libreoffice:latest
	```

- Install *Obsidian*
  ```bash
	docker run -d \
	--name=obsidian \
	--security-opt seccomp=unconfined \
	-e PUID=1000 \
	-e PGID=1000 \
	-e TZ=Etc/UTC \
	-p 3003:3000 \
	-p 3004:3001 \
	--shm-size="1gb" \
	--restart unless-stopped \
	lscr.io/linuxserver/obsidian:latest
	```

- Install *Kali-linux*
```bash
docker run -d \
--name=kali-linux \
--security-opt seccomp=unconfined '#optional' \
-e PUID=1000 \
-e PGID=1000 \
-e TZ=Etc/UTC \
-e SUBFOLDER=/ '#optional' \
-e TITLE="Kali Linux" '#optional' \
-p 3011:3000 \
-p 3009:3001 \
--device /dev/dri:/dev/dri '#optional' \
--shm-size="1gb" '#optional' \
--restart unless-stopped \
lscr.io/linuxserver/kali-linux:latest
```
