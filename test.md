## A Sample Architecture

![83be064ab36293f155c6259f89cb630c.png](:/6d01275155b443969f0a79b7d876df77)


<br />

## Data Partitioning

![1d05fe07f83d81eeedc6edc117176b17.png](:/b594905cc72b45f5a58348cc3f34dc06)



**Vertical** - store properties A,D on one partition and properties B,C,E on another one - applies on all data entries
**Horizontal** - store orders for customers A-K on one partition, store orders for customers L-Z on another one
**Functional** - partition, store invoices on another partition
    - 💡 can use different data source types optimized for the data
    - ❗ data joins might be difficult

<br />

### Azure SQL Sharding

![761ded9df3cb0542aa22a6151c24c9b8.png](:/da89f7f0e58e4fc7935097b1a428b035)


- Databases have the same schema, but different data sets = horizontal partitioning
- In multi-tenant application "Tenant Id" is a popular sharding key
- Use Azure **SQL Elastic Pool** to work with data across shard sets

<br />

## CAP Theorem

![a9a5c86fc778335dc6d0a972b491981f.png](:/3a10dc91db944a81a19e71b36fa3d349)

**Consistency** - data store always reflect the actual state
- 💡 ACID transactions
- 💡 eventual consistency

**Availability** - every request receives a response

**Partition Tolerance** - system can work when some parts of the system are unavailable (network partitioning)

> 🎓 A system cannot have all three of these guarantees at the same time. We need to find an intersection that suits our needs most.

<br />


## Redis Cache
### Use low-level API

```csharp
var connection = ConnectionMultiplexer.Connect(ConnectionString);
_redis = connection.GetDatabase();
```

### Use asp net core `IDistributedCache`
```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "redis connection string...";
});
```
💡 Can point to a in-medory DB during development and to a Redis cache in production

### NuGet Packages

![597fe8b764dab7723435b178c20fbb6c.png](:/ae0b409462c54e3fb9e7c087a194cb7c)


## Content Delivery Network (CDN)

👍 content arrives faster due to low latency
👍 offloading requests from your app

### Creating a CDN Profile

![ad6e4a5e4eb01bbd56910e368b896861.png](:/c1bf296fde354ab786745aea0d8b864d)

### Using CDN from a web app

This will load an image normally from the web server:
````html
<img src="./images/myimage.png" />
````

This will load image from the CDN endpoint. If there is no image yet, the CDN will get it from origin and cache:
````html
<img src="https://helloazure.azureedge.net/images/myimage.png" />
````

## API Management
Azure API Management is a proxy between a user and a real API.

There are a lot of things you can do with API Management:
- Import API from different sources like Open Api (swagger), Azure API app, Azure Functions etc
- Use "Products" and "Subscriptions" to have different availability models of your API
- Use authentication to secure your back end API and to only allow the API Gateway to communicate with it
- Manipulate requests and resonses
- Configure every API operation separately from others
- and much more...




