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

## v2/v3 API Core and Extensions Compared to vNext

### Servers

Purpose                 | v2 call                 | v3 call                 | vNext call
------------------------|-------------------------|-------------------------|----------------------
Retrieve a project's servers | GET /{project}/servers | GET /{project}/servers | GET /projects/{project}/servers
Retrieve a server | GET /{project}/servers/{id} | GET /{project}/servers/{id} | GET /servers/{id}
Get a server's status | n/a (use full GET) | n/a (use full GET) | HEAD /servers/{id}
Launch a server | POST /{project}/servers | POST /{project}/servers | POST /projects/{project/servers
Launch multiple servers | n/a | POST /{project}/servers | POST /projects/{project/servers
Terminate a server | DELETE /{project}/servers/{id} | DELETE /{project}/servers/{id} | POST /servers/{id}/tasks
Reboot a server | POST /{project}/servers/{id}/action | POST /{project}/servers/{id}/action | POST /servers/{id}/tasks
Rebuild a server | POST /{project}/servers/{id}/action | POST /{project}/servers/{id}/action | POST /servers/{id}/tasks
Resize a server | POST /{project}/servers/{id}/action | POST /{project}/servers/{id}/action | POST /servers/{id}/tasks
