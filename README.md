# A Proposed Next Version of the OpenStack Compute API

This repository contains a single document (this one) describing a proposed new
OpenStack Compute API. I've used [API Blueprint](http://apiblueprint.org/)
for formatting and describing the proposed API.

# Project [/projects/{id}]

A project is a grouping of related server resources.

+ Parameters
    + id (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.

## Retrieve Project [GET]

Retrieve links to project resource by a project's *id*.

+ Response 200 (application/json)
    + Body
    ```json
    {
        "servers": "/projects/a7728150-a34f-11e3-a5e2-0800200c9a66/servers",
    }
    ```

# Server Type [/server\_types/{id}]

A server type describes the capacity and capabilities of a class of servers.

+ Parameters
    + id (string, `7a6aba30-a3c0-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server type

## Retrieve Server Type [GET]

Retrieve a server type by its *id*.

+ Response 200 (application/json)
    + Body
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

+ Parameters
    + id (string, `fe48b370-a352-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server template

## Retrieve Server Template [GET]

Retrieve a server template by its *id*.

+ Response 200 (application/json)
    + Body
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

+ Parameters
    + id (string, `5f634509-ee40-4406-9c45-e5f343f01f62`) ... A UUID identifier
      for the server group

## Retrieve Server Group [GET]

Retrieve a server group by its *id*.

+ Response 200 (application/json)
    + Body
    ```json
    {
        "description": "Application servers running Windows Server 2008 R2",
        "defaults": {
            "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
            "type_id": "96d639b0-a3ca-11e3-a5e2-0800200c9a66",
            "hostname_pattern": "win-app-%(rand_num)5d"
        },
        "id": "5f634509-ee40-4406-9c45-e5f343f01f62",
        "name": "Windows App servers",
        "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
        "slug": "windows-app-servers",
        "tags": [
            "windows"
        ]
    }
    ```

# Server Group (Variant) [/projects/{project}/server\_groups/{slug}]

An alternate way for a project to access one of its server groups using a slug
instead of an ID value.

+ Parameters
    + project (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.
    + slug (string, `windows-app-servers`) ... A slugified name of the server group

## Retrieve Server Group (Variant) [GET]

Retrieve a server group within a *project* by its *slug*.

+ Response 200 (application/json)
    + Body
    ```json
    {
        "description": "Application servers running Windows Server 2008 R2",
        "defaults": {
            "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
            "type_id": "96d639b0-a3ca-11e3-a5e2-0800200c9a66",
            "hostname_pattern": "win-app-%(rand_num)5d"
        },
        "id": "5f634509-ee40-4406-9c45-e5f343f01f62",
        "name": "Windows App servers",
        "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
        "slug": "windows-app-servers",
        "tags": [
            "windows"
        ]
    }
    ```

# Group Servers

The server is the main resource exposed by the OpenStack Compute API. There
are a number of subresources and collections of subresources that are
attached to a server resource. This group describes operations on the server
and these subresources.

## Server [/servers/{id}]

A server is a virtual machine that runs in an OpenStack cloud. It is owned by
a single *project*, has a single *server type* that describes its capabilities
and capacity, and is launched from a *server template* that is a base disk
image, a bootable volume, or a snapshot of another server.

+ Parameters
    + id (string, `53626cb0-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server

### Retrieve Server [GET]

Retrieve a server by its *id*.

+ Response 200 (application/json)
    + Body
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

### Update Server [PATCH]

### Delete Server [DELETE]

### Create Server [PUT]

## Server (Variation) [/projects/{project}/servers/{hostname}]

An alternate way for a project to access one of its servers using a hostname
instead of an ID value.

+ Parameters
    + project (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.
    + hostname (string, `app-server-1`) ... A hostname of a server in the project.

### Retrieve Server (Variant) [GET]

Retrieve a server by its *project* and *hostname*.

+ Response 200 (application/json)
    + Body
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

## Servers [project/{project\_id}/servers{?limit}]

A collection of servers in a specific *project*.

### Retrieve a list of Servers [GET]

+ Parameters
    + limit = `20` (optional, number) ... Maximum number of results to return

+ Response 200 (application/json)
    + Body
    ```json
    {
        "servers": [
            {
                "group_id": "bd0bf800-a356-11e3-a5e2-0800200c9a66",
                "hostname": "example-server-1",
                "id": "53626cb0-a34f-11e3-a5e2-0800200c9a66",
                "launched_at": "2014-03-02T23:20:19",
                "launched_by": "c54b5b30-a353-11e3-a5e2-0800200c9a66",
                "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
                "state": "ACTIVE",
                "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
                "type_id": "1593e080-a354-11e3-a5e2-0800200c9a66"
            },
            {
                "group_id": "bd0bf800-a356-11e3-a5e2-0800200c9a66",
                "hostname": "example-server-2",
                "id": "3699f74d-af95-406d-b38e-d2b86f84a9d0"
                "launched_at": "2014-03-03T03:20:19",
                "launched_by": "c54b5b30-a353-11e3-a5e2-0800200c9a66",
                "project_id": "a7728150-a34f-11e3-a5e2-0800200c9a66",
                "state": "ACTIVE",
                "template_id": "fe48b370-a352-11e3-a5e2-0800200c9a66",
                "type_id": "1593e080-a354-11e3-a5e2-0800200c9a66"
            }
        ],
        "links": {
            "self": "/project/a7728150-a34f-11e3-a5e2-0800200c9a66/servers?limit=2",
            "next": "/project/a7728150-a34f-11e3-a5e2-0800200c9a66/servers?limit=2&marker=3699f74d-af95-406d-b38e-d2b86f84a9d0"
        }
    }
    ```

### Create One or More Servers [POST]
