# Alerting

## Alert on user impact

Start from service-level objectives and the four operational signals:

| Signal | Question |
| --- | --- |
| Latency | Are users waiting longer than the objective? |
| Traffic | Has expected demand disappeared or spiked? |
| Errors | Are operations failing beyond the tolerated rate? |
| Saturation | Is a bounded resource approaching exhaustion? |

Prefer multi-window burn-rate alerts for SLOs. A fast window detects severe incidents; a slower window prevents short harmless spikes from paging operators.

## Every alert needs an action

Define:

- the owner and severity;
- the exact condition and duration;
- expected user impact;
- a dashboard or query for confirmation;
- a runbook with immediate mitigation and escalation.

Do not page on raw causes when no user-facing objective is threatened. Use tickets or dashboards for capacity trends and low-urgency hygiene.

## Cardinality and missing data

Aggregate away unbounded labels before alert evaluation. Decide explicitly whether missing metrics mean healthy, absent traffic, failed collection, or a broken service.

## Go runtime signals

Runtime metrics can support diagnosis, but fixed universal thresholds are unreliable. Alert on sustained deviation from a service-specific baseline or on saturation tied to user impact. Typical supporting signals include goroutine growth, heap growth, GC CPU, file descriptors, and connection-pool utilization.

## Validation

Test expressions against representative historical data or synthetic series. Verify both firing and recovery paths and ensure notifications group related instances without hiding independent failures.
