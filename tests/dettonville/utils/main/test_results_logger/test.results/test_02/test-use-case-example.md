
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
    test_case_base_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_jf3o3uqi/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/vars/tests
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
    test_results_dir: /Users/ljohnson/.ansible/tmp/.test_jobs_jf3o3uqi/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results


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
  message: The test results file has been created successfully at /Users/ljohnson/.ansible/tmp/.test_jobs_jf3o3uqi/test.dettonville.utils/tests/dettonville/utils/main/test_results_logger/testrun/test-results/junit-report.xml


```


### Resulting file content after running the playbook

test-results/junit-report.xml:
```xml
<?xml version="1.0" ?>
<testsuites assertions="8" disabled="0" errors="0" failures="1" skipped="5" tests="7" time="0.0">
	<testsuite assertions="8" disabled="0" errors="0" failures="1" name="export_dicts" skipped="2" tests="4" time="0">
		<properties>
			<property name="collection_version" value="2024.5.3"/>
			<property name="component_git_commit_hash" value="98f3c9c"/>
			<property name="date" value="2024-05-25T18:17:27Z"/>
			<property name="failed" value="False"/>
			<property name="job_link" value="[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)"/>
		</properties>
		<testcase name="01" assertions="4" classname="export_dicts">
			<properties>
				<property name="assertions" value="{'validate_changed': {'failed': False, 'msg': 'All assertions passed'}, 'validate_failed': {'failed': False, 'msg': 'All assertions passed'}, 'validate_message': {'failed': False, 'msg': 'All assertions passed'}, 'validate_results': {'failed': False, 'msg': 'All assertions passed'}}"/>
				<property name="collection_version" value="2024.5.3"/>
				<property name="component_git_branch" value="develop-lj"/>
				<property name="component_git_commit_hash" value="98f3c9c"/>
				<property name="date" value="2024-05-25T18:17:27Z"/>
				<property name="description" value="CSV test"/>
				<property name="failed" value="False"/>
				<property name="job_link" value="[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)"/>
			</properties>
		</testcase>
		<testcase name="02" classname="export_dicts">
			<skipped type="skipped">test_suite_id:test_case_id=export_dicts:02 skipped</skipped>
		</testcase>
		<testcase name="03" assertions="4" classname="export_dicts">
			<failure type="failure">validate_results: {'failed': True, 'msg': 'Difference found between test_results and test_expected!'}</failure>
			<properties>
				<property name="assertions" value="{'validate_changed': {'failed': False, 'msg': 'All assertions passed'}, 'validate_failed': {'failed': False, 'msg': 'All assertions passed'}, 'validate_message': {'failed': False, 'msg': 'All assertions passed'}, 'validate_results': {'failed': True, 'msg': 'Difference found between test_results and test_expected!'}}"/>
				<property name="collection_version" value="2024.5.3"/>
				<property name="component_git_branch" value="develop-lj"/>
				<property name="component_git_commit_hash" value="98f3c9c"/>
				<property name="date" value="2024-05-25T18:17:27Z"/>
				<property name="description" value="CSV test"/>
				<property name="failed" value="False"/>
				<property name="job_link" value="[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)"/>
			</properties>
		</testcase>
		<testcase name="04" classname="export_dicts">
			<skipped type="skipped">test_suite_id:test_case_id=export_dicts:04 skipped</skipped>
		</testcase>
	</testsuite>
	<testsuite disabled="0" errors="0" failures="0" name="git_acp" skipped="3" tests="3" time="0">
		<testcase name="01" classname="git_acp">
			<skipped type="skipped">test_suite_id:test_case_id=git_acp:01 skipped</skipped>
		</testcase>
		<testcase name="02" classname="git_acp">
			<skipped type="skipped">test_suite_id:test_case_id=git_acp:02 skipped</skipped>
		</testcase>
		<testcase name="03" classname="git_acp">
			<skipped type="skipped">test_suite_id:test_case_id=git_acp:03 skipped</skipped>
		</testcase>
	</testsuite>
</testsuites>

```

test-results/test-logger-results.yml:
```yml
!TestSuites
test_suites:
  export_dicts: !TestSuite
    name: export_dicts
    test_cases:
      '01': !TestCase
        name: '01'
        assertions: 4
        elapsed_sec:
        timestamp:
        classname: export_dicts
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties:
          collection_version: 2024.5.3
          component_git_branch: develop-lj
          component_git_commit_hash: 98f3c9c
          date: '2024-05-25T18:17:27Z'
          description: CSV test
          failed: false
          job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)'
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
        errors: []
        failures: []
        skipped: []
        allow_multiple_subelements: false
      '02': !TestCase
        name: '02'
        assertions:
        elapsed_sec:
        timestamp:
        classname: export_dicts
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
          output: test_suite_id:test_case_id=export_dicts:02 skipped
        allow_multiple_subelements: false
      '03': !TestCase
        name: '03'
        assertions: 4
        elapsed_sec:
        timestamp:
        classname: export_dicts
        status:
        category:
        file:
        line:
        log:
        url:
        stdout:
        stderr:
        is_enabled: true
        properties:
          collection_version: 2024.5.3
          component_git_branch: develop-lj
          component_git_commit_hash: 98f3c9c
          date: '2024-05-25T18:17:27Z'
          description: CSV test
          failed: false
          job_link: '[test job link](https://infracicdd1s1.example.org/jenkins/job/INFRA/job/repo-test-automation/job/dettonville.utils/job/run-module-tests/job/develop-lj/949/)'
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
        errors: []
        failures:
        - message:
          output: "validate_results: {'failed': True, 'msg': 'Difference found between test_results and test_expected!'}"
          type:
        skipped: []
        allow_multiple_subelements: false
      '04': !TestCase
        name: '04'
        assertions:
        elapsed_sec:
        timestamp:
        classname: export_dicts
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
          output: test_suite_id:test_case_id=export_dicts:04 skipped
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
  git_acp: !TestSuite
    name: git_acp
    test_cases:
      '01': !TestCase
        name: '01'
        assertions:
        elapsed_sec:
        timestamp:
        classname: git_acp
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
          output: test_suite_id:test_case_id=git_acp:01 skipped
        allow_multiple_subelements: false
      '02': !TestCase
        name: '02'
        assertions:
        elapsed_sec:
        timestamp:
        classname: git_acp
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
          output: test_suite_id:test_case_id=git_acp:02 skipped
        allow_multiple_subelements: false
      '03': !TestCase
        name: '03'
        assertions:
        elapsed_sec:
        timestamp:
        classname: git_acp
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
          output: test_suite_id:test_case_id=git_acp:03 skipped
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

