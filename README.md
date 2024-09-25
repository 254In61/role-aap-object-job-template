# role-aap-object-credential
Ansible role to build AAP workflow objects

# Ref
https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/content/module/schedule/


# design
- Role takes in a list and runs with it.

# How to use

Step 1: Install the role in your environment.
   - You could have roles/requirements.yml if running on AAP.
   - Or simple install on your environment.

Step 2: Define your variables in the structure below

job: true/false # Bool value to switch role on off.

in_list_name:
   - name
   - playbook
   - credentials
   - execution_environment
   - timeout
   - instance_groups
   - job_type
   - description
   - organization
   - inventory
   - project
   - state

Step 3: Call the role from your playbook.

# Example playbook

## varible definition in group_vars/*.yml
f5_job_template: 
  - "{{ f5_config_backup_job_template_name }}"
  - "{{ f5_config_backup_playbook }}"
  - ["{{ f5_config_backup_creds }}" , "{{ git_credential_as_network_type[0] }}"] 
  - "{{ f5_exec_env }}"
  - "{{ job_template_timeout }}"
  - "{{ exec_nodes_instance_groups }}"
  - "{{ job_type }}"
  - "{{ common_description }}"
  - "{{ organization }}"
  - "{{ inventory_name }}"
  - "{{ project_name }}"
  - "{{ job_state }}"

cisco_job_template: 
  - "{{ cisco_config_backup_job_template_name }}"
  - "{{ cisco_config_backup_playbook }}"
  - ["{{ cisco_config_backup_creds }}" , "{{ git_credential_as_network_type[0] }}"] 
  - "{{ custom_network_exec_env }}"
  - "{{ job_template_timeout }}"
  - "{{ exec_nodes_instance_groups }}"
  - "{{ job_type }}"
  - "{{ common_description }}"
  - "{{ organization }}"
  - "{{ inventory_name }}"
  - "{{ project_name }}"
  - "{{ job_state }}"
  
## playbook

---
- name: Playbook to configure AAP
  hosts: localhost
  gather_facts: false
 
  pre_tasks:
    - name: Include specific project variables
      ansible.builtin.include_vars:
        dir: group_vars

  tasks:
    !
    !
    - name: Import role-aap-object-job-template
      ansible.builtin.include_role:
        name: role-aap-object-job-template
      vars:
        job: "{{ job_template_build }}"
        in_list: "{{ item }}"
      loop:
        - "{{ cisco_job_template }}"
        - "{{ f5_job_template }}" 
    !
    !