# Part 1 – Architecture Notes (Shared Gateway Problem)

## Understanding the Problem

The system integrates with an external API that has a strict limit of 3 requests per second.

However, during peak hours, the system generates more than 20 requests per second across multiple customers. This leads to failed responses and unreliable behavior.

---

## Approach

Instead of introducing a completely new system (like Kafka), I chose to reuse Redis and Celery since they are already part of the existing stack.

This keeps the solution practical and easier to integrate.

---

## Proposed Flow

Django API → Redis (per-customer queues) → Scheduler → Celery Workers → External API

---

## Key Decisions

### Rate Limiting

I used a Token Bucket approach implemented via Redis.

Reason:
- Simple to implement
- Works well for strict limits like 3 req/sec

Limitation:
- During spikes, queue length can grow quickly

---

### Fairness

I used Round Robin scheduling across customer queues.

Reason:
If one customer sends 100 requests and another sends 1, the smaller request should not be blocked behind all 100.

---

### Failure Handling

- Retry with exponential backoff
- Log failures for debugging
- Push failed tasks to a dead letter queue

---

## Trade-offs

- Simple and maintainable, but not infinitely scalable
- No priority-based scheduling (all customers treated equally)
- Redis queue could grow during peak load

---

## What I Would Improve

- Add priority queues for premium customers
- Introduce better monitoring (alerts + metrics)
- Explore distributed rate limiting if scale increases

---

## Final Thought

I intentionally avoided over-engineering this solution. Given the constraint of 3 requests per second, a simpler design is easier to maintain and sufficient for the current scale.
