
# Test Use Case - Example [12]

Use Case [12]: implied file format


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.export_dicts"
  register: __test_component__test_result
  dettonville.utils.export_dicts:
    export_list:
    - key1: value11
      key2: value12
      key3: value13
      key4: value14
    - key1: value21
      key2: value22
      key3: value23
      key4: value24
    - key1: value31
      key2: value32
      key3: value33
      key4: value34
    - key1: value41
      key2: value42
      key3: value43
      key4: value44
    file: /Users/ljohnson/.ansible/tmp/.test_jobs_4bsc9pyd/test.dettonville.utils/tests/dettonville/utils/main/export_dicts/testrun/testdata.md


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
  message: The markdown file has been created successfully at /Users/ljohnson/.ansible/tmp/.test_jobs_4bsc9pyd/test.dettonville.utils/tests/dettonville/utils/main/export_dicts/testrun/testdata.md


```


### Resulting file content after running the playbook

testdata.md:
```md
| key1 | key2 | key3 | key4 |
| --- | --- | --- | --- |
| value11 | value12 | value13 | value14 |
| value21 | value22 | value23 | value24 |
| value31 | value32 | value33 | value34 |
| value41 | value42 | value43 | value44 |

```

