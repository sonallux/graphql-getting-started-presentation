# GraphQL Demo

## GraphiQL
- "Swagger UI für GraphQL"
- IDE für GraphQL

### Query

```graphql
query {
  searchPerson(name: "Muster") {id name}
}
```

### Mutation

```graphql
mutation {
  createPerson(name: "Test person", age: 42) {id name age aboutMe}
}
```

### Subscription

```graphql
subscription {
  friendshipAdded{person1Id, person2Id}
}
```

- Neuen Tab daneben öffnen!
- Authorization header einfügen!
- Leon Mustermann: 4206

```graphql
mutation {
  addFriendship(person2Id: "4206")
}
```

Mapping auf HTTP zeigen im Netzwerk Tab

vllt Query in Postman

# Live Coding

## Setup zeigen
- pom.xml
- Schema zeigen

## PostController implementieren

- Für jede Property eines Types eine Funktion (graphqljs: resolver, graphql-java: DataFetcher, spring-graphql: Handler)
- Mapping Funktionen für Post implementieren
- Standard Annotation ist `@SchemaMapping` und deren Ableitungen `@QueryMapping`, `@MutationMapping` und `@SubscriptionMapping`
- Es gibt vorimplementierte Resolver die Standard getter verwenden.

## Authorization pro Handler Funktion
-  mit Standard spring-security Mechanismus `@PreAuthorize` Annotation (aktiviert über `@EnableReactiveMethodSecurity`)

## Tests
- `PersonGraphQLControllerTest`

## Exception handling
- `GraphQLExceptionResolver` zeigen
- `addFriendship` oder `createPerson` mutation zeigen mit ungültiger Eingabe

## Batching to solve N+1 Problem
- kurz Slides zeigen
- Log einträge zeigen, wenn posts von Freunden abgefragt werden:
```
query {person(id: "4205"){friends{posts{id title}}}}
```

- `@BatchMapping` variante einkommentiere

### Direkter Zugriff auf DataLoader

```java
public BookController(BatchLoaderRegistry registry) {
  registry.forTypePair(Long.class, Author.class).registerMappedBatchLoader((authorIds, env) -> {
    // return Map<Long, Author>
  });
}

@SchemaMapping
public CompletableFuture<Author> author(Book book, DataLoader<Long, Author> loader) {
  return loader.load(book.getAuthorId());
}
```
