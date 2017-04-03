VirtualMachine
----------------
  1. Start UbuntuVM
  2. Download Java, Firefox, xterm
  3. login to vpn.manheim.com
    - set a root password on VM
  2. Download Java, Firefox, xterm
    - enable java on your browser
    - search ubuntu for "control" and open the Java security tab
    - add https://vpnuser.manheim.com to the list
    - restart firefox
  3. https://vpnuser.manheim.com
    - login
  4. Open port on Ubuntu

Host
--------
  1. get sshuttle on host
  2. configure VPN to forward 2222 on host to 22 on vm
  3. `sudo sshuttle -r drew@localhost:2222 10.0.0.0/8 -v`
  3. get `/etc/hosts` file with all manheim stuff

Enabling Java in Browser
-----------------------

Mozilla Firefox

  - Become the root user by running the su command and then enter the super-user password. Type:

`sudo -s`

  - Create a directory called plugins if you do not have it. Type:

`mkdir -p /usr/lib/firefox-addons/plugins`

  - Go to Mozilla plugins directory before you make the symbolic link. Type:

`cd /usr/lib/firefox-addons/plugins`

  - Create a symbolic link. Type:

`ln -s /usr/local/java/jre1.7.0/lib/amd64/libnpjp2.so`

  - Restart your browser and test Java

Troubleshooting no network
---------------------------
  - rebooting VM should be tried too

  1. `ps aux | grep ncsvc`
    - kill it if it comes up
  2. kill all java processes
  3. restart firefox
  4. disable and re-enable networking
