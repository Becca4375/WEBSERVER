# Group_02_webInfrastructure

Submission for the **Web Infrastructure** module (BSc(Hons) Software Engineering, Year 1) — building and securing a mini production-ready web infrastructure on a single Ubuntu 26.04 (WSL2) machine.

## Team Webserver
Edith Amondi, Rebecca Adhiambo, Leatitia Mizero, Nelson Fodjo Kamdoum

## What we did

We used one Linux machine (Ubuntu 26.04 under WSL2) and created two separate user accounts — `becca` (client/admin) and `webserver` (SSH server target) — to genuinely demonstrate client-server interaction, login, and permission boundaries, even without a second physical machine.

- **Linux environment**: Identified the environment (`hostname`, `whoami`, `ip addr`, `/etc/os-release`) and created the second user account (`webserver`) to serve as the SSH target.
- **SSH (client & server)**: Installed and started `sshd`, inspected `sshd_config`, demonstrated password-based login (and a deliberately failed attempt with log evidence), then generated an Ed25519 key pair and deployed key-based authentication — verified via `ssh -v` that public-key auth (not password) authenticated the session. Transferred files across the user boundary with both `scp` and `sftp`.
- **NGINX**: Installed NGINX, explained the config directory hierarchy and the `sites-available` → `sites-enabled` symlink activation model, built a custom site (`index.html`, `404.html`) with a server block (custom header, `try_files`, custom error page), validated syntax with `nginx -t`, and confirmed it was served via `curl`.
- **HTTP methods & status codes**: Tested GET/POST/PUT/DELETE/HEAD against the static site and explained why only GET/HEAD succeed. Deliberately produced all four required status codes (200, 403, 404, 405) with real evidence for each.
- **Telnet & raw HTTP**: Verified port reachability (22, 80), showed the raw SSH banner in cleartext, proved "connection refused" vs "listening" behavior by stopping/starting NGINX, and hand-typed raw HTTP requests over Telnet (including a request for a non-existent page and a malformed request missing the `Host` header) — with an explanation of why this technique only works over unencrypted HTTP, not HTTPS.
- **API interaction**: Used the public JSONPlaceholder API to run three distinct requests (GET single resource, GET filtered collection, POST new resource), documented status codes and JSON structure, then deliberately triggered a 404 (invalid resource ID) and corrected it with a valid one.
- **Troubleshooting scenario**: Simulated and resolved a real 403 Forbidden error caused by `index.html` having `000` permissions — followed a full 5-stage root-cause process (symptom → log evidence → technical investigation → root cause → fix & verification), restoring access with `chmod 644`.

Overall, the assignment ties together user identities, SSH, NGINX, HTTP, Telnet, and a public API to show that "client" and "server" are roles defined by behavior, not separate hardware — and that ports, permissions, protocols, and processes are the real mechanisms underneath.

## Structure

```
Group_02_webInfrastructure/
├── Technical_Report.pdf        # Full write-up: architecture, all parts A–G, conclusion
├── Demo/
│   └── demo_link.txt           # Link to demo video/walkthrough
├── Website/
│   ├── index.html              # Custom site homepage
│   └── 404.html                # Custom error page
├── NGINX/
│   └── nginx_configuration.txt # Server block config (sites-available/teamwebserver)
├── SSH/
│   ├── ssh_configuration.txt   # sshd_config values used
│   └── id_ed25519.pub          # Public key ONLY — no private key included
├── API/
│   └── Postman_Collection.json # (or API_Requests.txt) — JSONPlaceholder requests
└── Evidence/
    ├── Linux/                  # Environment identification, user creation
    ├── SSH/                    # Password + key-based auth, SCP/SFTP
    ├── NGINX/                  # Install, symlinks, site validation
    ├── HTTP/                   # Methods, status codes, headers
    ├── Telnet/                 # Port checks, raw HTTP requests
    ├── API/                    # GET/POST requests + error handling
    └── Troubleshooting/        # 403 Forbidden scenario (5-stage RCA)
```
