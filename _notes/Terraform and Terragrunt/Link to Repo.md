[[Link to Repo]]

https://github.com/obieze375/terraform-on-aws-ec2


### EMR Cluster

  

## source.hcl

  

``` 

  

terraform {

    source = "git::ssh//<repo>"

    }

  

locals { local variable declaration

  

       default_vpc

  

}

  

inputs {

  
  
  

}

  

``` 

  

## configuration.hcl

  

``` 

  

locals {}

  

``` 

  
  

## terragrunt.hcl

  

``` 

  

include "root" {

  path = find_in_parent_folders()

}

  
  

/* Looks for all the common files needed for terragrunt code to work by looking up the root hierachy*/

  

include "directory_name" {

  path           = "${dirname(find_in_parent_folders())}/directory/directory/source.hcl"

  merge_strategy = "deep"

}

  
  

/* Defines the values inputted into new terraform resource from terraform template pre-existing values can also be editing */

inputs = {

  vpc_name =

  kms_key_arn =

  /*KMS key encryption is added here for EBS volumes and S3 buckets aswell as TLS*/

  

  security_config_json = << EOF

  {

    "EncryptionConfiguration": {

        "EnableInTransitEncryption": true,

        "EnableAtRestEncryption": true,

        "InTransitEncryptionConfiguration": {

            "TLSCertificateConfiguration": {

                "CertificateProviderType": "PEM",

                "S3Object": "s3//<S3_bucket_obj_filepath>"

            }

        },

        "AtRestEncryptionConfiguration": {

            "S3EncryptionConfiguration": {

                "EncryptionMode": "SSE-KMS",

                "AwsKmsKey": "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012"

            },

            "LocalDiskEncryptionConfiguration": {

                "EncryptionKeyProviderType": "AwsKms",

                "AwsKmsKey": "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012"

            }

        }

    }

}

  

EOF

  
  
  
  
  
  

``` 

  

## KMS Inputs Block

  

``` 

  

kms_secrets = [

  

{

  kms_key_suffix_names = [  "kms_key" ]

  

  account_access_ids = ["arn:aws:iam::<role>"]

  admin_identifiers = ["arn:aws:iam::<role>"]

  persistent_resource_usage = true

    },

  

]

  

``` 

  
  
  
  

``` 

  

When the merge_strategy for the include block is set to deep, Terragrunt will perform a deep merge of the included config. For Terragrunt config, deep merge is defined as follows:

  

For simple types, the child overrides the parent.

For lists, the two attribute lists are combined together in concatenation.

For maps, the two maps are combined together recursively. That is, if the map keys overlap, then a deep merge is performed on the map value.

For blocks, if the label is the same, the two blocks are combined together recursively. Otherwise, the blocks are appended like a list. This is similar to maps, with block labels treated as keys.

``` 