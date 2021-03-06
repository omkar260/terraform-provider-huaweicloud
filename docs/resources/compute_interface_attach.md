---
subcategory: "Elastic Cloud Server (ECS)"
---

# huaweicloud\_compute\_interface\_attach

Attaches a Network Interface to an Instance.
This is an alternative to `huaweicloud_compute_interface_attach_v2`

## Example Usage

### Basic Attachment

```hcl
data "huaweicloud_vpc_subnet" "mynet" {
  name = "subnet-default"
}

resource "huaweicloud_compute_instance" "myinstance" {
  name              = "instance"
  image_id          = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  flavor_id         = "s6.small.1"
  key_pair          = "my_key_pair_name"
  security_groups   = ["default"]
  availability_zone = "cn-north-4a"

  network {
    uuid = "55534eaa-533a-419d-9b40-ec427ea7195a"
  }
}

resource "huaweicloud_compute_interface_attach" "attached" {
  instance_id = huaweicloud_compute_instance.myinstance.id
  network_id  = data.huaweicloud_vpc_subnet.mynet.id
}
```

### Attachment Specifying a Fixed IP

```hcl
data "huaweicloud_vpc_subnet" "mynet" {
  name = "subnet-default"
}

resource "huaweicloud_compute_instance" "myinstance" {
  name              = "instance"
  image_id          = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  flavor_id         = "s6.small.1"
  key_pair          = "my_key_pair_name"
  security_groups   = ["default"]
  availability_zone = "cn-north-4a"

  network {
    uuid = "55534eaa-533a-419d-9b40-ec427ea7195a"
  }
}

resource "huaweicloud_compute_interface_attach" "attached" {
  instance_id = huaweicloud_compute_instance.myinstance.id
  network_id  = data.huaweicloud_vpc_subnet.mynet.id
  fixed_ip    = "10.0.10.10"
}

```

### Attachment Using an Existing Port

```hcl
data "huaweicloud_vpc_subnet" "mynet" {
  name = "subnet-default"
}

resource "huaweicloud_networking_port" "myport" {
  name           = "port"
  network_id     = data.huaweicloud_vpc_subnet.mynet.id
  admin_state_up = "true"
}

resource "huaweicloud_compute_instance" "myinstance" {
  name              = "instance"
  image_id          = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  flavor_id         = "s6.small.1"
  key_pair          = "my_key_pair_name"
  security_groups   = ["default"]
  availability_zone = "cn-north-4a"

  network {
    uuid = "55534eaa-533a-419d-9b40-ec427ea7195a"
  }
}

resource "huaweicloud_compute_interface_attach" "attached" {
  instance_id = huaweicloud_compute_instance.myinstance.id
  port_id     = huaweicloud_networking_port.myport.id
}

```

## Argument Reference

The following arguments are supported:

* `instance_id` - (Required) The ID of the Instance to attach the Port or Network to.

* `port_id` - (Optional) The ID of the Port to attach to an Instance.
   _NOTE_: This option and `network_id` are mutually exclusive.

* `network_id` - (Optional) The ID of the Network to attach to an Instance. A port will be created automatically.
   _NOTE_: This option and `port_id` are mutually exclusive.

* `fixed_ip` - (Optional) An IP address to assosciate with the port.
   _NOTE_: This option cannot be used with port_id. You must specifiy a network_id. The IP address must lie in a range on the supplied network.

## Attributes Reference

The following attributes are exported:

* `instance_id` - See Argument Reference above.
* `port_id` - See Argument Reference above.
* `network_id` - See Argument Reference above.
* `fixed_ip`  - See Argument Reference above.

## Import

Interface Attachments can be imported using the Instance ID and Port ID
separated by a slash, e.g.

```
$ terraform import huaweicloud_compute_interface_attach.ai_1 89c60255-9bd6-460c-822a-e2b959ede9d2/45670584-225f-46c3-b33e-6707b589b666
```
