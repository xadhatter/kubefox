# Repositories

## Core (Main? Controller? ???)

Stores clusters, environments, and unmanaged components.

## System

Stores components and applications.

### Example

Below is a set of descriptions and examples describing the Git repository layout using a sample system named `Mail`. It contains four components; `Mail Sender`, `POP Server`, `SMTP Server`, and `Web Client`. The components are split between the two applications; `Mail Client` and `Mass Mailer`.

```tree
┌─ components
│  ├─ mail-sender
│  │  ├─ component.yaml
│  │  ├─ main.py
│  │  ├─ Pipfile
│  │  ├─ Pipfile.lock
│  │  └─ README.md
│  ├─ pop-server
│  |  ├─ src
│  |  |  └─ main
│  |  |     └─ com
│  |  |        └─ acme
│  |  |           └─ Server.java
│  │  ├─ build.gradle
│  │  ├─ component.yaml
│  │  └─ README.md
│  ├─ smtp-server
│  │  ├─ component.yaml
│  │  ├─ main.go
│  │  ├─ go.mod
│  │  ├─ go.sum
│  │  └─ README.md
│  └─ web-client
│     ├─ public
│     |  └─ index.html
│     ├─ src
│     |  └─ index.js
│     ├─ component.yaml
│     ├─ package-lock.json
│     ├─ package.json
│     └─ README.md
├─ applications
│  ├─ mail-client
│  |  ├─ application.yaml
│  │  └─ README.md
│  └─ mass-mailer
│     ├─ application.yaml
│     └─ README.md
├─ system.yaml
└─ README.md
```
