
# Test Use Case - Example [04]

Use Case [04]: SSH - expect default `add` path work


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.git_pacp"
  register: __test_component__test_result
  dettonville.utils.git_pacp:
    branch: main
    comment: ' - SSH - expect default `add` path work'
    path: /Users/ljohnson/.ansible/tmp/.test_jobs_4mz15b8v/test.dettonville.utils
    ssh_params:
      accept_hostkey: true
      key_file: /Users/ljohnson/.ansible/tmp/.test_jobs_4mz15b8v/ansible_repo.key
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
  git.commit: "[main 37511ad]  - SSH - expect default `add` path work\n 1 file changed,
    29 insertions(+)\n create mode 100644 tests/dettonville/utils/main/git_pacp/testrun/foobar123.yml\n"
  git.pull: 'Already up to date.

    '
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   3dc7b50..37511ad
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

