# HW4 — Short Answers

## 1. Rolling-update safety
Why does the Deployment use `maxUnavailable: 0`, and what would change if it were `maxUnavailable: 1`?

> `maxUnavailable: 0` means Kubernetes may never take an old pod out of service until a replacement pod (allowed by `maxSurge: 1`) has passed its readiness probe, so serving capacity never drops below 3 ready replicas during the rollout. With `maxUnavailable: 1`, Kubernetes could terminate an old pod before its replacement is ready, temporarily leaving only 2 pods to absorb all traffic. The rollout would finish faster, but under bursty load the remaining pods could be overloaded and requests could slow down or fail.

## 2. Health probes
Why do the liveness/readiness probes target `/health` instead of `/predict`?

> `/health` is a cheap GET that only reports whether the process is up and does not depend on Redis, so a slow or unavailable feature store won't make Kubernetes mark healthy pods unready or restart them in a loop. `/predict` is a POST that needs a real transaction body, runs model inference, and calls Redis, so probing it would add CPU load (skewing the HPA), produce fake transactions in logs and metrics, and tie pod health to an external dependency.

## 3. Autoscaling under load
Your HPA scales at 40% CPU up to 8 pods. If request volume doubled, what would you expect to happen — and what happens once it reaches the maximum?

> When volume doubles, average CPU per pod (measured against the 100m request) rises above 40%, so the HPA adds replicas roughly in proportion (desired = current × currentCPU / 40%). For example, 3 pods at 80% would grow to about 6, bringing utilization back toward the target. Once it reaches 8 replicas the HPA stops adding pods even if CPU stays above target. Each pod is then capped at its 250m CPU limit and throttled, so latency rises and requests can time out until load drops or someone raises `maxReplicas` or adds cluster capacity.
