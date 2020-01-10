# How to mount a host folder into a vm created with docker-machine (boot2docker tinycore)

## Share a folder on the host machine

## Download and install cifs on the vm

tce-load -wi cifs-utils.tcz

## Create mounting point in vm

mkdir -p udemy/dockermastery

## Mount host shared folder into vm

sudo mount.cifs //<IP_ADDRESS>/udemy-docker-mastery /udemy/dockermastery -o user=<USER_NAME>
