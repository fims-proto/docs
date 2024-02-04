# Todo List and Desicions

## Todos

- [x] Oathkeeper alternative, since it's still in beta
    - Seems we can use [K8s Gateway API](https://gateway-api.sigs.k8s.io/)
- [ ] Investigate: how to serve frontend with K8s
- [ ] Build fims chart
- [ ] Subscribe [Ory Security Newsletter](https://www.ory.sh/docs/ecosystem/upgrading)

## PoC

One Ory Kratos instance per tenant, and one Ory Oathkeeper for all tenants. This seems feasible: <https://www.ory.sh/docs/oathkeeper/api-access-rules#match-strategy-behavior> and <https://github.com/ory/oathkeeper/discussions/933>.

Some diagram like [Identity and Access Proxy](../archived/design.md#identity-and-access-proxy)
