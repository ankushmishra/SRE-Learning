# Day 13 - Security fundamentals

## Session goal

Practice least privilege, SSH key basics, listening-port inspection, and a
simple Ubuntu firewall configuration. Prepare the personal lab for Docker on
Day 14.

Suggested time: 90-120 minutes.

## 1. Fundamentals

Security reduces the chance and impact of unauthorized access or misuse.
**Least privilege** means an account, process, or service gets only the access
it needs for its task. A network listener exposes a service on an interface and
port; every exposed listener should have a reason and appropriate controls.

SSH key authentication uses a private key held by the client and a matching
public key installed for the account on the server. Protect the private key;
never paste or commit it. A host firewall can allow or deny traffic based on
rules. Firewall changes can cut off remote access, so use a local VM console
for this lab and confirm rules before enabling the firewall.

## 2. Practical lab (personal Ubuntu VM only)

Do not make these changes on a work, production, or remote server. Keep the VM
console available. If `~/.ssh/sre-day13-lab` already exists, do not overwrite
it; choose a new filename or use the existing lab key.

### 2.1 Generate a lab SSH key pair

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Then create the key:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/sre-day13-lab -C sre-day13-lab
```

Choose a passphrase when prompted. The private key is `~/.ssh/sre-day13-lab`;
the public key ends in `.pub`. Inspect only the public key if needed:

```bash
cat ~/.ssh/sre-day13-lab.pub
```

Do not upload the private key. This exercise creates a key pair; it does not
configure a remote login.

### 2.2 Inspect listening ports

```bash
ss -lntup
```

Record the listening address, port, and process where shown. The output may
vary by VM and permissions. A listener bound to `127.0.0.1` is local-only;
`0.0.0.0` listens on all IPv4 interfaces.

### 2.3 Inspect and enable UFW

First inspect state and available application profiles:

```bash
sudo ufw status verbose
sudo ufw app list
```

If you use SSH to reach this VM, add the SSH allow rule before enabling UFW:

```bash
sudo ufw allow OpenSSH
```

Enable and verify:

```bash
sudo ufw enable
sudo ufw status verbose
```

If the `OpenSSH` profile is unavailable, consult the local UFW profile list and
allow the actual SSH port before enabling. In a VM used only through its
console, record that no SSH allow rule is needed for the console connection.
Do not experiment with restrictive rules unless the console is available for
recovery.

### 2.4 Prepare Docker for Day 14

Check the Docker Engine installation guide for your exact Ubuntu release and
architecture. Install only inside the disposable lab, following the official
steps. Verify the engine with `docker run hello-world`. Docker access can
provide host-level control; do not add users to the Docker group casually.

## 3. Review and Day 13 notes

1. What does least privilege mean?
2. Which key is private, and how should it be protected?
3. Which services were listening before firewall changes?
4. What rule did you allow before enabling UFW?
5. Which console access would let you recover from a firewall mistake?

- Key pair filename (do not record key contents):
- Listeners observed:
- UFW status/rules:
- Docker hello-world result:
- Three things learned:
  1.
  2.
  3.
- Confusing command / resolution:

## Done when

- [ ] I generated or safely identified a lab-only SSH key pair.
- [ ] I recorded the VM's listening ports with `ss`.
- [ ] I verified the intended UFW rules from the VM console.
- [ ] Docker is installed in the lab and `hello-world` runs.

## Further reading

- [Ubuntu Server firewall documentation](https://ubuntu.com/server/docs/firewalls/)
- [Docker Engine installation on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [`ssh-keygen` manual](https://man.openbsd.org/ssh-keygen)

