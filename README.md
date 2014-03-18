# Summary

This repository contains an [API Blueprint](http://apiblueprint.org/) document
(apiary.apib) describing a proposed new OpenStack Compute API.

Please see this link for formatted documentation about this new
proposed OpenStack Compute API vNext:

    http://docs.oscomputevnext.apiary.io/

# Gap Analysis

The following is a gap analysis between the v2/v3 Compute API and what appears
in the vNext proposed API. I put it here partly as a way to keep track of my
progress towards porting much of the v2/v3 API functionality and partly as a
way to see differences between them.

## API Call Comparison

### Servers

Purpose                 | v2/3 call               | vNext call
------------------------|-------------------------|----------------------
Retrieve a project's servers | `GET /{project}/servers` | `GET /projects/{project}/servers`
Retrieve a server | `GET /{project}/servers/{id} | `GET /servers/{id}`
Retrieve a server's details | `GET /{project}/servers/{id}/detail | N/A
Launch one or more servers | `POST /{project}/servers` | `POST /projects/{project/servers`
Update a server | `PUT /{project}/servers/{id}` | *TODO* `PATCH /servers/{id}`
Terminate a server | `DELETE /{project}/servers/{id}` | `POST /servers/{id}/tasks`

## Server Actions (Tasks in vNext)

Purpose                 | v2/3 call               | vNext call
------------------------|-------------------------|----------------------
Get a server's power/virt status | `GET /{project}/servers/{id}` | `HEAD /servers/{id}`
Get a server's task status | `GET /{project}/servers/{id}` | `GET /servers/{id}/tasks`
Update server admin password | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Reboot a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Rebuild a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Resize a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Confirm resize of a server | `POST /{project}/servers/{id}/action` | N/A **No separate confirmation step is necessary in the vNext API**
Revert resize of a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`

### Flavors (Server Types in vNext)

Purpose                 | v2/3 call               | vNext call
------------------------|-------------------------|----------------------
Retrieve a list of flavors | `GET /{project}/flavors` | `GET /server_types`
Retrieve a flavor | `GET /{project}/flavors/{id}` | `GET /server_types/{id}`
Create a new flavor | **Extension** `POST /{project}/flavors` | `POST /server_types`
List projects with access to a flavor | **Extension** `GET /{project}/flavors/{id}/os-flavor-access` | `GET /server_types/{id}` **A `share` property lists users or projects that can see the server type**
Add access to flavor | **Extension** `POST /{project}/flavors/{id}/action` | `POST|PATCH /server_types/{id}` **A `share` property lists users or projects that can see the server type** *TODO* `PATCH`
Delete access to flavor | **Extension** `DELETE /{project}/flavors/{id}/action` | `POST /server_types/{id}` **A `share` property lists users or projects that can see the server type**
