
# Test Use Case - Example [03]

Use Case [03]: SSH - add test file with explicit `add` path


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.git_pacp"
  register: __test_component__test_result
  dettonville.utils.git_pacp:
    add:
    - foobar123.yml
    branch: main
    comment: ' - SSH - add test file with explicit `add` path'
    path: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_sae3xm11/ansible_repo.key
      ssh_opts: -o UserKnownHostsFile=/dev/null
    url: git@github.com:dettonville/ansible-test-automation.git


- name: "Display __test_component__test_result"
  ansible.builtin.debug:
    var: __test_component__test_result

```



## Playbook Run Results

The test example playbook run results:

```shell
TASK [Run test on dettonville.utils.git_pacp]
TASK [Display __test_component__test_result]
ok: [localhost] =>
  changed: true
  failed: false
  git.commit: "[main 9a4e39b]  - SSH - add test file with explicit `add` path\n 1 file
    changed, 29 insertions(+)\n create mode 100644 tests/dettonville/utils/main/git_pacp/testrun/foobar123.yml\n"
  git.pull: 'Already up to date.

    '
  git.push: "Warning: Permanently added 'github.com,140.82.114.4' (ECDSA) to the list
    of known hosts.\r\nTo github.com:dettonville/ansible-test-automation.git\n   661009b..9a4e39b
    \ main -> main\n"
  message: ''


```


### Resulting file content after running the playbook

foobar123.yml:
```yml
all:
  children:
    admin_qa_site1:
      vars:
        infra_group: DCC
  hosts:
    admin01.qa.site1.example.int:
      trace_var: host_vars/admin01.qa.site1.example.int
    admin02.qa.site1.example.int:
      trace_var: host_vars/admin02.qa.site1.example.int
    app01.qa.site1.example.int:
      trace_var: host_vars/app01.qa.site1.example.int
    app02.qa.site1.example.int:
      trace_var: host_vars/app02.qa.site1.example.int
    vmlnx123.qa.site1.example.int:
      provisioning_data:
        infra_group: AIM
    vmlnx124.qa.site1.example.int:
      provisioning_data:
        infra_group: AIM
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

