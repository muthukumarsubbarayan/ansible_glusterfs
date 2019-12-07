## Ansible::GlusterFS

### Replicated GlusterFS cluster setup

Creates
glusterfs cluster with 1 brick/host to be used as persistent storage for docker{swarm)/k8s. Tested on debian/Ubuntu.

*$ ansible-playbook /etc/ansible/roles/glusterfs/glusterfs.yml  --flush-cache*

**Playbooks**

	/etc/ansible/roles/glusterfs/
	├── defaults
	│   └── main.yml
	├── files
	├── glusterfs.yml
	├── handlers
	├── meta
	├── tasks
	│   ├── configure_glusterfs.yml
	│   ├── install_debian.yml
	│   ├── install_ubuntu.yml
	│   ├── main.yml
	│   ├── prepare_block_devices.yml
	│   └── test_glusterfs_volume.yml
	└── templates
			
**#Ansible play output:** Debian with three nodes

	PLAY [Install GlusterFS] 
	************************************************************************************

	TASK [Gathering Facts] 
	************************************************************************************
	Saturday 07 December 2019  01:06:31 -0500 (0:00:00.243)       0:00:00.243 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1011.mydomain.com]
	ok: [debian1010.mydomain.com]

	TASK [glusterfs : Preparing Block Devices] 
	************************************************************************************
	Saturday 07 December 2019  01:06:34 -0500 (0:00:02.958)       0:00:03.202 *****
	included: /opt/data/mydev/github/ansible/etcconfigs/roles/glusterfs/tasks/prepare_block_devices.yml for debian1010.mydomain.com, debian1011.mydomain.com, debian1012.mydomain.com

	TASK [glusterfs : Installing parted, lvm2, xfsprogs] 
	**********************************************************************************
	Saturday 07 December 2019  01:06:35 -0500 (0:00:00.417)       0:00:03.620 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1010.mydomain.com]
	ok: [debian1011.mydomain.com]

	TASK [glusterfs : Reading Disk Info /dev/sdb] 
	************************************************************************************
	Saturday 07 December 2019  01:06:37 -0500 (0:00:02.489)       0:00:06.110 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1011.mydomain.com]
	ok: [debian1010.mydomain.com]

	TASK [glusterfs : Current partition Scheeme:] 
	************************************************************************************
	Saturday 07 December 2019  01:06:39 -0500 (0:00:01.496)       0:00:07.606 *****
	ok: [debian1010.mydomain.com] =>
	  msg:
	  - 'Current partition Scheeme:'
	  - changed: false
		disk:
		  dev: /dev/sdb
		  logical_block: 512
		  model: Msft Virtual Disk
		  physical_block: 4096
		  size: 15360.0
		  table: gpt
		  unit: mib
		failed: false
		partitions:
		- begin: 1.0
		  end: 15359.0
		  flags: []
		  fstype: ''
		  name: GPT
		  num: 1
		  size: 15358.0
		  unit: mib
		script: unit 'MiB' print
	ok: [debian1011.mydomain.com] =>
	  msg:
	  - 'Current partition Scheeme:'
	  - changed: false
		disk:
		  dev: /dev/sdb
		  logical_block: 512
		  model: Msft Virtual Disk
		  physical_block: 4096
		  size: 15360.0
		  table: gpt
		  unit: mib
		failed: false
		partitions:
		- begin: 1.0
		  end: 15359.0
		  flags: []
		  fstype: ''
		  name: GPT
		  num: 1
		  size: 15358.0
		  unit: mib
		script: unit 'MiB' print
	ok: [debian1012.mydomain.com] =>
	  msg:
	  - 'Current partition Scheeme:'
	  - changed: false
		disk:
		  dev: /dev/sdb
		  logical_block: 512
		  model: Msft Virtual Disk
		  physical_block: 4096
		  size: 15360.0
		  table: gpt
		  unit: mib
		failed: false
		partitions:
		- begin: 1.0
		  end: 15359.0
		  flags: []
		  fstype: ''
		  name: GPT
		  num: 1
		  size: 15358.0
		  unit: mib
		script: unit 'MiB' print

	TASK [glusterfs : Creating partions on /dev/sdb] 
	************************************************************************************
	Saturday 07 December 2019  01:06:39 -0500 (0:00:00.306)       0:00:07.913 *****
	ok: [debian1010.mydomain.com]
	ok: [debian1011.mydomain.com]
	ok: [debian1012.mydomain.com]

	TASK [glusterfs : Creating LVM Volume Group vgdocker] 
	*********************************************************************************
	Saturday 07 December 2019  01:06:40 -0500 (0:00:01.550)       0:00:09.463 *****
	ok: [debian1011.mydomain.com]
	ok: [debian1012.mydomain.com]
	ok: [debian1010.mydomain.com]

	TASK [glusterfs : Creating logical volume: /dev/vgdocker/lvdocker] 
	********************************************************************
	Saturday 07 December 2019  01:06:42 -0500 (0:00:01.469)       0:00:10.933 *****
	fatal: [debian1011.mydomain.com]: FAILED! => changed=false
	  msg: Sorry, no shrinking of lvdocker to 0 permitted.
	...ignoring
	fatal: [debian1010.mydomain.com]: FAILED! => changed=false
	  msg: Sorry, no shrinking of lvdocker to 0 permitted.
	...ignoring
	fatal: [debian1012.mydomain.com]: FAILED! => changed=false
	  msg: Sorry, no shrinking of lvdocker to 0 permitted.
	...ignoring

	TASK [glusterfs : Creating xfs file systemc] 
	************************************************************************************
	Saturday 07 December 2019  01:06:43 -0500 (0:00:01.468)       0:00:12.402 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1011.mydomain.com]
	ok: [debian1010.mydomain.com]

	TASK [glusterfs : Creating Mount Point] 
	************************************************************************************
	Saturday 07 December 2019  01:06:45 -0500 (0:00:01.244)       0:00:13.647 *****
	ok: [debian1011.mydomain.com]
	ok: [debian1010.mydomain.com]
	ok: [debian1012.mydomain.com]

	TASK [glusterfs : Mounting /dev/mapper/vgdocker-lvdocker at /glusterfs/dockerdata/br
	************************************
	Saturday 07 December 2019  01:06:46 -0500 (0:00:01.340)       0:00:14.987 *****
	ok: [debian1010.mydomain.com]
	ok: [debian1011.mydomain.com]
	ok: [debian1012.mydomain.com]

	TASK [glusterfs : Getting Brick capacity] 
	************************************************************************************
	Saturday 07 December 2019  01:06:47 -0500 (0:00:01.328)       0:00:16.316 *****
	changed: [debian1010.mydomain.com]
	changed: [debian1012.mydomain.com]
	changed: [debian1011.mydomain.com]

	TASK [glusterfs : Current Brick(s) Capacity] 
	************************************************************************************
	Saturday 07 December 2019  01:06:48 -0500 (0:00:01.079)       0:00:17.396 *****
	ok: [debian1010.mydomain.com] =>
	  msg:
	  - Filesystem                     Size  Used Avail Use% Mounted on
	  - /dev/mapper/vgdocker-lvdocker   15G   48M   15G   1% /glusterfs/dockerdata/brick1
	ok: [debian1011.mydomain.com] =>
	  msg:
	  - Filesystem                     Size  Used Avail Use% Mounted on
	  - /dev/mapper/vgdocker-lvdocker   15G   48M   15G   1% /glusterfs/dockerdata/brick1
	ok: [debian1012.mydomain.com] =>
	  msg:
	  - Filesystem                     Size  Used Avail Use% Mounted on
	  - /dev/mapper/vgdocker-lvdocker   15G   48M   15G   1% /glusterfs/dockerdata/brick1

	TASK [glusterfs : Installing Glusterd packages on Debian Host] 
	************************************************************************
	Saturday 07 December 2019  01:06:49 -0500 (0:00:00.201)       0:00:17.597 *****
	included: /opt/data/mydev/github/ansible/etcconfigs/roles/glusterfs/tasks/install_debian.yml for debian1010.mydomain.com, debian1011.mydomain.com, debian1012.mydomain.com

	TASK [glusterfs : Installing pkgs to manage apt repos] 
	********************************************************************************
	Saturday 07 December 2019  01:06:49 -0500 (0:00:00.573)       0:00:18.170 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1010.mydomain.com]
	ok: [debian1011.mydomain.com]

	TASK [glusterfs : Installing pkgs for gpg verfication for python2] 
	********************************************************************
	Saturday 07 December 2019  01:06:51 -0500 (0:00:01.844)       0:00:20.014 *****
	ok: [debian1010.mydomain.com]
	ok: [debian1012.mydomain.com]
	ok: [debian1011.mydomain.com]

	TASK [glusterfs : Installing pkgs for gpg verfication python3] 
	************************************************************************
	Saturday 07 December 2019  01:06:53 -0500 (0:00:02.260)       0:00:22.275 *****
	skipping: [debian1010.mydomain.com]
	skipping: [debian1011.mydomain.com]
	skipping: [debian1012.mydomain.com]

	TASK [glusterfs : Adding rpm gpg key GlusterFs repo] 
	**********************************************************************************
	Saturday 07 December 2019  01:06:53 -0500 (0:00:00.233)       0:00:22.508 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1011.mydomain.com]
	ok: [debian1010.mydomain.com]

	TASK [glusterfs : Getting dpkg architecture] 
	************************************************************************************
	Saturday 07 December 2019  01:06:57 -0500 (0:00:03.457)       0:00:25.966 *****
	changed: [debian1011.mydomain.com]
	changed: [debian1010.mydomain.com]
	changed: [debian1012.mydomain.com]

	TASK [glusterfs : Adding GlusterFs Repo] 
	************************************************************************************
	Saturday 07 December 2019  01:06:58 -0500 (0:00:00.878)       0:00:26.844 *****
	ok: [debian1012.mydomain.com]
	ok: [debian1010.mydomain.com]
	ok: [debian1011.mydomain.com]

	TASK [glusterfs : Installing GlusterFS Server, Client pkgs] 
	***************************************************************************
	Saturday 07 December 2019  01:07:00 -0500 (0:00:01.738)       0:00:28.583 *****
	changed: [debian1011.mydomain.com]
	changed: [debian1012.mydomain.com]
	changed: [debian1010.mydomain.com]

	TASK [glusterfs : Enable Start glusterd service] 
	************************************************************************************
	Saturday 07 December 2019  01:09:25 -0500 (0:02:25.612)       0:02:54.195 *****
	changed: [debian1010.mydomain.com]
	changed: [debian1011.mydomain.com]
	changed: [debian1012.mydomain.com]

	TASK [glusterfs : Configuring GlusterFS volume dockerdata on Debian Host] 
	*************************************************************
	Saturday 07 December 2019  01:09:34 -0500 (0:00:08.990)       0:03:03.186 *****
	skipping: [debian1011.mydomain.com]
	skipping: [debian1012.mydomain.com]
	included: /opt/data/mydev/github/ansible/etcconfigs/roles/glusterfs/tasks/configure_glusterfs.yml for debian1010.mydomain.com

	TASK [glusterfs : Creating glusterfs cluster for dockerdata] 
	**************************************************************************
	Saturday 07 December 2019  01:09:34 -0500 (0:00:00.318)       0:03:03.505 *****
	changed: [debian1010.mydomain.com]

	TASK [glusterfs : Pause for 30 seconds for peer probing to get complete] 
	**************************************************************
	Saturday 07 December 2019  01:09:43 -0500 (0:00:08.090)       0:03:11.595 *****
	Pausing for 30 seconds
	(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
	ok: [debian1010.mydomain.com]

	TASK [glusterfs : Display create_volume params] 
	************************************************************************************
	Saturday 07 December 2019  01:10:13 -0500 (0:00:30.065)       0:03:41.661 *****
	ok: [debian1010.mydomain.com] =>
	  msg:
	  - dockerdata
	  - /glusterfs/dockerdata/brick1/brick
	  - debian1010.mydomain.com,debian1011.mydomain.com,debian1012.mydomain.com
	  - '3'

	TASK [glusterfs : Creating dockerdata volume] 
	************************************************************************************
	Saturday 07 December 2019  01:10:13 -0500 (0:00:00.101)       0:03:41.762 *****
	changed: [debian1010.mydomain.com]

	TASK [glusterfs : Details of newsly created dockerdata volume] 
	************************************************************************
	Saturday 07 December 2019  01:10:26 -0500 (0:00:13.399)       0:03:55.162 *****
	ok: [debian1010.mydomain.com] =>
	  msg:
		ansible_facts:
		  glusterfs:
			peers:
			  debian1011.mydomain.com:
			  - 15ed09ee-902c-4ad1-8ecb-65ebf8e57642
			  - Peer in Cluster (Connected)
			  debian1012.mydomain.com:
			  - 377ec7cf-431b-4a22-83f2-8793984a8054
			  - Peer in Cluster (Connected)
			quotas: {}
			volumes:
			  dockerdata:
				bricks:
				- debian1010.mydomain.com:/glusterfs/dockerdata/brick1/brick
				- debian1011.mydomain.com:/glusterfs/dockerdata/brick1/brick
				- debian1012.mydomain.com:/glusterfs/dockerdata/brick1/brick
				id: aeb522d8-8b2f-407e-8def-e79cef7ea052
				name: dockerdata
				options:
				  nfs.disable: 'on'
				  performance.client-io-threads: 'off'
				  storage.fips-mode-rchecksum: 'on'
				  transport.address-family: inet
				quota: false
				replicas: '3'
				status: Started
				transport: tcp
		changed: true
		failed: false

	TASK [glusterfs : Testing newly created volume] 
	************************************************************************************
	Saturday 07 December 2019  01:10:26 -0500 (0:00:00.096)       0:03:55.258 *****
	skipping: [debian1011.mydomain.com]
	skipping: [debian1012.mydomain.com]
	included: /opt/data/mydev/github/ansible/etcconfigs/roles/glusterfs/tasks/test_glusterfs_volume.yml for debian1010.mydomain.com

	TASK [glusterfs : Installing glusterfs-fuse on the test host] 
	*************************************************************************
	Saturday 07 December 2019  01:10:27 -0500 (0:00:00.414)       0:03:55.672 *****
	fatal: [debian1010.mydomain.com]: FAILED! => changed=false
	  msg: No package matching 'glusterfs-fuse' is available
	...ignoring

	TASK [glusterfs : Creating client mount point /dockerdata_replicated on test host] 
	****************************************************
	Saturday 07 December 2019  01:10:28 -0500 (0:00:01.506)       0:03:57.179 *****
	changed: [debian1010.mydomain.com]

	TASK [glusterfs : Mounting dockerdata on test host] 
	***********************************************************************************
	Saturday 07 December 2019  01:10:29 -0500 (0:00:00.564)       0:03:57.743 *****
	changed: [debian1010.mydomain.com]

	TASK [glusterfs : Client mount details] 
	*************************************************************************************
	Saturday 07 December 2019  01:10:29 -0500 (0:00:00.737)       0:03:58.481 *****
	ok: [debian1010.mydomain.com] =>
	  msg:
		changed: true
		dump: '0'
		failed: false
		fstab: /etc/fstab
		fstype: glusterfs
		name: /dockerdata_replicated
		opts: defaults
		passno: '0'
		src: debian1010.mydomain.com:dockerdata

	PLAY RECAP 	*************************************************************************
	debian1010.mydomain.com   : ok=32   changed=8    unreachable=0    failed=0    skipped=1    rescued=0    ignored=2
	debian1011.mydomain.com   : ok=21   changed=4    unreachable=0    failed=0    skipped=3    rescued=0    ignored=1
	debian1012.mydomain.com   : ok=21   changed=4    unreachable=0    failed=0    skipped=3    rescued=0    ignored=1

	Saturday 07 December 2019  01:10:29 -0500 (0:00:00.070)       0:03:58.551 *****
	===============================================================================
	
	glusterfs : Installing GlusterFS Server, Client pkgs -------------------------------------------- 145.61s 
	glusterfs : Pause for 30 seconds for peer probing to get complete -------------------------------- 30.07s 
	glusterfs : Creating dockerdata volume ----------------------------------------------------------- 13.40s 
	glusterfs : Enable Start glusterd service --------------------------------------------------------- 8.99s 
	glusterfs : Creating glusterfs cluster for dockerdata --------------------------------------------- 8.09s 
	glusterfs : Adding rpm gpg key GlusterFs repo ----------------------------------------------------- 3.46s 
	Gathering Facts ----------------------------------------------------------------------------------- 2.96s 
	glusterfs : Installing parted, lvm2, xfsprogs ----------------------------------------------------- 2.49s 
	glusterfs : Installing pkgs for gpg verfication for python2 --------------------------------------- 2.26s 
	glusterfs : Installing pkgs to manage apt repos --------------------------------------------------- 1.84s 
	glusterfs : Adding GlusterFs Repo ----------------------------------------------------------------- 1.74s 
	glusterfs : Creating partions on /dev/sdb --------------------------------------------------------- 1.55s 
	glusterfs : Installing glusterfs-fuse on the test host -------------------------------------------- 1.51s 
	glusterfs : Reading Disk Info /dev/sdb ------------------------------------------------------------ 1.50s 
	glusterfs : Creating LVM Volume Group vgdocker ---------------------------------------------------- 1.47s 
	glusterfs : Creating logical volume: /dev/vgdocker/lvdocker --------------------------------------- 1.47s 
	glusterfs : Creating Mount Point ------------------------------------------------------------------ 1.34s 
	glusterfs : Mounting /dev/mapper/vgdocker-lvdocker at /glusterfs/dockerdata/brick1 => Brick ------- 1.33s 
	glusterfs : Creating xfs file systemc ------------------------------------------------------------- 1.24s 
	glusterfs : Getting Brick capacity ---------------------------------------------------------------- 1.08s 
	Playbook run took 0 days, 0 hours, 3 minutes, 58 seconds

**#Ansible play output:** Ubuntu with two nodes

	PLAY [Install GlusterFS] 
	**************************************************************************************

	TASK [Gathering Facts] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:33 -0500 (0:00:00.261)       0:00:00.261 *****
	ok: [ub203.mydomain.com]
	ok: [ub202.mydomain.com]

	TASK [glusterfs : Preparing Block Devices] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:36 -0500 (0:00:03.651)       0:00:03.912 *****
	included: /etc/ansible/roles/glusterfs/tasks/prepare_block_devices.yml for ub202.mydomain.com, ub203.mydomain.com

	TASK [glusterfs : Installing parted, lvm2, xfsprogs] 
	**********************************************************************************
	Saturday 07 December 2019  17:55:36 -0500 (0:00:00.235)       0:00:04.148 *****
	ok: [ub203.mydomain.com]
	ok: [ub202.mydomain.com]

	TASK [glusterfs : Reading Disk Info /dev/sdb] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:39 -0500 (0:00:02.882)       0:00:07.030 *****
	ok: [ub202.mydomain.com]
	ok: [ub203.mydomain.com]

	TASK [glusterfs : Current partition Scheeme:] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:41 -0500 (0:00:01.459)       0:00:08.490 *****
	ok: [ub202.mydomain.com] =>
	  msg:
	  - 'Current partition Scheeme:'
	  - changed: false
		disk:
		  dev: /dev/sdb
		  logical_block: 512
		  model: Msft Virtual Disk
		  physical_block: 4096
		  size: 51200.0
		  table: gpt
		  unit: mib
		failed: false
		partitions:
		- begin: 1.0
		  end: 51199.0
		  flags: []
		  fstype: ''
		  name: GPT
		  num: 1
		  size: 51198.0
		  unit: mib
		script: unit 'MiB' print
	ok: [ub203.mydomain.com] =>
	  msg:
	  - 'Current partition Scheeme:'
	  - changed: false
		disk:
		  dev: /dev/sdb
		  logical_block: 512
		  model: Msft Virtual Disk
		  physical_block: 4096
		  size: 51200.0
		  table: gpt
		  unit: mib
		failed: false
		partitions:
		- begin: 1.0
		  end: 51199.0
		  flags: []
		  fstype: ''
		  name: GPT
		  num: 1
		  size: 51198.0
		  unit: mib
		script: unit 'MiB' print

	TASK [glusterfs : Creating partions on /dev/sdb] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:41 -0500 (0:00:00.156)       0:00:08.646 *****
	ok: [ub203.mydomain.com]
	ok: [ub202.mydomain.com]

	TASK [glusterfs : Creating LVM Volume Group vgk8s] 
	************************************************************************************
	Saturday 07 December 2019  17:55:42 -0500 (0:00:01.042)       0:00:09.689 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Creating logical volume: /dev/vgk8s/lvl8s] 
	**************************************************************************
	Saturday 07 December 2019  17:55:44 -0500 (0:00:01.629)       0:00:11.318 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Creating xfs file system] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:45 -0500 (0:00:01.882)       0:00:13.200 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Creating Mount Point] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:47 -0500 (0:00:01.873)       0:00:15.074 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Mounting /dev/mapper/vgk8s-lvl8s at /glusterfs/k8sdata/brick1 => Brick] 
	*********************************************
	Saturday 07 December 2019  17:55:48 -0500 (0:00:00.818)       0:00:15.893 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Getting Brick capacity] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:49 -0500 (0:00:01.193)       0:00:17.086 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Current Brick(s) Capacity] 
	**************************************************************************************
	Saturday 07 December 2019  17:55:50 -0500 (0:00:00.906)       0:00:17.992 *****
	ok: [ub202.mydomain.com] =>
	  msg:
	  - Filesystem               Size  Used Avail Use% Mounted on
	  - /dev/mapper/vgk8s-lvl8s   50G   84M   50G   1% /glusterfs/k8sdata/brick1
	ok: [ub203.mydomain.com] =>
	  msg:
	  - Filesystem               Size  Used Avail Use% Mounted on
	  - /dev/mapper/vgk8s-lvl8s   50G   84M   50G   1% /glusterfs/k8sdata/brick1

	TASK [glusterfs : Installing Glusterd packages on Debian Host] 
	************************************************************************
	Saturday 07 December 2019  17:55:50 -0500 (0:00:00.137)       0:00:18.130 *****
	skipping: [ub202.mydomain.com]
	skipping: [ub203.mydomain.com]

	TASK [glusterfs : Installing Glusterd packages on Ubuntu Host] 
	************************************************************************
	Saturday 07 December 2019  17:55:51 -0500 (0:00:00.126)       0:00:18.256 *****
	included: /etc/ansible/roles/glusterfs/tasks/install_ubuntu.yml for ub202.mydomain.com, ub203.mydomain.com

	TASK [glusterfs : Installing pkgs to manage apt repos] 
	********************************************************************************
	Saturday 07 December 2019  17:55:51 -0500 (0:00:00.497)       0:00:18.754 *****
	changed: [ub203.mydomain.com]
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Adding GlusterFs Repo] 
	****************************************************************************************
	Saturday 07 December 2019  17:58:14 -0500 (0:02:22.670)       0:02:41.424 *****
	changed: [ub202.mydomain.com]
	changed: [ub203.mydomain.com]

	TASK [glusterfs : Installing GlusterFS Server, Client pkgs] 
	***************************************************************************
	Saturday 07 December 2019  17:58:55 -0500 (0:00:41.057)       0:03:22.482 *****
	changed: [ub202.mydomain.com]
	changed: [ub203.mydomain.com]

	TASK [glusterfs : Enable Start glusterd service] 
	**************************************************************************************
	Saturday 07 December 2019  18:03:42 -0500 (0:04:47.459)       0:08:09.942 *****
	ok: [ub202.mydomain.com]
	ok: [ub203.mydomain.com]

	TASK [glusterfs : Configuring GlusterFS volume k8sdata on Debian Host] 
	****************************************************************
	Saturday 07 December 2019  18:03:45 -0500 (0:00:03.221)       0:08:13.163 *****
	skipping: [ub203.mydomain.com]
	included: /etc/ansible/roles/glusterfs/tasks/configure_glusterfs.yml for ub202.mydomain.com

	TASK [glusterfs : Creating glusterfs cluster for k8sdata] 
	*****************************************************************************
	Saturday 07 December 2019  18:03:46 -0500 (0:00:00.185)       0:08:13.348 *****
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Pause for 30 seconds for peer probing to get complete] 
	**************************************************************
	Saturday 07 December 2019  18:03:50 -0500 (0:00:04.133)       0:08:17.482 *****
	Pausing for 30 seconds
	(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
	ok: [ub202.mydomain.com]

	TASK [glusterfs : Display create_volume params] 
	***************************************************************************************
	Saturday 07 December 2019  18:04:20 -0500 (0:00:30.046)       0:08:47.528 *****
	ok: [ub202.mydomain.com] =>
	  msg:
	  - k8sdata
	  - /glusterfs/k8sdata/brick1/brick
	  - ub202.mydomain.com,ub203.mydomain.com
	  - '2'

	TASK [glusterfs : Creating k8sdata volume] 
	*****************************************************************************************
	Saturday 07 December 2019  18:04:20 -0500 (0:00:00.201)       0:08:47.729 *****
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Details of newsly created k8sdata volume] 
	***************************************************************************
	Saturday 07 December 2019  18:04:32 -0500 (0:00:11.536)       0:08:59.266 *****
	ok: [ub202.mydomain.com] =>
	  msg:
		ansible_facts:
		  glusterfs:
			peers:
			  ub203.mydomain.com:
			  - d7056540-6d08-470e-8f86-744df84a299d
			  - Peer in Cluster (Connected)
			quotas: {}
			volumes:
			  k8sdata:
				bricks:
				- ub202.mydomain.com:/glusterfs/k8sdata/brick1/brick
				- ub203.mydomain.com:/glusterfs/k8sdata/brick1/brick
				id: cd8eca5b-9d3e-4ab5-a708-88a25bba6c14
				name: k8sdata
				options:
				  nfs.disable: 'on'
				  performance.client-io-threads: 'off'
				  storage.fips-mode-rchecksum: 'on'
				  transport.address-family: inet
				quota: false
				replicas: '2'
				status: Started
				transport: tcp
		changed: true
		failed: false

	TASK [glusterfs : Testing newly created volume] 
	***************************************************************************************
	Saturday 07 December 2019  18:04:32 -0500 (0:00:00.087)       0:08:59.354 *****
	skipping: [ub203.mydomain.com]
	included: /etc/ansible/roles/glusterfs/tasks/test_glusterfs_volume.yml for ub202.mydomain.com

	TASK [glusterfs : Installing glusterfs-fuse on the test host] 
	*************************************************************************
	Saturday 07 December 2019  18:04:32 -0500 (0:00:00.236)       0:08:59.590 *****
	fatal: [ub202.mydomain.com]: FAILED! => changed=false
	  msg: No package matching 'glusterfs-fuse' is available
	...ignoring

	TASK [glusterfs : Creating client mount point /k8sdata on test host] 
	******************************************************************
	Saturday 07 December 2019  18:04:33 -0500 (0:00:01.629)       0:09:01.220 *****
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Mounting k8sdata on test host] 
	**************************************************************************************
	Saturday 07 December 2019  18:04:34 -0500 (0:00:00.393)       0:09:01.613 *****
	changed: [ub202.mydomain.com]

	TASK [glusterfs : Client mount details] 
	******************************************************************************************
	Saturday 07 December 2019  18:04:35 -0500 (0:00:00.757)       0:09:02.370 *****
	ok: [ub202.mydomain.com] =>
	  msg:
		changed: true
		dump: '0'
		failed: false
		fstab: /etc/fstab
		fstype: glusterfs
		name: /k8sdata
		opts: defaults
		passno: '0'
		src: ub202.mydomain.com:k8sdata

	PLAY RECAP 
	******************************************************************************************
	ub202.mydomain.com        : ok=29   changed=13   unreachable=0    failed=0    skipped=1    rescued=0    ignored=1
	ub203.mydomain.com        : ok=18   changed=9    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

	Saturday 07 December 2019  18:04:35 -0500 (0:00:00.041)       0:09:02.412 *****
	===============================================================================
	glusterfs : Installing GlusterFS Server, Client pkgs -------------------------------------- 287.46s
	glusterfs : Installing pkgs to manage apt repos ------------------------------------------- 142.67s
	glusterfs : Adding GlusterFs Repo ---------------------------------------------------------- 41.06s
	glusterfs : Pause for 30 seconds for peer probing to get complete -------------------------- 30.05s
	glusterfs : Creating k8sdata volume -------------------------------------------------------- 11.54s
	glusterfs : Creating glusterfs cluster for k8sdata ------------------------------------------ 4.13s 
	Gathering Facts ----------------------------------------------------------------------------- 3.65s
	glusterfs : Enable Start glusterd service --------------------------------------------------- 3.22s
	glusterfs : Installing parted, lvm2, xfsprogs ----------------------------------------------- 2.88s
	glusterfs : Creating logical volume: /dev/vgk8s/lvl8s --------------------------------------- 1.88s
	glusterfs : Creating xfs file system -------------------------------------------------------- 1.87s
	glusterfs : Installing glusterfs-fuse on the test host -------------------------------------- 1.63s
	glusterfs : Creating LVM Volume Group vgk8s ------------------------------------------------- 1.63s
	glusterfs : Reading Disk Info /dev/sdb ------------------------------------------------------ 1.46s
	glusterfs : Mounting /dev/mapper/vgk8s-lvl8s at /glusterfs/k8sdata/brick1 => Brick ---------- 1.19s
	glusterfs : Creating partions on /dev/sdb --------------------------------------------------- 1.04s
	glusterfs : Getting Brick capacity ---------------------------------------------------------- 0.91s
	glusterfs : Creating Mount Point ------------------------------------------------------------ 0.82s
	glusterfs : Mounting k8sdata on test host --------------------------------------------------- 0.76s
	glusterfs : Installing Glusterd packages on Ubuntu Host ------------------------------------- 0.50s 
	Playbook run took 0 days, 0 hours, 9 minutes, 2 seconds
