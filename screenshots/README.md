# Screenshots

| # | File | Shows |
|---|------|-------|
| 1 | `01-get-pods-svc-hpa.png` | `kubectl get pods,svc,hpa -o wide`: 3/3 ready, LoadBalancer, HPA 40% / max 8 with a real CPU target |
| 2 | `02-predict-200.png` | `smoke_test.sh` / curl to `/predict` returning a decision |
| 3 | `03-self-healing.png` | Pod deleted, replacement created, back to 3/3 |
| 4 | `04-rolling-update-0-dropped.png` | `rolling_update_demo_incluster.sh` to `:v2` reporting 0 dropped |
| 5 | `05-bluegreen-cutover.png` | Service selector flipped blue → green |
| 6 | `06-bluegreen-rollback.png` | Service selector flipped back green → blue |
| 7 | `07-azure-predict-200.png` | Azure Container Apps public `/predict` returning 200 |
| 8 | `08-azure-teardown.png` | `az group exists -n rg-data789-hw4-chnodewitt` → `false` |
