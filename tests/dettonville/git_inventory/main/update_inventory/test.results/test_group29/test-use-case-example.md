
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
    enforce_global_groups_must_already_exist: false
    git_comment_prefix: PR-2648
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
      key_file: /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_l42p0h8f/ansible_repo.key
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
  changed: false
  exception: "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1754587390.2190795-32017-228813962215009/AnsiballZ_update_inventory.py\",
    line 107, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1754587390.2190795-32017-228813962215009/AnsiballZ_update_inventory.py\",
    line 99, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/root/.ansible/tmp/ansible-tmp-1754587390.2190795-32017-228813962215009/AnsiballZ_update_inventory.py\",
    line 47, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1015, in <module>\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1011, in main\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1000, in run_module\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 324, in update_inventory\nTypeError: not enough arguments for format string\n"
  failed: true
  module_stderr: "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1754587390.2190795-32017-228813962215009/AnsiballZ_update_inventory.py\",
    line 107, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1754587390.2190795-32017-228813962215009/AnsiballZ_update_inventory.py\",
    line 99, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/root/.ansible/tmp/ansible-tmp-1754587390.2190795-32017-228813962215009/AnsiballZ_update_inventory.py\",
    line 47, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1015, in <module>\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1011, in main\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1000, in run_module\n  File \"/tmp/ansible_dettonville.git_inventory.update_inventory_payload_hrhyhgmv/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 324, in update_inventory\nTypeError: not enough arguments for format string\n"
  module_stdout: 'INFO:root:GitInventoryUpdater.get_internal_collection_version(): module_parts=[''dettonville.git_inventory'',
    ''update_inventory'']

    INFO:root:GitInventoryUpdater.init(): inventory_base_dir => /tmp/update_inventory82fb7gou

    INFO:root:Git.execute_git_command(): Executing git command: /usr/bin/git clone --single-branch
    --branch main --depth=1 --origin origin git@github.com:dettonville/ansible-test-automation.git
    /tmp/update_inventory82fb7gou with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'':
    ''C'', ''LC_CTYPE'': ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_l42p0h8f/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /tmp/update_inventory82fb7gou

    INFO:root:Git.execute_git_command(): Executing git command: /usr/bin/git pull origin
    main with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'': ''C'', ''LC_CTYPE'':
    ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /home/jenkins/agent/workspace/_inventory_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_l42p0h8f/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /tmp/update_inventory82fb7gou

    INFO:root:InventoryParser.init(): inventory_file=SANDBOX/hosts.yml

    INFO:root:InventoryParser.init(): inventory_base_dir=/tmp/update_inventory82fb7gou

    '
  msg: 'MODULE FAILURE

    See stdout/stderr for the exact error'
  rc: 1


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
        jira_id: PR-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: PR-1102
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
