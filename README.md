# demo-site-prometheus-ansible-grafana-alertmanager-node-exporter

# 🚀 This repository provides a demo site for prometheus, alertmanager, prometheus exporters, and grafana. Site is provisioned with ansible running every day and on all commits to master branch. Everything is fully automated with travis ci pipeline. 🚀

https://github.com/coding-to-music/demo-site-prometheus-ansible-grafana-alertmanager-node-exporter

From / By https://prometheus.io/docs/prometheus/latest/getting_started/

## Environment variables:

```java

```

## GitHub

```java
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/demo-site-prometheus-ansible-grafana-alertmanager-node-exporter.git
git push -u origin main
```

## see what ports are in use

```
sudo netstat -tunlp
```

## Remove lfs objects / references

List lfs objects

```
git lfs ls-files
```

Output

```
d73e3668f0 - playbooks/files/img/digitalocean.png
```

Tried:

```
git lfs uninstall
```

Output

```
open /mnt/volume_nyc1_01/demo-site-prometheus-ansible-grafana-alertmanager-node-exporter/.git/hooks/pre-push: no such file or directory
Hooks for this repository have been removed.
Global Git LFS configuration has been removed.
```

Try to push

```
git push -u origin main
```

```
Enumerating objects: 1081, done.
Counting objects: 100% (1081/1081), done.
Delta compression using up to 2 threads
Compressing objects: 100% (443/443), done.
Writing objects: 100% (1081/1081), 248.52 KiB | 27.61 MiB/s, done.
Total 1081 (delta 530), reused 1072 (delta 526)
remote: Resolving deltas: 100% (530/530), done.
remote: error: GH008: Your push referenced at least 3 unknown Git LFS objects:
remote: d73e3668f0e0c173f62c727f1aa4e2a6f9851fb748847e05242f1d213a360375
remote: 937fcb1b18d96fcff58e5c08ac5821bb15617e071034b555fafa5bbeb7ea662e
remote: a3e0ecbc79175ec327623470b7d0a645f6cac5d1ee3d048144c4efc8e40a568d
remote: Try to push them with 'git lfs push --all'.
To github.com:coding-to-music/demo-site-prometheus-ansible-grafana-alertmanager-node-exporter.git
! [remote rejected] main -> main (pre-receive hook declined)
error: failed to push some refs to 'git@github.com:coding-to-music/demo-site-prometheus-ansible-grafana-alertmanager-node-exporter.git'
```

Tried this suggestion:

https://github.com/git-lfs/git-lfs/issues/4148

```
git lfs migrate export --include="*" --everything
```

```
migrate: override changes in your working copy? [Y/n] y
migrate: changes in your working copy will be overridden ...
[937fcb1b18d96fcff58e5c08ac5821bb15617e071034b555fafa5bbeb7ea662e] Object does not exist on the server: [404] Object does not exist on the server
```

I ended up removing git history completely, which is excessive

```
rm -rf .git
```

But then I was able to save to GitHub

```
git push -u origin main
```

Output

```
Enumerating objects: 45, done.
Counting objects: 100% (45/45), done.
Delta compression using up to 2 threads
Compressing objects: 100% (37/37), done.
Writing objects: 100% (45/45), 18.11 KiB | 927.00 KiB/s, done.
Total 45 (delta 0), reused 0 (delta 0)
To github.com:coding-to-music/demo-site-prometheus-ansible-grafana-alertmanager-node-exporter.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

# Prometheus monitoring demo site

[![Build Status](https://circleci.com/gh/prometheus/demo-site.svg?style=svg)](https://circleci.com/gh/prometheus/demo-site)
[![License](https://img.shields.io/badge/license-Apache%20License-brightgreen.svg)](https://opensource.org/licenses/Apache-2.0)
[![IRC](https://img.shields.io/badge/chat-on%20freenode-blue.svg)](http://webchat.freenode.net/?channels=prometheus)

## [demo.do.prometheus.io](https://demo.do.prometheus.io)

This repository provides a demo site for [prometheus](https://github.com/prometheus/prometheus), [alertmanager](https://github.com/prometheus/alertmanager), prometheus exporters, and [grafana](https://github.com/grafana/grafana).
Site is provisioned with ansible running every day and on all commits to master branch. Everything is fully automated with travis ci pipeline. If you want to check `ansible-playbook` output, go to [last build](https://app.circleci.com/pipelines/github/prometheus/demo-site).

Have a look at configuration files in [group_vars/](group_vars).

## Applications

All applications should be running on their default ports.

| App name      | Address (HTTP)                                   | Address (HTTPS)                                          |
| ------------- | ------------------------------------------------ | -------------------------------------------------------- |
| node_exporter | [demo.do.prometheus.io:9100][node_exporter_http] | [node.demo.do.prometheus.io][node_exporter_https]        |
| prometheus    | [demo.do.prometheus.io:9090][prometheus_http]    | [prometheus.demo.do.prometheus.io][prometheus_https]     |
| alertmanager  | [demo.do.prometheus.io:9093][alertmanager_http]  | [alertmanager.demo.do.prometheus.io][alertmanager_https] |
| grafana       | [demo.do.prometheus.io:3000][grafana_http]       | [grafana.demo.do.prometheus.io][grafana_https]           |

## Important notice

Before running, golang is required to be installed on deployer machine (necessary to install random_exporter).

Most services can be accessed in two ways (links in [Applications](#Applications) section. As an example, prometheus can be accessed via:

- **http**://demo.do.prometheus.io:9090 - default way
- **https**://prometheus.do.prometheus.io - workaround which in background communicates with prometheus via insecure, "default" channel mentioned above

This workaround was needed to solve issue [cloudalchemy/demo-site#13](https://github.com/cloudalchemy/demo-site/issues/13).

## Run yourself

You can easily run such setup yourself without much knowledge how any part of this works. You just need to do two things:

### copy example hosts and valut

```
cp group_vars/grafana/vault.example group_vars/grafana/vault

cp hosts.example hosts
```

#### Change ansible inventory

First of all you need to configure your inventory, ours is located in [`hosts`](hosts) file. Here you set up your target hosts by changing value of `ansible_host` variable. Also here you can exclude parts of this demo site, so if you don't need our website, you just remove this part:

```
[web]
demo
```

Accordingly you can exclude grafana, prometheus.

#### Change passwords

For security measures we encrypted some of our passwords, but it is easy to use yours! You can do it by replacing a file located at [`group_vars/grafana/vault`](group_vars/grafana/vault) with following content:

```
vault_grafana_password: <<INSERT_YOUR_GRAFANA_PASSWORD>>
```

#### Download the 'random' exporter binary

You will have to manually run `go` command to download & copy the [`random`](https://github.com/prometheus/client_golang/tree/master/examples/random) exporter binary to [`playbooks/files`](playbooks/files) directory.

- The binary will be downloaded at `GOPATH` location. The value of `GOPATH` can be found by running `go env|grep GOPATH` command on your system.

```
go get -u github.com/prometheus/client_golang/examples/random
cp <GOPATH>/bin/random /path/to/demo-site/playbooks/files/
```

#### Run as usual Ansible playbook

```bash
# Download roles
ansible-galaxy install -r roles/requirements.yml

# Run playbook
ansible-playbook site.yml
# or when using vault encrypted variables
ansible-playbook --vault-id @prompt site.yml
```

#

demo site is deployed using [Cloud Alchemy](https://github.com/cloudalchemy) ansible roles.

[![DigitalOcean](https://snapshooter.io/powered_by_digital_ocean.png)](https://digitalocean.com)

[node_exporter_http]: http://demo.do.prometheus.io:9100
[node_exporter_https]: https://node.demo.do.prometheus.io
[prometheus_http]: http://demo.do.prometheus.io:9090
[prometheus_https]: https://prometheus.demo.do.prometheus.io
[alertmanager_http]: http://demo.do.prometheus.io:9093
[alertmanager_https]: https://alertmanager.demo.do.prometheus.io
[grafana_http]: http://demo.do.prometheus.io:3000
[grafana_https]: https://grafana.demo.do.prometheus.io
