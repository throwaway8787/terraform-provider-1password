# Terraform OnePassword Provider

[![GolangCI](https://golangci.com/badges/github.com/anasinnyk/terraform-provider-1password.svg)](https://golangci.com/r/github.com/anasinnyk/terraform-provider-1password)
[![Build Status](https://travis-ci.com/anasinnyk/terraform-provider-1password.svg?branch=master)](https://travis-ci.com/anasinnyk/terraform-provider-1password)

## Table of Contents

[Provider](#Provider)

* [onepassword_vault](#onepassword_vault)
* [onepassword_item_common](#onepassword_item_common)
* [onepassword_item_document](#onepassword_item_document)
* [onepassword_item_identity](#onepassword_item_identity)
* [onepassword_item_login](#onepassword_item_login)
* [onepassword_item_password](#onepassword_item_password)
* [onepassword_item_secure_note](#onepassword_item_secure_note)
* [onepassword_item_software_license](#onepassword_item_software_license)
* [onepassword_item_credit_card](#onepassword_item_credit_card)

## Provider

Terraform provider for 1password usage with your infrastructure, for example you can share password from your admin panel via some vault in you 1password company account. This provider based on 1Password CLI client version 0.5.5, but you can rewrite it by env variable `OP_VERSION`

### Example Usage

```hcl
provider "onepassword" {
    email      = "john.smith@example.com"
    password   = "super secret master password"
    secret_key = "A3-XXXXXX-XXXXXXX-XXXXX-XXXXX-XXXXX-XXXXX"
    sumdomain  = "company"
}
```

### Argument Reference

The following arguments are supported:

* `email` - (Required) your email address in 1password or via env variable `OP_EMAIL`.
* `password` - (Required) your master password from 1password or via env variable `OP_PASSWORD`.
* `secret_key` - (Required) secret key which you can download after registration or via env variable `OP_SECRET_KEY`.
* `subdomain` - (Optional) If you use corporate account you must fill subdomain form your 1password site. Defaults to `my` or via env variable `OP_SUBDOMAIN`.

## onepassword_vault

This resource create/load vault in your 1password account.

### Example Usage

#### Resource

```hcl
resource "onepassword_vault" "this" {
    name = "new-vault"
}
```

#### Data Source

```hcl
data "onepassword_vault" "this" {
    name = "exist-vault"
}
```

### Argument Reference

* `name` - (Required) vault name.

### Attribute Reference

* `id` - (Required) vault id.
Note: all arguments.

### onepassword_item_common

This resource can create/load any other item without required fields like Database/Membership/Wireless Router/Driver License/Outdoor License/Passport/Email Account/Reward Program/Social Security Number/Bank Account/Server in your 1password account.

### Example Usage

#### Resource

```hcl
resource "onepassword_item_common" "this" {
  name  = "Coupone"
  vault = "${var.vault_id}"

  template = "Reward Program"

  section = {
    field = {
      name   = "company name"
      string = "MacPaw"
    }

    field = {
      name   = "member name"
      string = "anasinnyk"
    }

    field = {
      name   = "member ID"
      string = "123"
    }

    field = {
      name      = "PIN"
      concealed = "123456qQ"
    }
  }

  section = {
    name = "More Information"

    field = {
      name   = "member ID (additional)"
      string = "321"
    }

    field = {
      name  = "customer service phone"
      phone = "+38 (000) 000 0000"
    }

    field = {
      name  = "phone for reserva​tions"
      phone = "+38 (000) 000 0000"
    }

    field = {
      name = "website"
      url  = "https://groupon.com"
    }

    field = {
      name       = "member since"
      month_year = 201903
    }
  }
}
```

#### Data Source

```hcl
resource "onepassword_item_common" "this" {
    name = "some-element-from-vault"
}
```

### Argument Reference

* `name` - (Required) your item title.
* `template` - (Required) your item categoty. Can be one of the next value `Database`, `Membership`, `Wireless Router`, `Driver License`, `Outdoor License`, `Passport`, `Email Account`, `Reward Program`, `Social Security Number`, `Bank Account`, `Server`.
* `vault` - (Optional) link to your vault, can be id (recommended) or name. If it's empty, it creates to default vault.
* `section` - (Optional) it's a block with additional information available in any other item type.

The `section` block support:

* `name` - (Optional) section title.
* `field` - (Optional) field in section.

The `field` block support:

* `name` - (Optional) field title.
* `string` - (Optional) if you have a text field use string.
* `url` - (Optional) if you have a URL field type (checks if URL is correct).
* `phone` - (Optional) if you have a phone number filed type.
* `email` - (Optional) if you have a email field type.
* `date` - (Optional) if you have a date field type should use a UNIXTIME.
* `month_year` - (Optional) if you have a month year field type, credit card expiration for example, use 6 number in next format `YYYYMM`.
* `totp` - (Optional) if you have a one time password you can save url in this type and 1password client can generate totp for you.
* `concealed` - (Optional) if you have a sensitive infromation, you can save it in this field type, it looks like a password.
* `sex` - (Optional) text field with information about geander, possible next vaules `male`,`female`.
* `card_type` - (Optional) text field with information about credit card type, possible next vaules `mc`, `visa`, `amex`, `diners`, `carteblanche`, `discover`, `jcb`, `maestro`, `visaelectron`, `laser`, `unionpay`.
* `reference` - (Optional) not supported yet. Potentially we can store reference between different items.
* `address` - (Optional) it a address block.

Note: MUST be one of there `string`,`url`,`phone`,`email`,`date`,`month_year`,`totp`,`concealed`,`address`,`sex`,`card_type`,`reference`.

The `address` block support:

* `street` - (Optional) street information.
* `counrty` - (Optional) ISO2 country code.
* `state` - (Optional) state name.
* `region` - (Optional) region name.
* `city` - (Optional) city name.
* `zip` - (Optional) zip code.

### Attribute Reference

* `id` - (Required) item id.
Note: all arguments.

### onepassword_item_document

### onepassword_item_identity

### onepassword_item_login

### onepassword_item_password

### onepassword_item_secure_note

### onepassword_item_software_license

### onepassword_item_credit_card
