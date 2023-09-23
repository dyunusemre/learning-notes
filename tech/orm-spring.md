# ORM / Spring DATA/ JPA
1. Basics of ORM and JPA:
	- ORM: Stands for Object-Relational Mapping. It's a technique that lets you interact with your database in an object-oriented manner. ORM frameworks (like Hibernate) provide a bridge between relational databases and object-oriented languages.
	- JPA: Java Persistence API. It's a Java specification for managing relational data in Java applications. Hibernate is a popular implementation of the JPA specification.
2. Entities and Annotations:
	- @Entity: Marks a class as a JPA entity, i.e., a database table.
	- @Table: Specifies the table's name. Useful if the table's name in the DB differs from the entity's class name.
	- @Id: Denotes a field as the primary key.
	- @GeneratedValue: Indicates that the primary key value will be automatically generated.
	- @Column: Specifies details of the column to which an entity's field is mapped.
	- @Transient: Indicates a field is not to be persisted in the DB.
3. Relationship Mapping:
	- JPA supports four types of relationships: @OneToOne, @OneToMany, @ManyToOne, and @ManyToMany.
	- @JoinColumn: Specifies a column for joining an entity association.
	- @JoinTable: Specifies the table that maps a @ManyToMany relationship.
	- N+1 Problem: Occurs when fetching parent entities also fetches child entities individually. It can be mitigated with eager fetching, JOIN FETCH, or Entity Graphs.
4. Fetching and Cascading:
	- Lazy Loading: Child entities are fetched only when accessed.
	- Eager Loading: Child entities are fetched immediately with the parent.
	- @Cascade: Determines which operations propagate from parent to child entities.
5. Spring Data JPA:
	- Simplifies CRUD operations by providing a set of repositories to operate on entities.
	- Repository: An interface that, when extended, provides CRUD methods.
	- Difference between Repositories: CrudRepository offers CRUD functions; JpaRepository offers CRUD + JPA-specific functions; PagingAndSortingRepository offers CRUD + pagination and sorting capabilities.
6. Transactions:
	- In Spring, transactions ensure a series of operations either complete successfully or roll back entirely.
	- @Transactional: Marks a method to run within a transactional context. If runtime exceptions occur, Spring rolls back the transaction.
7. Optimization and Performance:
	- Use indexing, lazy/eager fetching wisely, utilize caching.
	- 2nd Level Cache: Hibernate feature that caches data across sessions. Helps improve performance by reducing DB hits.
	- For pagination, Spring Data JPA provides Pageable and Page interfaces.
8. Queries:
	- Custom queries can be defined in Spring Data JPA using @Query.
	- JPQL: Java Persistence Query Language, used to query entities.
	- SQL: Standard language for querying databases.
	- Criteria API: Type-safe querying using Java.
	- Native SQL queries can be executed with @Query(nativeQuery = true).
9. Database Configuration and Profiles:
	- Datasource configuration usually goes in application.properties or application.yml.
	- For multiple datasources, configure multiple beans and EntityManagerFactory for each datasource.
	- Spring Profiles: Allows for different configurations based on the environment (dev, test, prod).
10. Auditing and Callbacks:
	- To track entity changes, use JPA Auditing (@CreatedBy, @LastModifiedBy, etc.).
	- JPA callbacks like @PrePersist and @PostLoad are methods in an entity that are called in response to certain entity lifecycle events.
11. Migration Tools:
	- Flyway and Liquibase: Tools integrated into Spring Boot for versioning and evolving the database schema.
12. Error Handling:
	- Spring Boot translates SQL exceptions to DataAccessException.
	- With @ControllerAdvice and @ExceptionHandler, you can customize error responses.

1. Basics of ORM and JPA:
	- ORM:
		- Pros: Increases developer productivity, abstracts DB complexities, and provides a cleaner and more object-oriented codebase.
		- Cons: Can introduce overhead, may not be optimal for complex queries, and can sometimes abstract too much, causing developers to be unaware of underlying inefficiencies.
	- JPA vs. Direct SQL:
		- Pros: Standardized API, database agnostic, and object-oriented.
		- Cons: Not always as performant as native SQL, learning curve involved.
2. Entities and Annotations:
	- @Entity & @Table:
		- Pros: Easy mapping between objects and database tables.
		- Cons: Too much reliance can lead to an "anemic domain model" where entities lack behavior.
3. Relationship Mapping:
	- Pros: Clear object relations, ORM handles the DB relationship mappings.
	- Cons: Incorrect configurations can lead to inefficient database operations (e.g., the N+1 problem).
4. Fetching and Cascading:
	- Lazy Loading vs. Eager Loading:
		- Lazy:
			- Pros: Can improve startup performance, reduces memory footprint.
			- Cons: Can lead to "LazyInitializationException", unexpected DB hits during object access.
		- Eager:
			- Pros: Fetch everything upfront, avoids unexpected DB hits later.
			- Cons: Can slow down the initial query and fetch more data than necessary.
5. Spring Data JPA:
	- Repositories:
		- Pros: Reduces boilerplate, quick and straightforward CRUD operations.
		- Cons: Can lead to generic designs; complex queries might be more cumbersome to implement.
6. Transactions:
	- @Transactional:
		- Pros: Clear demarcation of transactional boundaries, automatic rollback on exceptions.
		- Cons: Overusing it can lead to large transaction scopes, potentially locking resources for longer than necessary.
7. Optimization and Performance:
	- 2nd Level Cache:
		- Pros: Reduces DB hits, can significantly improve performance for frequently read data.
		- Cons: Complexity in invalidating stale data, potential synchronization issues in clustered setups.
8. Queries:
	- JPQL vs. SQL:
		- JPQL Pros: Database agnostic, safer due to type-checking.
		- SQL Pros: Can be more performant, offers finer control over queries.
		- Cons for both: A learning curve, especially if switching from another query language.
9. Database Configuration and Profiles:
	- Multiple Datasources:
		- Pros: Separate concerns, potentially better performance.
		- Cons: Increased configuration complexity, harder transaction management across multiple sources.
10. Auditing and Callbacks:
	- Pros: Automated data integrity and traceability, improves debugging.
	- Cons: Can introduce overhead, potential for unexpected behaviors if callbacks are misused.
11. Migration Tools:
	- Pros: Version control for DB schema, ensures consistency across environments.
	- Cons: Learning curve, potential for conflicts if not properly managed in team settings.
12. Error Handling:
	- Pros: Better user experience, easier debugging, cleaner codebase.
	- Cons: Introducing custom error handling can sometimes hide underlying issues if not done properly.

**EntityManager in JPA:**

- EntityManager is the primary interface used to interact with the persistence context in JPA. It's responsible for managing entities, which includes operations like persisting, merging, removing, and finding entities.

**Key operations**:

1. persist(): Adds an entity to the persistence context.
2. merge(): Merges the state of a detached entity with the current persistence context.
3. remove(): Removes an entity from the persistence context.
4. find(), getReference(): Retrieve entities based on their primary key.
5. createQuery(), createNamedQuery(): Used to create and execute JPQL queries.

**Java Persistence API (JPA)** is a specification in the Java EE framework that provides a set of features for managing relational data in Java applications. Here are the core components of JPA:

1. EntityManagerFactory:
	- It's a factory class responsible for producing instances of EntityManager. Typically, one EntityManagerFactory is created for each database, and it's thread-safe.
2. EntityManager:
	- Acts as the interface between a Java application and the database. It provides operations for CRUD (Create, Read, Update, Delete) functions, querying, and managing transaction boundaries.
	- It's associated with a persistence context, which is a set of entity instances that the entity manager will manage.
3. Entity:
	- Represents a table in the database. Entities are plain old Java objects (POJOs) annotated with @Entity and other annotations to map the class and its fields to database tables and columns, respectively.
4. EntityTransaction:
	- Used to manage transactions in resource-local scenarios (i.e., outside of JTA managed environments). Provides basic transaction operations like begin(), commit(), and rollback().
5. Persistence Unit:
	- Represents a set of all entity classes that are managed by the EntityManager in an application. Defined in the persistence.xml file.
6. JPQL (Java Persistence Query Language):
	- An object-oriented query language defined by JPA. Allows you to write queries against entities as opposed to native SQL which queries the database directly.
7. Criteria API:
	- A type-safe way to write queries programmatically. Useful when building queries dynamically at runtime.
8. Annotations:
	- Metadata that helps map Java classes and fields to database tables and columns. Some common annotations include @Entity, @Table, @Id, @GeneratedValue, @Column, and relationship annotations like @OneToMany, @ManyToOne, @ManyToMany, and @OneToOne.
9. Persistence Context:
	- A set of managed entity instances within a particular EntityManager. It ensures that within a particular transaction, a retrieved entity will always be the same instance.
10. Entity Lifecycle:
	- Entities go through various states: New (Transient), Managed (Persistent), Detached, and Removed. JPA defines lifecycle callback methods that can be triggered when entities transition through these states.
11. Caching:
	- JPA supports two levels of caching:
		- 1st Level Cache (Session Cache): Associated with an EntityManager and its persistence context. Ensures each entity is retrieved only once within a transaction.
		- 2nd Level Cache: Session-independent cache that can be shared across multiple EntityManagers.
12. Embeddables:
	- These are value types that don't have their own identity. They're used to represent a set of attributes in an entity class, and they're annotated with @Embeddable.
13. NamedQueries:
	- Static queries with a predefined name, defined using annotations or in persistence.xml. They're used to organize queries in a central place and can be invoked using their names.

These components work together to provide a cohesive and consistent mechanism for object-relational mapping, query execution, and transaction management in Java applications.


#### 1st Level Cache (Session Cache or Persistence Context Cache)

- Every time an EntityManager instance is created, a new 1st level cache is also associated with it. This cache operates at the EntityManager or transactional level.
- Flush & Clear: Operations like flush() and clear() on the EntityManager affect the 1st level cache.


#### 2nd Level Cache

The 2nd level cache, on the other hand, is associated with the EntityManagerFactory (or the session factory in the case of Hibernate). It's shared across multiple EntityManager instances and can span multiple transactions and sessions.




**Characteristics & Functionality:**

1. Configuration: Unlike the 1st level cache, the 2nd level cache needs to be configured to be activated.
2. Lifespan: It survives multiple transactions and is scoped to the application, not a single transaction. The data in this cache persists across sessions until either the cache is explicitly cleared or the data expires based on cache eviction settings.
3. Purpose: The main goal is to optimize performance across sessions. If an entity is already in the 2nd level cache, it won't be re-fetched from the database in a new session, leading to performance gains.
4. Region-Based: Typically, the 2nd level cache is divided into regions. Each entity or collection type can be configured to use a different cache region. This allows for granular eviction policies and optimization strategies.
5. Concurrency: Since multiple sessions can access the 2nd level cache simultaneously, the caching solution must support concurrent access. Many caching providers offer varying strategies for this (e.g., read-write, nonstrict read-write, and transactional).
6. Eviction & Expiry: The data in the cache can be configured to expire after a certain period or based on certain conditions. This ensures that the cache doesn't serve stale data.
7. Providers: There are various third-party cache providers, such as EhCache, Infinispan, and Hazelcast, that integrate with JPA and Hibernate to offer 2nd level caching.

**Trade-offs**:

1. 1st Level Cache:
	- Pros: Always on and ensures data consistency within a session.
	- Cons: Limited to a single session's scope. No cross-session performance benefits.
2. 2nd Level Cache:
	- Pros: Provides substantial performance benefits across multiple sessions. Reduces database hits.
	- Cons: Complexity in ensuring fresh data (risk of serving stale data). Needs proper tuning and eviction strategies.
The annotation @Cache(usage = CacheConcurrencyStrategy.READ_WRITE) is a Hibernate-specific annotation that's used to configure caching behavior for an entity (or a collection). When you see this annotation on a class, it provides directives on how Hibernate should interact with the 2nd level cache for instances of that class.

**Let's break down the annotation**:

- @Cache: This is a Hibernate annotation that indicates the entity should be cached in the 2nd level cache.
- usage = CacheConcurrencyStrategy.READ_WRITE: This attribute defines the caching strategy that should be used for the entity. Different strategies offer trade-offs in terms of performance and consistency.
	- READ_WRITE: This is a common caching strategy that allows the cache to be read and written to by concurrent sessions. When an entity is updated, a soft "lock" is set on it, ensuring that other transactions don't work with stale data. Once the update is complete and the transaction is committed, the entity in the cache is updated and the lock is released. This strategy aims to strike a balance between performance and consistency.

**Other values for CacheConcurrencyStrategy include**:

- *READ_ONLY*: Suitable for entities that are never updated (they're only read). It's the simplest and most performative caching strategy but is only suitable when you're sure the entity will never be modified.
- *NONSTRICT_READ_WRITE*: A more lenient version of the READ_WRITE strategy. It doesn't use locks. Instead, it relies on the "last commit wins" strategy, where the last transaction to commit its changes will overwrite any concurrent changes made in the cache. This can lead to stale data in certain scenarios.
- *TRANSACTIONAL*: This strategy offers fully transactional cache entries. It ensures strong transactional isolation, making the cache behave as part of the transaction. It's the most consistent but may have a performance impact. It also requires the underlying cache provider to support XA-based transactions.

When using @Cache, it's crucial to choose the correct concurrency strategy based on your specific use case and consistency requirements. The READ_WRITE strategy, as indicated by the annotation you provided, is often a good middle ground for many typical scenarios where entities are both read and updated.

When using the **READ_WRITE** cache concurrency strategy in Hibernate, a "soft lock" mechanism is employed to ensure data consistency when an entity is being updated. Here's a step-by-step explanation of how this works:

1. *Update Begins*: When a transaction starts updating an entity:
	- A soft lock is placed on the cached version of that entity.
	- This soft lock isn't a traditional lock as we think of them in databases; instead, it's a marker or a token that indicates the entity is being updated.
2. *Another Transaction Accesses the Entity:*
	- If another transaction tries to read the entity while the soft lock is in place, the cache will not provide the entity, effectively behaving as if the entity isn't cached. This prompts Hibernate to fetch the entity from the database.
	- If another transaction tries to update the entity concurrently, it will detect the soft lock. It will then either:
		- Wait for the lock to be released (depends on the transaction isolation level and configuration).
		- Throw an exception indicating a concurrent modification (in certain configurations).
		- Proceed with the operation and possibly overwrite the previous changes in a "last commit wins" manner, depending on the setup.
3. *Original Update Completes*:
	- Once the initial transaction finishes updating the entity and commits, the soft lock on the entity in the cache is replaced with the updated entity.
	- Any subsequent transactions will see this updated entity until a new update comes along and the process repeats.

The soft lock mechanism ensures that the cache doesn't provide stale data to other transactions while an update is in progress. The READ_WRITE strategy strikes a balance between data consistency and performance:

- It provides consistency by ensuring that other transactions won't get stale data from the cache during an update.
- It provides performance benefits by allowing other transactions to read the (non-cached) data from the database directly when the cache entry is locked, rather than blocking them until the update completes.

However, it's essential to understand that the soft lock mechanism is more about cache consistency than database row-level locking. Database row-level locks (or other isolation mechanisms) will still play a role in governing concurrent access to the actual data in the database.