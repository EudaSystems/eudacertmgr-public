<!-- Generated file: README.md is produced from this template. Do not edit README.md directly. -->
# EudaCertMgr™

**Stop managing TLS / SSL certificates manually!**

**On-premises, behind your firewall.** EudaCertMgr runs on your hardware, under your control — no SaaS dashboard, no third-party-hosted control plane, no inbound internet listener. Your certificates, private keys, target inventory, and configuration stay on the orchestrator host you own.

EudaCertMgr handles issuance, renewal, and deployment across your entire Linux and Windows fleet — automatically, from a single control host inside your network.

**The problem:** The CA/Browser Forum — the industry body that governs every publicly-trusted certificate authority — has mandated a phased reduction of TLS certificate lifetimes for all publicly-trusted certificates: a maximum of 200 days by March 2026, 100 days by March 2027, and **47 days by March 2029**. At that cadence, manually renewing and deploying certificates across a fleet of servers is no longer feasible — every missed renewal is an outage waiting to happen.

**The solution:** EudaCertMgr eliminates the manual work. It renews certificates before they expire, deploys them to every Linux and Windows server that needs one, verifies the deployment is actually live, warns about anything nearing expiry, and notifies your team only when something changes or something fails.

---

## Features

- **Easy install** — up and running in as little as 15 minutes
- **Easy setup** — menu-driven prompts guide you through onboarding targets, configuring certificates, and every administrative task; no config-file editing required
- **Fully on-premises** — runs entirely inside your network on hardware you control. No SaaS dashboard, no third-party-hosted control plane, no agent on managed hosts. Certificates, private keys, target list, and configuration live only on the orchestrator host. The orchestrator initiates every connection it makes; no inbound internet listener is required
- **Automatic renewal** — nightly checks renew certificates before expiry; no manual intervention required
- **Monitor ANY URL certificate** — get email alerts about impending expirations (configurable lead time) for any certificate, even ones not managed by EudaCertMgr. Watch sites whose cert renewal you don't control (WordPress, Squarespace, Shopify, GoDaddy, Wix, self-managed certbot deployments, etc.) and catch silent renewal failures before they become outages
- **Any ACME-compatible CA, selectable per certificate** — Let's Encrypt, ZeroSSL, Google Public CA, Buypass Go SSL, SSL.com, and others
- **180+ DNS providers supported** — Cloudflare, Route53, Azure DNS, GoDaddy, DigitalOcean, and more via acme.sh
- **Multi-target deployment** — deploy to any Linux or Windows system that uses on-disk certificates (web servers, mail servers, load balancers, etc.) in a single operation
- **Customizable deployment scripts** — for the unusual cases (Tomcat, Java keystores, non-systemd daemons, HA pairs, container restarts), open a per-target deployment script in your editor — pre-populated with a heavily-commented default template that walks you through every step, so you can adapt it for your stack without bash or PowerShell expertise
- **Wildcard, subdomain wildcard, single-host, and multi-SAN certificates** — maintain as many certificates as you need side-by-side: a wildcard (`*.example.com`), subdomain wildcards (`*.api.example.com`), specific hostname certs (`proxy.example.com`), or a single cert covering several names via Subject Alternative Names (e.g. `example.com` + `example.io` + `*.us.example.com`). Each certificate has its own deployment targets, renewal settings, and alerts.
- **Local self-signed CA for internal hostnames** — for hosts that can't reach a public ACME CA (split-DNS, lab boxes, machines on `*.internal.example.com` that aren't world-routable), EudaCertMgr runs a single local Certificate Authority that signs internal leaves alongside the ACME-issued ones. Linux targets get the root pushed into their OS trust store automatically during onboarding. For Windows, EudaCertMgr writes the root directly into Active Directory over LDAPS — no SSH, no PowerShell remoting, no certutil on the DC required — and AD's built-in root-cert propagation distributes it to every domain-joined Windows machine on the next gpupdate. For Macs, EudaCertMgr exports the root as a `.mobileconfig` Configuration Profile that drops directly into Jamf / Kandji / Intune / Mosyle for fleet-wide push, or that a user can double-click to install with the trust setting already baked in. One LDAPS publish, forest-wide reach. One profile, fleet-wide reach.
- **Built-in verification** — after every deploy, EudaCertMgr connects to the target to confirm it is actually serving the new certificate
- **Automatic HTTPS enablement for HTTP-only sites** — the scanner detects web server sites with no TLS configured and offers to set TLS up correctly, and can automatically add HTTP→HTTPS redirect rules (nginx and Apache only)
- **Per-vhost TLS health check + take-over (nginx, Apache, IIS)** — when you add or update a deployment target, EudaCertMgr walks every vhost / IIS site it finds — including sites defined in custom `Include` paths anywhere on disk — surfaces issues per site (expired cert, cert/key mismatch, site name not covered by the cert's SANs, self-signed cert, missing port-80 redirect, missing HTTPS binding), and offers — one Y/N at a time — to take the site over and replace the broken cert on the next deploy. IIS sites with no HTTPS binding can be auto-bound at deploy time. Re-running the wizard on an already-onboarded host is the supported path for adding a newly-created vhost; the existing target config is merged, not overwritten, so any manual entries are preserved.
- **Automatic domain ownership verification** — EudaCertMgr automatically proves domain ownership to the certificate authority via API calls to your DNS provider; once set up, it's all automatic and hands-off
- **Automatic target provisioning** — onboarding automatically creates and configures the service account, SSH access, and permissions on each new target
- **DNS delegation setup** — creates the challenge sub-delegation zone needed for DNS-01 validation and shows you exactly what to add to your DNS to get started
- **Fast-path renewal** — skips the certificate authority entirely when the local certificate still has plenty of life, avoiding unnecessary API calls
- **Previous-certificate retention** — each renewal keeps the prior certificate on disk so you can roll back a deployment if needed
- **Per-target enable/disable** — flip a single flag from the menu to skip a target during nightly runs without removing it
- **Per-certificate configuration** — independent targets, alerts, renewal thresholds, retention, and DNS provider for each certificate
- **Encrypted backup and restore of the entire configuration**

In addition to the automated nightly mode, EudaCertMgr provides a full interactive menu that lets you:

- Show the current configuration — certificates, targets, thresholds, and global settings at a glance
- Onboard new Linux and Windows targets (IIS, RDP/WinRM, and generic `LocalMachine\My` drops for custom apps)
- Force a reissue or deploy-only operation for any certificate
- Add, remove, edit, or temporarily disable deployment targets
- Manage TLS verification URLs — both managed endpoints (auto-populated by the wizard) and external-only monitors for any third-party HTTPS site whose cert you want EudaCertMgr to watch
- Manage email recipients, expiry thresholds, log retention, DNS provider, and DNS credentials
- Change the nightly timer schedule
- Back up and restore the installation (encrypted backups + auto-reprovision of remote service accounts after restore)
- Initialize and manage a local self-signed CA for lab boxes, split-DNS internal services, and anything that can't reach a public ACME CA — including pushing the root into Linux targets' trust stores, publishing it forest-wide to Active Directory for Windows GPO distribution, and exporting it as an Apple Configuration Profile for MDM-managed Macs
- Manage licensing — view each license slot and its status, buy a license, or cancel a license or trial; each issuer/domain pair carries its own license

---

## EudaCertMgr vs. the Others

EudaCertMgr is the only product that combines all of these capabilities in a single self-hosted, menu-driven tool:

| Capability | EudaCertMgr | Certify The Web | certbot + scripts | Sectigo CM / DigiCert CertCentral / Keyfactor / Venafi |
|---|:---:|:---:|:---:|:---:|
| Cross-platform: Linux + Windows from one orchestrator | ✓ | ✗ (Windows Server only) | ✗ (DIY) | ✓ |
| No agent / app on managed hosts (SSH key only) | ✓ | ✗ (Windows app per server) | ✓ | ✗ (agents) |
| Time from installer to first deployed cert | **under 15 min** | 30+ min per server | days | weeks |
| Per-vhost TLS audit + take-over (nginx / Apache / IIS) | ✓ | ✗ | ✗ | partial |
| Bundled local self-signed CA for internal / lab / split-DNS hosts | ✓ | ✗ | ✗ | partial (separate product) |
| External-URL TLS expiry monitoring (third-party sites) | ✓ | ✗ | ✗ | ✓ |
| Route 53 `_acme-challenge` sub-delegation setup | ✓ | ✗ | ✗ | ✗ |
| DNS providers for DNS-01 | **180+** | 36 | partial | ✓ |
| Encrypted backup + auto-reprovision restore | ✓ | ✗ | ✗ | ✓ |
| Headless CLI + automation-friendly | ✓ | partial (GUI-first) | ✓ | ✓ |
| Fully self-hosted, no cloud dependency | ✓ | partial (dashboard is cloud) | ✓ | partial (mostly SaaS) |
| Pricing model | **flat $549 / domain / yr** | per-server tiers | free | enterprise quoting |

*Certify The Web's "centralized dashboard" is renewal-monitoring only — every managed server still runs its own copy of the Windows desktop app. EudaCertMgr's model is one Linux orchestrator that pushes to every target over SSH; targets carry no EudaCertMgr software footprint at all.*

EudaCertMgr is fully on-premises — it runs on your hardware, behind your firewall, with no cloud dashboard to maintain and no third-party-hosted control plane sitting between you and your certificates. There's no API surface to secure, no HSM to provision, no Kubernetes operator to run. You install it, point it at your fleet, and never think about TLS renewals again.

---

## Requirements

### EudaCertMgr Server

Supported distributions: RHEL, Rocky Linux, AlmaLinux, CentOS Stream, Debian, Ubuntu, and any modern systemd-based Linux. Linux/amd64 and linux/arm64 binaries are published for each release.

- bash 4.0+ (used by the per-target deployment wrappers)
- openssh-client (ssh, scp, ssh-keygen)

The Go binary embeds acme.sh, the SMTP client, and TLS verification logic — no separate `openssl`, `curl`, or `s-nail` packages are required at runtime.

### DNS Provider Credentials

EudaCertMgr uses DNS-01 challenges, which require API access to your DNS provider. Refer to the [acme.sh DNS API documentation](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) for the credentials your provider requires. The installer prompts for these during first-time setup and stores them securely on the EudaCertMgr server.

### Linux Targets

- Any application that uses on-disk certificates (e.g. nginx, Apache, Postfix)
- An admin account with SSH access and sudo capability (passwordless or password-based)

Onboarding connects as that admin account and handles the rest automatically.

### Windows Targets

- Windows Server 2016+
- Administrator access to copy and run a PowerShell script on the target

Onboarding generates a customized PowerShell bootstrap script that handles OpenSSH installation, user creation, and SSH key setup.

---

## Installation

Download the installer tarball for your architecture from the [latest release](https://github.com/EudaSystems/eudacertmgr-public/releases/latest), copy it to the host that will be your EudaCertMgr server, and run it as root:

```bash
tar -xzf eudacertmgr-installer-linux-amd64.tar.gz
sudo ./eudacertmgr-installer
```

(Use `eudacertmgr-installer-linux-arm64.tar.gz` on ARM64 hosts.)

The installer is interactive and idempotent. It will:

1. Display the End User License Agreement and Disclaimer (shown in full at the bottom of this README) and require you to type `ACCEPT` to continue
2. Create the `eudacertmgr` system user and group
3. Install the EudaCertMgr binary and embedded deploy wrappers to `/opt/eudacertmgr/`
4. Generate an Ed25519 SSH keypair for the `eudacertmgr` user at `/opt/eudacertmgr/.ssh/id_ed25519`
5. Install and reload the systemd service and timer units
6. Enable the nightly renewal timer

After install, finish setup from the interactive menu:

```bash
sudo /opt/eudacertmgr/eudacertmgr
```

→ **7) Manage system settings** to configure SMTP / EMAIL_FROM / DNS provider, then onboard targets via **4) Manage deployment targets**.

Re-running the installer detects an existing install and offers two paths:

- **Upgrade** — replaces the binary and bash deploy wrappers; configs, certs, SSH keys, and acme.sh state stay untouched.
- **Overwrite** — wipes `/opt/eudacertmgr/` first (destructive; second confirm required).

---

## Onboarding Deployment Targets

All target onboarding is done through the interactive menu. Start it by running:

```bash
sudo /opt/eudacertmgr/eudacertmgr
```

### Linux

From the main menu, select **4) Manage deployment targets → A) Add a deployment target → 1) Linux target**. You will be prompted for the target hostname and an admin account username. EudaCertMgr connects over SSH as that admin user (prompting for a sudo password if required) and automatically:

1. Creates the `eudacertmgr` user on the target
2. Configures passwordless sudo for the `eudacertmgr` user
3. Installs the EudaCertMgr server public key into the target's `eudacertmgr` authorized keys
4. Verifies end-to-end key-based SSH and passwordless sudo
5. Scans the target for web and mail services (nginx, Apache, HAProxy, Postfix, Dovecot) and either registers existing TLS configurations or offers to enable HTTPS on sites that don't yet have it
6. Scans `/etc` for certificate files already on disk that match any configured domain and offers to add each as a deployment target
7. Optionally accepts a manual certificate path for services or layouts the scanner doesn't recognize
8. Registers the target under a certificate of your choice — an existing wildcard, subdomain wildcard, or single-host cert, or creates a new one

### Windows

From the main menu, select **4) Manage deployment targets → A) Add a deployment target → 2) Windows target**. EudaCertMgr generates a customized PowerShell bootstrap script for the target. Copy that file to the Windows server and run it in an elevated PowerShell session:

```powershell
powershell -ExecutionPolicy Bypass -File bootstrap_<hostname>.ps1
```

The bootstrap script automatically:

1. Installs OpenSSH (using the bundled MSI if not already present)
2. Creates the `eudacertmgr` local user with Administrator rights
3. Configures SSH key authentication for the EudaCertMgr server public key
4. Starts and enables the OpenSSH service

Once complete, return to the onboarding flow — it will verify SSH connectivity, check IIS status, and register the target under the certificate you choose.

### Removing or Disabling a Target

To permanently remove a target, use **4) Manage deployment targets → pick the target → R) Remove** from the menu (you'll be offered the option to also clean up the remote service account). To temporarily disable a target without removing it, use **4) Manage deployment targets → pick the target → 1) Toggle enable/disable**.

### Customizing the Deployment Script

Every deployment target has a deployment script that runs on the remote host whenever a cert renews. EudaCertMgr ships with a built-in default that handles the common case (copy the cert files into place, reload the service) for nginx, Apache, postfix, IIS, and most other on-disk-cert workflows. For targets where the default doesn't fit — a Java keystore, Tomcat on Windows, a non-systemd daemon, an HA pair, an app that needs a special reload, a service that wants the cert in a non-standard format — switch the target to Custom mode and edit the script directly.

#### Default vs Custom

Each target is in one of two states:

- **Default** — EudaCertMgr runs its built-in cert-copy + reload logic. Recommended unless you have a specific reason to customize.
- **Custom** — your edited script replaces the built-in logic for that target. Other targets are unaffected.

Switching is per-target: a wildcard cert can deploy to ten servers where nine use the default and one runs a custom script.

#### Editing the deployment script

You can switch a target to Custom from two places:

- **During onboarding**, answer `C` to the *Deployment script* prompt (default is `D`).
- **After onboarding**, go to **4) Manage deployment targets → pick the target → 4) Edit deployment script**.

Either path opens an editor on the target's deployment script, pre-populated with the built-in default as an extensively commented starting point. You can insert steps before or after the copy, change the reload to suit your service, or replace the script entirely.

#### Reverting to the default

To remove a custom script and revert a target to the built-in default, go to **4) Manage deployment targets → pick the target → 5) Delete custom deployment script**. The option only shows up when the target is currently on Custom.

#### What the listings show

Targets running a custom script show a `(custom)` suffix on the hostname in both the *Manage deployment targets* listing and the *Show current configuration* output. Multi-cert hosts where some certs run custom and others run the default get `(custom*)`. Default targets show no suffix.

#### What the script can use

When EudaCertMgr runs your script on the remote, it sets these environment variables:

| Variable | Meaning |
|---|---|
| `EUDACERTMGR_CERT_FQDN` | Certificate FQDN (e.g. `*.example.com`) |
| `EUDACERTMGR_TARGET_HOST` | Target host FQDN |
| `EUDACERTMGR_DEPLOY_TS` | Unix seconds at deploy start |
| `EUDACERTMGR_FORCE_DEPLOY` | `1` if force-deploy, else `0` |
| `EUDACERTMGR_CERT_PATH` (Linux) | Path to the renewed cert PEM, already staged on the target |
| `EUDACERTMGR_KEY_PATH` (Linux) | Path to the renewed private key PEM, already staged on the target |
| `EUDACERTMGR_CERT_STYLE` (Linux) | `combined` or `separate` |
| `EUDACERTMGR_REMOTE_PFX` (Windows) | Path to the uploaded PFX on the Windows host |
| `EUDACERTMGR_PFX_PASSWORD` (Windows) | PFX password — passed in fresh on every deploy, never stored in the script |
| `EUDACERTMGR_CERT_FILE_KEY` (Windows) | Cert FQDN with the leading `*.` stripped, useful for binding-match logic |

#### Failure handling

Exit non-zero from your script to mark the deployment as failed. EudaCertMgr fires the same failure-email path it uses for any other error and continues the nightly run with the next target.

---

## Local Self-Signed CA

For hosts that can't satisfy public ACME validation — split-DNS internal services, lab machines, anything on a hostname that isn't reachable from Let's Encrypt's validation infrastructure — EudaCertMgr can issue, deploy, and renew certificates from its own local Certificate Authority. The CA lives on the orchestrator host alongside the ACME state and is invisible to ACME certificates: every certificate routes through its configured issuer independently, so `mail.example.com` (public, ACME) and `de01.example.com` (internal, self-signed) coexist under the same root domain without any cross-talk.

### Initialize the CA

From the top-level menu: **11) Manage local self-signed CA → Initialize CA**.

You're prompted for:

- **Common Name** — defaults to `EudaCertMgr Local Root (<orchestrator-hostname>)`. This is the name your operators (and `openssl x509`) will see when inspecting issued certs.
- **Key algorithm** — ECDSA P-256 (recommended) or RSA-2048.
- **Validity** — in years, default 20. The root is long-lived; each leaf signed by it picks its own validity (see below).

On success, EudaCertMgr also installs the new CA root into the orchestrator's own OS trust store (`update-ca-trust` on RHEL family, `update-ca-certificates` on Debian family) so the post-deploy TLS verification step can chain-validate self-signed certs without any extra configuration.

You can re-run **Initialize CA** later to rotate the root, but it requires typing `ROTATE` to confirm — a rotation invalidates every cert previously signed by the old root and every target's trust store needs the new root pushed.

### Create a self-signed certificate

The new-certificate wizard now starts with an issuer prompt:

```
Issuer for this cert
> ACME (public CA)
  Self-signed (local CA)
```

Picking **Self-signed** skips the DNS provider prompt entirely (there's no DNS-01 challenge) and offers an optional per-certificate validity override in days (blank inherits the system default of 90). If the CA hasn't been initialized yet, the wizard offers a one-keypress confirm to initialize it inline with defaults — you don't have to bounce back to the top-level menu.

The resulting `cert.conf` records `ISSUER="selfsigned"` (and `SELFSIGN_VALIDITY_DAYS=N` if overridden). ACME certificates omit `ISSUER` entirely, so every cert.conf authored before self-signed support shipped is treated as ACME and continues to behave identically.

### Trust distribution to targets

The CA root is pushed to a target's OS trust store at two moments:

1. **Onboarding** — `RunLinux` / `RunWindows` installs the CA root onto the target once key-based SSH is verified. Reprovisioning an existing target re-installs (idempotent).
2. **Manage local self-signed CA → Push CA root to existing target** — for targets onboarded before the CA was initialized, or to re-push after a root rotation.

Linux uses `update-ca-trust extract` (RHEL family) or `update-ca-certificates` (Debian family). Windows uses `certutil -addstore Root`. The deploy pipeline itself is unchanged — it copies cert bytes to the configured service paths exactly as it does for ACME-issued certs.

### Validity and renewal

Self-signed leaves resolve their lifetime in this order: per-cert `SELFSIGN_VALIDITY_DAYS` → system-level `SELFSIGN_DEFAULT_VALIDITY_DAYS` (in `eudacertmgr.conf`) → built-in 90 days. The renewer's "is this cert within the renewal window" check reads each cert's actual `NotAfter` and applies the same `CERT_RENEWAL_DAYS` threshold (default 10 days) that ACME certs use — there's no separate cadence to keep track of.

### Removing a self-signed certificate

Deleting a self-signed cert from EudaCertMgr leaves the CA root installed in the orchestrator's and every target's trust store. The CA root is shared across every self-signed cert on this orchestrator, so removing one cert doesn't invalidate the root.

To revoke trust completely (rare — usually because you're decommissioning the local CA entirely):

```bash
# RHEL/Rocky/Alma/CentOS family
sudo rm /etc/pki/ca-trust/source/anchors/eudacertmgr-local-ca.pem
sudo update-ca-trust extract

# Debian/Ubuntu family
sudo rm /usr/local/share/ca-certificates/eudacertmgr-local-ca.crt
sudo update-ca-certificates

# Windows (from elevated PowerShell)
certutil -delstore Root <thumbprint>
```

These commands need to run on each target — EudaCertMgr does not push trust uninstalls automatically.

### Caveats

- **The CA private key is stored on disk with `0600` permissions, no passphrase.** A passphrase would force either a stored-passphrase file (same effective threat model) or break unattended renewals. Protect `<base>/ca/` like the SSH private key it sits next to.
- **CRL / OCSP are not implemented.** Once a self-signed cert is issued, the only way to "revoke" it is to remove it from the targets it was deployed to and let it expire naturally — short validity periods (the default 90 days) keep the effective revocation window tight.
- **Backups include the CA.** `Back up configuration` writes `<base>/ca/` and `<base>/selfsigned/` into the tarball when present. A restore brings the CA private key back — keep encrypted backups in a place you trust accordingly.

---

## How It Works

Once configured, you don't need to do anything else. EudaCertMgr runs every day automatically, checking each of your certificates. If a certificate is within the renewal threshold (default 14 days, configurable at the system and per-certificate level), EudaCertMgr handles the whole process — using a DNS-01 challenge to prove domain ownership, obtaining a fresh certificate from your chosen authority, and pushing it out to every server that needs it. It then reaches out to each site to confirm the new certificate is actually live. If any deployed certificate is within the expiry warning window (default 10 days, also configurable) you'll get a warning email. Your team gets an email when something new is issued, and another if anything goes wrong. On nights when nothing needs to happen, it stays quiet.

Nightly runs are resilient — a single failed target (unreachable host, expired credential, service down) doesn't abort the batch. EudaCertMgr logs the failure, continues on to the remaining targets, and reports everything in the failure email.

---

## Usage

Everything EudaCertMgr does is menu-driven. Launch the menu:

```bash
sudo /opt/eudacertmgr/eudacertmgr
```

Pick an option, follow the prompts. Reasonable defaults are pulled from your existing config, so most operations are just hitting Enter. You won't need to edit any files in `/opt/eudacertmgr/` directly — the menu manages them all for you.

### Deployment Target Selection

When an interactive operation prompts for deployment targets you can deploy to all servers, only Linux, only Windows, or any specific subset:

| Input | Action |
|---|---|
| `A` | All servers (default) |
| `L` | All Linux servers |
| `W` | All Windows servers |
| `1,3,5` | Specific servers by number |
| `Q` | Back / cancel |

---

## Automation — systemd Timer

The timer fires nightly and calls `eudacertmgr renew`, which processes every enabled certificate and every enabled target within it.

```bash
systemctl status eudacertmgr.timer      # Check timer status
systemctl list-timers eudacertmgr       # Show next scheduled run
journalctl -u eudacertmgr.service       # View service logs
```

The renewal schedule can be changed from the menu under **Manage system settings → Change timer schedule**.

---

## Logs and Notifications

Per-run logs are written to `/opt/eudacertmgr/logs/` with filenames of the form `<cert-fqdn>_<timestamp>.log` (wildcard certs use the literal string `wildcard` in place of the `*`). Old logs are pruned automatically on a configurable retention schedule.

Email is sent on these events:

- **A new certificate was issued** — success summary with certificate details (configurable — `NOTIFY_ON_SUCCESS`, default ON)
- **A step failed** — error summary with the last 100 lines of the run log (always on, cannot be disabled)
- **A certificate is nearing expiry** — warning when a deployed certificate is within the configured expiry threshold (default 7 days); fires as a failure during the nightly TLS verification pass
- **The workflow ran but nothing changed** — no-op summary when a cert is already current and all TLS checks pass (configurable — `NOTIFY_ON_NOOP`, default OFF)

`NOTIFY_ON_SUCCESS` and `NOTIFY_ON_NOOP` are set globally in `eudacertmgr.conf` and can be overridden per certificate. Failure emails cannot be suppressed. With the defaults, nights where no renewal was needed and no expiry warnings tripped are silent.

The default SMTP transport is plaintext (port 25). For relays that require encryption, set `SMTP_STARTTLS=auto` or `always`, and `SMTP_TLS_SKIP_VERIFY=1` if the relay presents a shared-tenant or self-signed certificate.

---

## Certificate Renewal Logic

A certificate is considered current when it has more than the renewal threshold (default 10 days) until expiry — renewal only happens when it's missing, within that threshold, or a force reissue is explicitly requested. Both the renewal threshold and the expiry warning window (default 7 days) can be set at the system level and overridden per certificate.

Deployment to a target is skipped when the target already has the identical certificate (verified by SHA-256 comparison); a force operation deploys regardless.

---

## Uninstalling

```bash
sudo eudacertmgr uninstall
```

The uninstaller optionally cleans up the `eudacertmgr` user on each registered remote target (all, one-by-one, or skip).

If you later restore onto a fresh install, EudaCertMgr auto-detects any deployment targets whose remote `eudacertmgr` user was removed by the uninstaller and offers to re-provision them in one pass.

---

## Security Notes

- The EudaCertMgr server authenticates to every target using SSH keys — no admin passwords are ever stored. The sudo password entered during Linux onboarding is used once for the initial account setup and is never written to disk.
- The eudacertmgr SSH private key grants remote sudo/Administrator access to all registered targets. Protect `/opt/eudacertmgr/` accordingly.
- PFX files are encrypted with a per-certificate password generated at onboarding.
- DNS provider credentials should be scoped to the minimum permissions required for DNS record management.
- The `eudacertmgr` OS account on each target is created by the onboarding flow and should not be used for interactive logins outside of eudacertmgr operations.
- Backups of the installation can optionally be password-encrypted from the menu — recommended when storing backups off-host, since an unencrypted backup contains the SSH private key and DNS provider credentials.
- Custom deployment scripts may contain secrets (API tokens, webhook URLs). They live as regular files alongside the target config (`<cert_dir>/targets/<host>.deploy.sh` on Linux, `.deploy.ps1` on Windows) at mode `640 eudacertmgr:eudacertmgr` — same protection as SSH keys and cert private keys.
- When the **local self-signed CA** is initialized, the CA private key at `/opt/eudacertmgr/ca/ca.key` can sign any internal cert chained to that root. Anyone with read access to that file can mint server certs trusted by every machine that imported the CA root — protect it like the SSH private key it sits next to (file mode `600`, owned by the `eudacertmgr` user, no passphrase by design so the renewer can run unattended under the systemd timer).

---

## End User License Agreement

This End User License Agreement ("Agreement") is a binding legal agreement between you, or the organization on whose behalf you are installing or using this software ("Licensee"), and Euda Systems, Inc., a Texas corporation ("Licensor"). By installing, copying, accessing, or otherwise using EudaCertMgr (the "Software"), Licensee agrees to be bound by the terms of this Agreement. If Licensee does not agree to these terms, Licensee must not install or use the Software.

**1. LICENSE GRANT.** Subject to Licensee's full and continuing compliance with this Agreement and payment of all applicable fees, Licensor grants Licensee a limited, non-exclusive, non-transferable, non-sublicensable license to install and run the Software solely for Licensee's own internal business operations, on hardware controlled by Licensee, and solely for the number of domains, hosts, or other licensable units covered by Licensee's active subscription or paid license entitlement.

**2. RESTRICTIONS.** Except as expressly permitted by this Agreement, Licensee shall not, and shall not permit any third party to:

- **(a)** copy, reproduce, or duplicate the Software, in whole or in part, except for a single archival backup copy retained solely for disaster-recovery purposes;
- **(b)** distribute, publish, sell, resell, sublicense, rent, lease, lend, host as a service, or otherwise make the Software available to any third party;
- **(c)** modify, adapt, translate, or create derivative works of the Software;
- **(d)** reverse engineer, decompile, disassemble, or otherwise attempt to derive the source code, underlying ideas, algorithms, structure, or organization of the Software, except to the limited extent applicable law expressly prohibits this restriction notwithstanding contractual waiver;
- **(e)** remove, obscure, or alter any copyright, trademark, license, or other proprietary-rights notices contained in or displayed by the Software;
- **(f)** use the Software to develop, train, or improve any product that competes with the Software, or to benchmark the Software for publication without Licensor's prior written consent;
- **(g)** circumvent, disable, or interfere with any licensing, security, authentication, or usage-metering mechanism in the Software, including but not limited to forging, altering, or replaying license keys or tampering with any signed configuration; or
- **(h)** use the Software in violation of any applicable law, regulation, or third-party right.

**3. OWNERSHIP.** The Software is licensed, not sold. Licensor and its licensors retain all right, title, and interest in and to the Software, including all copyrights, patents, trade secrets, trademarks, and other intellectual property rights. No rights are granted to Licensee except as expressly set forth in this Agreement. All rights not expressly granted are reserved by Licensor.

**4. CONFIDENTIALITY.** The Software, including its source code, structure, organization, and non-public functionality, constitutes the confidential and proprietary information of Licensor. Licensee shall protect the Software with at least the same degree of care it uses to protect its own confidential information of similar importance, and in no event less than a reasonable degree of care.

**5. THIRD-PARTY COMPONENTS.** The Software includes or interoperates with third-party open-source components, each of which is licensed under its own terms. Those terms govern the use of the corresponding component and are unaffected by this Agreement.

**6. TERM AND TERMINATION.** This Agreement is effective upon Licensee's first use of the Software and continues until terminated. Licensor may terminate this Agreement immediately upon written notice if Licensee breaches any term of this Agreement and fails to cure such breach within ten (10) days of written notice. Upon termination, Licensee shall immediately cease all use of the Software and destroy or permanently remove all copies in its possession or control. Sections 2, 3, 4, 6, 7, 8, 9, 10, 11, and 12 survive termination.

**7. DISCLAIMER OF WARRANTIES.** The Software is provided to Licensee as set forth in the accompanying Disclaimer, the terms of which are incorporated into this Agreement by reference. Without limiting that disclaimer, Licensor makes no warranty that the Software will be uninterrupted, error-free, or meet Licensee's specific requirements.

**8. LIMITATION OF LIABILITY.** To the maximum extent permitted by applicable law, in no event shall Licensor's aggregate liability arising out of or related to this Agreement or the Software exceed the fees paid by Licensee to Licensor for the Software during the twelve (12) months immediately preceding the event giving rise to the claim. In no event shall Licensor be liable for any indirect, incidental, special, consequential, or punitive damages, or for lost profits, lost revenue, lost data, or business interruption, even if advised of the possibility of such damages.

**9. GENERAL.** This Agreement constitutes the entire agreement between the parties with respect to the Software and supersedes all prior or contemporaneous understandings. This Agreement is governed by the laws of the State of Texas, without regard to its conflict-of-laws rules. Any dispute arising under this Agreement shall be brought exclusively in the state or federal courts located in Dallas County, Texas, and the parties consent to the personal jurisdiction of those courts. If any provision of this Agreement is held unenforceable, the remaining provisions remain in full force and effect. Failure to enforce any provision is not a waiver of the right to enforce it later. Licensee may not assign this Agreement without Licensor's prior written consent; Licensor may assign this Agreement freely. This Agreement does not create any agency, partnership, joint venture, or employment relationship.

**10. JURY TRIAL WAIVER.** Each party hereby irrevocably and unconditionally waives any right it may have to a trial by jury in any legal proceeding directly or indirectly arising out of or relating to this Agreement, the Software, or the transactions contemplated by this Agreement, whether sounding in contract, tort, or otherwise. Each party acknowledges that this waiver is a material inducement for the other party to enter into this Agreement.

**11. WAIVER OF TEXAS DTPA.** Licensee, after consultation (or the opportunity to consult) with an attorney of its own selection, voluntarily waives the provisions of the Texas Deceptive Trade Practices-Consumer Protection Act, Texas Business and Commerce Code §17.41 et seq., a law that gives consumers special rights and protections, except for §17.555 (action for contribution or indemnity). Licensee represents that it is a business consumer with assets of $25 million or more, or is owned or controlled by a corporation or entity with assets of $25 million or more, OR is acquiring the Software for commercial or business use, has knowledge and experience in financial and business matters that enable it to evaluate the merits and risks of the transaction, and is not in a significantly disparate bargaining position.

**12. EXPORT COMPLIANCE.** The Software may be subject to United States export-control laws and regulations, including the Export Administration Regulations (15 CFR Parts 730-774). Licensee shall comply with all applicable export and re-export-control laws and shall not, directly or indirectly, export, re-export, transfer, or release the Software to (a) any country subject to a comprehensive United States embargo or otherwise identified on relevant United States government restricted-country lists; (b) any individual or entity identified on the United States Treasury Department's List of Specially Designated Nationals, the United States Commerce Department's Denied Persons List or Entity List, or any equivalent list maintained by an applicable governmental authority; or (c) any end use prohibited by applicable export-control laws, including nuclear, chemical, biological, or missile end uses. Licensee represents that it is not located in, under the control of, or a national or resident of any such country, and is not on any such restricted-party list.

By proceeding with installation or upgrade, Licensee accepts these terms on behalf of itself and the organization deploying the Software.

---

## Disclaimer

EudaCertMgr is provided **"AS IS", WITHOUT WARRANTY OF ANY KIND**, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement.

In no event shall Euda Systems, Inc., its officers, employees, or contributors be liable for any claim, damages, or other liability — whether in an action of contract, tort, or otherwise — arising from, out of, or in connection with this software or the use or other dealings in this software.

You are responsible for testing EudaCertMgr in your own environment before relying on it in production, monitoring its output, and keeping backups of your certificate state. Certificate management mistakes can take services offline; the operator is responsible for verifying every deployment.

By proceeding with installation or upgrade, you accept these terms on behalf of yourself and the organization deploying this software.

---

## Copyright

Copyright &copy; 2026 Euda Systems, Inc. All Rights Reserved.
Author: Allen Swope

This release: v7.4
