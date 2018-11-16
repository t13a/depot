# Depot

A light-weight and general-purpose and self-hosted dependency management tool. This project is currently heavily in development. The documents are not maintained and the APIs are unstable.

## Prerequisites

- `bash`
- `curl`
- `git`
- `python`
    - `yaml` module

## Getting Started

To bootstrap Depot, execute following command in empty repository.

```sh
$ curl -L https://raw.githubusercontent.com/t13a/depot/v0.2.0/contrib/bootstrap/bootstrap | sh
```

Then these files are created.

```
.
├── .depot
│   └── vendor
│       └── github.com
│           └── t13a
│               └── depot.git
├── .depot.yaml
├── .gitignore
└── vendor
    └── github.com
        └── t13a
            ├── depot
            │   ├── LICENSE
            │   ├── README.md
            │   ├── contrib
            │   ├── depot
            │   └── depot-file
            │   ├── depot-git
            └── depot.lock
```

To add your own dependencies, edit manifest file (`.depot.yaml`) like following. Any files or Git repositories are supported. Currently there is no mechanism to prevent inconsistency due to updating of Depot itself, so please write down the item of Depot at the end.

```yaml
api: v1alpha2
dir: vendor
env:
  DEPOT_FILE_OUT_PREFIX: ../.depot/vendor/
  DEPOT_FILE_PATH_FORMAT: hostpath
  DEPOT_GIT_CLONE_DIR_PREFIX: ../.depot/vendor/
  DEPOT_GIT_PATH_FORMAT: hostpath
sync:
- sync:
  - git:
      repo: https://YOUR_DOMAIN/YOUR_REPO
  - git:
      repo: git@YOUR_DOMAIN:YOUR_REPO
      branch: YOUR_BRANCH
      paths: [ YOUR_PATH, ... ]
  - file:
      url: https://YOUR_DOMAIN/YOUR_FILE
  - env: { EXT: .tar.gz }
    file:
      url: https://YOUR_DOMAIN/YOUR_ARCHIVE${EXT}
      post:
        cmd: |
          DIR="${OUT%${EXT}}"
          mkdir -p "${DIR}"
          tar -xzf "${OUT}" -C "${DIR}"
  - env: { EXT: .zip }
    file:
      url: https://YOUR_DOMAIN/YOUR_ARCHIVE${EXT}
      post:
        cmd: |
          unzip -d "${OUT%${EXT}}" "${OUT}"
- git:
    repo: https://github.com/t13a/depot
    tag: v0.2.0
```

To apply manifest file, execute `run` command:

```sh
$ vendor/github.com/t13a/depot/depot run .depot.yaml
```
