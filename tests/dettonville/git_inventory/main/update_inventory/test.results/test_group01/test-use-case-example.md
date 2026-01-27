
# Test Use Case - Example [group01]

Use Case [group01]: Add groups


## Starting/Initial Setup

### Initial Files

_test_inventory/SANDBOX/hosts.yml:
```yaml
---
all:
  children:
    environment_test:
      hosts:
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
        ## example comment here for admin apps
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/environment_test
    location_site1:
      hosts:
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/location_site1
    network_internal:
      hosts:
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/network_internal
    rhel6:
      hosts:
        admin01.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/rhel6
    rhel7:
      hosts:
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/rhel7
    ungrouped: {}
    ######################################################
    ## TODO: groups with prefix `ansible_controller_*` are deprecated/replaced/obsolesced by new group
    ## with name prefix `ansible_localhost_*`.
    ##
    ## E.g., replace all `ansible_controller_iam` references with `ansible_localhost_iam`
    ##
    ## The original group name prefix `ansible_controller_*` has the problem/challenge/issue that it
    ## could easily be mistaken/misconstrued to mean the ansible controller host instead of
    ## the intended `localhost` execution environment container making it ambiguous and not clear.
    ##
    ## Naming the localhost groups with `ansible_localhost_*` makes it far more clearer that this group targets
    ## the localhost.
    ######################################################
    ansible_localhost:
      children:
        ansible_localhost_iam: {}
        ansible_controller_iam: {}
  hosts:
    admin01.qa.site1.example.int:
      trace_var: host_vars/admin01.qa.site1.example.int
    admin02.qa.site1.example.int:
      trace_var: host_vars/admin02.qa.site1.example.int
    app01.qa.site1.example.int:
      trace_var: host_vars/app01.qa.site1.example.int
    app02.qa.site1.example.int:
      trace_var: host_vars/app02.qa.site1.example.int
    web01.qa.site1.example.int:
      provisioning_data:
        jira_id: INFRA-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: INFRA-1102
      trace_var: host_vars/web02.qa.site1.example.int
```


## Playbook Example

```yaml
---

- name: "Run test on dettonville.git_inventory.update_inventory"
  register: __test_component__test_result
  dettonville.git_inventory.update_inventory:
    always_add_child_group_to_root: true
    enforce_global_groups_must_already_exist: false
    git_comment_prefix: 'JENKINS-14 - '
    group_list:
    - group_name: admin_qa_site1
      group_vars:
        infra_group: DCC
      parent_groups:
      - vmware_flavor_large
      - ntp_server
      - nfs_server
      - ldap_server
    - group_name: webapp01_qa_site1
      group_vars:
        app_version: 2023086
      parent_groups:
      - vmware_flavor_small
      - ntp_client
      - nfs_client
      - ldap_client
      - web_server
    - group_name: webapp02_qa_site1
      group_vars:
        app_version: 2023086
      parent_groups:
      - vmware_flavor_small
      - ntp_client
      - nfs_client
      - ldap_client
      - web_server
    - group_name: ocp_common
      group_vars:
        ocp_namespace_configuration:
        - name1
        - name2
    - group_name: ocp_dev_s1
      group_vars:
        ocp_environment: dev
        ocp_site: s1
      parent_groups:
      - ocp_common
    - group_name: ocp_qa_s1
      group_vars:
        ocp_environment: qa
        ocp_site: s1
      parent_groups:
      - ocp_common
    - group_name: ocp_prod_s1
      group_vars:
        ocp_environment: prod
        ocp_site: s1
      parent_groups:
      - ocp_common
    - group_name: ocp_dev_s4
      group_vars:
        ocp_environment: dev
        ocp_site: s4
      parent_groups:
      - ocp_common
    - group_name: ocp_qa_s4
      group_vars:
        ocp_environment: qa
        ocp_site: s4
      parent_groups:
      - ocp_common
    - group_name: ocp_prod_s4
      group_vars:
        ocp_environment: prod
        ocp_site: s4
      parent_groups:
      - ocp_common
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_t4t3qej2/ansible_repo.key
    use_vars_files: false


- name: "Display __test_component__test_result"
  ansible.builtin.debug:
    var: __test_component__test_result
```


## Playbook Run Results

The run Result

```shell
TASK [Run test on dettonville.git_inventory.update_inventory]
TASK [Display __test_component__test_result]
ok: [localhost] =>
  backup_files: null
  changed: true
  check_mode: false
  failed: false
  git.add: ''
  git.commit: "[main a05c6bf] JENKINS-14 -  - dettonville.git_inventory.update_inventory:
    updated inventory\n 1 file changed, 96 insertions(+), 19 deletions(-)\n"
  git.pull: "Already up to date.\nFrom github.com:dettonville/ansible-test-automation\n
    * branch            main       -> FETCH_HEAD\n"
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   5bebd46..a05c6bf
    \ main -> main\n"
  inventory_base_dir: /tmp/update_inventory8sdnhpp0
  message: Inventory updated successfully
  warnings:
  - Using a template for task args is unsafe in some situations (see https://docs.ansible.com/ansible/devel/reference_appendices/faq.html#argsplat-unsafe).


```

### Resulting file content after running the playbook

_test_inventory/SANDBOX/hosts.yml:
```yaml
---
all:
  children:
    admin_qa_site1:
      vars:
        infra_group: DCC
    ######################################################
    ## TODO: groups with prefix `ansible_controller_*` are deprecated/replaced/obsolesced by new group
    ## with name prefix `ansible_localhost_*`.
    ##
    ## E.g., replace all `ansible_controller_iam` references with `ansible_localhost_iam`
    ##
    ## The original group name prefix `ansible_controller_*` has the problem/challenge/issue that it
    ## could easily be mistaken/misconstrued to mean the ansible controller host instead of
    ## the intended `localhost` execution environment container making it ambiguous and not clear.
    ##
    ## Naming the localhost groups with `ansible_localhost_*` makes it far more clearer that this group targets
    ## the localhost.
    ######################################################
    ansible_localhost:
      children:
        ansible_controller_iam: {}
        ansible_localhost_iam: {}
    environment_test:
      hosts:
        ## example comment here for admin apps
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/environment_test
    ldap_client:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
    ldap_server:
      children:
        admin_qa_site1: {}
    location_site1:
      hosts:
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/location_site1
    network_internal:
      hosts:
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/network_internal
    nfs_client:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
    nfs_server:
      children:
        admin_qa_site1: {}
    ntp_client:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
    ntp_server:
      children:
        admin_qa_site1: {}
    ocp_common:
      children:
        ocp_dev_s1: {}
        ocp_dev_s4: {}
        ocp_prod_s1: {}
        ocp_prod_s4: {}
        ocp_qa_s1: {}
        ocp_qa_s4: {}
      vars:
        ocp_namespace_configuration:
          - name1
          - name2
    ocp_dev_s1:
      vars:
        ocp_environment: dev
        ocp_site: s1
    ocp_dev_s4:
      vars:
        ocp_environment: dev
        ocp_site: s4
    ocp_prod_s1:
      vars:
        ocp_environment: prod
        ocp_site: s1
    ocp_prod_s4:
      vars:
        ocp_environment: prod
        ocp_site: s4
    ocp_qa_s1:
      vars:
        ocp_environment: qa
        ocp_site: s1
    ocp_qa_s4:
      vars:
        ocp_environment: qa
        ocp_site: s4
    rhel6:
      hosts:
        admin01.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/rhel6
    rhel7:
      hosts:
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/rhel7
    ungrouped: {}
    vmware_flavor_large:
      children:
        admin_qa_site1: {}
    vmware_flavor_small:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
    web_server:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
    webapp01_qa_site1:
      vars:
        app_version: 2023086
    webapp02_qa_site1:
      vars:
        app_version: 2023086
  hosts:
    admin01.qa.site1.example.int:
      trace_var: host_vars/admin01.qa.site1.example.int
    admin02.qa.site1.example.int:
      trace_var: host_vars/admin02.qa.site1.example.int
    app01.qa.site1.example.int:
      trace_var: host_vars/app01.qa.site1.example.int
    app02.qa.site1.example.int:
      trace_var: host_vars/app02.qa.site1.example.int
    web01.qa.site1.example.int:
      provisioning_data:
        jira_id: INFRA-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: INFRA-1102
      trace_var: host_vars/web02.qa.site1.example.int

```

