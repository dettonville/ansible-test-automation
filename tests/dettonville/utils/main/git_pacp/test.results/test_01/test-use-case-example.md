
# Test Use Case - Example [01]

Use Case [01]: SSH - NO-OP - expect result with changed: false


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.git_pacp"
  register: __test_component__test_result
  dettonville.utils.git_pacp:
    add:
    - .
    branch: main
    comment: ' - SSH - NO-OP - expect result with changed: false'
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
  changed: false
  failed: false


```


### Resulting file content after running the playbook


No Files changed.
