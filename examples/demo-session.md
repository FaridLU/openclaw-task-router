# Demo Session — OpenClaw Task Router in Action

This is an example of what a real routing session looks like when using the Task Router.

---

## User Request

```
Plan a microservices architecture for our e-commerce app
```

## Router Response

```
🔀 Routing to plan agent → GLM-5.1 (thinking: high)

Task Dispatched:
- Agent: plan
- Model: GLM-5.1
- Thinking: high
- Task: Design microservices architecture for e-commerce

I'll report back with the architecture plan once the agent completes.
```

---

## Heartbeat (2 minutes later)

```
Router
├── plan (Microservices architecture) → GLM-5.1 (thinking: high) [RUNNING]
└── Summary: Analyzing domain boundaries and service decomposition patterns
```

---

## Heartbeat (4 minutes later)

```
Router
├── plan (Microservices architecture) → GLM-5.1 (thinking: high) [FINISHED: 8-service architecture with event-driven communication, API Gateway, and CQRS patterns]
└── Summary: Architecture complete. Services identified: User, Product, Cart, Order, Payment, Inventory, Notification, Analytics
```

---

## Result

The plan agent returns a full architecture document with:
- Service boundaries and responsibilities
- Communication patterns (events vs REST)
- Data ownership per service
- Deployment topology recommendation

---

## Second Request

```
Debug the 500 error in production
```

## Router Response

```
🔀 Routing to debug agent → DeepSeek V4 Pro (thinking: medium)

Task Dispatched:
- Agent: debug
- Model: DeepSeek V4 Pro
- Thinking: medium
- Task: Investigate production 500 error
```

---

## Heartbeat

```
Router
├── plan (Microservices architecture) → GLM-5.1 (thinking: high) [FINISHED]
├── debug (Production 500 error) → DeepSeek V4 Pro (thinking: medium) [RUNNING]
└── Summary: Analyzing stack traces and log patterns
```

---

## Heartbeat

```
Router
├── plan (Microservices architecture) → GLM-5.1 (thinking: high) [FINISHED]
├── debug (Production 500 error) → DeepSeek V4 Pro (thinking: medium) [FINISHED: NullPointerException in OrderService.calculateTotal() — missing null check on discount field]
└── Summary: All tasks complete. OrderService fix identified.
```

---

## Key Observations

1. **Async by design** — user never waits. Confirmation is instant.
2. **Multiple agents** — plan and debug ran simultaneously.
3. **Status visible** — heartbeat trees show exactly what's happening.
4. **Results flow back** — when agents finish, their output appears in the session.
5. **No blocking** — user could have sent more tasks while others were running.