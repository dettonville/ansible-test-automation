
# Test Use Case - Example [01]

Use Case [01]: init test


## Starting/Initial Setup

### Initial Files

test-results/vars/tests/export_dicts/testdata_01.yml:
```yaml
---
test_description: "CSV test"
test_file_format: "csv"

```

test-results/vars/tests/export_dicts/testdata_02.yml:
```yaml
---
test_description: "CSV test - empty key value"
test_file_format: "csv"

```

test-results/vars/tests/export_dicts/testdata_03.yml:
```yaml
---
test_description: "CSV test - encoded string values"

```

test-results/vars/tests/export_dicts/testdata_04.yml:
```yaml
---
test_description: "CSV test - export with specified columns"

```

test-results/vars/tests/git_acp/testdata_01.yml:
```yaml
---
test_description: "SSH - NO-OP - expect result with changed: false"

```

test-results/vars/tests/git_acp/testdata_02.yml:
```yaml
---
test_description: "SSH - add test file"

```

test-results/vars/tests/git_acp/testdata_03.yml:
```yaml
---
test_description: "SSH - add test file with explicit `add` path"

```


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.test_results_logger"
  register: __test_component__test_result
  dettonville.utils.test_results_logger:
    test_case_base_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_8mdf8m5k/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/vars/tests
    test_case_file_prefix: testdata_
    test_results_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_8mdf8m5k/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results


- name: "Display __test_component__test_result"
  ansible.builtin.debug:
    var: __test_component__test_result

```



## Playbook Run Results

The test example playbook run results:

```shell
TASK [Run test on dettonville.utils.test_results_logger]
TASK [Display __test_component__test_result]
ok: [localhost] =>
  changed: true
  failed: false
  message: The test results file has been created successfully at /Users/ljohnson/.ansible/tmp/.test_jobs_8mdf8m5k/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results/junit-report.xml


```


### Resulting file content after running the playbook

vars/tests/export_dicts/testdata_01.yml:
```yml
---
test_description: "CSV test"
test_file_format: "csv"

```

vars/tests/export_dicts/testdata_02.yml:
```yml
---
test_description: "CSV test - empty key value"
test_file_format: "csv"

```

vars/tests/export_dicts/testdata_03.yml:
```yml
---
test_description: "CSV test - encoded string values"

```

vars/tests/export_dicts/testdata_04.yml:
```yml
---
test_description: "CSV test - export with specified columns"

```

vars/tests/git_acp/testdata_01.yml:
```yml
---
test_description: "SSH - NO-OP - expect result with changed: false"

```

vars/tests/git_acp/testdata_02.yml:
```yml
---
test_description: "SSH - add test file"

```

vars/tests/git_acp/testdata_03.yml:
```yml
---
test_description: "SSH - add test file with explicit `add` path"

```

