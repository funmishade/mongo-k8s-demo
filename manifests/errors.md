
---

## 3️⃣ Notes / Documentation

For every error you had (like `Waiting for mongo:27017…`, endpoints `<none>`, port-forward timeouts), create a small markdown file under `notes/`:

Example: `deployment-issues.md`

```markdown
# Deployment Issues

## Issue 1: mongoexpress pod could not reach MongoDB
- Logs: `/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument`
- Cause: Service selector did not match pod labels
- Fix: Updated `mongoexpress-service.yaml` selector to match `app: mongo-express`

## Issue 2: Port-forward timeout
- Logs: `error: timed out waiting for the condition`
- Cause: Endpoints were `<none>` because pods were not ready or selector mismatch
- Fix: Correct labels and confirmed pods are running

1. indicated ns in the config file but no ns created on the cluster, so had the error.
fix - created the ns and applied the file again



2. kubectl port forward kept timing out. http://localhost:8081 didnt return the app.
Because there was no endpoint created.
`mongoexpress-service` **had no endpoints at that time**.
`kubectl port-forward` kept timing out — there was **no pod for the service to forward to**.
Endpoints are automatically created when the service finds matching pods.
Why:
**Label mismatch** - i had app:  munexpress in the selector.matchlabel of my deployment file; while i had app: mongoexpress in the service.yaml file.2. kubectl port forward kept timing out. http://localhost:8081 didnt return the app.
Because there was no endpoint created.
`mongoexpress-service` **had no endpoints at that time**.
`kubectl port-forward` kept timing out — there was **no pod for the service to forward to**.
Endpoints are automatically created when the service finds matching pods.
Why:
**Label mismatch** - i had app:  munexpress in the selector.matchlabel of my deployment file; while i had app: mongoexpress in the service.yaml file.
