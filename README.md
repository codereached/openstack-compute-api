# A Proposed Next Version of the OpenStack Compute API

This repository contains a single document (this one) describing a proposed new
OpenStack Compute API. I've used [API Blueprint](http://apiblueprint.org/)
for formatting and describing the proposed API.

# Project [/projects/{id}]

A project is a grouping of related server resources.

## Retrieve Project [GET]

Retrieve links to project resource by a project's *id*.

+ Response 200 (application/json)

```json
{
    "servers": "/projects/a7728150-a34f-11e3-a5e2-0800200c9a66/servers",
}
```

# Server Type [/server\_types/{id}]

A server type describes the capacity and capabilities of a class of servers.

## Retrieve Server Type [GET]

Retrieve a server type by its *id*.

+ Response 200 (application/json)

```json
{
    "description": "General purpose low-CPU, low-memory, "
                   "small-root-disk type of server.",
    "id": "7a6aba30-a3c0-11e3-a5e2-0800200c9a66",
    "name": "m1.micro",
    "size": {
        "memory_mb": 128,
        "root_disk_gb": 8,
        "cpu_units": 1
    },
    "tags": [
        "general-purpose"
    ]
}
```

# Server Template [/server\_templates/{id}]

A server template is a base disk image, a bootable volume, or a snapshot of
a server instance that is used as the basis of a server when it is launched.

## Retrieve Server Template [GET]

Retrieve a server template by its *id*.

+ Response 200 (application/json)

```json
{
    "description": "Windows Server 2008 R2 disk image",
    "id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
    "requirements": {
        "min_disk_gb": 80,
        "min_memory_mb": 2048,
        "min_cpu_units": 2
    },
    "tags": [
        "windows"
    ]
}
```

# Server Group [/server\_groups/{id}]

A server group is a user-defined collection of servers that provides defaults
for servers launched in the group.

## Retrieve Server Group [GET]

Retrieve a server group by its *id*.

+ Response 200 (application/json)

```json
{
    "description": "Application servers running Windows Server 2008 R2",
    "defaults": {
        "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
        "type_id": "96d639b0-a3ca-11e3-a5e2-0800200c9a66",
        "hostname_pattern": "win-app-%(rand_num)5d"
    },
    "name": "Windows App servers",
    "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
    "slug": "windows-app-servers",
    "tags": [
        "windows"
    ]
}
```

# Server Group (Variation) [/projects/{project}/server\_groups/{slug}]

An alternate way for a project to access one of its server groups using a slug
instead of an ID value.

## Retrieve Server Group (Variant) [GET]

Retrieve a server group within a *project* by its *slug*.

+ Response 200 (application/json)

```json
{
    "description": "Application servers running Windows Server 2008 R2",
    "defaults": {
        "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
        "type_id": "96d639b0-a3ca-11e3-a5e2-0800200c9a66",
        "hostname_pattern": "win-app-%(rand_num)5d"
    },
    "name": "Windows App servers",
    "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
    "slug": "windows-app-servers",
    "tags": [
        "windows"
    ]
}
```

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

# Project Servers [project/{project\_id}/servers]

## Retrieve a list of Servers [GET]

## Create One or More Servers [POST]

# Server (Variation) [/projects/{project}/servers/{hostname}]

An alternate way for a project to access one of its servers using a hostname
instead of an ID value.

## Retrieve Server (Variat) [GET]

Retrieve a server by its *project* and *hostname*.

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
