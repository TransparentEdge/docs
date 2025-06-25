# Relaunch or requeue jobs

We can relaunch failed orders via our [API](https://api.transparentcdn.com/docs), which would help to relaunch multiple jobs programmatically. To do this we must be [authenticated](https://docs.transparentedge.eu/api/autenticacion), which is a necessary condition to be able to make calls to our API.

The endpoint that we should use would be the next:

```
https://api.transparentcdn.com/media/{company_id}/transcode_requeue/{order_id}/
```

Example:

```
https://api.transparentcdn.com/v1/media/111/transcode_requeue/5596731515-3324
```
