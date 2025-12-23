# dependabot-core-issue-11215

Public repository to reproduce this issue: https://github.com/dependabot/dependabot-core/issues/11215 --> A fix is in progress there: https://github.com/dependabot/dependabot-core/pull/13842.

Seems to be happening with a combination of:
- Multi-stage build,
- With same images but one with another suffix, example: `-dev`, `-chiseled` or `-nonroot`,
- With digest.

I'm able to consistently reproduce this issue here:
- `dhi-node-digest/Dockerfile`
  - `dhi.io/node:24-debian13-dev` and `dhi.io/node:24-debian13`
  - [error logs](https://github.com/mathieu-benoit/dependabot-core-issue-11215/actions/runs/20465249466/job/58807268053)
- `issue-with-dotnet/Dockerfile`
  - `mcr.microsoft.com/dotnet/aspnet:10.0.1-noble` and `mcr.microsoft.com/dotnet/aspnet:10.0.1-noble-chiseled`
  - [error logs](https://github.com/mathieu-benoit/dependabot-core-issue-11215/actions/runs/20465189538/job/58807069065)
- `issue-with-distroless/Dockerfile`
  - `gcr.io/distroless/static-debian13:debug` and `gcr.io/distroless/static-debian13:debug-nonroot`
  - [error logs](https://github.com/mathieu-benoit/dependabot-core-issue-11215/actions/runs/20465189380/job/58807069004)

Example of the logs:
```none
updater | 2025/12/23 15:50:33 INFO <job_1189631726> Updating the /dhi-node-digest directory.
updater | 2025/12/23 15:50:33 INFO <job_1189631726> Checking if node 25-debian13 needs updating
  proxy | 2025/12/23 15:50:34 [012] GET https://dhi.io:443/v2/node/tags/list
2025/12/23 15:50:34 [012] * authenticating docker registry request (host: dhi.io)
  proxy | 2025/12/23 15:50:34 [012] 200 https://dhi.io:443/v2/node/tags/list
updater | 2025/12/23 15:50:34 INFO <job_1189631726> Original tag components: 
updater | 2025/12/23 15:50:34 INFO <job_1189631726> Latest version is 25-debian13
updater | 2025/12/23 15:50:34 INFO <job_1189631726> Adding dependencies as handled: (node).
  proxy | 2025/12/23 15:50:34 [016] HEAD https://dhi.io:443/v2/node/manifests/25-debian13
2025/12/23 15:50:34 [016] * authenticating docker registry request (host: dhi.io)
  proxy | 2025/12/23 15:50:35 [016] 200 https://dhi.io:443/v2/node/manifests/25-debian13
updater | 2025/12/23 15:50:35 INFO <job_1189631726> Requirements to unlock own
updater | 2025/12/23 15:50:35 INFO <job_1189631726> Requirements update strategy 
updater | 2025/12/23 15:50:35 INFO <job_1189631726> Original tag components: dev
  proxy | 2025/12/23 15:50:35 [018] HEAD https://dhi.io:443/v2/node/manifests/25-debian13-dev
2025/12/23 15:50:35 [018] * authenticating docker registry request (host: dhi.io)
  proxy | 2025/12/23 15:50:36 [018] 200 https://dhi.io:443/v2/node/manifests/25-debian13-dev
updater | 2025/12/23 15:50:36 INFO <job_1189631726> Creating dependency change for node (25-debian13) in group ci
updater | 2025/12/23 15:50:36 INFO <job_1189631726> Updating node from 25-debian13 to 25-debian13
  proxy | 2025/12/23 15:50:36 [020] POST /update_jobs/1189631726/record_update_job_unknown_error
  proxy | 2025/12/23 15:50:36 [020] 204 /update_jobs/1189631726/record_update_job_unknown_error
  proxy | 2025/12/23 15:50:36 [022] POST /update_jobs/1189631726/record_update_job_error
  proxy | 2025/12/23 15:50:36 [022] 204 /update_jobs/1189631726/record_update_job_error
  proxy | 2025/12/23 15:50:36 [024] POST /update_jobs/1189631726/increment_metric
  proxy | 2025/12/23 15:50:36 [024] 204 /update_jobs/1189631726/increment_metric
  proxy | 2025/12/23 15:50:36 [026] POST /update_jobs/1189631726/record_update_job_unknown_error
  proxy | 2025/12/23 15:50:36 [026] 204 /update_jobs/1189631726/record_update_job_unknown_error
updater | 2025/12/23 15:50:36 ERROR <job_1189631726> Error processing node (RuntimeError)
2025/12/23 15:50:36 ERROR <job_1189631726> No files changed!
```