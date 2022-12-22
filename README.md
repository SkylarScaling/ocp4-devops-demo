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
Red Hat OpenShift 4.10+ cluster. 

### Tools

There are a handful of tools that you'll need to be able to effectively work with this repository. Please make sure you have the following things installed. 
Any commands provided are for linux systems.

- [Python 3+](https://www.python.org/downloads/)
- [Git 2.27+](https://git-scm.com/downloads)
- [Ansible 2.9+](https://docs.ansible.com/ansible/2.9/installation_guide/intro_installation.html)

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

#### Additional Required Ansible Module
The included Ansible playbooks also use modules from a collection that must be installed from ansible galaxy:
```
$ ansible-galaxy collection install community.general
```

## Facilitator Usage
### Provisioning a Cluster

Facilitators for this Workshop with access to RHPDS can order the [OCP4 Pipelines Workshop](https://demo.redhat.com/catalog?search=pipeline&item=babylon-catalog-prod%2Fsandboxes-gpte.ocp4-wksp-pipelines.prod) 
catalog item. 

This cluster will include a PV, which is required for the pipelines in this workshop. You will then need
to update the cluster to OCP 4.10+ for the OpenShift Pipelines Operator version used in this Workshop. 

Any other OCP 4.10+ cluster with access to a PV can be used with this workshop as well.

### Deploying the Workshop

Facilitators of this workshop can automatically deploy the requisite components using the supplied playbooks. You
must have `cluster-admin` or an administrative role that allows creation and modification of `CustomResourceDefinitions`
and `InstallPlans` at the cluster scope in order to install the operator components used in this repo.

#### 1) Fork repos
Fork this repository, then clone it to the location from which you would like to deploy the workshop.

You will also need to fork the repos for the pipeline example app so you can demonstrate how triggers and webhooks can
be used to automatically kick off Pipeline Runs:

[Pipelines UI Repo](https://github.com/SkylarScaling/pipelines-vote-ui) \
[Pipelines API Repo](https://github.com/SkylarScaling/pipelines-vote-api)

#### 2) Populate user_vars.yaml
Populate the user_vars.yaml file with the required information to connect to your cluster.

cluster_admin_user: _< username >_ \
cluster_admin_pass: _< password >_ \
cluster_name: _< cluster name >_ \
cluster_base_domain: _< cluster domain >_ \
application_api_repo: _< api repo >_ \
application_ui_repo: _< ui repo >_ \
github_user: _< GitHub username >_ \
github_token: _< github token >_ 

_e.g._ \
cluster_admin_user: _my-cluster-admin_ \
cluster_admin_pass: _mY-pA$sWoRd_ \
cluster_name: _cluster-123a4_ \
cluster_base_domain: _123a4.sandbox123.opentlc.com_ \
application_api_repo: _SkylarScaling/pipelines-vote-api_ \
application_ui_repo: _SkylarScaling/pipelines-vote-ui_ \
github_user: _SkylarScaling_ \
github_token: _ghp_abcd1234xyz_

> **Note: DO NOT save these values to your repository!**
> 
> If you want to ignore changes to this file for your repo, use the following command: 
> ```
> $ git update-index --assume-unchanged user_vars.yaml
> ```

#### 3) Run configure-cluster.yaml playbook
Run the following playbook to deploy the workshop content to your cluster:

```
$ ansible-playbook ansible/playbooks/configure-cluster.yaml
```
