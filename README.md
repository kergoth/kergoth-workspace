# kergoth-workspace

Very simple scripts used to manage my workspaces for work. I prefer to create
a new development workspace for each major development task to keep changes
isolated, particularly when the project consists of multiple repositories.
These little scripts ease this workflow for my purposes.

## Environment variables

```sh
WORKSPACE_SETUP_SCRIPT=./workspace-setup
WORKSPACE_PROJECT_ROOT=~/Projects
WORKSPACE_PROJECT_PATTERN="*"
# The attach script is assumed to attach if the session exists,
# or create if it does not. The first argument is session name,
# optional additional arguments are commands to run in the session.
WORKSPACE_ATTACH="tmx"
WORKSPACE_LIST="tmx -l"
```

## Scripts

- new-workspace: given a workspace name, create the workspace in the current
  directory, under the specified name, and if it does not exist or has not
  been set up, run the setup script. We look for the setup script relative to
  `$PWD` and the project root.
- list-workspaces: list the known workspaces using the project root and
  pattern, as well as the list command. paths are relative to
  `WORKSPACE_PROJECT_ROOT` if it's not empty, otherwise it shows absolute
  paths
- attach-workspace: given a workspace name, attach to an existing session or
  create a new session. requires the workspace exist
- workspace-picker: use fzf to interactively select and attach a workspace

## Example

```sh
$ export WORKSPACE_PROJECT_PATTERN="*/*/*"
$ cd ~/Projects/yocto/sumo
$ cat workspace-setup
#!/bin/sh
git clone https://github.com/openembedded/bitbake
git clone https://github.com/openembedded/openembedded-core oe-core
. ./oe-core/oe-init-build-env build bitbake
# echo 'BUILDDIR="cd "$(dirname "$0")" && pwd -P)"'>build/setup-environment
$ new-workspace yocto-1234
$ list-workspaces
yocto/sumo/yocto-1234
$ attach-workspace yocto/sumo/yocto-1234
```

## TODO

- [ ] Split out workspace-name to be used by list-workspaces as well as
    attach-workspace
- [ ] list-workspaces: use workspace-name
- [ ] new-workspace: adjust name relative to project root, if the new
    workspace directory is relative to it. I.e. 'elm$ new-workspace test'
    should result in an `elm/test` workspace, assuming pwd is
    `<project_root>/elm`. Can I make that extensible?
- [ ] list-workspaces: add sorting
  - [ ] Prioritize existing sessions
- [ ] Implement workspace-picker: `list-workspaces | fzf`
  - [ ] Use `WORKSPACE_PICKER_MARKUP` to add extra info to session lines
- [ ] Consider optional workspace registry as an alternative to the
    root+pattern approach to locating projects
