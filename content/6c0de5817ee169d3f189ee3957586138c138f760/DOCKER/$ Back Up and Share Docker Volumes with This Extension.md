Refer: [link](https://www.docker.com/blog/back-up-and-share-docker-volumes-with-this-extension/)

back up volumes from Docker Desktop with extension
You can now back up volumes with just a few clicks using the new Volumes Backup & Share extension. This [extension](https://hub.docker.com/extensions/docker/volumes-backup-extension) is available in the Marketplace and works on macOS, Windows, and Linux. And you can check out the OSS code on [GitHub](https://github.com/docker/volumes-backup-extension) to see how the extension was developed.

![Volumes backup share](https://www.docker.com/wp-content/uploads/2022/09/volumes-backup-share-extension.gif "- Volumes Backup Share")

_How to back up a volume to a local file in your host_

## What can I do with the extension?

The extension allows you to:

- Back up data that is persisted in a volume (for example, database data from Postgres or MySQL) into a compressed file.

- Upload your backup to Docker Hub and share it with anyone.

- Create a new volume from an existing backup or restore the state of an existing volume.

- Transfer your local volumes to a different Docker host (through SSH).

- Other basic volume operations like clone, empty, and delete a volume.

In the scenario below, John, Alex, and Emma are using Docker Desktop with the Volume Backup & Share extension. John is using the extension to share his volume (my-app-volume) with the rest of their teammates via Docker Hub. The volume is uploaded to Docker Hub as an image (`john/my-app-volume:0.0.1`) by using the “Export to Registry” option. His colleagues, Alex and Emma, will use the same extension to import the volume from Docker Hub into their own volumes by using the “Import from Registry” option.

![Share volume docker](https://www.docker.com/wp-content/uploads/2022/09/share-volume-docker.png "- Share Volume Docker")
