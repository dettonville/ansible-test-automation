
# Test Use Case - Example [combined03]

Use Case [combined03]: Overwrite groups and hosts


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
    group_list:
    - group_name: location_site1
      group_vars:
        site: site1
    - group_name: admin_qa_site1
      group_vars:
        provisioning_data: {}
    - group_name: webapp01_qa_site1
      group_vars:
        app_version: 2023086
      parent_groups:
      - vmware_flavor_medium
      - rhel7
      - network_internal
      - location_site1
      - ntp_client
      - web_server
    - group_name: webapp02_qa_site1
      group_vars:
        app_version: 2023086
      parent_groups:
      - vmware_flavor_small
      - rhel7
      - network_dmz
      - location_site1
      - ntp_client
      - nfs_client
      - ldap_client
      - web_server
      - unica_proxy
    host_list:
    - host_name: admin01.qa.site1.example.int
      host_vars:
        provisioning_data: {}
    - host_name: web01.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: AIM
          service_domains:
          - webapp101.qa.example.int
      parent_groups:
      - vmware_flavor_medium
      - rhel7
      - network_internal
      - location_site1
      - ntp_client
      - web_server
    - host_name: web02.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: AIM
          service_domains:
          - webapp102.qa.example.int
      parent_groups:
      - vmware_flavor_small
      - rhel7
      - network_dmz
      - location_site1
      - ntp_client
      - nfs_client
      - ldap_client
      - web_server
      - unica_proxy
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    ssh_params:
      accept_hostkey: true
      key_file: /Users/ljohnson/.ansible/tmp/.test_jobs_avv5jmlh/ansible_repo.key
    state: overwrite
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
  changed: false
  exception: "Traceback (most recent call last):\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1754919751.3578181-92877-207678093039128/AnsiballZ_update_inventory.py\",
    line 259, in <module>\n    _ansiballz_main()\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1754919751.3578181-92877-207678093039128/AnsiballZ_update_inventory.py\",
    line 249, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1754919751.3578181-92877-207678093039128/AnsiballZ_update_inventory.py\",
    line 122, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1013, in <module>\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1009, in main\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 998, in run_module\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 348, in update_inventory\nNameError: name 'state' is not defined\n"
  failed: true
  module_stderr: "Traceback (most recent call last):\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1754919751.3578181-92877-207678093039128/AnsiballZ_update_inventory.py\",
    line 259, in <module>\n    _ansiballz_main()\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1754919751.3578181-92877-207678093039128/AnsiballZ_update_inventory.py\",
    line 249, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1754919751.3578181-92877-207678093039128/AnsiballZ_update_inventory.py\",
    line 122, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1013, in <module>\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1009, in main\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 998, in run_module\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_vmjee2gh/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 348, in update_inventory\nNameError: name 'state' is not defined\n"
  module_stdout: 'INFO:root:GitInventoryUpdater.get_internal_collection_version(): module_parts=[''dettonville.git_inventory'',
    ''update_inventory'']

    INFO:root:GitInventoryUpdater.init(): inventory_base_dir => /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventorynvjhpp3v

    INFO:root:Git.execute_git_command(): Executing git command: /usr/local/bin/git clone
    --single-branch --branch main --depth=1 --origin origin git@github.com:dettonville/ansible-test-automation.git
    /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventorynvjhpp3v with env:
    {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'': ''C'', ''LC_CTYPE'': ''C'',
    ''GIT_SSH_COMMAND'': ''ssh -i /Users/ljohnson/.ansible/tmp/.test_jobs_avv5jmlh/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventorynvjhpp3v

    INFO:root:Git.execute_git_command(): Executing git command: /usr/local/bin/git pull
    origin main with env: {''LANG'': ''C'', ''LC_ALL'': ''C'', ''LC_MESSAGES'': ''C'',
    ''LC_CTYPE'': ''C'', ''GIT_SSH_COMMAND'': ''ssh -i /Users/ljohnson/.ansible/tmp/.test_jobs_avv5jmlh/ansible_repo.key
    -o IdentitiesOnly=yes -o BatchMode=yes -o StrictHostKeyChecking=no'', ''GIT_AUTHOR_NAME'':
    ''ansible'', ''GIT_COMMITTER_NAME'': ''ansible'', ''GIT_AUTHOR_EMAIL'': ''ansible@example.org'',
    ''GIT_COMMITTER_EMAIL'': ''ansible@example.org''} in cwd: /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventorynvjhpp3v

    INFO:root:InventoryParser.init(): inventory_file=tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml

    INFO:root:InventoryParser.init(): inventory_base_dir=/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventorynvjhpp3v

    '
  msg: 'MODULE FAILURE: No start of json char found

    See stdout/stderr for the exact error'
  rc: 1


```

### Resulting file content after running the playbook

_test_inventory/SANDBOX/hosts.yml:
```yaml
all:
  children:
    admin_qa_site1:
      vars:
        provisioning_data: {}
    ansible_localhost:
      children:
        ansible_controller_iam: {}
        ansible_localhost_iam: {}
    environment_test:
      hosts:
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/environment_test
    ldap_client:
      children:
        webapp02_qa_site1: {}
      hosts:
        web02.qa.site1.example.int: {}
    location_site1:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        site: site1
    network_dmz:
      children:
        webapp02_qa_site1: {}
      hosts:
        web02.qa.site1.example.int: {}
    network_internal:
      children:
        webapp01_qa_site1: {}
      hosts:
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/network_internal
    nfs_client:
      children:
        webapp02_qa_site1: {}
      hosts:
        web02.qa.site1.example.int: {}
    ntp_client:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
    rhel6:
      hosts: {}
      vars:
        trace_var: group_vars/rhel6
    rhel7:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
      hosts:
        admin02.qa.site1.example.int: {}
        app01.qa.site1.example.int: {}
        app02.qa.site1.example.int: {}
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
      vars:
        trace_var: group_vars/rhel7
    ungrouped: {}
    unica_proxy:
      children:
        webapp02_qa_site1: {}
      hosts:
        web02.qa.site1.example.int: {}
    vmware_flavor_medium:
      children:
        webapp01_qa_site1: {}
      hosts:
        web01.qa.site1.example.int: {}
    vmware_flavor_small:
      children:
        webapp02_qa_site1: {}
      hosts:
        web02.qa.site1.example.int: {}
    web_server:
      children:
        webapp01_qa_site1: {}
        webapp02_qa_site1: {}
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
    webapp01_qa_site1:
      vars:
        app_version: 2023086
    webapp02_qa_site1:
      vars:
        app_version: 2023086
  hosts:
    admin01.qa.site1.example.int:
      provisioning_data: {}
    admin02.qa.site1.example.int:
      trace_var: host_vars/admin02.qa.site1.example.int
    app01.qa.site1.example.int:
      trace_var: host_vars/app01.qa.site1.example.int
    app02.qa.site1.example.int:
      trace_var: host_vars/app02.qa.site1.example.int
    web01.qa.site1.example.int:
      provisioning_data:
        infra_group: AIM
        service_domains:
        - webapp101.qa.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: AIM
        service_domains:
        - webapp102.qa.example.int
```

