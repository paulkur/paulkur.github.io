---
title: Terraform learn
date: 2023-05-28 10:59:00 +0100
categories: [Terraform,Zorro,setup]
tags: [terraform,code,docs,zorro,setup]     # TAG names should always be lowercase
pin: true
---

## Links

- [Links](#links)
  - [Terraform Init](#terraform-init)
  - [Different ways using `vars`](#different-ways-using-vars)

Useful links👇

- [Terraform Providers](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [GitLab project](https://gitlab.com/paulkurpis/terraform-learn/-/tree/main)

When you want to run any script (example CSVtoHistory) you need to move file from folder to `Strategy` folder, then will see it in Zorro's dropdown list.

### Terraform Init

set terminal where is your project: `cd terraform/main.tf`

```bash
terraform init
```

```bash
terraform plan
```

```bash
terraform apply -auto-approve
```

- Other useful commands

```bash
terraform destroy
terraform apply -var-file
terraform destroy -target <target_name>
terraform state list
terraform state show
```

### Different ways using `vars`

```yml
variable "prox_vars" {
  description = "proxmox API connection"
  type = list(object({
    name = string 
    api_url = string
    api_token_id = string
    api_token_secret = string
  }))
}

provider "proxmox" {
  pm_api_url = var.prox_vars[0].api_url
  pm_api_token_id = var.prox_vars[0].api_token_id
  pm_api_token_secret = var.prox_vars[0].api_token_secret
  pm_tls_insecure = true
}
```

- `.tfvars` structure

```yml
prox_vars = [
    {name = "server-1",
    api_url = "https://YOUR_IP_ADDRESS:8006/api2/json", 
    api_token_id = "terraform-prov@pam!terraform",
    api_token_secret = "SECRET_DIGITS"},
    {name = "server-2",
    api_url = "https://YOUR_IP_ADDRESS:8007/api2/json", 
    api_token_id = "terraform-prov@pam!terraform",
    api_token_secret = "SECRET_DIGITS"}
]
```

[Back to Top](#links)
