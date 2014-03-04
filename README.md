# A Proposed Next Version of the OpenStack Compute API

This repository contains a single document (this one) describing a proposed new
OpenStack Compute API. I've used (http://apiblueprint.org/)[API Blueprint]
for formatting and describing the proposed API.

# Project [/projects/{id}]

A project is a grouping of related server resources.

# Server Type [/server\_types/{id}]

A server type describes the capacity and capabilities of a class of servers.

# Server Template [/server\_templates/{id}]

A server template is a base disk image, a bootable volume, or a snapshot of
a server instance that is used as the basis of a server when it is launched.

# Server [/servers/{id}]

A server is a virtual machine that runs in an OpenStack cloud. It is owned by
a single *project*, has a single *server type* that describes its capabilities
and capacity, and is launched from a *server template* that is a base disk
image, a bootable volume, or a snapshot of another server.

## Retrieve Server [GET]

Retrieve a server by its *id*.

+ Response 200 (application/json)

```json
{
    "block_devices": [
        {
            "id": "50795ce0-a357-11e3-a5e2-0800200c9a66",
            "type": "ephemeral",
            "device": "/dev/sda"
        },
        {
            "id": "b502f900-a357-11e3-a5e2-0800200c9a66",
            "type": "persistent",
            "device": "/dev/sdb"
        }
    ],
    "group_id": "bd0bf800-a356-11e3-a5e2-0800200c9a66",
    "hostname": "example-server-1",
    "id": "53626cb0-a34f-11e3-a5e2-0800200c9a66",
    "launched_at": "2014-03-02T23:20:19",
    "launched_by": "c54b5b30-a353-11e3-a5e2-0800200c9a66",
    "networking": {
        "access": [
            "65.6.29.230",
            "::babe:65.6.29.230"
        ],
        "interfaces": {
            "eth0": "10.0.1.4",
            "eth1": "65.6.29.230"
        }
    },
    "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
    "properties": {
        "role": "appserver",
    },
    "state": "ACTIVE",
    "tags": [
        "linux",
        "ubuntu"
    ],
    "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
    "type_id": "1593e080-a354-11e3-a5e2-0800200c9a66"
}
```

## Update Server [PATCH]

## Delete Server [DELETE]

## Create Server [PUT]

# Servers [/servers]

## Retrieve a list of Servers [GET]

## Create One or More Servers [POST]
