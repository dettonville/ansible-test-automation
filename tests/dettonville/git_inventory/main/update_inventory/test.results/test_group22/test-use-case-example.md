
# Test Use Case - Example [group22]

Use Case [group22]: Vars overwrite for deep nested dict group var


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
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_l42p0h8f/ansible_repo.key
    use_vars_files: true
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
  exception: "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1754587024.0361068-24148-135781698185083/AnsiballZ_update_inventory.py\",
    line 107, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1754587024.0361068-24148-135781698185083/AnsiballZ_update_inventory.py\",
    line 99, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/root/.ansible/tmp/ansible-tmp-1754587024.0361068-24148-135781698185083/AnsiballZ_update_inventory.py\",
    line 47, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1015, in <module>\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1011, in main\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1000, in run_module\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 324, in update_inventory\nTypeError: not enough arguments for format string\n"
  failed: true
  module_stderr: "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1754587024.0361068-24148-135781698185083/AnsiballZ_update_inventory.py\",
    line 107, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1754587024.0361068-24148-135781698185083/AnsiballZ_update_inventory.py\",
    line 99, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/root/.ansible/tmp/ansible-tmp-1754587024.0361068-24148-135781698185083/AnsiballZ_update_inventory.py\",
    line 47, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1015, in <module>\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1011, in main\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1000, in run_module\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_l042799w/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 324, in update_inventory\nTypeError: not enough arguments for format string\n"
  module_stdout: 'INFO:root:GitInventoryUpdater.get_internal_collection_version(): module_parts=[''dettonville.git_inventory'',
    ''update_inventory'']

    INFO:root:GitInventoryUpdater.init(): inventory_base_dir => /tmp/update_inventoryzqo86n4z

    INFO:root:Git.execute_git_command(): Executing git command: /usr/bin/git clone --single-branch
    --branch main --depth=1 --origin origin git@github.com:dettonville/ansible-test-automation.git
    /tmp/update_inventoryzqo86n4z with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'':
    ''C'', ''LC_CTYPE'': ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_l42p0h8f/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /tmp/update_inventoryzqo86n4z

    INFO:root:Git.execute_git_command(): Executing git command: /usr/bin/git pull origin
    main with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'': ''C'', ''LC_CTYPE'':
    ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_l42p0h8f/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /tmp/update_inventoryzqo86n4z

    INFO:root:InventoryParser.init(): inventory_file=tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml

    INFO:root:InventoryParser.init(): inventory_base_dir=/tmp/update_inventoryzqo86n4z

    '
  msg: 'MODULE FAILURE

    See stdout/stderr for the exact error'
  rc: 1


```

### Resulting file content after running the playbook

_test_inventory/SANDBOX/group_vars/test_app.yml:
```yaml
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
        jira_id: PR-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: PR-1102
      trace_var: host_vars/web02.qa.site1.example.int
```

