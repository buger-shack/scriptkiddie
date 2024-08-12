# SSH Tunnelling / Port Forwarding

### Forward Connections

Creating a **forward** (or "_local_") **SSH tunnel** can be done from the attacking box when we have SSH access to the target.

#### `Port Forwarding :` -L switch, which creates a link to a Local port.

* For example, if we had SSH access to 172.16.0.5 and there's a webserver running on 172.16.0.10, we could use this command to create a link to the server on 172.16.0.10:

```bash
ssh -L 8000:132.227.89.21:80 21108020@ssh.ufr-info-p6.jussieu.fr -fN

-f backgrounds the shell
-N tells SSH that it doesn't need to execute any commands,only set up the fucking connection
```

You can now access the website on 172.16.0.10 (through 172.16.0.5) by navigating to port 8000 on our own attacking machine with `localhost:8000`.

> Good Practice use a high port, out of the way, for the local connection.

#### `Creating a proxy :` -D switch

* For example: -D 1337. This will open up port 1337 on the attacking box as a **proxy** to send data through into the _protected network_.

This is useful when combined with a tool such as **proxychains**. An example of this command would be:

```bash
ssh -D 1337 user@172.16.0.5 -fN
```

***

### Reverse Connections

_Reverse connections_ are very possible with the **SSH client** (and indeed may be preferable if you have **a shell** on the **compromised server**, but not **SSH access**).

**They are, however, riskier** as you inherently must access **your attacking machine from the target** -- be it by using credentials, or preferably a key based system.

`Before we can make a reverse connection safely, there are a few steps to take:`

```bash
# 1. generate a new set of SSH keys and store them somewhere safe
ssh-keygen

# 2. Copy the contents of the public key, then edit the ~/.ssh/authorized_keys file on your own attacking machine.

# 3. New line, type the following line, then paste in the public key:
command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty ssh-public-key

# 4. start the ssh service your the attacking machine
sudo systemctl start ssh

# 5. transfer the private key to the target server

# 6. With the key transferred, connect back with a reverse port forward (on the target server)
ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN

# or (In newer versions of the SSH client)
ssh -R 1337 USERNAME@ATTACKING_IP -i KEYFILE -fN
```

```bash
ssh -p port -okexAlgorithms=+<algo> user@0.0.0.0

ssh -o StrictHostKeyChecking=no -p 0.0.0.0

ssh -i id_rsa -L localport:127.0.0.1:remoteport user@0.0.0.0

#to get public key from a private key
ssh-keygen -y -e -f keyfile
```
