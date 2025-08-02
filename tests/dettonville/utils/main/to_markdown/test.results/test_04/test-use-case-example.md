
# Test Use Case - Example [04]

Use Case [04]: markdown test - export with specified columns


## Starting/Initial Setup

### Initial Files


## Playbook Example


```yaml
---

- name: "Set test_object"
  ignore_errors: yes
  ansible.builtin.set_fact:
    test_object: 
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


- name: "Run test on dettonville.utils.to_markdown"
  ignore_errors: yes
  ansible.builtin.set_fact:
    __test_filter_result: "{{ test_object | dettonville.utils.to_markdown(column_list=[{'name': 'key1', 'header': 'Key #1'}, {'name': 'key2', 'header': 'Key #2'}, {'name': 'key3', 'header': 'Key #3'}, {'name': 'key4', 'header': 'Key #4'}]) }}"
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
TASK [Run test on dettonville.utils.to_markdown]

TASK [Display __test_component__test_result]
ok: [localhost] =>
  ansible_facts:
    __test_filter_result: '| Key #1 | Key #2 | Key #3 | Key #4 |

      | --- | --- | --- | --- |

      | value11 | value12 | value13 | value14 |

      | value21 | value22 | value23 | value24 |

      | value31 | value32 | value33 | value34 |

      | value41 | value42 | value43 | value44 |

      '
  changed: false
  failed: false


TASK [Display __test_filter_result]
ok: [localhost] =>
  '| Key #1 | Key #2 | Key #3 | Key #4 |

    | --- | --- | --- | --- |

    | value11 | value12 | value13 | value14 |

    | value21 | value22 | value23 | value24 |

    | value31 | value32 | value33 | value34 |

    | value41 | value42 | value43 | value44 |

    '


```


