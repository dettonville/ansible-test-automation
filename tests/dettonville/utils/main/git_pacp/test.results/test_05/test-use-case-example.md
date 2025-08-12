
# Test Use Case - Example [05]

Use Case [05]: SSH - add test file with remote alias defined


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.git_pacp"
  register: __test_component__test_result
  dettonville.utils.git_pacp:
    branch: main
    comment: ' - SSH - add test file with remote alias defined'
    path: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results
    remote: origin
    ssh_params:
      accept_hostkey: true
      key_file: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main@tmp/.ansible/tmp/.test_jobs_mxz8z83b/ansible_repo.key
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
  git.add: ''
  git.commit: "[main f409af7]  - SSH - add test file with remote alias defined\n 1 file
    changed, 29 insertions(+)\n create mode 100644 tests/dettonville/utils/main/git_pacp/testrun/foobar123.yml\n"
  git.pull: "Already up to date.\nFrom github.com:dettonville/ansible-test-automation\n
    * branch            main       -> FETCH_HEAD\n"
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   227a5f0..f409af7
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

