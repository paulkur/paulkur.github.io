---
title: Terraform setup
date: 2023-05-14 14:59:00 +0100
categories: [Terraform,Zorro,setup]
tags: [terraform,code,docs,zorro,setup]     # TAG names should always be lowercase
pin: true
---

## Links

- [Links](#links)
  - [Terraform Init](#terraform-init)

Useful links👇

- [Terraform Providers](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [GitLab project](https://gitlab.com/paulkurpis/terraform-learn/-/tree/main)

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

Other commands

```bash
terraform destroy
```

```bash
terraform destroy -target <target_name>
```

```bash
terraform apply -var-file
```

```bash
terraform state list
```

```bash
terraform state show
```

[Back to Top](#links)

