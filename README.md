# .Net Core API REST with DDD and MongoDB
A simple CRUD Rest API, using DDD (Driven Domain Design) and MongoDB.

## How to start
1. Clone or download this project
2. Configure your connection in appsettings.json, in Products.API project
3. Start the API (F5)

## Projects
- Products.API - API Project where contains the controllers.

- Products.Application - The application layer, where it contains the interfaces, services and the AutoMapper profiles.
- Products.Domain - Domain layer, where it contains the models, interfaces and DTO's.
- Products.Infra.Data - Data layer, where it contains the MongoContext (the class where we configure the MongoDB), mappings, repositories
and the Unit of Work class.
- Products.Infra.IoC - Inversion of Control layer, where we configure common structures for your application, like DI (Dependency Injection)
and conventions for MongoDB.

## API Routes

| Type    | Route             | Params in route | Body params  | Action|
| --------| ----------------- | --------------- | ------------ | ------- |
|  GET    |  api/Product      | None            | None         | Returns all products from database |
|  GET    |  api/Product/id   | id - GUID       | None         | Returns a specific product from database |
|  POST   | api/Product       | None            | ProductDto*  | Creates a new product |
|  PUT    |  api/Product/id   | id - GUID            | ProductDto*  | Updates a existing product |
|  DELETE |  api/Product/id   | id - GUID       | None         | Delete a existing product |
 
### ProductDto Model

```json
{
    "name": "Test product 1",
    "description": "Test description 1",
    "price": 20.10,
    "active": true
}
```

## Configurations
### Connection string and database name
In Products.API we have a json named **appsettings.json**. We can configure the mongo connection and database name in this file:
```json
  "ConnectionStrings": {
    "MongoConnection": "mongodb://localhost:27017",
    "MongoDatabaseName": "ProductsDB"
  }
```
### MongoDbConfiguration
Location: Products.Infra.IoC

This class is responsible for defining the default configurations wich we want to use in our application.

```C#
BsonDefaults.GuidRepresentation = GuidRepresentation.CSharpLegacy;
```
This tells the MongoDB that we want to store a GUID instead of a default object id of MongoDB.

### ConventionPack
```C#
var pack = new ConventionPack
{
  new CamelCaseElementNameConvention()
};
```
You can pass one or more conventions as you like. In this case we are adding a camel case convention, in .NET patterns we use Pascal Case,
and in MongoDB the default is Cammel Case.

Example:

In .NET we have:
```C#
public class Product : Entity
{
    public string Name { get; set; }
    public string Description { get; set; }
}
```

But for MongoDB we want to be cammel case, so with the camel case convention, our document will be generated like this:

```javascript
{
  "_id" : NUUID("6780be07-3b8b-4d10-832d-7df431017160"),
  "name" : "Test product 1",
  "description" : "Test description 1"
}
```
