
# Test Use Case - Example [host02]

Use Case [host02]: Update hosts


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
    host_list:
    - host_name: admin01.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: DCC
          service_domains:
          - admin.qa.example.int
      parent_groups:
      - vmware_flavor_large
      - ntp_server
      - nfs_server
      - ldap_server
    - host_name: web01.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: INFRA
          service_domains:
          - webapp101.qa.example.int
          - webapp101.qa.site1.example.int
      parent_groups:
      - vmware_flavor_small
      - ntp_client
      - nfs_client
      - ldap_client
      - web_server
    - host_name: web02.qa.site1.example.int
      host_vars:
        provisioning_data:
          infra_group: INFRA
          service_domains:
          - webapp102.qa.example.int
      parent_groups:
      - vmware_flavor_small
      - ntp_client
      - nfs_client
      - ldap_client
      - web_server
    inventory_file: tests/dettonville/git_inventory/main/update_inventory/testrun/_test_inventory/SANDBOX/hosts.yml
    inventory_repo_branch: main
    inventory_repo_url: git@github.com:dettonville/ansible-test-automation.git
    jira_id: TEST-1001
    ssh_params:
      accept_hostkey: true
      key_file: /Users/ljohnson/.ansible/tmp/.test_jobs_aa155xeh/ansible_repo.key
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
  backup_files: null
  changed: true
  check_mode: false
  failed: false
  git.add: ''
  git.commit: "[main d50ef2c] TEST-1001 - dettonville.git_inventory.update_inventory:
    updated inventory\n 1 file changed, 63 insertions(+), 21 deletions(-)\n"
  git.pull: "Already up to date.\nFrom github.com:dettonville/ansible-test-automation\n
    * branch            main       -> FETCH_HEAD\n"
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   66e5388..d50ef2c
    \ main -> main\n"
  inventory_base_dir: /var/folders/w6/3rcdpp211v5cxml6vg45ww3r0000gn/T/update_inventorykel08s8p
  message: Inventory updated successfully


```

### Resulting file content after running the playbook

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
    ldap_client:
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
    ldap_server:
      hosts:
        admin01.qa.site1.example.int: {}
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
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
    nfs_server:
      hosts:
        admin01.qa.site1.example.int: {}
    ntp_client:
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
    ntp_server:
      hosts:
        admin01.qa.site1.example.int: {}
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
      hosts:
        admin01.qa.site1.example.int: {}
    vmware_flavor_small:
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
    web_server:
      hosts:
        web01.qa.site1.example.int: {}
        web02.qa.site1.example.int: {}
  hosts:
    admin01.qa.site1.example.int:
      provisioning_data:
        infra_group: DCC
        service_domains:
        - admin.qa.example.int
      trace_var: host_vars/admin01.qa.site1.example.int
    admin02.qa.site1.example.int:
      trace_var: host_vars/admin02.qa.site1.example.int
    app01.qa.site1.example.int:
      trace_var: host_vars/app01.qa.site1.example.int
    app02.qa.site1.example.int:
      trace_var: host_vars/app02.qa.site1.example.int
    web01.qa.site1.example.int:
      provisioning_data:
        infra_group: INFRA
        jira_id: INFRA-1101
        service_domains:
        - webapp101.qa.example.int
        - webapp101.qa.site1.example.int
      trace_var: host_vars/web01.qa.site1.example.int
    web02.qa.site1.example.int:
      provisioning_data:
        infra_group: INFRA
        jira_id: INFRA-1102
        service_domains:
        - webapp102.qa.example.int
      trace_var: host_vars/web02.qa.site1.example.int
```

