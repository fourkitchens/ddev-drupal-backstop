[![tests](https://github.com/ddev/ddev-drupal-backstop/actions/workflows/tests.yml/badge.svg)](https://github.com/ddev/ddev-drupal-backstop/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2024.svg)

# ddev-drupal-backstop <!-- omit in toc -->

* [What is ddev-drupal-backstop?](#what-is-ddev-drupal-backstop)
* [Components of the repository](#components-of-the-repository)
* [Getting started](#getting-started)
* [How to debug in Github Actions](#how-to-debug-tests-github-actions)

## What is ddev-drupal-backstop?

This repository contains a backstopJS service integration along a ddev custom command to help running the backstop tool.

In DDEV addons can be installed from the command line using the `ddev get` command, for example, `ddev get fourkitchens/ddev-drupal-backstop` or `ddev get fourkitchens/ddev-drupal-backstop`.



## Components of the repository

* The fundamental contents of the add-on service or other component. For example, in this template there is a [docker-compose.addon-template.yaml](docker-compose.addon-template.yaml) file.
* An [install.yaml](install.yaml) file that describes how to install the service or other component.
* A test suite in [test.bats](tests/test.bats) that makes sure the service continues to work as expected.
* [Github actions setup](.github/workflows/tests.yml) so that the tests run automatically when you push to the repository.

## Getting started

- To run a Bats test locally, you have to [install bats-core](https://bats-core.readthedocs.io/en/stable/installation.html) first. Then you download your add-on, and finally run `bats ./tests/test.bats` within the root of the uncompressed directory. To learn more about Bats see the [documentation](https://bats-core.readthedocs.io/en/stable/).

- Test manually with `ddev get <owner/repo>`.
- You can test PRs with `ddev get https://github.com/<user>/<repo>/tarball/<branch>`


Add-ons were covered in [DDEV Add-ons: Creating, maintaining, testing](https://www.dropbox.com/scl/fi/bnvlv7zswxwm8ix1s5u4t/2023-11-07_DDEV_Add-ons.mp4?rlkey=5cma8s11pscxq0skawsoqrscp&dl=0) (part of the [DDEV Contributor Live Training](https://ddev.com/blog/contributor-training)).

Note that more advanced techniques are discussed in [DDEV docs](https://ddev.readthedocs.io/en/latest/users/extend/additional-services/#additional-service-configurations-and-add-ons-for-ddev).

## How to debug tests (Github Actions)

1. You need an SSH-key registered with GitHub. You either pick the key you have already used with `github.com` or you create a dedicated new one with `ssh-keygen -t ed25519 -a 64 -f tmate_ed25519 -C "$(date +'%d-%m-%Y')"` and add it at `https://github.com/settings/keys`.

2. Add the following snippet to `~/.ssh/config`:

```
Host *.tmate.io
    User git
    AddKeysToAgent yes
    UseKeychain yes
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/tmate_ed25519
```
3. Go to `https://github.com/<user>/<repo>/actions/workflows/tests.yml`.

4. Click the `Run workflow` button and you will have the option to select the branch to run the workflow from and activate `tmate` by checking the `Debug with tmate` checkbox for this run.


5. After the `workflow_dispatch` event was triggered, click the `All workflows` link in the sidebar and then click the `tests` action in progress workflow.

7. Pick one of the jobs in progress in the sidebar.

8. Wait until the current task list reaches the `tmate debugging session` section and the output shows something like:

```
106 SSH: ssh PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
107 or: ssh -i <path-to-private-SSH-key> PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
108 SSH: ssh PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
109 or: ssh -i <path-to-private-SSH-key> PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
```

9. Copy and execute the first option `ssh PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io` in the terminal and continue by pressing either <kbd>q</kbd> or <kbd>Ctrl</kbd> + <kbd>c</kbd>.

10. Start the Bats test with `bats ./tests/test.bats`.

For a more detailed documentation about `tmate` see [Debug your GitHub Actions by using tmate](https://mxschmitt.github.io/action-tmate/).

**Contributed and maintained by [@fourkitchens](https://github.com/fourkitchens)**
