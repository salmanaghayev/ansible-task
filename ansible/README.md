#### The following command run playbook with vault password which defined in the _ansible.cfg_ file (To use this feature we need to define vault password file in the **vault_password_file** variable):
```bash
$ ansible-playbook deploy-frontend.yml --extra-vars "env=prod variable_host=frontend"
```

#### With the following command we input vault password from console:
```bash
$ ansible-playbook --ask-vault-pass deploy-frontend.yml --extra-vars "env=prod variable_host=frontend"
```

#### Edit crypted file:
```bash
$ ansible-vault edit vars/prod.yml
```

#### To create new crypted file just copy content from previus file and delete it. Then open new file with the following syntax and paste buffer content inside of this new file (It will ask about new password for this file):
```bash
$ ansible-vault create ./inventories/prod/hosts
```

#### To encrypt exising file just use the following command:
```bash
$ ansible-vault encrypt ./inventories/dev/hosts
```

#### To connect to the each of the hosts defined in the **inventories/env/hosts** file with **ansible_host** variable, we need just to change the valuses of the **ansible_host**, **ansible_ssh_user** and **ansible_ssh_pass** variables. If we need to change the value of the **bamboo_username**, **bamboo_pass**, **bamboo_buildname**, **bamboo_build_artifact_folder** and **bamboo_build_artifact_zip** then just edit the **./vars/prod.yml** file. Just remember when we will input _--extra-vars "env=prod"_ from CLI ansible firstly will go to read **vars** folder for the variables because of we have not defined inventory file by hands.  

#### Note: If we define inventory file from CLI by hands then it will go to read **inventories/prod/group_vars/all.yml** file directly for the variables (Syntax like as following):
```bash
$ ansible-playbook deploy-frontend.yml -i inventories/prod/hosts --extra-vars "variable_host=frontend"
```

#### In the following example we use invetory file because of we have defined in the _asnible.cfg_ inventory _inventory = ./inventories_ folder. Inside of the _./inventories_ folder have a lot of environment inventory files, that is why we have used directly file with the _-i_ option. All variables like as _bamboo.depEnvName_, _bamboo.deployServers_, _bamboo.buildName_, _bamboo.artifactFldrName_, _bamboo.artifactZipFileName_ and _bamboo.bambooLink_ will come from Bamboo CI/CD.
```bash
$ ansible-playbook deploy-frontend.yml -i inventories/${bamboo.depEnvName}/hosts --extra-vars "env=${bamboo.depEnvName} variable_host=${bamboo.deployServers} buildName=${bamboo.buildName} artifactFldrName=${bamboo.artifactFldrName} artifactZipFileName=${bamboo.artifactZipFileName} bambooLink=${bamboo.bambooLink}"
```

#### To run APIGATEWAY service use the following command:
```bash
$ ansible-playbook deploy-apigateway.yml -i inventories/dev/hosts --extra-vars "env=dev variable_host=apigateway"
```

#### To run MPEAI service use the following command:
```bash
$ ansible-playbook deploy-apigateway.yml -i inventories/dev/hosts --extra-vars "env=dev variable_host=mpeai"
```
