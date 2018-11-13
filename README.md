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
$ curl -L https://git.io/fpYrR | sh
```

Then these files are created.

```
.
├── .depot
│   └── vendor
│       └── github.com
│           └── t13a
│               └── depot
│                   └── .git
├── .depot.yaml
├── .gitignore
└── vendor
    └── github.com
        └── t13a
            └── depot
                ├── LICENSE
                ├── README.md
                ├── contrib
                │   └── bootstrap
                ├── depot
                ├── depot-git
                └── depot-url
```

To add your own dependencies, edit manifest file (`.depot.yaml`) like following. Any URL resources or Git repositories are supported. Currently there is no mechanism to prevent inconsistency due to updating of Depot itself, so please write down the item of Depot at the end.

```yaml
api: v1alpha1
dir: vendor
env:
  DEPOT_GIT_CLONE_DIR_PREFIX: ../.depot/vendor/
  DEPOT_GIT_PATH_FORMAT: hostpath
  DEPOT_URL_PATH_FORMAT: hostpath
sync:
- env: { BRANCH: YOUR_BRANCH }
  url:
    in: https://YOUR_DOMAIN/YOUR_REPO/archive/${BRANCH}.zip
    post:
      cmd: |
        unzip ${OUT} YOUR_REPO-${BRANCH}/YOUR_FILE # OUT=vendor/YOUR_DOMAIN/YOUR_REPO/archive/YOUR_BRANCH.zip
- git:
    repo: git@YOUR_HOST:YOUR_REPO
    branch: YOUR_BRANCH
- git:
    repo: https://github.com/t13a/depot
    branch: master
```

To apply manifest file, execute `run` command:

```sh
$ vendor/github.com/t13a/depot/depot run .depot.yaml
```
