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

Purpose                 | v2 call                 | v3 call                 | vNext call
------------------------|-------------------------|-------------------------|----------------------
Servers
Retrieve a project's servers | GET /{project}/servers | GET /{project}/servers | GET /projects/{project}/servers
Retrieve a server | GET /{project}/servers/{id} | GET /{project}/servers/{id} | GET /servers/{id}
