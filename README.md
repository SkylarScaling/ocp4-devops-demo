# OpenShift 4 DevOps Demo

Author:
 - Skylar Scaling <sscaling@redhat.com>


## Description

This repo is intended as a quick start for a demo of DevOps / GitOps using Infrastructure as Code (IaC)

The following technologies are the focus of this demo: \
* [OpenShift](https://docs.openshift.com/container-platform/4.11/cicd/index.html)
* [OpenShift Pipelines (Tekton)](https://docs.openshift.com/container-platform/4.11/cicd/pipelines/understanding-openshift-pipelines.html#understanding-openshift-pipelines)
* [OpenShift GitOps (ArgoCD)](https://docs.openshift.com/container-platform/4.11/cicd/gitops/understanding-openshift-gitops.html#understanding-openshift-gitops)
* [Ansible](https://www.ansible.com/)

## Prerequisites
### Environment

For the best possible performance, it is recommended the workshop be conducted on a hosted (either bare-metal or via cloud provider)
Red Hat OpenShift 4.x cluster. 

### Tools

There are a handful of tools that you'll need to be able to effectively work with this repository. Please make sure you have the following things installed. 
Any commands provided are for linux systems.

- [Python 3+](https://www.python.org/downloads/)
- [Git 2.27+](https://git-scm.com/downloads)
- [Ansible 2.8+](https://docs.ansible.com/ansible/2.9/installation_guide/intro_installation.html)

> NOTE: Ansible will need python 3 in order to use the required modules and libraries, so it is recommended that you use 
> the following command to install Ansible: 
>
> ```
> $ pip3 install ansible
> ```
> You can then confirm which python version is being used by running:
> ```
> $ ansible --version | grep "python version"
> ```
> Example output:
> ```
> python version = 3.6.8 (default, Mar 18 2021, 08:58:41) [GCC 8.4.1 20200928 (Red Hat 8.4.1-1)]
> ```

The utilites in the list below are also required, but can be installed automatically by executing the Ansible playbook listed below, once Ansible is installed.

- [OpenShift CLI](https://docs.openshift.com/container-platform/4.7/cli_reference/openshift_cli/getting-started-cli.html) Installed
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) Installed
- [OpenShift Installer CLI](https://cloud.redhat.com/openshift/install/aws/installer-provisioned) Installed
- [Python Modules](https://docs.python.org/3/installing/index.html):
  - [kubernetes](https://pypi.org/project/kubernetes/)
  - [openshift](https://pypi.org/project/openshift/)
  - [boto3](https://pypi.org/project/boto3/)
  
They can be automatically installed by running the following command: 

```
$ ansible-playbook ansible/playbooks/setup-prereqs.yaml --ask-become-pass
```

> **Notes:**
>
> *Arguments*
>
> `--ask-become-pass` This will prompt for your root password, so required python libraries can be installed.

You can force the OpenShift binaries to update (if you already have oc and openshift-install on your system), by using the following command:
```
$ ansible-playbook playbooks/setup-prereqs.yaml --ask-become-pass -e force_update=true
```

> **Notes:**
>
> *Arguments*
>
> `force_update` This will tell Ansible to delete the existing oc and openshift-install binaries, forcing new versions to be installed.


## Facilitator Usage

Facilitators of this workshop can automatically provision the requisite components using the supplied playbooks. You
must have `cluster-admin` or an administrative role that allows creation and modification of `CustomResourceDefinitions`
and `InstallPlans` at the cluster scope in order to install the operator components used in this repo.

```
$ ansible-playbook ansible/playbooks/configure-cluster.yaml
```