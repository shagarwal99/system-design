## Design Local Delivery Service

### Functional Requirements
1. Customers should be able to query availability of items based on location, deliverable in under ~1hr
2. Customers should be able to order multiple items at the same time.
3. Handling payments/purchases(optional)
4. Handling driver routing and deliveries(optional)
5. Cancellations and returns

### Non-Functional Requirements
- Availability requests should be fast(<100ms) to support use-cases like search
- Ordering should be strongly consistent : two customers should not be ablr to purchase the same physical product
- Order volumne should be ~10m orders/day
- System should be able to support 10k distributon centers and 100k items across distribution centers


### Api Design
To meet our requirements we only need two APIs: the first API allows us to get availability of items given a location (and maybe a keyword), and the second API allows us to place an order. We'll include pagination in our availability API to avoid overwhelming the client with more data than it needs.

```
GET /v1/availability?lat=${lat}&long=${long}&keyword=${keyword}&page_size=${}&page_num=${}
-> {
    items : {
        name : ITEM_NAME,
        quantity : ITEM_QUANTITY
    }[]
}
```

```
POST /v1/order
{
    lat : LAT,
    long : LONG,
    items : ITEM1,ITEM2,ITEM3
} -> Order|Failure
```

### High Level Design

- #### Customers should be able to query availability of items
