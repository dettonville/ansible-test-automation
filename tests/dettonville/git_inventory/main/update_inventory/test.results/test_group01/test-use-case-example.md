
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
        jira_id: AIM-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: AIM-1102
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
    git_comment_prefix: AIM-2648
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
      key_file: /Users/ljohnson/.ansible/tmp/.test_jobs_kp136fy3/ansible_repo.key
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
  exception: "Traceback (most recent call last):\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1751996249.6373272-87678-181716118709383/AnsiballZ_update_inventory.py\",
    line 259, in <module>\n    _ansiballz_main()\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1751996249.6373272-87678-181716118709383/AnsiballZ_update_inventory.py\",
    line 249, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1751996249.6373272-87678-181716118709383/AnsiballZ_update_inventory.py\",
    line 122, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_z_fydt31/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 842, in <module>\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_z_fydt31/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 39, in <module>\nImportError: cannot import name 'get_yaml_parser' from 'ansible_collections.dettonville.git_inventory.plugins.module_utils.git_inventory_parser'
    (/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_z_fydt31/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_parser.py).
    Did you mean: 'git_yaml_parser'?\n"
  failed: true
  module_stderr: "Traceback (most recent call last):\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1751996249.6373272-87678-181716118709383/AnsiballZ_update_inventory.py\",
    line 259, in <module>\n    _ansiballz_main()\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1751996249.6373272-87678-181716118709383/AnsiballZ_update_inventory.py\",
    line 249, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1751996249.6373272-87678-181716118709383/AnsiballZ_update_inventory.py\",
    line 122, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_z_fydt31/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 842, in <module>\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_z_fydt31/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 39, in <module>\nImportError: cannot import name 'get_yaml_parser' from 'ansible_collections.dettonville.git_inventory.plugins.module_utils.git_inventory_parser'
    (/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_z_fydt31/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_parser.py).
    Did you mean: 'git_yaml_parser'?\n"
  module_stdout: ''
  msg: 'MODULE FAILURE: No start of json char found

    See stdout/stderr for the exact error'
  rc: 1


```

### Resulting file content after running the playbook

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
        jira_id: AIM-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: AIM-1102
      trace_var: host_vars/web02.qa.site1.example.int
```

