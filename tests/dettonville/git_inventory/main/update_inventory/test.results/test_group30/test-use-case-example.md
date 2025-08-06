
# Test Use Case - Example [group30]

Use Case [group30]: Add groups to global groups file


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

_test_inventory/SANDBOX/xenv_groups.yml:
```yaml
all:
  children:
    ansible_localhost:
      children:
        ansible_controller_iam: {}
        ansible_localhost_iam: {}
    app_abc123:
      children:
        app_abc123_dev: {}
        app_abc123_prod: {}
        app_abc123_qa: {}
        app_abc123_sandbox: {}

```


## Playbook Example

```yaml
---

- name: "Run test on dettonville.git_inventory.update_inventory"
  register: __test_component__test_result
  dettonville.git_inventory.update_inventory:
    enforce_global_groups_must_already_exist: true
    git_comment_prefix: PR-2648
    git_user_email: ansible@dettonville.org
    git_user_name: ansible
    global_groups_file: xenv_groups.yml
    group_list:
    - group_name: app_abc123_web_qa
      parent_groups:
      - app_abc123_qa
    - group_name: app_abc123_db_prod
      parent_groups:
      - app_abc123_prod
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_cu92b9em/ansible_repo.key


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
  git.commit: "[main f71de7c] PR-2648 - dettonville.git_inventory.update_inventory:
    updated inventory\n 5 files changed, 34 insertions(+), 20 deletions(-)\n create
    mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/app_abc123_db_prod.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/app_abc123_prod.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/app_abc123_qa.yml\n
    create mode 100644 tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/group_vars/app_abc123_web_qa.yml\n"
  git.pull: 'Already up to date.

    '
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   ebdf4c6..f71de7c
    \ main -> main\n"
  inventory_base_dir: /tmp/update_inventorybz3bzco8
  message: Inventory updated successfully


```

### Resulting file content after running the playbook

_test_inventory/SANDBOX/group_vars/app_abc123_db_prod.yml:
```yaml
{}
```

_test_inventory/SANDBOX/group_vars/app_abc123_prod.yml:
```yaml
{}
```

_test_inventory/SANDBOX/group_vars/app_abc123_qa.yml:
```yaml
{}
```

_test_inventory/SANDBOX/group_vars/app_abc123_web_qa.yml:
```yaml
{}
```

_test_inventory/SANDBOX/hosts.yml:
```yaml
all:
  children:
    ansible_localhost:
      children:
        ansible_controller_iam: {}
        ansible_localhost_iam: {}
    app_abc123_prod:
      children:
        app_abc123_db_prod: {}
    app_abc123_qa:
      children:
        app_abc123_web_qa: {}
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

_test_inventory/SANDBOX/xenv_groups.yml:
```yaml
all:
  children:
    ansible_localhost:
      children:
        ansible_controller_iam: {}
        ansible_localhost_iam: {}
    app_abc123:
      children:
        app_abc123_dev: {}
        app_abc123_prod: {}
        app_abc123_qa: {}
        app_abc123_sandbox: {}
```

