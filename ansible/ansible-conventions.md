# ansible/conventions


# General

* YAML files - All yaml files should use 2 space indents and end with .yml
* Variables 
  * Use jinja variable syntax - {{ var }} not * $var
  * Use spaces around jinja variable names - {{ var }} not {{var}}
  * Use snake_case for variable names - my_int_var
* Variables meant to be passed in from the ansible CLI MUST have a prefix of cli_ - ex: `ansible-playbook -e cli_foo=bar someplays.yml`
* Variables that are environment specific and that need to be overridden should be in *ALL CAPS*
* Variables that are internal to the role should be lowercase
* Prefix all input variables defined in a role with the uppercase name/abbrv of the role - ex: MYROLE_var
* Use `yes/no` for booleans - prefer this over other booleans in ansible (True/False, true/false, yes/no, 1/0)

* Keep roles self contained - Roles should avoid including tasks from other roles when possible
* Plays should do nothing more than include a list of roles except where pre_tasks and post_tasks are required

* Do not use handlers - If you need to restart an app when specific tasks run, just add a task to do so at the end of the playbook

* Separators - prefer hyphens for easy typing even though issues:
  * module names cannot have hyphens as it's not valid Python name
  * ansible galaxy standardize to use underscores
  * not valid jinja2 variable names
  * playbook use _hyphens_ for word separators and _underscore_ for topic separators - ex: p6-2_cleanup-images.yml
* Do not include trailing slashes in path - ex: /foo, {{ path }}/foo

* Quote strings and prefer single quotes over double quotes
  * use double quotes when they are nested within single quotes
  * use double quotes for escapes
  ```
  # bad
  - name: start robot named S1m0ne
    service:
      name: s1m0ne
      state: started
      enabled: true
    become: true

  # good
  - name: 'start robot named S1m0ne'
    service:
      name: 's1m0ne'
      state: 'started'
      enabled: true
    become: true

  # double quotes w/ nested single quotes
  - name: 'start all robots'
    service:
      name: '{{ item["robot_name"] }}''
      state: 'started'
      enabled: true
    with_items: '{{ robots }}'
    become: true

  # double quotes to escape characters
  - name 'print some text on two lines'
    debug:
      msg: "This text is on\ntwo lines"

  # folded scalar style
  - name: 'robot infos'
    debug:
      msg: >
        Robot {{ item['robot_name'] }} is {{ item['status'] }} and in {{ item['az'] }}
        availability zone with a {{ item['curiosity_quotient'] }} curiosity quotient.
    with_items: robots

  # folded scalar when the string has nested quotes already
  - name: 'print some text'
    debug:
      msg: >
        “I haven’t the slightest idea,” said the Hatter.

  # don't quote booleans/numbers
  - name: 'download google homepage'
    get_url:
      dest: '/tmp'
      timeout: 60
      url: 'https://google.com'
      validate_certs: true

  # variables example 1
  - name: 'set a variable'
    set_fact:
      my_var: 'test'

  # variables example 2
  - name: 'print my_var'
    debug:
      var: my_var
    when: ansible_os_family == 'Darwin'

  # variables example 3
  - name: 'set another variable'
    set_fact:
      my_second_var: '{{ my_var }}'
  ```


# Conditionals and Return Status

* Always use when: for conditionals - To check if a variable is defined when: my_var is defined or when: my_var is not defined
* To verify return status (see conditionals)
  ```
  - command: /bin/false
      register: my_result
      ignore_errors: True
  - debug: msg="task failed"
      when: my_result|failed
  ```


# Formatting

* Use yaml-style / map blocks
  ```
  # good
  - file:
      dest: "{{ test }}"
      src: "./foo.txt"
      mode: 0770
      state: present
      user: "root"
      group: "wheel"
  
  # bad
  - file: >
      dest={{ test }} src=./foo.txt mode=0770
      state=present user=root group=wheel
  ```

* Break long lines using yaml line continuation.
  Reference: http://docs.ansible.com/playbooks_intro.html
  ```
  - shell: >
      python a very long command --with=very --long-options=foo
      --and-even=more_options --like-these
  ```


# Roles

## Role Variables

Role variables are defined as variables contained in (or passed into) a role.

* Role variables MUST have a prefix of at least 3 characters that based on the role name
  ```
  # 3 or more word example - take the first letter of each of the words
  # Role name: made_up_role  ->  Prefix=mur
  mur_var: some_value
  # Role with 2 (or less) words in the name - make up a prefix that makes sense
  # Role name: ansible  ->  Prefix=ans
  ans_var: some_value
  ```
* Role name prefix conflicts
  * most time non-issue as role variables are scoped to the role
  ```
  # example
  - hosts: localhost
    roles:
    - { role: made_up_role, mur_var1: val1 }
    - { role: my_uber_role, mur_var1: val2 }
  ```

* "common" role - Contains tasks that apply to all roles
* "common_vars" role - Contains vars that apply to all roles
* Roles variables - Variables specific to a role should be defined in /vars/main.yml. All variables should be * prefixed with the role name.
* Role defaults - Default variables should configure a role 
* Variables that are environment specific should be all caps
* Variables that need to be overridden should start with uppercase role name / abbreviation


# Sections

* `host` sections
  * host declaration
  * host options in alphabetical order
  * pre_tasks
  * roles
  * tasks
  ```
  # example
  - hosts: 'webservers'
    remote_user: 'centos'
    vars:
      tomcat_state: 'started'
    pre_tasks:
      - name: 'set the timezone to America/Boise'
        lineinfile:
          dest: '/etc/environment'
          line: 'TZ=America/Boise'
          state: 'present'
        become: true
    roles:
      - { role: 'tomcat', tags: 'tomcat' }
    tasks:
      - name: 'start the tomcat service'
        service:
          name: 'tomcat'
          state: '{{ tomcat_state }}'
  ```

* `task` sections
  * task name
  * tags
  * task map declaration (e.g. service:)
  * task parameters in alphabetical order (remember to always use multi-line map syntax)
  * loop operators (e.g. with_items)
  * task options in alphabetical order (e.g. become, ignore_errors, register)
  ```
  # example
  - name: 'create some ec2 instances'
    tags: 'ec2'
    ec2:
      assign_public_ip: true
      image: 'ami-c7d092f7'
      instance_tags:
        Name: '{{ item }}'
      key_name: 'my_key'
    with_items: '{{ instance_names }}'
    ignore_errors: true
    register: ec2_output
    when: ansible_os_family == 'Darwin'
  ```


# Secure vs. Insecure data

As a general policy we want to protect the following data:
* Usernames
* Public keys (keys are OK to be public, but can be used to figure out usernames)
* Hostnames
* Passwords, API keys
