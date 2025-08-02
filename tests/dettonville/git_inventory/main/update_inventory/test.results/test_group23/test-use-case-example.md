
# Test Use Case - Example [group23]

Use Case [group23]: Vars overwrite for deep nested dict group var using vars_overwrite_depth=3


## Starting/Initial Setup

### Initial Files

_test_inventory/SANDBOX/group_vars/test_app.yml:
```yaml
full_dict:
  outer1_dict:
    inner11_dict:
      outer111_dict:
        inner111_dict:
          action: dont change me!
          foo: bar111
        inner111_key: inner111 key value
      outer111_key: outer111 key value
      outer112_dict:
        inner112_dict:
          action: dont change me!
          foo: bar112
        inner112_key: inner112 key value
      outer112_key: outer112 key value
    inner11_key: inner11 key value
    inner12_dict:
      outer121_dict:
        inner121_dict:
          action: change me
          foo: bar121
        inner121_key: inner121 key value
      outer121_key: outer121 key value
      outer122_dict:
        inner122_dict:
          action: dont change me!
          foo: bar122
        inner122_key: inner122 key value
      outer122_key: outer122 key value
    inner12_key: inner12 key value2
  outer1_key: outer1 key value
  outer2_dict:
    inner21_dict:
      outer211_dict:
        inner211_dict:
          action: dont change me!
          foo: bar211 key
        inner211_key: inner211 key value
      outer211_key: outer211 key value
      outer212_dict:
        inner212_dict:
          action: dont change me!
          foo: bar212 key
        inner212_key: inner212 key value
      outer212_key: outer212 key value
    inner21_key: inner21 key value
    inner22_dict:
      outer221_dict:
        inner221_dict:
          action: dont change me!
          foo: bar221 key
        inner221_key: inner221 key value
      outer221_key: outer221 key value
      outer222_dict:
        inner222_dict:
          action: dont change me!
          foo: bar222 key
        inner222_key: inner222 key value
      outer222_key: outer222 key value
    inner22_key: inner22 key value
  outer2_key: outer2 key value

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
    - group_name: test_app
      group_vars:
        full_dict:
          outer1_dict:
            inner12_dict:
              outer121_dict:
                inner121_dict:
                  action: inner121_dict had a complete change
                  foo: we overwrote inner121_dict
                  mynewkey: a shiny brand new key
                inner121_key: NEW inner121 key value
              outer121_key: NEW outer121 key value
            inner12_key: NEW inner12 key value
          outer1_key: NEW outer1 key value
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_nf51shlw/ansible_repo.key
    use_vars_files: true
    vars_overwrite_depth: '3'
    vars_state: overwrite


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
  changed: false
  cmd: /usr/bin/git config --local user.name
  failed: true
  msg: ''
  rc: 1
  stderr: ''
  stderr_lines: []
  stdout: ''
  stdout_lines: []


```

### Resulting file content after running the playbook

_test_inventory/SANDBOX/group_vars/test_app.yml:
```yaml
full_dict:
  outer1_dict:
    inner11_dict:
      outer111_dict:
        inner111_dict:
          action: dont change me!
          foo: bar111
        inner111_key: inner111 key value
      outer111_key: outer111 key value
      outer112_dict:
        inner112_dict:
          action: dont change me!
          foo: bar112
        inner112_key: inner112 key value
      outer112_key: outer112 key value
    inner11_key: inner11 key value
    inner12_dict:
      outer121_dict:
        inner121_dict:
          action: inner121_dict had a complete change
          foo: we overwrote inner121_dict
          mynewkey: a shiny brand new key
        inner121_key: NEW inner121 key value
      outer121_key: NEW outer121 key value
    inner12_key: NEW inner12 key value
  outer1_key: NEW outer1 key value
  outer2_dict:
    inner21_dict:
      outer211_dict:
        inner211_dict:
          action: dont change me!
          foo: bar211 key
        inner211_key: inner211 key value
      outer211_key: outer211 key value
      outer212_dict:
        inner212_dict:
          action: dont change me!
          foo: bar212 key
        inner212_key: inner212 key value
      outer212_key: outer212 key value
    inner21_key: inner21 key value
    inner22_dict:
      outer221_dict:
        inner221_dict:
          action: dont change me!
          foo: bar221 key
        inner221_key: inner221 key value
      outer221_key: outer221 key value
      outer222_dict:
        inner222_dict:
          action: dont change me!
          foo: bar222 key
        inner222_key: inner222 key value
      outer222_key: outer222 key value
    inner22_key: inner22 key value
  outer2_key: outer2 key value
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
    test_app: {}
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

