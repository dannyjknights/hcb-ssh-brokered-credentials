# Demonstrate using HashiCorp Boundary native credential store to provide injected application credentials using static SSH keys.

![HashiCorp Boundary Logo](https://www.hashicorp.com/_next/static/media/colorwhite.997fcaf9.svg)

## Overview

This repo demonstrates how to levearge Boundary's built-in credential store and library to offer injected application credentials to an EC2 target, but by using a static SSH key. For organisations, there may be valid reasons why some resources need to have a long-lived static credential. With this, there is an inherit security risk if that long-lived credential needs to be passed around to multiple users to use. When we store this within Boundary, it is secure and can be injected into the SSH sessino without the end users having any visiblity or knowledge that they are connected securely.

## SSH Credential Injection using HCP Boundary's credential library

This repo does the following:

1. Configure HCP Boundary.
2. Deploy a Boundary Worker in a public network.
3. Establish a connection between the Boundary Controller and the Boundary Worker.
4. Deploy a server instance in a public subnet
5. Configure Boundary to allow access to resources in the public network.

NOTE: 
> The fact that this repo has a server resource residing in an public subnet and therefore having a public IP attached is not supposed to mimic a production environment. This is purely to demonstrate the integration between Boundary and Vault.

IMPORTANT:
> The `credential-library.tf` file references a `boundary.pem` file within the `boundary_credential_ssh_private_key` resource. This was the name of the `.pem` I created for this example and you will need to create your own file when running this code.

Your HCP Boundary needs to be created prior to executing the Terraform code. For people new to HCP, a trial can be utilised, which will give $50 credit to try, which is ample to test this solution.

## tfvars Variables

The following tfvars variables have been defined in a terraform.tfvars file.

- `boundary_addr`: The HCP Boundary address, e.g. "https://xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.boundary.hashicorp.
cloud"
- `auth_method_id`: "ampw_xxxxxxxxxx"                            
- `password_auth_method_login_name`: = "loginname"
- `password_auth_method_password`:   = "loginpassword"
- `aws_vpc_cidr`:                    = "172.x.x.x/16"
- `aws_subnet_cidr`:                 = "172.31.x.x/24"
- `aws_subnet_cidr2`:                = "172.31.x.x/24"
- `aws_access`:                      = ""
- `aws_secret`:                      = ""
- `vault_addr`:                      = "https://vault-cluster-address.hashicorp.cloud:8200"
- `vault_token`:                     = "hvs.xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"