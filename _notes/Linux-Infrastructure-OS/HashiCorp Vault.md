
[[Index]] 

## HashiCorp Vault Commands

  

# DEV Server

Start DEV server

```ps

vault server -dev

```

  

# Before login

Set vault address before log in

```ps

$env:VAULT_ADDR="http://127.0.0.1:8200"

```

  

# Login

## Login using token

```ps

vault login

```

  

## Login using userpass

```ps

vault login -method=userpass username=$userName

```

  

## Secret / KV

### Set a secret in KV Secret Engine

```ps

vault kv put $kvName/$secretName $key=$value

```

  

```ps

vault kv put secret/secretName key=value

// or old syntax

vault write secret/secretName key=value

```

  

### Get a secret from KV Secret Engine

```ps

vault kv get secret/secretName

// or old syntax

vault read secret/secretName

```

  

### Move a secret between KV Secret Engines

```ps

vault secrets move kvSecret1/secretName kvSecret2/secretName

```

  

### List all secrets in a Secret Engine

```ps

vault kv list secretEngineName

```

  

### List all Secrets Engines

```ps

vault secrets list

```

  

### Upgrade Secret Engine to v2 (v1 is default)

```ps

vault kv enable-versioning secret

```

### Create new KV Secret Engine

```ps

vault secrets enable -path=devkv kv

```

  

# Token

## Get information about current token

```ps

vault token lookup

```

  

# Userpass

## Login

```ps

vault login -method=userpass username=$userName

```

  

## Add a user

```ps

vault write auth/userpass/users/arthur password=dent

```

  

## List users

```ps

vault list auth/userpass/users

```

  

# Policies

## List all policies

```ps

vault policy list

```


## Ansible Vault commands

  

# Check Vault version

  

~~~~

vault --version

~~~~

  

# Adding credentials into vault

  

~~~~

for env in env1 env2 env 3;

do

vault kv patch \

-address "web_link_to_vault" \

-tls-skip-verify=true secret/path/to/$env

api_token_var="value1" \

api_token_var="value2" ;

done

~~~~



[[Index]] 