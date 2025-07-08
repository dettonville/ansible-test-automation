
# Test Use Case - Example [02]

Use Case [02]: update test


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
    test_case_base_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_tt7fjqf5/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/vars/tests
    test_case_file_prefix: testdata_
    test_results:
      test_suites:
        export_dicts:
          properties:
            collection_version: 2024.5.3
            component_git_commit_hash: 98f3c9c
            date: '2024-05-25T18:17:27Z'
            failed: false
            job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)'
          test_cases:
            '01':
              properties:
                assertions:
                  validate_changed:
                    failed: false
                    msg: All assertions passed
                  validate_failed:
                    failed: false
                    msg: All assertions passed
                  validate_message:
                    failed: false
                    msg: All assertions passed
                  validate_results:
                    failed: false
                    msg: All assertions passed
                collection_version: 2024.5.3
                component_git_branch: develop-lj
                component_git_commit_hash: 98f3c9c
                date: '2024-05-25T18:17:27Z'
                description: CSV test
                failed: false
                job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)'
            '03':
              properties:
                assertions:
                  validate_changed:
                    failed: false
                    msg: All assertions passed
                  validate_failed:
                    failed: false
                    msg: All assertions passed
                  validate_message:
                    failed: false
                    msg: All assertions passed
                  validate_results:
                    failed: true
                    msg: Difference found between test_results and test_expected!
                collection_version: 2024.5.3
                component_git_branch: develop-lj
                component_git_commit_hash: 98f3c9c
                date: '2024-05-25T18:17:27Z'
                description: CSV test
                failed: false
                job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)'
    test_results_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_tt7fjqf5/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results


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
  message: The test results file has been created successfully at /Users/ljohnson/.ansible/tmp/.test_jobs_tt7fjqf5/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results/junit-report.xml


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

