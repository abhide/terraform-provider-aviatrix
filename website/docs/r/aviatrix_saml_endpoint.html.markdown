---
layout: "aviatrix"
page_title: "Aviatrix: aviatrix_saml_endpoint"
description: |-
  Creates and manages an Aviatrix SAML Endpoint
---

# aviatrix_saml_endpoint

The **aviatrix_saml_endpoint** resource allows the creation and management of an Aviatrix SAML endpoint.

## Example Usage

```hcl
# Create an Aviatrix AWS SAML Endpoint
resource "aviatrix_saml_endpoint" "test_saml_endpoint" {
  endpoint_name     = "saml-test"
  idp_metadata_type = "Text"
  idp_metadata      = "${var.idp_metadata}"
}
```
```hcl
# Create an Aviatrix AWS SAML Endpoint for Controller Login
resource "aviatrix_saml_endpoint" "test_saml_endpoint" {
  endpoint_name     = "saml-test"
  idp_metadata_type = "Text"
  idp_metadata      = "${var.idp_metadata}"
  controller_login  = true
  access_set_by     = "controller"
  rbac_groups       = [
    "admin",
    "read_only",
  ]
}
```

## Argument Reference

The following arguments are supported:

### Required
* `endpoint_name` - (Required) The SAML endpoint name.
* `idp_metadata_type` - (Required) The IDP Metadata type. At the moment only "Text" is supported.
* `idp_metadata` - (Required) The IDP Metadata from SAML provider. Normally the metadata is in XML format which may contain special characters. Best practice is encode metadata in base64 and set here `${base64decode(var.idp_metadata)}`.

### Custom
* `custom_entity_id` - (Optional) Custom Entity ID. Required to be non-empty for 'Custom' Entity ID type, empty for 'Hostname' Entity ID type.
* `custom_saml_request_template` - (Optional) Custom SAML Request Template in string.

### Controller Login
* `controller_login` - (Optional) Valid values: true, false. Default value: false. Set true for creating a saml endpoint for controller login.
* `access_set_by` - (Optional) Access type. Valid values: "controller", "profile_attribute". Default value: "controller". 
* `rbac_groups` - (Optional) List of rbac groups. Required for controller login and "access_set_by" of "controller".

## Import

**saml_endpoint** can be imported using the SAML `endpoint_name`, e.g.

```
$ terraform import aviatrix_saml_endpoint.test saml-test
```
