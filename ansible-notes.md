# ansible-notes.md

## running ansible locally in cli

* pass inventory on cli

  `$ ansible-playbook --inventory localhost, test.yml`

* specify host and connction in yml

    ```
    ---
    - name: dbg
    hosts: localhost
    connection: local
    tasks:
        - debug: msg="hellow world"
    ```

## specifying variables from cli

example: `--extra-vars "who=blah"`

    ```
    $ cat test.yml ; ansible-playbook --inventory localhost, --extra-vars "who=blah" test.yml
    ---
    - name: dbg
    hosts: localhost
    connection: local
    tasks:
        - debug: msg="hello world to {{ who }}"

    PLAY [dbg] *************************************************************************************************************

    TASK [Gathering Facts] *************************************************************************************************
    ok: [localhost]

    TASK [debug] ***********************************************************************************************************
    ok: [localhost] => {
        "msg": "hello world to blah"
    }

    PLAY RECAP *************************************************************************************************************
    localhost                  : ok=2    changed=0    unreachable=0    failed=0
    ```

## information query by ansible

    `$ ansible localhost -m setup`


## infromation from host var
ansible -i ansible/inventory.yml -m debug cloud -a "var=hostvars[inventory_hostname].var0"
ansible -i ansible/inventory.yml -m debug cloud -a "var=hostvars[inventory_hostname].var1"

## dump variable to file

    ```
    $ date ; cat test.yml; \
    > ansible-playbook --inventory localhost,  test.yml; ls -l ddbg
    Tue Jul 16 14:10:36 EDT 2019
    ---
    - name: dbg
    hosts: localhost
    tasks:
        - local_action: copy content="{{ ansible_facts }}" dest=ddbg

    PLAY [dbg] *************************************************************************************************************

    TASK [Gathering Facts] *************************************************************************************************
    ok: [localhost]

    TASK [copy] ************************************************************************************************************
    changed: [localhost -> localhost]

    PLAY RECAP *************************************************************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

    -rw-r--r--  1 plee38  staff  11035 Jul 16 14:10 ddbg
    ```

## specify ansible's python via ansible_python_interpreter variable

    `ansible-playbook --inventory localhost,  create_resource.yml -e 'ansible_python_interpreter=/usr/local/bin/python3'`

## developing ansible modules
* run as main [exercise module locally](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html#exercising-module-code-locally)
  use json file with `{ "ANSIBLE_MODULE_ARGS": { "arg1": "val1" } }`
  run it as `$ python -m ansible.modules.my_test /tmp/args.json`
  
## structuring
* A role is a collection of tasks and templates
* A role contains several directories with `main.yml` as entry point (only tasks directory is required)
  * defaults/main.yml
  * tasks/main.yml
  * handlers/main.yml
  * meta/main.yml
  * templates
  * files

## tasks/main.yml - tag style
3 ways:
```
- import_role: name=localhost tasks_from=debug
  tags: ['never', 'debug']
  delegate_to: localhost
  vars:
    echo_debug_level: False
```
```
- import_tasks: debug.yml
  become: true
  tags: [never,debug]
```
```
- import_tasks: foobar-install.yml
  when: foobar
```

