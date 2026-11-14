# mysql-backend-checker

An HTTP MySQL backend checker for HAProxy. A GET returns 200 after a recent
successful check with MySQL's `read_only` set to `OFF`. It returns 503 during
startup, on a failed or read-only check, and once a successful result expires.

## Setup

Use Python 3 with a virtual environment.

```sh
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
cp backend_monitor.cfg backend_monitor.local.cfg
chmod 600 backend_monitor.local.cfg
```

Fill in the MySQL credentials in the local configuration. Set `mysql_socket`
for a Unix socket connection. The database name is optional. The HTTP listener
defaults to loopback. If HAProxy runs on another host, set `bind_address` to an
interface reachable from that host.

```sh
BACKEND_CHECKERCFG="$PWD/backend_monitor.local.cfg" .venv/bin/python backend_monitor
```

The default config path is `/usr/local/bin/backend_monitor.cfg`. Existing
`[mysql]` and `[http]` settings are accepted. The monitor now runs in the
foreground and logs to stderr. Replace the old `start`, `stop`, and
`restart` commands with your service manager and configure log collection
there. The former `[daemon]` PID and log file settings are obsolete.
