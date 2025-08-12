
# Test Use Case - Example [10]

Use Case [10]: non-existing file directory test


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
    file: bad/location/testdata.csv
    format: csv


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
  changed: false
  failed: true
  msg: Destination directory bad/location does not exist!
  rc: 257


```


### Resulting file content after running the playbook


No Files changed.
