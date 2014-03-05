FORMAT: 1A

# OpenStack Compute API vNext

This repository contains a single document (this one) describing a proposed new
OpenStack Compute API. I've used [API Blueprint](http://apiblueprint.org/)
for formatting and describing the proposed API.

# OpenStack Compute API Root [/]

## GET /

Retrieve a JSON Home document that describes the OpenStack Compute API.

> **Note:** For information about JSON-Home, please see the
  [online specification](http://tools.ietf.org/html/draft-nottingham-json-home-03)

+ Response 200 (application/json-home)
    + Body

```json
{
    "resources": {
        "rel/project": {
            "href-template": "/projects/{project_id}",
            "href-vars": {
                "project_id": "param/project_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/server_type": {
            "href-template": "/server_types/{server_type_id}",
            "href-vars": {
                "server_type_id": "param/server_type_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/server_template": {
            "href-template": "/server_templates/{server_template_id}",
            "href-vars": {
                "server_template_id": "param/server_template_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/server_group": {
            "href-template": "/server_groups/{server_group_id}",
            "href-vars": {
                "server_group_id": "param/server_group_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/project_server_group": {
            "href-template": "/project/{project_id}/server_groups/{server_group_slug}",
            "href-vars": {
                "project_id": "param/project_id",
                "server_group_slug": "param/server_group_slug"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/server_task": {
            "href-template": "/server_tasks/{server_task_id}",
            "href-vars": {
                "server_task_id": "param/server_task_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/project_server_tasks": {
            "href-template": "/project/{project_id}/server_tasks",
            "href-vars": {
                "project_id": "param/project_id"
            },
            "hints": {
                "allow": ["GET", "POST"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/server": {
            "href-group": "/servers/{server_id}",
            "href-vars": {
                "server_id": "param/server_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/project_server": {
            "href-group": "/project/{project_id}/servers/{server_hostname}",
            "href-vars": {
                "project_id": "param/project_id",
                "server_hostname": "param/server_hostname"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        },
        "rel/project_servers": {
            "href-group": "/project/{project_id}/servers/",
            "href-vars": {
                "project_id": "param/project_id"
            },
            "hints": {
                "allow": ["GET"],
                "formats": {
                    "application/json": {}
                }
            }
        }
    }
}
```

# Project

A project is a grouping of related server resources.

+ Model (application/hal+json)

```json
{
    "_links": {
        "rel/project_servers": {
            "href": "/projects/a7728150-a34f-11e3-a5e2-0800200c9a66/servers"
        }
    }
}
```

## GET /projects/{id}

Retrieve links to project resource by a project's *id*.

+ Parameters
    + id (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.

+ Response 200 (application/json)
    
    [Project][]

# Server Type

A server type describes the capacity and capabilities of a class of servers.

+ Model (application/hal+json)

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
    ],
    "_links":  {
        "self": {
            "href": "/server_types/7a6aba30-a3c0-11e3-a5e2-0800200c9a66"
        }
    }
}
```

## GET /server\_types/{id}

Retrieve a server type by its *id*.

+ Parameters
    + id (string, `7a6aba30-a3c0-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server type

+ Response 200 (application/json)
    
    [Server Type][]

# Server Template

A server template is a base disk image, a bootable volume, or a snapshot of
a server instance that is used as the basis of a server when it is launched.

+ Model (application/hal+json)

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
    ],
    "_links":  {
        "self": {
            "href": "/server_templates/fe48b370-a352-11e3-a5e2-0800200c9a66"
        }
    }
}
```

## GET /server\_templates/{id}

Retrieve a server template by its *id*.

+ Parameters
    + id (string, `fe48b370-a352-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server template

+ Response 200 (application/json)
    
    [Server Template][]

# Server Group

A server group is a user-defined collection of servers that provides defaults
for servers launched in the group.

+ Model (application/hal+json)

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
    ],
    "_links":  {
        "self": {
            "href": "/server_groups/5f634509-ee40-4406-9c45-e5f343f01f62"
        }
    }
}
```

## GET /server\_groups/{id}

Retrieve a server group by its *id*.

+ Parameters
    + id (string, `5f634509-ee40-4406-9c45-e5f343f01f62`) ... A UUID identifier
      for the server group

+ Response 200 (application/json)
    
    [Server Group][]

## GET /projects/{project}/server\_groups/{slug}

Retrieve a server group within a *project* by its *slug*.

+ Parameters
    + project (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.
    + slug (string, `windows-app-servers`) ... A slugified name of the server group

+ Response 200 (application/json)
    
    [Server Group][]

# Server Task

A server task is a related set of actions that are or have been executed
against a server.

+ Model (application/hal+json)

```json
{
    "action": "BUILD_SERVER",
    "created_at": "2014-04-14T02:15:15Z",
    "created_by": "c54b5b30-a353-11e3-a5e2-0800200c9a66",
    "id": "85fc465a-8809-4b7a-bce2-4c6ff5b78763",
    "server_id": "53626cb0-a34f-11e3-a5e2-0800200c9a66",
    "state": "RUNNING",
    "_links":  {
        "self": {
            "href": "/server_tasks/53626cb0-a34f-11e3-a5e2-0800200c9a66"
        },
        "rel/server_task_subtasks": {
            "href": "/server_tasks/53626cb0-a34f-11e3-a5e2-0800200c9a66/subtasks"
        }
    }
}
```

# Server Tasks

A collection of server tasks.

+ Model (application/hal+json)

```json
{
    "tasks": [
        {
            "action": "BUILD_SERVER",
            "created_at": "2014-04-14T02:15:15Z",
            "created_by": "c54b5b30-a353-11e3-a5e2-0800200c9a66",
            "id": "85fc465a-8809-4b7a-bce2-4c6ff5b78763",
            "server_id": "53626cb0-a34f-11e3-a5e2-0800200c9a66",
            "state": "RUNNING",
        },
        {
            "action": "RESIZE_SERVER",
            "created_at": "2013-12-23T10:02:42Z",
            "created_by": "c54b5b30-a353-11e3-a5e2-0800200c9a66",
            "id": "3759ef44-2b7d-4981-b286-418ca50fe005",
            "server_id": "53626cb0-a34f-11e3-a5e2-0800200c9a66",
            "state": "ABORTED",
        }
    ],
    "_links": {
        "self": {
            "href": "/servers/53626cb0-a34f-11e3-a5e2-0800200c9a66/tasks?limit=2"
        },
        "next": {
            "href": "/project/a7728150-a34f-11e3-a5e2-0800200c9a66/tasks?limit=2&marker=3759ef44-2b7d-4981-b286-418ca50fe005"
        }
    }
}
```

## GET /server\_tasks/{id}

Retrieve a server task by its *id*.

+ Parameters
    + id (string, `85fc465a-8809-4b7a-bce2-4c6ff5b78763`) ... A UUID identifier
      for the server task

+ Response 200 (application/json)
    
    [Server Task][]

# Server

A server is a virtual machine that runs in an OpenStack cloud. It is owned by
a single *project*, has a single *server type* that describes its capabilities
and capacity, and is launched from a *server template* that is a base disk
image, a bootable volume, or a snapshot of another server.

The server is the main resource exposed by the OpenStack Compute API. There
are a number of subresources and collections of subresources that are
attached to a server resource. This group describes operations on the server
and these subresources.

+ Model (application/hal+json)

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
    "type_id": "1593e080-a354-11e3-a5e2-0800200c9a66",
    "_links": {
        "self": {
            "href": "/servers/53626cb0-a34f-11e3-a5e2-0800200c9a66"
        },
        "rel/server_tasks": {
            "href": "/servers/53626cb0-a34f-11e3-a5e2-0800200c9a66/tasks"
        }
    }
}
```

## GET /servers/{id}

Retrieve a server by its *id*.

+ Parameters
    + id (string, `53626cb0-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server

+ Response 200 (application/json)
    
    [Server][]

## GET /projects/{project}/servers/{hostname}

Retrieve a server by its *project* and *hostname*.

+ Parameters
    + project (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.
    + hostname (string, `app-server-1`) ... A hostname of a server in the project.

+ Response 200 (application/json)

    [Server][]

### PATCH /servers/{id}

### DELETE /servers/{id}

### PUT /servers/{id}

### GET /servers/{server}/tasks{?limit,marker}

Retrieve a collection of servers in a specific *project*.

+ Parameters
    + server (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.
    + limit = `20` (optional, number) ... Maximum number of results to return
    + marker (optional, string, `3699f74d-af95-406d-b38e-d2b86f84a9d0`) ... A UUID
      identifier of the last record on the previous page of results.

+ Response 200 (application/json)

    [Server Tasks][]

### POST /servers/{server}/tasks

Starts a task against a *server*.

+ Parameters
    + server (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the server.

+ Request (application/json)
    + Body

```json
{
    "action": "BUILD_SERVER",
    "timeout": 120
}
```

+ Response 202 (application/json)

    [Server Task][]

## Servers 

A collection of servers.

+ Model (application/hal+json)

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
    "_links": {
        "self": {
            "href": "/project/a7728150-a34f-11e3-a5e2-0800200c9a66/servers?limit=2"
        },
        "next": {
            "href": "/project/a7728150-a34f-11e3-a5e2-0800200c9a66/servers?limit=2&marker=3699f74d-af95-406d-b38e-d2b86f84a9d0"
        }
    }
}
```

### GET /project/{project}/servers{?limit,marker}

Retrieve a collection of servers in a specific *project*.

+ Parameters
    + project (string, `a7728150-a34f-11e3-a5e2-0800200c9a66`) ... A UUID identifier
      for the project.
    + limit = `20` (optional, number) ... Maximum number of results to return
    + marker (optional, string, `3699f74d-af95-406d-b38e-d2b86f84a9d0`) ... A UUID
      identifier of the last record on the previous page of results.

+ Response 200 (application/json)

    [Servers][]

### POST /project/{project}/servers
