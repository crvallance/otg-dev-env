# Build Notes

Basic steps for the Radxa Zero 3 setup

## Zero 3E

This works fine on the the Zero 3E at minimum, I will try out the 3W once I get one. Currently running b6 Debian Xfce because this was the only version I could get to boot at the time.

1. Download and install image with something like etcher
2. Update apt and upgrade packages
   1. `sudo apt update && sudo apt upgrade --assume-yes`
3. Setup OTG if it's not already enabled
   1. Run radxa's config utility `rsetup`
   2. Go to Hardware - USB OTG services
   3. Enable `radxa-usbnet@fcc00000.dwc3`
   4. Create a new user with `adduser`
4. Install general stuff I want
   1. `sudo apt install python3 python3-venv pipx git`
5. Clone this repo
   1. `git clone https://github.com/crvallance/otg-dev-env`
6. Copy the network config file
   1. `sudo cp /network/usb0.network /etc/systemd/network/usb0.network`
7. Use pipx to install ansible
   1. `pipx install --include-deps ansible`
8. [Install docker](https://docs.docker.com/engine/install/debian/#install-using-the-repository) and do [post-install steps](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) for your user.
9. Install docker compose
   1. `sudo apt install docker-compose`
10. Adjust the desired packages in the [Containerfile](/code-server-docker/Containerfile) and build an image
    1. `docker build -t crvdevenv:v1 -f Containerfile .`
11. Adjust any of the items in the compose file as needed
    1. PUID/GUID
    2. Timezone
    3. DEFAULT_WORKSPACE
        1. I have this mapping through to a mount
    4. SUDO_PASSWORD
    5. config volume mount
       1. This holds the config for code-server
    6. bind mount
       1. This lets me bind a local folder in to the container for the default workspace
