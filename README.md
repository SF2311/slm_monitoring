# slm_monitoring

This role installs or uninstalls [cAdvisor](https://github.com/google/cadvisor) on a host.
It has been developed against Debian bookworm, but should also work against other Debian based distributions.

## Requirements

The system should run an up-to-date version of Debian.

## Role Variables

| Variable Name                              | Required                 | Default value                                                           | Description                                                                                     |
| ------------------------------------------ | ------------------------ | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `slm_monitoring_cadvisor_install_location` | :heavy_multiplication_x: | `/usr/local/bin/cadvisor`                                               | The location of the `cadvisor` binary.                                                          |
| `slm_monitoring_cadvisor_user`             | :heavy_multiplication_x: | `root`                                                                  | You can use this variable to run cAdvisor under a non-root user.                                |
| `slm_monitoring_cadvisor_port`             | :heavy_multiplication_x: | `8080`                                                                  | The port cAdvisor should listen on.                                                             |
| `slm_monitoring_cadvisor_version`          | :heavy_multiplication_x: | `latest`                                                                | Version of cAdvisor to install. See [Versions](#versions) for details.                          |
| `state`                                    | :heavy_multiplication_x: | `present`                                                               | If state is `present` the role installs, if it is `absent` the role cleans up the installation. |
| `slm_monitoring_proc_exp_install_location` | :heavy_multiplication_x: | `/usr/local/bin/process-exporter`                                       | The location of the `process-exporter` binary.                                                  |
| `slm_monitoring_proc_exp_version`          | :heavy_multiplication_x: | `latest`                                                                | Version of `process-exporter` to install. See [Versions](#versions) for details.                |
| `slm_monitoring_proc_exp_user`             | :heavy_multiplication_x: | `root`                                                                  | You can use this variable to run process-exportre under a non-root user.                        |
| `slm_monitoring_proc_exp_config_path`      | :heavy_multiplication_x: | `/etc/process_exporter/proc_exp.yml`                                    | Location of the config file for process-exporter.                                               |
| `slm_monitoring_proc_exp_config`           | :heavy_multiplication_x: | See [Default process-exporter config](#default-process-exporter-config) | Content of the config file for process-exporter.                                                |

### Versions

By default this role determines the version of the latest release and installs it.
You can pin the version of cadvisor by setting `slm_monitoring_cadvisor_version` to a version tag.
Likewise, you can pin the version of process-exporter by setting `slm_monitoring_proc_exp_version`.

### Default process-exporter config

```yml
process_names:
  - name: "{{.Comm}}"
    cmdline:
    - '.+'
```


### Example Playbook

This playbook assumes, the repo has been cloned to `roles/slm_monitoring`

```yml
---
- name: Sample
  hosts: example
  become: true
  gather_facts: true
  roles:
    - slm_monitoring
  vars:
    - state: absent
```
