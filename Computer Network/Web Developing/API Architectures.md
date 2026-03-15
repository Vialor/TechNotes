**API Application Programming Interface**
# REST Representational State Transfer
*Representational*: resources sent between the clients and servers are in certain formats (json, xml and etc)
*State Transfer*: the clients transfer their states to the servers, so servers know the state of clients
6 Constraints: Uniform interface, Stateless, Cacheable, Client-server, Layered system, Code on demand (optional)
> Swagger: document generator for REST API
# GraphQL
Query, Mutation, Subscription
```GraphQl
query GetAllEggs ($eggId: ID!) {
	egg (id: $eggId) {
		name
	}
}
```
# RPC Remote Procedure Call
> Usually used among microservices in distributed systems, not supported by browsers

gRPC: protocol buffers
tRPC
# Webhook
Event driven call back mechanism