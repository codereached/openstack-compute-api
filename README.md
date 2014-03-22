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
Retrieve a server | `GET /{project}/servers/{id}` | `GET /servers/{id}`
Retrieve a server's details | `GET /{project}/servers/{id}/detail` | N/A
Launch one or more servers | `POST /{project}/servers` | `POST /projects/{project/servers`
Update a server | `PUT /{project}/servers/{id}` | *TODO:* `PATCH /servers/{id}`
Terminate a server | `DELETE /{project}/servers/{id}` | `POST /servers/{id}/tasks`
Get a server's diagnostics | `GET /{project}/servers/{id}` ***Extension*** | *TODO:* `GET /servers/{id}/diagnostics`

## Server Actions (Tasks in vNext)

Purpose                 | v2/3 call               | vNext call
------------------------|-------------------------|----------------------
Get a server's power/virt status | `GET /{project}/servers/{id}` | `HEAD /servers/{id}`
Get a server's task status | `GET /{project}/servers/{id}` | `GET /servers/{id}/tasks`
Update server admin password | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Reboot a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Rebuild a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Resize a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Confirm resize of a server | `POST /{project}/servers/{id}/action` | **N/A** *(No separate confirmation step is necessary in the vNext API)*
Revert resize of a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Create a snapshot (image) of a server | `POST /{project}/servers/{id}/action` | `POST /servers/{id}/tasks`
Stop a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Start a (stopped) server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Pause a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Unpause a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Suspend a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Resume a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Migrate a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks` *(Note: vNext task action is called `MOVE`)*
Reset networking on a server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A**
Inject networking info into a server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A**
Lock a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Unlock a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`
Live migrate a server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks` *(Note: vNext task action is called `MOVE`)*
Create a server backup | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A** *(Note: This is what Heat is for.)*
Reset server state | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A**
Evacuate server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A** *(Note: This is an operator-level action)*
Add security group to a server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A** *(Note: This is what Neutron is for.)*
Remove security group to a server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A** *(Note: This is what Neutron is for.)*
Add floating IP address to a server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A** *(Note: This is what Neutron is for.)*
View the server's console output | `POST /{project}/servers/{id}/action` ***Extension*** | `GET /servers/{id}/console_log`
Get a URI to grab a console for a server | `POST /{project}/servers/{id}/action` ***Extension*** | `GET /servers/{id}/consoles/{type}`
Force delete a server | `POST /{project}/servers/{id}/action` ***Extension*** | **N/A** *(Note: The archival of terminated servers is an operator-level action)*
Restore a deleted server | `POST /{project}/servers/{id}/action` ***Extension*** | `POST /servers/{id}/tasks`

### Flavors (Server Types in vNext)

Purpose                 | v2/3 call               | vNext call
------------------------|-------------------------|----------------------
Retrieve a list of flavors | `GET /{project}/flavors` | `GET /server_types`
Retrieve a flavor | `GET /{project}/flavors/{id}` | `GET /server_types/{id}`
Create a new flavor | `POST /{project}/flavors` ***Extension*** | `POST /server_types`
Update a flavor | **N/A** | `PATCH /server_types/{id}`
Delete a flavor | `DELETE /{project}/flavors/{id}` ***Extension*** | *TODO:* `DELETE /server_types/{id}`
List projects with access to a flavor | `GET /{project}/flavors/{id}/os-flavor-access`  ***Extension*** | `GET /server_types/{id}` *(Note: A `share` property lists users or projects that can see the server type)*
Add access to flavor | `POST /{project}/flavors/{id}/action`  ***Extension*** | `POST /server_types/{id}` *(Note: A `share` property lists users or projects that can see the server type)* *TODO:* `PATCH /server_types/{id}` for changing shares
Delete access to flavor | `DELETE /{project}/flavors/{id}/action`  ***Extension*** | `POST /server_types/{id}` *(Note: A `share` property lists users or projects that can see the server type)*
List a flavor's extra specs | `GET /{project}/flavors/{id}/os-extra-specs` ***Extension*** | **N/A** *(Note: vNext server types do not have key/value pairs, but do have tags)*

## Operator API Calls

It won't take long for the reader to notice that the proposed vNext Compute API
has no API calls that are intended for operators or deployers of clouds. For
example, all of these API extensions in the v2/v3 API have no corresponding
API call in the proposed vNext API:

 * `/os-hosts`: Bare metal compute nodes that run OpenStack services like nova-compute or nova-network
 * `/os-cells`: Semi-autonomous Nova installations that share certain database and message queue services
 * `/os-aggregates`: Groups of compute nodes that advertise a particular hypervisor or capabilities
 * `/os-agents`: Guest agents that live in server instances for use by Nova
 * `/os-baremetal-nodes`: Bare metal compute nodes that are treated like regular cloud resources for users
 * `/os-hypervisors`: The hypervisors themselves
 * `/os-services`: OpenStack service endpoints

Does the fact that the proposed vNext Compute API lacks the above resources and
calls mean that those resources are not important? No, of course not. It's just
that the above resources and calls are not something that we feel should be exposed
in the same public Compute API that is used by regular and elevated (administrative)
users of cloud resources.

Should there be a separate RESTful "Operators Compute API" that contains some or all of the
above? Perhaps. Or perhaps not. But we don't feel that these calls belong in the
same API as the one used by consumers/users of cloud resources.

Maybe an entirely separate Operator API -- possibly one based on something other than
HTTP -- would be A Good Thing for the long-term modularity and architectural soundness
of Nova (and other OpenStack projects). Separating out operator-level interfaces from
consumer-level interfaces may force some needed cleanup and clarity of internal
Nova programming interfaces, allowing an operator-level API to work directly with
more internal interfaces. Something to think about...
