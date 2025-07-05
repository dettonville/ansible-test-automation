
# Test Use Case - Example [04]

Use Case [04]: update nested test cases with junit report


## Starting/Initial Setup

### Initial Files

test-results/vars/update_groups/testdata_group01.yml:
```yaml
---
test_description: "Add groups"

```

test-results/vars/update_groups/testdata_group02.yml:
```yaml
---
test_description: "Update groups"

```

test-results/vars/update_groups/testdata_group03.yml:
```yaml
---
test_description: "Overwrite groups"

```

test-results/vars/update_inventory/testdata_combined01.yml:
```yaml
---
test_description: "Add groups and hosts"

```

test-results/vars/update_inventory/update_groups/testdata_group01.yml:
```yaml
---
test_description: "Add groups"

```

test-results/vars/update_inventory/update_groups/testdata_group02.yml:
```yaml
---
test_description: "Update groups"

```

test-results/vars/update_inventory/update_groups/testdata_group03.yml:
```yaml
---
test_description: "Overwrite groups"

```


## Playbook Example


```yaml
---

- name: "Run test on dettonville.utils.test_results_logger"
  register: __test_component__test_result
  dettonville.utils.test_results_logger:
    test_case_base_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_gq9ijguh/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/vars
    test_case_file_prefix: testdata_
    test_results:
      test_suites:
        update_inventory:
          properties:
            test_collection_version: 2024.6.2
            test_component_git_branch: feature/AIM-2648-dettonville.inventory-module-enhancements
            test_component_git_commit_hash: a29a72b
            test_date: '2024-06-16T01:30:10Z'
            test_description: Add hosts with global groups enforcement
            test_failed: false
            test_job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/integration-tests/job/dettonville.inventory/job/run-module-tests/job/feature%252FAIM-2648-dettonville.inventory-module-enhancements/23/)'
          test_cases:
            combined01:
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
              test_collection_version: 2024.6.2
              test_component_git_branch: feature/AIM-2648-dettonville.inventory-module-enhancements
              test_component_git_commit_hash: 552f0e2
              test_date: '2024-06-15T23:44:02Z'
              test_description: Add groups
              test_failed: false
              test_job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/integration-tests/job/dettonville.inventory/job/run-module-tests/job/feature%252FAIM-2648-dettonville.inventory-module-enhancements/22/)'
            group01:
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
              test_collection_version: 2024.6.2
              test_component_git_branch: feature/AIM-2648-dettonville.inventory-module-enhancements
              test_component_git_commit_hash: 552f0e2
              test_date: '2024-06-15T23:44:02Z'
              test_description: Add groups
              test_failed: false
              test_job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/integration-tests/job/dettonville.inventory/job/run-module-tests/job/feature%252FAIM-2648-dettonville.inventory-module-enhancements/22/)'
    test_results_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_gq9ijguh/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results


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
  message: The test results file has been created successfully at /Users/ljohnson/.ansible/tmp/.test_jobs_gq9ijguh/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results/junit-report.xml


```


### Resulting file content after running the playbook

test-results/junit-report.xml:
```xml
<?xml version="1.0" ?>
<testsuites assertions="0" disabled="0" errors="0" failures="0" skipped="5" tests="7" time="0.0">
	<testsuite disabled="0" errors="0" failures="0" name="update_groups" skipped="3" tests="3" time="0">
		<testcase name="group01" classname="update_groups">
			<skipped type="skipped">test_suite_id:test_case_id=update_groups:group01 skipped</skipped>
		</testcase>
		<testcase name="group02" classname="update_groups">
			<skipped type="skipped">test_suite_id:test_case_id=update_groups:group02 skipped</skipped>
		</testcase>
		<testcase name="group03" classname="update_groups">
			<skipped type="skipped">test_suite_id:test_case_id=update_groups:group03 skipped</skipped>
		</testcase>
	</testsuite>
	<testsuite disabled="0" errors="0" failures="0" name="update_inventory" skipped="2" tests="4" time="0">
		<properties>
			<property name="test_collection_version" value="2024.6.2"/>
			<property name="test_component_git_branch" value="feature/AIM-2648-dettonville.inventory-module-enhancements"/>
			<property name="test_component_git_commit_hash" value="a29a72b"/>
			<property name="test_date" value="2024-06-16T01:30:10Z"/>
			<property name="test_description" value="Add hosts with global groups enforcement"/>
			<property name="test_failed" value="False"/>
			<property name="test_job_link" value="[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/integration-tests/job/dettonville.inventory/job/run-module-tests/job/feature%252FAIM-2648-dettonville.inventory-module-enhancements/23/)"/>
		</properties>
		<testcase name="combined01" classname="update_inventory"/>
		<testcase name="group01" classname="update_inventory"/>
		<testcase name="group02" classname="update_inventory">
			<skipped type="skipped">test_suite_id:test_case_id=update_inventory:group02 skipped</skipped>
		</testcase>
		<testcase name="group03" classname="update_inventory">
			<skipped type="skipped">test_suite_id:test_case_id=update_inventory:group03 skipped</skipped>
		</testcase>
	</testsuite>
</testsuites>

```

test-results/test-logger-results.yml:
```yml
!TestSuites
test_suites:
  update_groups: !TestSuite
    name: update_groups
    test_cases:
      group01: !TestCase
        name: group01
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_groups
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped:
        - message:
          output: test_suite_id:test_case_id=update_groups:group01 skipped
        allow_multiple_subelements: false
      group02: !TestCase
        name: group02
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_groups
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped:
        - message:
          output: test_suite_id:test_case_id=update_groups:group02 skipped
        allow_multiple_subelements: false
      group03: !TestCase
        name: group03
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_groups
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped:
        - message:
          output: test_suite_id:test_case_id=update_groups:group03 skipped
        allow_multiple_subelements: false
    timestamp:
    hostname:
    id:
    package:
    file:
    log:
    url:
    stdout:
    stderr:
    properties:
  update_inventory: !TestSuite
    name: update_inventory
    test_cases:
      combined01: !TestCase
        name: combined01
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_inventory
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped: []
        allow_multiple_subelements: false
      group01: !TestCase
        name: group01
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_inventory
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped: []
        allow_multiple_subelements: false
      group02: !TestCase
        name: group02
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_inventory
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped:
        - message:
          output: test_suite_id:test_case_id=update_inventory:group02 skipped
        allow_multiple_subelements: false
      group03: !TestCase
        name: group03
        assertions:
        elapsed_sec:
        timestamp:
        classname: update_inventory
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties: {}
        errors: []
        failures: []
        skipped:
        - message:
          output: test_suite_id:test_case_id=update_inventory:group03 skipped
        allow_multiple_subelements: false
    timestamp:
    hostname:
    id:
    package:
    file:
    log:
    url:
    stdout:
    stderr:
    properties:

```

vars/update_groups/testdata_group01.yml:
```yml
---
test_description: "Add groups"

```

vars/update_groups/testdata_group02.yml:
```yml
---
test_description: "Update groups"

```

vars/update_groups/testdata_group03.yml:
```yml
---
test_description: "Overwrite groups"

```

vars/update_inventory/testdata_combined01.yml:
```yml
---
test_description: "Add groups and hosts"

```

vars/update_inventory/update_groups/testdata_group01.yml:
```yml
---
test_description: "Add groups"

```

vars/update_inventory/update_groups/testdata_group02.yml:
```yml
---
test_description: "Update groups"

```

vars/update_inventory/update_groups/testdata_group03.yml:
```yml
---
test_description: "Overwrite groups"

```

