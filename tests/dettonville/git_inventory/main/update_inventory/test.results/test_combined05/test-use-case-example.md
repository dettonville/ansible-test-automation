
# Test Use Case - Example [combined05]

Use Case [combined05]: Add groups and hosts using var files


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
        jira_id: PR-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: PR-1102
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
    git_comment_prefix: PR-2648
    git_user_email: ansible@dettonville.org
    git_user_name: ansible
    group_list:
    - group_name: admin_qa_site1
      group_vars:
        infra_group: DCC
      parent_groups:
      - vmware_flavor_large
      - ntp_server
      - nfs_server
      - ldap_server
    host_list:
    - host_name: vmlnx123.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: AIM
      parent_groups:
      - vmware_flavor_medium
      - ntp_client
      - ldap_client
    - host_name: vmlnx124.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: AIM
      parent_groups:
      - vmware_flavor_medium
      - ntp_client
      - nfs_client
      - ldap_client
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_cu92b9em/ansible_repo.key
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
  git.commit: "[main ca7c218] PR-2648 - dettonville.git_inventory.update_inventory:
    updated inventory\n 8 files changed, 63 insertions(+), 19 deletions(-)\n create
    mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/admin_qa_site1.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/ldap_server.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/nfs_server.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/ntp_server.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/vmware_flavor_large.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/host_vars/vmlnx123.qa.site1.example.int.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/host_vars/vmlnx124.qa.site1.example.int.yml\n"
  git.pull: 'Already up to date.

    '
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   b889d0d..ca7c218
    \ main -> main\n"
  inventory_base_dir: /tmp/update_inventoryflcfy6oe
  message: Inventory updated successfully


```

### Resulting file content after running the playbook

_test_inventory/SANDBOX/group_vars/admin_qa_site1.yml:
```yaml
infra_group: DCC
```

_test_inventory/SANDBOX/group_vars/ldap_server.yml:
```yaml
{}
```

_test_inventory/SANDBOX/group_vars/nfs_server.yml:
```yaml
{}
```

_test_inventory/SANDBOX/group_vars/ntp_server.yml:
```yaml
{}
```

_test_inventory/SANDBOX/group_vars/vmware_flavor_large.yml:
```yaml
{}
```

_test_inventory/SANDBOX/host_vars/vmlnx123.qa.site1.example.int.yml:
```yaml
provisioning_data:
  infra_group: AIM
```

_test_inventory/SANDBOX/host_vars/vmlnx124.qa.site1.example.int.yml:
```yaml
provisioning_data:
  infra_group: AIM
```

_test_inventory/SANDBOX/hosts.yml:
```yaml
all:
  children:
    admin_qa_site1: {}
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
    ldap_client:
      hosts:
        vmlnx123.qa.site1.example.int: {}
        vmlnx124.qa.site1.example.int: {}
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
      hosts:
        vmlnx124.qa.site1.example.int: {}
    nfs_server:
      children:
        admin_qa_site1: {}
    ntp_client:
      hosts:
        vmlnx123.qa.site1.example.int: {}
        vmlnx124.qa.site1.example.int: {}
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
    vmware_flavor_medium:
      hosts:
        vmlnx123.qa.site1.example.int: {}
        vmlnx124.qa.site1.example.int: {}
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
        jira_id: PR-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: PR-1102
      trace_var: host_vars/web02.qa.site1.example.int
```

