
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
    logging_level: DEBUG
    ssh_params:
      accept_hostkey: true
      key_file: /Users/ljohnson/.ansible/tmp/.test_jobs_ddahjfg7/ansible_repo.key
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
  exception: "Traceback (most recent call last):\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1752172630.62538-39577-194890467958091/AnsiballZ_update_inventory.py\",
    line 259, in <module>\n    _ansiballz_main()\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1752172630.62538-39577-194890467958091/AnsiballZ_update_inventory.py\",
    line 249, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1752172630.62538-39577-194890467958091/AnsiballZ_update_inventory.py\",
    line 122, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1037, in <module>\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1033, in main\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1021, in run_module\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 305, in update_inventory\nAttributeError: 'GitInventoryUpdater' object has
    no attribute 'test_mode'\n"
  failed: true
  module_stderr: "Traceback (most recent call last):\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1752172630.62538-39577-194890467958091/AnsiballZ_update_inventory.py\",
    line 259, in <module>\n    _ansiballz_main()\n  File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1752172630.62538-39577-194890467958091/AnsiballZ_update_inventory.py\",
    line 249, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n
    \ File \"/Users/ljohnson/.ansible/tmp/ansible-tmp-1752172630.62538-39577-194890467958091/AnsiballZ_update_inventory.py\",
    line 122, in invoke_module\n    runpy.run_module(mod_name='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    init_globals=dict(_module_fqn='ansible_collections.dettonville.git_inventory.plugins.modules.update_inventory',
    _modlib_path=modlib_path),\n  File \"<frozen runpy>\", line 226, in run_module\n
    \ File \"<frozen runpy>\", line 98, in _run_module_code\n  File \"<frozen runpy>\",
    line 88, in _run_code\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1037, in <module>\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1033, in main\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/modules/update_inventory.py\",
    line 1021, in run_module\n  File \"/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/ansible_dettonville.git_inventory.update_inventory_payload_36hsvdx5/ansible_dettonville.git_inventory.update_inventory_payload.zip/ansible_collections/dettonville/git_inventory/plugins/module_utils/git_inventory_updater.py\",
    line 305, in update_inventory\nAttributeError: 'GitInventoryUpdater' object has
    no attribute 'test_mode'\n"
  module_stdout: "DEBUG:root:GitInventoryUpdater.init(): loglevel=DEBUG\nDEBUG:root:GitInventoryUpdater.init():
    module_name => dettonville.git_inventory.update_inventory\nDEBUG:root:GitInventoryUpdater.init():
    module_fqcn => dettonville.git_inventory\nDEBUG:root:GitInventoryUpdater.init():
    collection_version=None\nDEBUG:root:GitInventoryUpdater.init(): inventory_repo_dir
    => /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc\nDEBUG:root:GitInventoryUpdater.load_git_repo():
    git_repo_config => {'remote': 'origin',\n 'repo_branch': 'main',\n 'repo_dir': '/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc',\n
    'repo_url': 'git@github.com:dettonville/ansible-test-automation.git',\n 'ssh_params':
    {'accept_hostkey': True,\n                'key_file': '/Users/ljohnson/.ansible/tmp/.test_jobs_ddahjfg7/ansible_repo.key'},\n
    'user_email': 'ansible@dettonville.org',\n 'user_name': 'ansible'}\nDEBUG:root:GitInventoryUpdater.load_git_repo():
    git_comment_module_stamp => dettonville.git_inventory.update_inventory\nDEBUG:root:GitInventoryUpdater.load_git_repo():
    git_comment_prefix => PR-2648\nDEBUG:root:GitInventoryUpdater.load_git_repo(): git_comment_body
    => None\nDEBUG:root:GitInventoryUpdater.load_git_repo(): git_commit_message => PR-2648
    - dettonville.git_inventory.update_inventory: updated inventory\nDEBUG:root:GitInventoryUpdater.load_git_repo():
    cloning repo\nDEBUG:root:self.repo_url=git@github.com:dettonville/ansible-test-automation.git\nDEBUG:root:Git.clone():
    started\nDEBUG:root:Git.clone(): command=['/usr/local/bin/git', 'clone', '--single-branch',
    '--branch', 'main', '--depth=1', '--origin', 'origin', 'git@github.com:dettonville/ansible-test-automation.git',
    '/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc']\nDEBUG:root:Git.clone():
    result => {}\nDEBUG:root:InventoryParser.init(): loglevel=INFO\nINFO:root:InventoryParser.init():
    inventory_repo_dir => /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc\nDEBUG:root:InventoryParser.init():
    inventory_dir => /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc/tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX\nDEBUG:root:InventoryParser.init():
    inventory_file_path => /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc/tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml\nDEBUG:root:InventoryParser.init():
    inventory_root_yaml_key=all\nDEBUG:root:InventoryParser.init(): vars_state => merge\nDEBUG:root:InventoryParser.init():
    vars_overwrite_depth => 2\nDEBUG:root:InventoryParser.init(): use_vars_files =>
    False\nDEBUG:root:InventoryParser.init(): global_groups_file => xenv_groups.yml\nDEBUG:root:InventoryParser.init():
    create_empty_groupvars_files => True\nDEBUG:root:InventoryParser.init(): create_empty_hostvars_files
    => False\nDEBUG:root:InventoryParser.init(): create_parent_groupvar_files => True\nDEBUG:root:InventoryParser.init():
    always_add_child_group_to_root => True\nDEBUG:root:InventoryParser.init(): always_add_host_to_root_hosts
    => False\nDEBUG:root:InventoryParser.init(): enable_groupvar_symlinks_for_child_inventories
    => True\nDEBUG:root:InventoryParser.init(): enforce_global_groups_must_already_exist
    => False\nDEBUG:root:InventoryParser.init(): symlink_subdirs => ['DEV', 'QA', 'PROD',
    'SANDBOX']\nDEBUG:root:InventoryParser.init(): backup => False\nDEBUG:root:InventoryParser.init():
    validate_inventory => True\nDEBUG:root:InventoryParser.init(): remove_repo_dir =>
    True\nDEBUG:root:InventoryParser.init(): test_mode => False\nDEBUG:root:InventoryParser.init():
    loading repo inventory file from /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc/tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml\nDEBUG:root:InventoryParser.init():
    self.inventory_yaml={'all': {'children': {'environment_test': {'hosts': {'app01.qa.site1.example.int':
    {}, 'app02.qa.site1.example.int': {}, 'web01.qa.site1.example.int': {}, 'web02.qa.site1.example.int':
    {}, 'admin01.qa.site1.example.int': {}, 'admin02.qa.site1.example.int': {}}, 'vars':
    {'trace_var': 'group_vars/environment_test'}}, 'location_site1': {'hosts': {'admin01.qa.site1.example.int':
    {}, 'admin02.qa.site1.example.int': {}, 'app01.qa.site1.example.int': {}, 'app02.qa.site1.example.int':
    {}, 'web01.qa.site1.example.int': {}, 'web02.qa.site1.example.int': {}}, 'vars':
    {'trace_var': 'group_vars/location_site1'}}, 'network_internal': {'hosts': {'admin01.qa.site1.example.int':
    {}, 'admin02.qa.site1.example.int': {}, 'app01.qa.site1.example.int': {}, 'app02.qa.site1.example.int':
    {}, 'web01.qa.site1.example.int': {}, 'web02.qa.site1.example.int': {}}, 'vars':
    {'trace_var': 'group_vars/network_internal'}}, 'rhel6': {'hosts': {'admin01.qa.site1.example.int':
    {}}, 'vars': {'trace_var': 'group_vars/rhel6'}}, 'rhel7': {'hosts': {'admin02.qa.site1.example.int':
    {}, 'app01.qa.site1.example.int': {}, 'app02.qa.site1.example.int': {}, 'web01.qa.site1.example.int':
    {}, 'web02.qa.site1.example.int': {}}, 'vars': {'trace_var': 'group_vars/rhel7'}},
    'ungrouped': {}, 'ansible_localhost': {'children': {'ansible_localhost_iam': {},
    'ansible_controller_iam': {}}}}, 'hosts': {'admin01.qa.site1.example.int': {'trace_var':
    'host_vars/admin01.qa.site1.example.int'}, 'admin02.qa.site1.example.int': {'trace_var':
    'host_vars/admin02.qa.site1.example.int'}, 'app01.qa.site1.example.int': {'trace_var':
    'host_vars/app01.qa.site1.example.int'}, 'app02.qa.site1.example.int': {'trace_var':
    'host_vars/app02.qa.site1.example.int'}, 'web01.qa.site1.example.int': {'provisioning_data':
    {'jira_id': 'PR-1101'}, 'trace_var': 'host_vars/web01.qa.site1.example.int'}, 'web02.qa.site1.example.int':
    {'provisioning_data': {'infra_group': 'DCC', 'jira_id': 'PR-1102'}, 'trace_var':
    'host_vars/web02.qa.site1.example.int'}}}}\nDEBUG:root:InventoryParser.init(): finished\nDEBUG:root:GitInventoryUpdater.init():
    finished\nDEBUG:root:InventoryParser.update_group(admin_qa_site1): group => {'group_name':
    'admin_qa_site1',\n 'group_vars': {'infra_group': 'DCC'},\n 'parent_groups': ['vmware_flavor_large',\n
    \                  'ntp_server',\n                   'nfs_server',\n                   'ldap_server']}\nDEBUG:root:InventoryParser.update_group_vars(admin_qa_site1):
    group => {'group_name': 'admin_qa_site1',\n 'group_vars': {'infra_group': 'DCC'},\n
    'parent_groups': ['vmware_flavor_large',\n                   'ntp_server',\n                   'nfs_server',\n
    \                  'ldap_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group => {'group_name': 'admin_qa_site1',\n 'group_vars': {'infra_group': 'DCC'},\n
    'parent_groups': ['vmware_flavor_large',\n                   'ntp_server',\n                   'nfs_server',\n
    \                  'ldap_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group => {'group_name': 'admin_qa_site1',\n 'group_vars': {'infra_group': 'DCC'},\n
    'parent_groups': ['vmware_flavor_large',\n                   'ntp_server',\n                   'nfs_server',\n
    \                  'ldap_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group => {'group_name': 'admin_qa_site1',\n 'group_vars': {'infra_group': 'DCC'},\n
    'parent_groups': ['vmware_flavor_large',\n                   'ntp_server',\n                   'nfs_server',\n
    \                  'ldap_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group => {'group_name': 'admin_qa_site1',\n 'group_vars': {'infra_group': 'DCC'},\n
    'parent_groups': ['vmware_flavor_large',\n                   'ntp_server',\n                   'nfs_server',\n
    \                  'ldap_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(admin_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.update_group(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.update_group_vars(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group => {'group_name': 'webapp01_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp01_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.update_group(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.update_group_vars(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group => {'group_name': 'webapp02_qa_site1',\n 'group_vars': {'app_version': 2023086},\n
    'parent_groups': ['vmware_flavor_small',\n                   'ntp_client',\n                   'nfs_client',\n
    \                  'ldap_client',\n                   'web_server']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(webapp02_qa_site1):
    group_children => None\nDEBUG:root:InventoryParser.update_group(ocp_common): group
    => {'group_name': 'ocp_common',\n 'group_vars': {'ocp_namespace_configuration':
    ['name1', 'name2']}}\nDEBUG:root:InventoryParser.update_group_vars(ocp_common):
    group => {'group_name': 'ocp_common',\n 'group_vars': {'ocp_namespace_configuration':
    ['name1', 'name2']}}\nDEBUG:root:InventoryParser.update_group(ocp_dev_s1): group
    => {'group_name': 'ocp_dev_s1',\n 'group_vars': {'ocp_environment': 'dev', 'ocp_site':
    's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.update_group_vars(ocp_dev_s1):
    group => {'group_name': 'ocp_dev_s1',\n 'group_vars': {'ocp_environment': 'dev',
    'ocp_site': 's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_dev_s1):
    group => {'group_name': 'ocp_dev_s1',\n 'group_vars': {'ocp_environment': 'dev',
    'ocp_site': 's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_dev_s1):
    group_children => None\nDEBUG:root:InventoryParser.update_group(ocp_qa_s1): group
    => {'group_name': 'ocp_qa_s1',\n 'group_vars': {'ocp_environment': 'qa', 'ocp_site':
    's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.update_group_vars(ocp_qa_s1):
    group => {'group_name': 'ocp_qa_s1',\n 'group_vars': {'ocp_environment': 'qa', 'ocp_site':
    's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_qa_s1):
    group => {'group_name': 'ocp_qa_s1',\n 'group_vars': {'ocp_environment': 'qa', 'ocp_site':
    's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_qa_s1):
    group_children => None\nDEBUG:root:InventoryParser.update_group(ocp_prod_s1): group
    => {'group_name': 'ocp_prod_s1',\n 'group_vars': {'ocp_environment': 'prod', 'ocp_site':
    's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.update_group_vars(ocp_prod_s1):
    group => {'group_name': 'ocp_prod_s1',\n 'group_vars': {'ocp_environment': 'prod',
    'ocp_site': 's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_prod_s1):
    group => {'group_name': 'ocp_prod_s1',\n 'group_vars': {'ocp_environment': 'prod',
    'ocp_site': 's1'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_prod_s1):
    group_children => None\nDEBUG:root:InventoryParser.update_group(ocp_dev_s4): group
    => {'group_name': 'ocp_dev_s4',\n 'group_vars': {'ocp_environment': 'dev', 'ocp_site':
    's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.update_group_vars(ocp_dev_s4):
    group => {'group_name': 'ocp_dev_s4',\n 'group_vars': {'ocp_environment': 'dev',
    'ocp_site': 's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_dev_s4):
    group => {'group_name': 'ocp_dev_s4',\n 'group_vars': {'ocp_environment': 'dev',
    'ocp_site': 's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_dev_s4):
    group_children => None\nDEBUG:root:InventoryParser.update_group(ocp_qa_s4): group
    => {'group_name': 'ocp_qa_s4',\n 'group_vars': {'ocp_environment': 'qa', 'ocp_site':
    's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.update_group_vars(ocp_qa_s4):
    group => {'group_name': 'ocp_qa_s4',\n 'group_vars': {'ocp_environment': 'qa', 'ocp_site':
    's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_qa_s4):
    group => {'group_name': 'ocp_qa_s4',\n 'group_vars': {'ocp_environment': 'qa', 'ocp_site':
    's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_qa_s4):
    group_children => None\nDEBUG:root:InventoryParser.update_group(ocp_prod_s4): group
    => {'group_name': 'ocp_prod_s4',\n 'group_vars': {'ocp_environment': 'prod', 'ocp_site':
    's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.update_group_vars(ocp_prod_s4):
    group => {'group_name': 'ocp_prod_s4',\n 'group_vars': {'ocp_environment': 'prod',
    'ocp_site': 's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_prod_s4):
    group => {'group_name': 'ocp_prod_s4',\n 'group_vars': {'ocp_environment': 'prod',
    'ocp_site': 's4'},\n 'parent_groups': ['ocp_common']}\nDEBUG:root:InventoryParser.add_group_to_parent_group(ocp_prod_s4):
    group_children => None\nDEBUG:root:InventoryParser.update_yaml_file(hosts.yml):
    tmpfile=/var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/tmpttx7c3_8\nDEBUG:root:InventoryParser.update_yaml_file(hosts.yml):
    yamllint => /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventoryig2bpqtc/tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml\nDEBUG:root:InventoryParser.update_yaml_file(hosts.yml):
    yaml_lint_errors => []\n"
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
        jira_id: PR-1101
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        jira_id: PR-1102
      trace_var: host_vars/web02.qa.site1.example.int

```

