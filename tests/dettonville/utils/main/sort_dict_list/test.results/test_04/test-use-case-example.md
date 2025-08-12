
# Test Use Case - Example [04]

Use Case [04]: empty list sort test


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Set test_object"
  ignore_errors: yes
  ansible.builtin.set_fact:
    test_object: 
      []


- name: "Run test on dettonville.utils.sort_dict_list"
  ignore_errors: yes
  ansible.builtin.set_fact:
    __test_filter_result: "{{ test_object | dettonville.utils.sort_dict_list(sort_keys=['platform_id', 'address', 'username']) }}"
  register: __test_component__test_result

- name: "Display __test_component__test_result"
  ansible.builtin.debug:
    var: __test_component__test_result

- name: "Display __test_filter_result"
  ansible.builtin.debug:
    var: __test_filter_result
    verbosity: 1

```



## Playbook Run Results

The test example playbook run results:

```shell
TASK [Run test on dettonville.utils.sort_dict_list]

TASK [Display __test_component__test_result]
ok: [localhost] =>
  ansible_facts:
    __test_filter_result: []
  changed: false
  failed: false


TASK [Display __test_filter_result]
ok: [localhost] =>
  []


```


