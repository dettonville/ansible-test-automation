
# Test Use Case - Example [01]

Use Case [01]: single key sort test


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Set test_object"
  ignore_errors: yes
  ansible.builtin.set_fact:
    test_object: 
      - address: 10.31.25.54
        automatic_management_enabled: true
        domain_type: local
        platform_account_type: recon
        platform_id: WND-Local-Managed-DMZ
        platform_logon_domain: 10.31.25.54
        platform_notes: WINANSD1S4.example.int
        safe: A-T-careconlocal
        username: careconlocal
      - address: 10.31.25.54
        automatic_management_enabled: true
        domain_type: local
        groups:
        - Administrators
        local_admin_username: administrator
        managed: true
        platform_account_type: platform
        platform_id: WND-Local-Managed-DMZ
        platform_logon_domain: 10.31.25.54
        platform_notes: WINANSD1S4.example.int
        safe: Windows-Server-Local-Admin
        username: administrator
      - address: 10.21.33.8
        automatic_management_enabled: true
        domain_type: local
        platform_account_type: recon
        platform_id: WND-Local-Managed-DMZ
        platform_logon_domain: 10.21.33.8
        platform_notes: WINANSD1S1.example.int
        safe: A-T-careconlocal
        username: careconlocal
      - address: 10.21.33.8
        automatic_management_enabled: true
        domain_type: local
        groups:
        - Administrators
        local_admin_username: administrator
        managed: true
        platform_account_type: platform
        platform_id: WND-Local-Managed-DMZ
        platform_logon_domain: 10.21.33.8
        platform_notes: WINANSD1S1.example.int
        safe: Windows-Server-Local-Admin
        username: administrator


- name: "Run test on dettonville.utils.sort_dict_list"
  ignore_errors: yes
  ansible.builtin.set_fact:
    __test_filter_result: "{{ test_object | dettonville.utils.sort_dict_list(sort_keys=username) }}"
  register: __test_component__test_result

- name: "Display __test_component__test_result"
  ansible.builtin.debug:
    var: __test_component__test_result

- name: "Display __test_filter_result"
  ansible.builtin.debug:
    var: __test_filter_result
    verbosity: 1

```



## Playbook Run Results

The test example playbook run results:

```shell
TASK [Run test on dettonville.utils.sort_dict_list]

TASK [Display __test_component__test_result]
ok: [localhost] =>
  ansible_facts:
    __test_filter_result:
    - address: 10.31.25.54
      automatic_management_enabled: true
      domain_type: local
      groups:
      - Administrators
      local_admin_username: administrator
      managed: true
      platform_account_type: platform
      platform_id: WND-Local-Managed-DMZ
      platform_logon_domain: 10.31.25.54
      platform_notes: WINANSD1S4.example.int
      safe: Windows-Server-Local-Admin
      username: administrator
    - address: 10.21.33.8
      automatic_management_enabled: true
      domain_type: local
      groups:
      - Administrators
      local_admin_username: administrator
      managed: true
      platform_account_type: platform
      platform_id: WND-Local-Managed-DMZ
      platform_logon_domain: 10.21.33.8
      platform_notes: WINANSD1S1.example.int
      safe: Windows-Server-Local-Admin
      username: administrator
    - address: 10.31.25.54
      automatic_management_enabled: true
      domain_type: local
      platform_account_type: recon
      platform_id: WND-Local-Managed-DMZ
      platform_logon_domain: 10.31.25.54
      platform_notes: WINANSD1S4.example.int
      safe: A-T-careconlocal
      username: careconlocal
    - address: 10.21.33.8
      automatic_management_enabled: true
      domain_type: local
      platform_account_type: recon
      platform_id: WND-Local-Managed-DMZ
      platform_logon_domain: 10.21.33.8
      platform_notes: WINANSD1S1.example.int
      safe: A-T-careconlocal
      username: careconlocal
  changed: false
  failed: false


TASK [Display __test_filter_result]
ok: [localhost] =>
  - address: 10.31.25.54
    automatic_management_enabled: true
    domain_type: local
    groups:
    - Administrators
    local_admin_username: administrator
    managed: true
    platform_account_type: platform
    platform_id: WND-Local-Managed-DMZ
    platform_logon_domain: 10.31.25.54
    platform_notes: WINANSD1S4.example.int
    safe: Windows-Server-Local-Admin
    username: administrator
  - address: 10.21.33.8
    automatic_management_enabled: true
    domain_type: local
    groups:
    - Administrators
    local_admin_username: administrator
    managed: true
    platform_account_type: platform
    platform_id: WND-Local-Managed-DMZ
    platform_logon_domain: 10.21.33.8
    platform_notes: WINANSD1S1.example.int
    safe: Windows-Server-Local-Admin
    username: administrator
  - address: 10.31.25.54
    automatic_management_enabled: true
    domain_type: local
    platform_account_type: recon
    platform_id: WND-Local-Managed-DMZ
    platform_logon_domain: 10.31.25.54
    platform_notes: WINANSD1S4.example.int
    safe: A-T-careconlocal
    username: careconlocal
  - address: 10.21.33.8
    automatic_management_enabled: true
    domain_type: local
    platform_account_type: recon
    platform_id: WND-Local-Managed-DMZ
    platform_logon_domain: 10.21.33.8
    platform_notes: WINANSD1S1.example.int
    safe: A-T-careconlocal
    username: careconlocal


```


