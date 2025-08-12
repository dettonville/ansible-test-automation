
# Test Use Case - Example [07]

Use Case [07]: markdown test - encoded string values


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.export_dicts"
  register: __test_component__test_result
  dettonville.utils.export_dicts:
    export_list:
    - key1: båz
      key2: value12
      key3: value13
      key4: value14
    - key1: value21
      key2: ﬀöø
      key3: value23
      key4: value24
    - key1: value31
      key2: value32
      key3: ḃâŗ
      key4: value34
    - key1: value41
      key2: value42
      key3: ﬀöø
      key4: båz
    file: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results/tests/dettonville/utils/HEAD/export_dicts/testrun/testdata.md
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
  message: The markdown file has been created successfully at /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results/tests/dettonville/utils/HEAD/export_dicts/testrun/testdata.md


```


### Resulting file content after running the playbook

testdata.md:
```md
| key1 | key2 | key3 | key4 |
| --- | --- | --- | --- |
| båz | value12 | value13 | value14 |
| value21 | ﬀöø | value23 | value24 |
| value31 | value32 | ḃâŗ | value34 |
| value41 | value42 | ﬀöø | båz |

```

