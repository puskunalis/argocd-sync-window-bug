# ArgoCD Sync Window Bug Test

Test repository for debugging ArgoCD sync behavior with slowly-applying resources.

## Structure

This repo creates a simple ArgoCD application that syncs resources slowly over ~1 minute:

- **5 ConfigMaps** (waves 0, 2, 4, 6, 8)
- **4 Sleep Jobs** (waves 1, 3, 5, 7) - 15 second delays each

## Sync Timeline

```
Wave 0: config-wave-0 (instant)
Wave 1: sleep-wave-1 (15s)
Wave 2: config-wave-2 (instant)
Wave 3: sleep-wave-3 (15s)
Wave 4: config-wave-4 (instant)
Wave 5: sleep-wave-5 (15s)
Wave 6: config-wave-6 (instant)
Wave 7: sleep-wave-7 (15s)
Wave 8: config-wave-8 (instant)

Total: ~60 seconds
```

## Usage

Apply the application to ArgoCD:
```bash
kubectl apply -f application.yaml
```

## Customization

To adjust sync duration:
- Modify sleep duration in `sleep-hook-*.yaml` files
- Add/remove waves by creating more ConfigMaps and sleep hooks
- Change wave numbers to control ordering

## Cleanup

```bash
kubectl delete application sync-test-app -n argocd
kubectl delete namespace sync-test
```
