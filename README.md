# sap-netweaver-rhel-qrg
sap-netweaver-rhel-qrg

**Lastupdated - 2018-08-13**
Changed the following files so that the server is 192 GB versus 256 GB because the 256 GB server is an "S1" server, which will soon no longer be available. Per Rainer "I absolutely agree to replace the S1 servers by the new S3 models - I heard that the S1 servers can soon no longer be ordered..."
**Note**: Even though the server has changed from 256 GB to 192 GB, the filenames still contain "256."

* rhel-index.md
* rhel-installing-256-GB-32-GB-server-three-tier-setup.md
* rhel-set-up-infrastructure-three-tier.md
* rhel-prepare-server-256GB.md
* rhel-partition-256GB.md
* rhel-prepare-network.md

Additionally, rhel-prepare-network.md will have the hostname changed from `e2e2690` to `sdb192`.

* rhel-set-up-infrastructure-32GB.md needs to have the server changed from SI.S1.NW32 to SI.S3.NW32. See above explanation regarding going from 256 GB to 192 GB.
