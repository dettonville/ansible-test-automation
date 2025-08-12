
# Test Use Case - Example [09]

Use Case [09]: csv test - empty export list


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.export_dicts"
  register: __test_component__test_result
  dettonville.utils.export_dicts:
    export_list: []
    file: /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results/tests/dettonville/utils/HEAD/export_dicts/testrun/testdata.csv
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
  changed: true
  failed: false
  message: The csv file has been created successfully at /home/jenkins/agent/workspace/ible-utils_run-module-tests_main/test-results/tests/dettonville/utils/HEAD/export_dicts/testrun/testdata.csv


```


### Resulting file content after running the playbook

testdata.csv:
```csv

```

