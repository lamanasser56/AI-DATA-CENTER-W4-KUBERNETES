# W4D3 Resource Policy

## Priority order

1. **Serving** is latency-sensitive and receives protected CPU and memory requests and limits first.
2. **Dashboard** may burst when capacity is available, but should retain a modest baseline request so it remains usable.
3. **Batch work** is the first workload throttled during contention. It must use explicit CPU limits and should be reduced before serving capacity is affected.

## Evidence informing the policy

The noisy-neighbour experiment measured no failed requests in either run. With 20 unlimited burners, the serving probe recorded p50 **3 ms** and p95 **4 ms**. With 20 burners limited to **500m CPU** each, it recorded p50 **2 ms** and p95 **4 ms**.

The unchanged p95 shows that this pair of measurements does not establish a p95 improvement from the CPU limits. The policy therefore treats limits as an isolation and contention-control mechanism, not as a guaranteed latency optimization.
