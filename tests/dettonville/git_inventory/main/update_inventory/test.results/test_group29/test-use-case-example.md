
# Test Use Case - Example [group29]

Use Case [group29]: Add groups with parent inventory_dir specified


## Starting/Initial Setup

### Initial Files

_test_inventory/DEV/hosts.yml:
```yaml
all:
  children:
    app_foobar_dev:
      hosts:
        foobar01.dev.example.int: {}

```

_test_inventory/PROD/hosts.yml:
```yaml
all:
  children:
    app_foobar_prod:
      hosts:
        foobar01.prod.example.int: {}

```

_test_inventory/QA/hosts.yml:
```yaml
all:
  children:
    app_foobar_qa:
      hosts:
        foobar01.qa.example.int: {}

```

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
    enforce_global_groups_must_already_exist: false
    git_comment_prefix: JENKINS-68
    group_list:
    - group_name: admin_qa_site1
      group_vars:
        infra_group: DCC
      parent_groups:
      - vmware_flavor_large
      - ntp_server
      - nfs_server
      - ldap_server
    inventory_dir: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory
    inventory_file: SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_hs_3hk5c/ansible_repo.key
    use_vars_files: true


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
  git.commit: "[main 1a29113] JENKINS-68 - dettonville.git_inventory.update_inventory:
    updated inventory\n 26 files changed, 61 insertions(+), 19 deletions(-)\n create
    mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/DEV/group_vars/admin_qa_site1.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/DEV/group_vars/ldap_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/DEV/group_vars/nfs_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/DEV/group_vars/ntp_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/DEV/group_vars/vmware_flavor_large.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/PROD/group_vars/admin_qa_site1.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/PROD/group_vars/ldap_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/PROD/group_vars/nfs_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/PROD/group_vars/ntp_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/PROD/group_vars/vmware_flavor_large.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/QA/group_vars/admin_qa_site1.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/QA/group_vars/ldap_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/QA/group_vars/nfs_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/QA/group_vars/ntp_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/QA/group_vars/vmware_flavor_large.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/admin_qa_site1.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/ldap_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/nfs_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/ntp_server.yml\n
    create mode 120000 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/vmware_flavor_large.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/group_vars/admin_qa_site1.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/group_vars/ldap_server.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/group_vars/nfs_server.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/group_vars/ntp_server.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/group_vars/vmware_flavor_large.yml\n"
  git.pull: "Already up to date.\nFrom github.com:dettonville/ansible-test-automation\n
    * branch            main       -> FETCH_HEAD\n"
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   2bb941b..1a29113
    \ main -> main\n"
  inventory_base_dir: /tmp/update_inventorypdz7c0lu
  message: Inventory updated successfully


```

### Resulting file content after running the playbook

_test_inventory/DEV/hosts.yml:
```yaml
all:
  children:
    app_foobar_dev:
      hosts:
        foobar01.dev.example.int: {}
```

_test_inventory/group_vars/admin_qa_site1.yml:
```yaml
infra_group: DCC
```

_test_inventory/group_vars/ldap_server.yml:
```yaml
{}
```

_test_inventory/group_vars/nfs_server.yml:
```yaml
{}
```

_test_inventory/group_vars/ntp_server.yml:
```yaml
{}
```

_test_inventory/group_vars/vmware_flavor_large.yml:
```yaml
{}
```

_test_inventory/PROD/hosts.yml:
```yaml
all:
  children:
    app_foobar_prod:
      hosts:
        foobar01.prod.example.int: {}
```

_test_inventory/QA/hosts.yml:
```yaml
all:
  children:
    app_foobar_qa:
      hosts:
        foobar01.qa.example.int: {}
```

_test_inventory/SANDBOX/hosts.yml:
```yaml
all:
  children:
    ansible_localhost:
      children:
        ansible_controller_iam: {}
        ansible_localhost_iam: {}
    environment_test:
      hosts:
        admin01.qa.site1.example.int: {}
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/environment_test
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
    nfs_server:
      children:
        admin_qa_site1: {}
    ntp_server:
      children:
        admin_qa_site1: {}
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

symlinks:
  - _test_inventory/DEV/group_vars/admin_qa_site1.yml
  - _test_inventory/DEV/group_vars/ldap_server.yml
  - _test_inventory/DEV/group_vars/nfs_server.yml
  - _test_inventory/DEV/group_vars/ntp_server.yml
  - _test_inventory/DEV/group_vars/vmware_flavor_large.yml
  - _test_inventory/PROD/group_vars/admin_qa_site1.yml
  - _test_inventory/PROD/group_vars/ldap_server.yml
  - _test_inventory/PROD/group_vars/nfs_server.yml
  - _test_inventory/PROD/group_vars/ntp_server.yml
  - _test_inventory/PROD/group_vars/vmware_flavor_large.yml
  - _test_inventory/QA/group_vars/admin_qa_site1.yml
  - _test_inventory/QA/group_vars/ldap_server.yml
  - _test_inventory/QA/group_vars/nfs_server.yml
  - _test_inventory/QA/group_vars/ntp_server.yml
  - _test_inventory/QA/group_vars/vmware_flavor_large.yml
  - _test_inventory/SANDBOX/group_vars/admin_qa_site1.yml
  - _test_inventory/SANDBOX/group_vars/ldap_server.yml
  - _test_inventory/SANDBOX/group_vars/nfs_server.yml
  - _test_inventory/SANDBOX/group_vars/ntp_server.yml
  - _test_inventory/SANDBOX/group_vars/vmware_flavor_large.yml
