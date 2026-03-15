# 🐘 PostgreSQL — Podman Compose

Deployment of a **PostgreSQL** instance in an isolated container, with data persistence on the host filesystem.

> ⚠️ **This compose file is designed for PostgreSQL >= 18.** For earlier versions, make sure to uncomment the appropriate variable in your `.env` file. See the Configuration section for details.

---

## 📋 Prerequisites

- [Podman](https://podman.io/)
- [podman-compose](https://github.com/containers/podman-compose)
- Linux host OS

---

## 📁 Project structure

```
.
├── .env           # Environment variables file (to create, do not commit)
├── .gitignore
├── compose.yaml   # PostgreSQL service definition
├── example.env    # Environment file template
└── README.md
```

---

## ⚙️ Configuration

### 1. Clone the repository

```bash
git clone git@github.com:ThomasDeOliv/postgresql-compose.git
cd postgresql-compose
```

### 2. Create the `.env` file

Copy `example.env` to `.env` at the root of the project, then edit the parameters according to your setup. All available variables and their roles are documented in `example.env`.

```bash
# Run from the project postgresql-compose directory
cp example.env .env
nano .env
# Edit the .env file and save your configuration
```

> ⚠️ **Please, never commit the `.env` file** — it contains sensitive information.

---

## 🚀 Usage

### Start the container

```bash
# From the project postgresql-compose directory
podman-compose up -d
```

### Stop the container

```bash
# From the project postgresql-compose directory
podman-compose down
```

> This compose file is also compatible with Docker.

---

## 📦 Compose file details

| Parameter         | Value           | Description                                                                                                                                       |
| ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Image             | `postgres:18.1` | Default version. Can be overridden via `PG_VERSION` in `.env`                                                                                     |
| Port              | `5432`          | Standard PostgreSQL port exposed on the host. Can be overridden via `PG_HOST_DB_PORT` in `.env`                                                   |
| Network           | `bridge`        | Dedicated internal network for the service                                                                                                        |
| Volume            | bind mount      | Data path defined via `DB_DATA_PATH` in `.env`                                                                                                    |
| `:Z`              | SELinux label   | SELinux relabeling for Podman. Allows the container to access the bind mount on SELinux enforcing systems (Fedora, RHEL…). Harmless under Docker. |
| `restart: always` | —               | Automatically restarts after a crash or reboot                                                                                                    |

---

## 🔒 Security

- The `.env` file **must never be versioned**.
- In production, prefer a secrets manager (Vault, Podman secrets, Docker secrets).
- Restrict access to port `5432` via a firewall if the database is not meant to be exposed on the network.

---

## 📄 License

This project is released under the [Unlicense](https://unlicense.org/).
