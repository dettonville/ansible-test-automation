
# Test Use Case - Example [11]

Use Case [11]: markdown test - export with specified columns where rows are missing values


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.export_dicts"
  register: __test_component__test_result
  dettonville.utils.export_dicts:
    columns:
    - header: 'Key #1'
      name: key1
    - header: 'Key #2'
      name: key2
    - header: 'Key #3'
      name: key3
    - header: 'Key #4'
      name: key4
    export_list:
    - key1: value11
      key2: value12
      key3: value13
      key4: value14
    - key1: value21
      key2: value22
      key3: value23
    - key1: value31
      key2: value32
      key3: value33
      key4: value34
    - key1: value41
      key3: value43
      key4: value44
    file: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results/tests/dettonville/utils/main/export_dicts/testrun/testdata.md
    format: md


- name: "Display __test_component__test_result"
  ansible.builtin.debug:
    var: __test_component__test_result

```



## Playbook Run Results

The test example playbook run results:

```shell
TASK [Run test on dettonville.utils.export_dicts]
TASK [Display __test_component__test_result]
ok: [localhost] =>
  changed: true
  failed: false
  message: The markdown file has been created successfully at /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results/tests/dettonville/utils/main/export_dicts/testrun/testdata.md


```


### Resulting file content after running the playbook

testdata.md:
```md
| Key #1 | Key #2 | Key #3 | Key #4 |
| --- | --- | --- | --- |
| value11 | value12 | value13 | value14 |
| value21 | value22 | value23 |  |
| value31 | value32 | value33 | value34 |
| value41 |  | value43 | value44 |

```

