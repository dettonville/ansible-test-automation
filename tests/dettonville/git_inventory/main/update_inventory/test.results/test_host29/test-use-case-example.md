
# Test Use Case - Example [host29]

Use Case [host29]: Add hosts with global groups enforcement that should fail due to missing global group


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
    global_groups_file: xenv_groups.yml
    host_list:
    - host_name: test123.prod.site1.example.int
      parent_groups:
      - app_abc123_prod
      - foobar
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_7o3araq7/ansible_repo.key


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
  exception: "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1754585550.355797-70487-274103583959075/AnsiballZ_update_inventory.py\",
    line 107, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1754585550.355797-70487-274103583959075/AnsiballZ_update_inventory.py\",
    line 99, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/root/.ansible/tmp/ansible-tmp-1754585550.355797-70487-274103583959075/AnsiballZ_update_inventory.py\",
    line 47, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1016, in <module>\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1012, in main\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1001, in run_module\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 357, in update_inventory\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 957, in update_host\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 1064, in add_host_to_groups\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 1085, in add_host_to_group_children\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 859, in validate_global_group_exists\nansible_collections.dettonville.git_inventory.plugins.module_utils.inventory_parser.InventoryParserException:
    InventoryParser.validate_global_group_exists(foobar): group_name=[foobar] not found
    in xenv_groups.yml\n"
  failed: true
  module_stderr: "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1754585550.355797-70487-274103583959075/AnsiballZ_update_inventory.py\",
    line 107, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1754585550.355797-70487-274103583959075/AnsiballZ_update_inventory.py\",
    line 99, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/root/.ansible/tmp/ansible-tmp-1754585550.355797-70487-274103583959075/AnsiballZ_update_inventory.py\",
    line 47, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1016, in <module>\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1012, in main\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1001, in run_module\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 357, in update_inventory\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 957, in update_host\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 1064, in add_host_to_groups\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 1085, in add_host_to_group_children\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_0ehct86h/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/inventory_parser.py\",
    line 859, in validate_global_group_exists\nansible_collections.dettonville.git_inventory.plugins.module_utils.inventory_parser.InventoryParserException:
    InventoryParser.validate_global_group_exists(foobar): group_name=[foobar] not found
    in xenv_groups.yml\n"
  module_stdout: 'INFO:root:GitInventoryUpdater.get_internal_collection_version(): module_parts=[''dettonville.git_inventory'',
    ''update_inventory'']

    INFO:root:GitInventoryUpdater.init(): inventory_base_dir => /tmp/update_inventoryzaa4w2bd

    INFO:root:Git.execute_git_command(): Executing git command: /usr/bin/git clone --single-branch
    --branch main --depth=1 --origin origin git@github.com:dettonville/ansible-test-automation.git
    /tmp/update_inventoryzaa4w2bd with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'':
    ''C'', ''LC_CTYPE'': ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_7o3araq7/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /tmp/update_inventoryzaa4w2bd

    INFO:root:Git.execute_git_command(): Executing git command: /usr/bin/git pull origin
    main with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'': ''C'', ''LC_CTYPE'':
    ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_7o3araq7/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /tmp/update_inventoryzaa4w2bd

    INFO:root:InventoryParser.init(): inventory_file=tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml

    INFO:root:InventoryParser.init(): inventory_base_dir=/tmp/update_inventoryzaa4w2bd

    ERROR:root:InventoryParser.validate_global_group_exists(foobar): group_name=[foobar]
    not found in xenv_groups.yml

    '
  msg: 'MODULE FAILURE

    See stdout/stderr for the exact error'
  rc: 1


```

### Resulting file content after running the playbook


No Files changed.
