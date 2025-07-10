
# Ansible Test Automation

The purpose of this repository is to store ansible integration test automation artifacts including:

1) starting fixtures
2) test results

For the following dettonville ansible collections:

- [dettonville.utils](https://github.com/dettonville/ansible-utils)
- [dettonville.git_inventory](https://github.com/dettonville/ansible-git-inventory)

The scope of the test resources include (but is not limited) the following collection plugins and/or roles:

```output
tree -A -d -L 4 tests
tests
└── dettonville
    ├── git_inventory
    │   └── main
    │       └── update_inventory
    └── utils
        └── main
            ├── export_dicts
            ├── git_pacp
            ├── remove_dict_keys
            ├── remove_sensitive_keys
            ├── sort_dict_list
            └── test_results_logger

13 directories

```
