
# Test Use Case - Example [06]

Use Case [06]: SSH - remove test file


## Starting/Initial Setup

### Initial Files

test-results/group_vars/app123.yml:
```yaml
appname: app123

```

test-results/host_vars/testhost124.yml:
```yaml
linux_firewalld__enabled: true

```


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.git_pacp"
  register: __test_component__test_result
  dettonville.utils.git_pacp:
    branch: main
    comment: ' - SSH - remove test file'
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
  git.commit: "[main 9b772af]  - SSH - remove test file\n 1 file changed, 1 deletion(-)\n
    delete mode 100644 tests/dettonville/utils/main/git_pacp/testrun/host_vars/testhost124.yml\n"
  git.pull: 'Already up to date.

    '
  git.push: "To github.com:dettonville/ansible-test-automation.git\n   3107896..9b772af
    \ main -> main\n"
  message: ''


```


### Resulting file content after running the playbook

group_vars/app123.yml:
```yml
appname: app123
```

