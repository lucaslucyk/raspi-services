# raspi-services
my raspberry pi local services


## Quickstart
Steps and guide to deploy all services on `Raspberry Pi 4b`.


### Install Raspberry Pi OS

Go to [Download page](https://www.raspberrypi.com/software/) and install OS on memory card.


### Disable Wifi and Bluetooth

Edit `/boot/firmware/config.txt` file adding `dtoverlay`.
```bash
# /boot/firmware/config.txt
#...

dtoverlay=disable-wifi
dtoverlay=disable-bt
```

### Github config

#### Create ssh

1. Create ssh key

```bash
cd .ssh/
ssh-keygen
```
2. Complete steps
3. Get public key
```bash
cat <ssh_key_name>.pub
```

#### Config on Github

Add ken on [Github SSH Keys](https://github.com/settings/keys)

#### Config Github params

git config --global user.name ...
git config --global user.email ...


#### Config ssh hosts

Create `.ssh/config` file with content like:

```
Host github.com
 Hostname ssh.github.com
 Port 443
 AddKeysToAgent yes
 PreferredAuthentications publickey
 IdentityFile ~/.ssh/<ssh_key_name>

Host gitlab.com
 Hostname gitlab.com
 AddKeysToAgent yes
 PreferredAuthentications publickey
 IdentityFile ~/.ssh/<ssh_key_name>
```


#### Config aliases

Add alias section on `.gitconfig` file.

```ini
[alias]
    cb = "rev-parse --abbrev-ref HEAD"
    cm = "commit -am"
    po = "push origin"
    cfg = "config --global -l"
    st = "status -sb"
    ll = "log --oneline"
    lc = "log -1 HEAD --stat"
    dv = "difftool -t vimdiff -y"
    search = "!git rev-list --all | xargs git grep -F"
    rv = "remote -v"
```

### Install `Docker`

Follow steps of [Docker install Guide](https://www.raspberrypi.com/software/)


#### Add `pi` user to `docker` group

Prevent use `sudo` for each `docker` command

```bash
sudo usermod -aG docker pi
```

### Enable `rclone`

Go to home dir and run

```bash
docker run -it -v ~/.config/rclone:/config/rclone rclone/rclone config
```

On finish, update file permission for docker

```bash
sudo chmod 0644 .config/rclone/rclone.conf
```

#### Info and detail

- Credentials: [link]([https://](https://console.cloud.google.com/apis/credentials?authuser=1&hl=es&project=digital-modem-417320))

- Info: [video](https://www.youtube.com/watch?v=mnDYJ2ZpdxU)


### Clone services and up

```bash
git clone ...
cd raspi-services
```

Create `.env` file with content like:

```ini
WEBPASSWORD=supersecret
```

Start containers

```bash
docker-compose up -d
```

