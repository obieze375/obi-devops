## Introducing Ansible Vault

Ansible might need access to sensitive data, such as passwords or API keys, to configure managed
hosts. Normally, this information is stored as plain text in inventory variables or other Ansible files.
In that case, however, any user with access to the Ansible files, or a version control system that
stores the Ansible files, would have access to this sensitive data. This poses an obvious security
risk.

Ansible Vault, which is included with Ansible, can be used to encrypt and decrypt any data file used
by Ansible. To use Ansible Vault, use the command-line tool named ansible-vault to create,
edit, encrypt, decrypt, and view files.

Ansible Vault can encrypt any data file used by Ansible. This might include inventory variables,
included variable files in a playbook, variable files passed as arguments when executing the
playbook, or variables defined in Ansible roles.

Ansible Vault does not implement its own cryptographic functions but rather uses
an external Python toolkit. Files are protected with symmetric encryption using
AES256 with a password as the secret key. Note that the way this is done has not
been formally audited by a third party.

## Creating an Encrypted File

To create a new encrypted file, use the ansible-vault create filename command. This
command prompts for the new Vault password and then opens a file using the default editor, vi.
You can export the EDITOR environment variable to specify a different default editor. For example,
to set the default editor to nano, run the export EDITOR=nano command.

```bash

[student@demo ~]$ ansible-vault create secret.yml
New Vault password: redhat
Confirm New Vault password: redhat

```



Instead of entering the Vault password through standard input, you can use a Vault password file
to store the Vault password. You need to carefully protect this file using file permissions and other
means.

```bash

[student@demo ~]$ ansible-vault create --vault-password-file=vault-pass secret.yml


```

The cipher used to protect files is AES256 in recent versions of Ansible, but files encrypted with
earlier versions might still use 128-bit AES.

## Viewing an Encrypted File

You can use the ansible-vault view filename command to view an Ansible Vault encrypted file without opening it for editing. 

```bash

[student@demo ~]$ ansible-vault view secret1.yml
Vault password: secret
my_secret: "yJJvPqhsiusmmPPZdnjndkdnYNDjdj782meUZcw"

```


## Editing an Existing Encrypted File


To edit an existing encrypted file, Ansible Vault provides the ansible-vault edit filename
command. This command decrypts the file to a temporary file and allows you to edit it. When
saved, it copies the content and removes the temporary file.

```bash

[student@demo ~]$ ansible-vault edit secret.yml
Vault password: redhat

```

## Encrypting an Existing File


To encrypt a file that already exists, use the ansible-vault encrypt filename command.
This command can take the names of multiple files to be encrypted as arguments. 

```bash

[student@demo ~]$ ansible-vault encrypt secret1.yml secret2.yml
New Vault password: redhat
Confirm New Vault password: redhat


```




## Encryption successful

An existing encrypted file can be permanently decrypted by using the ansible-vault decrypt
filename command. When decrypting a single file, you can use the --output option to save the
decrypted file under a different name.

Use the --output=OUTPUT_FILE option to save the encrypted file with a new name. You can
only use one input file with the --output option.


```bash

[student@demo ~]$ ansible-vault decrypt secret1.yml --output=secret1-decrypted.yml
Vault password: redhat
Decryption successful

```

## Changing the Password of an Encrypted File

You can use the ansible-vault rekey filename command to change the password of an
encrypted file. This command can rekey multiple data files at the same time. It prompts for the
original password and then the new password.

[student@demo ~]$ ansible-vault rekey secret.yml
Vault password: redhat
New Vault password: RedHat
Confirm New Vault password: RedHat
Rekey successful


When using a Vault password file, use the --new-vault-password-file option:

```bash
[student@demo ~]$ ansible-vault rekey \
> --new-vault-password-file=NEW_VAULT_PASSWORD_FILE secret.yml
```



## Playbooks and Ansible Vault

To run a playbook that accesses files encrypted with Ansible Vault, you need to provide the
encryption password to the ansible-navigator command. If you do not provide the password,
the playbook returns an error:

```bash

[student@demo ~]$ ansible-navigator run -m stdout test-secret.yml
ERROR! Attempting to decrypt but no vault secrets found

```




You can provide the Vault password using one of the following options:
• Prompt interactively
• Specify the Vault password file
• Use the ANSIBLE_VAULT_PASSWORD_FILE environment variable

To provide the Vault password interactively, use --playbook-artifact-enable false and
--vault-id @prompt as illustrated in the following example:

```bash
[student@demo ~]$ ansible-navigator run -m stdout \
> --playbook-artifact-enable false \
> site.yml --vault-id @prompt
Vault password (default): redhat

```

You can use the ansible-navigator --playbook-artifact-enable false command to
disable playbook artifacts.

You can also disable playbook artifacts by modifying your project ansible-navigator.yml file
or the .ansible-navigator.yml file in your home directory. Set the playbook-artifact
setting in that file to enable: false.
The following minimal ansible-navigator.yml file disables playbook artifacts:
ansible-navigator:
 playbook-artifact:
 enable: false

Instead of providing the Vault encryption password interactively, you can specify a file that stores
the encryption password in plain text by using the --vault-password-file option.

The password must be a string stored as a single line in the file. Because that file contains the
sensitive plain text password, it is vital that it be protected through file permissions and other
security measures.

```bash
[student@demo ~]$ ansible-navigator run -m stdout site.yml \
> --vault-password-file=vault-pw-file

```



You can also use the ANSIBLE_VAULT_PASSWORD_FILE environment variable to specify the
default location of the password file.








