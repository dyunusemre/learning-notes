# **Chapter 1. Analyzing Business Domains**

# **Chapter 2. Discovering Domain Knowledge**

1. ## Business Problems
   
   1. The software systems we are building are solutions to business problems
   2. Business problems appear both at the business domain and subdomain levels.
      1. Subdomains are finer-grained problem domains whose goal is to provide solutions for specific business capabilities.
      2. A knowledge management subdomain optimizes the process of storing and retrieving information
2. ## Knowledge Discovery
   
   1. To design an effective software solution, we have to grasp at least the basic knowledge of the business domain.
   2. To be effective, the software has to mimic the domain experts’ way of thinking about the problem
   3. **As Alberto Brandolini1 says, software development is a learning process; working code is a side effect.**
   4. A software project’s success depends on the effectiveness of knowledge sharing between domain experts and software engineers.
   5. Effective knowledge sharing between domain experts and software engineers requires effective communication.
3. ## Communication
   
   1. software projects require the collaboration of stakeholders in different roles: domain experts, product owners, engineers, UI and UX designers, project managers, testers, analysts, and others
   2. ![Alt text](img/image.png)
   3. In any translation, information is lost; in this case, domain knowledge that is essential for solving business problems gets lost on its way to the software engineers.
   4. Domain-driven design proposes a better way to get the knowledge from domain experts to software engineers: by using a ubiquitous language.
4. ## What Is a Ubiquitous Language?
   
   1. Using a ubiquitous language is the cornerstone practice of domain-driven design.
   2. if parties need to communicate efficiently, instead of relying on translations, they have to speak the same language.
   3. The traditional software development lifecycle implies the following translations:
      - Domain knowledge into an analysis model
      - Analysis model into requirements
      - Requirements into system design
      - System design into source code
   4. Instead of continuously translating domain knowledge, domain-driven design calls for cultivating a single language for describing the business domain: the ubiquitous language.
   5. All project-related stakeholders—software engineers, product owners, domain experts, UI/UX designers—should use the ubiquitous language when describing the business domain.
5. ## Language of the Business
   
   1. It’s crucial to emphasize that the ubiquitous language is the language of the business
   2. As such, it should consist of business domain–related terms only. No technical jargon!
      1. ### Consistency
         
         1. The ubiquitous language must be precise and consistent.
         2. each term of the ubiquitous language should have one and only one meaning
            1. *Ambiguous terms*
               - Ubiquitous language demands a single meaning for each term, so “policy” should be modeled explicitly using the two terms regulatory rule and insurance contract.
            2. *Synonymous terms*
               - Two terms cannot be used interchangeably in a ubiquitous language.
6. ## Model of the Business Domain
   
   1. ### What Is a Model?
      
      1. A model is not a copy of the real world but a human construct that helps us make sense of real-world systems.
   2. ### Effective Modeling
      
      1. All models have a purpose, and an effective model contains only the details needed to fulfill its purpose.
      2. a useful model is not a copy of the real world. Instead, a model is intended to solve a problem, and it should provide just enough information for that purpose.
      3. In its essence, a model is an abstraction
         1. The notion of abstraction allows us to handle complexity by omitting unnecessary details and leaving only what’s needed for solving the problem at hand.
   3. ### Modeling the Business Domain
      
      1. The model is supposed to capture the domain experts’ mental models
      2. The model has to reflect the involved business entities and their behavior, cause and effect relationships, and invariants.
      3. The ubiquitous language we use is not supposed to cover every possible detail of the domain
      4. Instead, the model is supposed to include just enough aspects of the business domain to make it possible to implement the required system
      5. Effective communication between engineering teams and domain experts is vital.
   4. ### Continuous Effort
      
      1. Only interactions with actual domain experts can uncover inaccuracies, wrong assumptions, or an overall flawed understanding of the business domain.
      2. All stakeholders should consistently use the ubiquitous language in all project-related communications to spread knowledge about and foster a shared understanding of the business domain.
      3. cultivation of a ubiquitous language is an ongoing process.
   5. ### Tools
      
      1. a wiki can be used as a glossary to capture and document the ubiquitous language
      2. It’s important to make glossary maintenance a shared effort. When a ubiquitous language is changed, all team members should be encouraged to go ahead and update the glossary
      3. Automated tests written in the Gherkin language are not only great tools for capturing the ubiquitous language but also act as an additional tool for bridging the gap between domain experts and software engineers.
      
      ```
      Scenario: Notify the agent about a new support case
              Given Vincent Jules submits a new support case saying:
              """
              I need help configuring AWS Infinidash
              """
              When the ticket is assigned to Mr. Wolf
              Then the agent receives a notification about the new ticket
      ```
      
      4. Finally, there are even static code analysis tools that can verify the usage of a ubiquitous language’s terms. A notable example for such a tool is NDepend
   6. ### Challenges
      
      1. In theory, cultivating a ubiquitous language sounds like a simple, straightforward process. In practice, it isn’t.
      2. The only way to access it is to ask questions to Domain Experts.
      3. Furthermore, you may encounter business domain concepts that lack explicit definitions
      4. the learning process is mutual—you are helping the domain experts better understand their field.
      5. The essential tool in such a situation is patience.
      6. My advice is to at least use English nouns for naming the business domain’s entities.
   7. ### Conclusion
      
      1. Effective communication and knowledge sharing are crucial for a successful software project.
      2. Software engineers have to understand the business domain in order to design and build a software solution.
      3. Domain-driven design’s ubiquitous language is an effective tool for bridging the knowledge gap between domain experts and software engineers.
      4. Cultivating a ubiquitous language is a continuous process. As the project evolves, more domain knowledge will be discovered. It’s important for such insights to be reflected in the ubiquitous language.
      5. To ensure effective communication, the ubiquitous language has to eliminate ambiguities and implicit assumptions. All of a language’s terms have to be consistent

# **Chapter 3. Managing Domain Complexity**

- **Notes**: To ensure a project’s success it’s crucial that you develop a ubiquitous language that can be used for communication by all stakeholders, from software engineers to domain experts. The language should reflect the domain experts’ mental models of the business domain’s inner workings and underlying principles.

1. ## Inconsistent Models
   
   1. each term should have one meaning. ubiquitous language has to be consistent
      - ![Alt text](img/image-1.png)
      - it is more difficult to represent such a divergent model of the business domain in software. Source code doesn’t cope well with ambiguity. If we were to bring the sales department’s complicated model into marketing, it would introduce complexity where it’s not needed
   2. a solution would be to prefix the problematic term with a definition of the context: “marketing lead” and “sales lead.”
   3. this approach has two main disadvantages. First, it induces cognitive load
      1. First, it induces cognitive load. The closer the implementations of the conflicting models are, the easier it is to make a mistake.
      2. Second, the implementation of the model won’t be aligned with the ubiquitous language. No one would use the prefixes in conversations.
   4. the domain-driven design pattern for tackling such scenarios: the bounded context pattern.
2. ## What Is a Bounded Context?
   
   1. The solution in domain-driven design is trivial: **divide the ubiquitous language into multiple smaller languages, then assign each one to the explicit context in which it can be applied: its bounded context.**
   2. marketing and sales. The term lead exists in both bounded contexts
      - ![Alt text](img/image-2.png)
   3. ### *Model Boundaries*
      
      1. A model is not a copy of the real world but a construct that helps us make sense of a complex system.
      2. The problem it is supposed to solve is an inherent part of a model—its purpose.
      3. A model cannot exist without a boundary; it will expand to become a copy of the real world. That makes defining a model’s boundary—its bounded contexts—an intrinsic part of the modeling process.
      4. **an ubiquitous language** in one bounded context can be completely irrelevant to the scope of another bounded context.
      5. **Bounded contexts** define the applicability of a ubiquitous language and of the model it represents.
      6. In other words, bounded contexts are the consistency boundaries of ubiquitous languages.
   4. ### *Ubiquitous Language Refined*
      
      1. Bounded contexts allow us to complete the definition of a ubiquitous language.
      2. A ubiquitous language is not universal.
      3. Instead, a ubiquitous language is ubiquitous only in the boundaries of its bounded context. The language is focused on describing only the model that is encompassed by the bounded context.
   5. ### *Scope of a Bounded Context*
      
      1. To model the business domain, we had to divide the model and define a strict applicability context for each fine-grained model—its bounded context.
      2. The consistency of the ubiquitous language only helps to identify the widest boundary of that language.
      3. ![Alt text](img/image-3.png)
      4. Defining the scope of a ubiquitous language—its bounded context—is a strategic design decision.
      5. Models shouldn’t necessarily be big or small. Models need to be useful.
      6. The wider the boundary of the ubiquitous language is, the harder it is to keep it consistent.
      
      - **Notes**:  It may be beneficial to divide a large ubiquitous language into smaller, more manageable problem domains, but striving for small bounded contexts can backfire too. The smaller they are, the more integration overhead the design induces.
      
      7. To avoid such ineffective decomposition, use the rule of thumb we discussed in Chapter 1 to find subdomains: identify sets of coherent use cases that operate on the same data and avoid decomposing them into multiple bounded contexts.
3. ## Bounded Contexts Versus Subdomains
   
   1. a business domain consists of multiple subdomains.
   2. ### *Subdomains*
      
      1. According to domain-driven design methodology, the analysis phase involves identifying the different subdomains (core, supporting, and generic).
      2. a subdomain resembles a set of interrelated use cases.
      3. we are analyzing the business domain to identify the subdomains.
      4. As software engineers, we do not define the requirements; that’s the responsibility of the business.
   3. ### *Bounded Contexts*
      
      1. Bounded contexts, on the other hand, are designed. Choosing models’ boundaries is a strategic design decision. We decide how to divide the business domain into smaller, manageable problem domains.
   4. ### *The Interplay Between Subdomains and Bounded Contexts*
      
      1. ![Monolithic Bounded Context](img/image-4.png)
      2. ![Bounded context driven by ubiquitous language](img/image-5.png)
      3. If the models are still large and hard to maintain, we can decompose them into even smaller bounded contexts
      4. ![Bounded contexts aligned with subdomain boundaries](img/image-6.png)
      5. Having a one-to-one relationship between bounded contexts and subdomains can be perfectly reasonable in some scenarios. In others, however, different decomposition strategies can be more suitable.
      6. It’s crucial to remember that subdomains are discovered and bounded contexts are designed.
      7. The subdomains are defined by the business strategy. However, we can design the software solution and its bounded contexts to address the specific project’s context and constraints.
4. ## Boundaries
   
   1. The bounded context pattern is the domain-driven design tool for defining physical and ownership boundaries.
   2. ### *Pyhsical Boundaries*,
      
      1. Each bounded context should be implemented as an individual service/project, meaning it is implemented, evolved, and versioned independently of other bounded contexts.
      2. Clear physical boundaries between bounded contexts allow us to implement each bounded context with the technology stack that best fits its needs.
      3. a bounded context can contain multiple subdomains. In such a case, the bounded context is a physical boundary, while each of its subdomains is a logical boundary.
   3. ### *Ownership Boundaries*
      
      1. we can leverage model boundaries—bounded contexts—for the peaceful coexistence of teams.
      2. The division of work between teams is another strategic decision that can be made using the bounded context pattern.
      3. A bounded context should be implemented, evolved, and maintained by one team only.
      4. No two teams can work on the same bounded context. This segregation eliminates implicit assumptions that teams might make about one another’s models. Instead, they have to define communication protocols for integrating their models and systems explicitly.
      5. ![a bounded context should be owned by only one team, a single team can own multiple bounded contexts](img/image-7.png)
5. ## Bounded Contexts in Real Life
   
   1. ### *Semantic Domains*
      
      1. It can be said that domain-driven design’s bounded contexts are based on the lexicographical notion of semantic domains. A semantic domain is defined as an area of meaning and the words used to talk about it. For example, the words monitor, port, and processor have different meanings in the software and hardware engineering semantic domains.
   2. ### *Science*
      
      1. Even though the two models can be seen as contradictory, both are useful in their suitable (bounded) contexts.
   3. Using two models, each optimized for its specific task, reflects the DDD approach to modeling business domains.
   4. Each model has its strict bounded context:
6. ## Conclusion
   
   1. Whenever we stumble upon an inherent conflict in the domain experts’ mental models, we have to decompose the ubiquitous language into multiple bounded contexts
   2. Bounded contexts decompose a system into physical components—services, subsystems, and so on

# **Chapter 4. Integrating Bounded Contexts**

1. Not only does the bounded context pattern protect the consistency of a ubiquitous language, it also enables modeling.
2. You cannot build a model without specifying its purpose—its boundary.
3. Moreover, models in different bounded contexts can be evolved and implemented independently.
4. There will always be touchpoints between bounded contexts. These are called contracts.
5. The need for contracts results from differences in bounded contexts’ models and languages.
6. ## Cooperation
   
   1. Cooperation patterns relate to bounded contexts implemented by teams with well-established communication.
   2. ### **Partnership**
      
      1. One team can notify a second team about a change in the API, and the second team will cooperate and adapt—no drama or conflicts. Both sides cooperate in solving any integration issues that might come up
      2. ![Partnership model](img/image-8.png)
      3. This pattern might not be a good fit for geographically distributed team
   3. ### **Shared Kernel**
      
      1. ![Alt text](img/image-9.png)
      2. **Shared scope**
         1. To minimize the cascading effects of changes, the overlapping model should be limited, exposing only that part of the model that has to be implemented by both bounded contexts
      3. **Implementation**
         1. If the organization uses the mono-repository approach, these can be the same source files referenced by multiple bounded contexts. If using a shared repository is not possible, the shared kernel can be extracted into a dedicated project and referenced in the bounded contexts as a linked library
      4. **When**
         1. only when integrating changes applied to the shared model by both bounded contexts will require more effort than coordinating the changes in the shared codebase
         2. The difference between the integration and duplication costs depends on the volatility of the model
         3. The more frequently it changes, the higher the integration costs will be. Therefore, the shared kernel will naturally be applied for the subdomains that change the most: the core subdomains.
         4. Finally, a shared kernel can be a good fit for integrating bounded contexts owned and implemented by the same team.
         5. A shared kernel can be used for explicitly defining the bounded contexts’ integration contracts.
7. ## Customer–Supplier
   
   1. ![Customer-Supplier](img/image-10.png)
   2. Provides a service for its customers. The service provider is “upstream” and the customer or consumer is “downstream.”
   3. both teams (upstream and downstream) can succeed independently. Consequently, in most cases we have an imbalance of power: either the upstream or the downstream team can dictate the integration contract
   4. **three patterns addressing such power differences**
      #### Conformist
      
      1. If the downstream team can accept the upstream team’s model, the bounded contexts’ relationship is called conformist
      2. the contract exposed by the upstream team may be an industry-standard, well-established model, or it may just be good enough for the downstream team’s needs and conformed by downstream team.
      
      #### Anticorruption Layer
      
      1. it can translate the upstream bounded context’s model into a model tailored to its own needs via an anticorruption layer
      
      - consumer is not willing to accept the supplier’s model.
      
      2. The anticorruption layer pattern addresses scenarios in which **it is not desirable** or **worth the effort to conform to the supplier’s model**
      
      - **When the downstream bounded context contains a core subdomain**
      - **When the upstream model is inefficient or inconvenient for the consumer’s needs**
      - **When the supplier’s contract changes often**
      
      #### Open-Host Service
      
      1. the supplier implements the translation of its internal model.
      2. the upstream supplier decouples the implementation model from the public interface
      3. Logging framework?
8. ## Separate Ways
   
   1. The last collaboration option is not to collaborate at all.
   2. The separate ways pattern should be avoided when integrating core subdomains. Duplicating the implementation of such subdomains would defy the company’s strategy to implement them in the most effective and optimized way.
9. ## Context Map
   
   1. After analyzing the integration patterns between a system’s bounded contexts, we can plot them on a context map
   
   - ![Alt text](img/image-11.png)
     - High-level design: A context map provides an overview of the system’s components and the models they implement.
     - Communication patterns: A context map depicts the communication patterns among teams
     - Organizational issues: A context map can give insight into organizational issues.
10. ## Conclusion
    
    1. Bounded contexts are not independent. They have to interact with one another. The following patterns define different ways bounded contexts can be integrated
       - **Partnership**
         Bounded contexts are integrated in an ad hoc manner.
       - **Shared kernel**
         - Two or more bounded contexts are integrated by sharing a limited overlapping model that belongs to all participating bounded contexts.
       - **Conformist**
         - The consumer conforms to the service provider’s model.
       - **Anticorruption layer**
         - The consumer translates the service provider’s model into a model that fits the consumer’s needs.
       - **Open-host service**
         - The service provider implements a published language—a model optimized for its consumers’ needs.
       - **Separate ways**
         - It’s less expensive to duplicate particular functionality than to collaborate and integrate it.

# **Chapter 5. Implementing Simple Business Logic**

- Business logic is the most important part of software. It’s the reason the software is being implemented in the first place

2. # Transaction Script [Transactional Behaviour]
   
   1. A system’s public interface can be seen as a collection of business transactions that consumers can execute
   2. The pattern organizes the system’s business logic based on procedures, where each procedure implements an operation that is executed by the system’s consumer via its public interface.
   3. the system’s public operations are used as encapsulation boundaries.
   
   ## Implementation
   
   1. The only requirement procedures have to fulfill is transactional behavior. Each operation should either succeed or fail but can never result in an invalid state.
   2. if execution of a transaction script fails at the most inconvenient moment, the system should remain consistent—either by rolling back any changes it has made up until the failure or by executing compensating actions.
   3. Below example shows ACID that we should remain system as invalid state.
   4. But its only applicable in relational local database.

   - **Problem** and **Solution**
    ```
    public class LogVisit
    {
        ...
        public void Execute(Guid userId, DataTime visitedOn)
        {
            _db.Execute("UPDATE Users SET last_visit=@p1 WHERE user_id=@p2",
                  visitedOn, userId);
            _db.Execute(@"INSERT INTO VisitsLog(user_id, visit_date)
                           VALUES(@p1, @p2)", userId, visitedOn);
        }
    }
    public class LogVisit
    {
        ...

        public void Execute(Guid userId, DataTime visitedOn)
        {
            try
            {
                _db.StartTransaction();

                _db.Execute(@"UPDATE Users SET last_visit=@p1
                        WHERE user_id=@p2",
                        visitedOn, userId);

                _db.Execute(@"INSERT INTO VisitsLog(user_id, visit_date)
                        VALUES(@p1, @p2)",
                        userId, visitedOn);

                _db.Commit();
            } catch {
                _db.Rollback();
                throw;
            }
        }
    }

    ```
   ## Distributed transactions
   1. In modern distributed systems, it’s a common practice to make changes to the data in a database and then notify other components of the system about the changes by publishing messages into a message bus.
   2. As in the previous example, any failure occurring after line 7 but before line 9 succeeds will corrupt the system’s state. Unfortunately, fixing the issue is not as easy as in the previous example.
   ```
   public void Execute(Guid userId, DataTime visitedOn)
   {
      _relational.Execute("UPDATE Users SET last_visit=@p1 WHERE user_id=@p2", visitedOn,userId);
      _messageBus.Publish("VISITS_TOPIC", new { UserId = userId, VisitDate = visitedOn });
   }

   ```
   ### Implicit distributed transactions
   1. ![Alt text](img/image-12.png)
   2. if below method is part of a REST service and there is a network outage.
   3. if any failure case, client migght retry network call again it can make system as invalid state. There is not simple fix for this.
   4. one way to ensure transactional behavior is to make the operation ***idempotent***:
   5. another way, we can ask the consumer to pass the value of the counter. To supply the counter’s value, the caller will have to read the current value first, increase it locally, and then provide the updated value as a parameter.
   ```
   public void Execute(Guid userId)
   {
       _db.Execute("UPDATE Users SET visits=visits+1 WHERE user_id=@p1",
                   userId);
   }
   ```
   ### When to Use Transaction Script
   1. in extract-transform-load (ETL) operations, each operation extracts data from a source, applies transformation logic to convert it into another form, and loads the result into the destination store.
   - ![ETL: ](img/image-13.png)
   2. The transaction script pattern naturally fits supporting subdomains where, by definition, the business logic is simple.
   3. this pattern won’t cope with the high complexity of a core subdomain’s business logic.
   4. Sometimes the pattern is even treated as an antipattern. 

2. # Active Record [anemic domain model antipattern]
- Notes: active record supports cases where the business logic is simple. however, the business logic may operate on more complex data structures.
- ![Alt text](img/image-14.png)
   
   ## Implementation
   
   1. Consequently, this pattern uses dedicated objects, known as active records, to represent complicated data structures.
   2. These objects also implement data access methods for creating, reading, updating, and deleting records—the so-called CRUD operations.
   3. As a result, the active record objects are coupled to an object-relational mapping (ORM) or some other data access framework.
   4. As in the previous pattern, the system’s business logic is organized in a transaction script. The difference between the two patterns is that in this case, instead of accessing the database directly, the transaction script manipulates active record objects.
   5. The pattern’s goal is to encapsulate the complexity of mapping the in-memory object to the database’s schema
   6. being responsible for persistence, the active record objects can contain business logic; for example, validating new values assigned to the fields, or even implementing business-related procedures that manipulate an object’s data.
   
   ```
   public void Execute(userDetails)
   {
      try
      {
          _db.StartTransaction();
          var user = new User();
          user.Name = userDetails.Name;
          user.Email = userDetails.Email;
          user.Save();
          _db.Commit();
      } catch {
          _db.Rollback();
          throw;
      }
   }
   ```
  
   ## When to Use Active Record
  
   1. Because an active record is essentially a transaction script that optimizes access to databases, this pattern can only support relatively simple business logic, such as CRUD operations, which, at most, validate the user’s input.
   2. The difference between the patterns [Transaction and Active Record] is that active record addresses the complexity of mapping complicated data structures to a database’s schema.
     - Active Record we used mapped database objects ***ORM***.
     - Transaction Pattern we directly access the database.
  3. The active record pattern is also known as an anemic domain model antipattern;

# **Chapter 6. Tackling Complex Business Logic**

- The domain model pattern
- The pattern is “domain model,” and the aggregates and value objects are its building blocks.
  
   ## Domain Model
  
   1. For complex business logic
   2. instead of CRUD interfaces, we deal with complicated state transitions, business rules, and invariants: rules that have to be protected at all times.
   ## Implementation
   
   1. A domain model is an object model of the domain that incorporates both behavior and data.1 DDD’s tactical patterns—aggregates, value objects, domain events, and domain services—are the building blocks of such an object model.
   2. ### Complexity
      1. objects implementing business logic without relying on or directly incorporating any infrastructural components or framework
   3. ### Ubiquitous language
      1. this pattern allows the code to “speak” the ubiquitous language and to follow the domain experts’ mental models.
   ## Building Blocks
   1. ### **Value object**
      1. A value object is an object that can be identified by the composition of its values.
         2. no explicit identification field is needed to identify colors.
         3. two instances of the same color must have the same values
         4. Changing the value of one of the fields will result in a new color
         -   ```
            class Color
            {
                  int _red;
                  int _green;
                  int _blue;
            }
            ```
         5. ![id field makes a bug not only reduntant!!](img/image-15.png)

      #### **Ubiquitous language**
         - Relying exclusively on the language’s standard library’s primitive data types—such as strings, integers, or dictionaries—to represent concepts of the business domain is known as the primitive obsession4 code smell.
         - ![Primitive obsession code smell](img/image-16.png)
         - the system cannot trust the user to always supply correct values, and as a result, the class has to validate all input fields.
         -  It will become even more challenging in the future, when the codebase will be evolved by other engineers.
         - ![With Value objects](img/image17.png)
         1. Notice the increased clarity. Take, for example, the country variable.
         2. There is no need to validate the values before the assignment, as the validation logic resides in the value objects themselves.
         3. A value object’s behavior is not limited to mere validation
         4. brightest when they centralize the business logic that manipulates the values
         - Compared to an integer-based value, the Height value object both makes the intent clear and decouples the measurement from a specific measurement unit.
         - ![Alt text](img/image18.png)
         - The PhoneNumber value object can encapsulate the logic of parsing a string value, validating it, and extracting different attributes of the phone number; for example, the country it belongs to and the phone number’s type—landline or mobile:
         - ![Alt text](img/image19.png)
         - As you can see in the preceding examples, value objects eliminate the need for conventions
         - makes using the object model less error prone and more intuitive.

      #### **Implemantation**
         - Since a change to any of the fields of a value object results in a different value, value objects are implemented as immutable objects. A change to one of the value object’s fields conceptually creates a different value—a different instance of a value object.
         - ![Alt text](img/ximage.png)
         - Since the equality of value objects is based on their values rather than on an id field or reference, it’s important to override and properly implement the equality checks
         - ![Alt text](img/ximage-1.png)
      #### **When to use value objects**
         1. The simple answer is, whenever you can. Not only do value objects make the code more expressive and encapsulate business logic that tends to spread apart, but the pattern makes the code safer. 
         2. to introduce a value object is when modeling money and other monetary values. 
         3. Relying on primitive types to represent money not only limits your ability to encapsulate all money-related business logic in one place, but also often leads to dangerous bugs, such as rounding errors and other precision-related issues.
   2. ### **Entities**
      1. An entity is the opposite of a value object. It requires an explicit identification field to distinguish between the different instances of the entity.
      2. Contrary to value objects, entities are not immutable and are expected to change. Another difference between entities and value objects is that value objects describe an entity’s properties.
      - ![Entitiy with an Id](img/ximage-2.png)
   3. ### **Aggregates** 
      1. An aggregate is an entity. it requires an explicit identification field and its state is expected to change during an instance’s lifecycle.
      2. **The goal of the pattern is to protect the consistency of its data.**
      3. Since an aggregate’s data is mutable, there are implications and challenges that the pattern has to address to keep its state consistent at all times
      #### Consistency enforcement
         1. Since an aggregate’s state can be mutated, it creates an opening for multiple ways in which its data can become corrupted.
         2. the aggregate pattern draws a clear boundary between the aggregate and its outer scope: the aggregate is a consistency enforcement boundary. The aggregate’s logic has **to validate all incoming modifications** and **ensure that the changes do not contradict its business rules.**
         3. The state-modifying methods exposed as an aggregate’s public interface are often referred to as commands, as in “a command to do something.”
         4.  A command can be implemented in two ways.
         - ![with params](img/ximage-3.png)
         - ![with param objects](img/ximage-4.png)
         5. An aggregate’s public interface is responsible for validating the input and enforcing all of the relevant business rules and invariants.
         6. This strict boundary also ensures that all business logic related to the aggregate is implemented in one place: the aggregate itself.
         7. This makes the application layer [Service Layer] that orchestrates operations on aggregates rather simple
         - ![Service layer finds the ticket, execute the command(or business) and return the result](img/ximage-5.png)
         8.  If multiple processes are concurrently updating the same aggregate, we have to prevent the latter transaction from blindly overwriting the changes committed by the first one.
         9. Hence, the database used for storing aggregates has to support concurrency management. In its simplest form, an aggregate should hold a version field that will be incremented after each update:
         - ![Alt text](img/ximage-6.png)
         10. ![Alt text](img/ximage-7.png)
         11. document databases lend themselves more toward working with aggregates.
         12.  it’s crucial to ensure that the database used for storing an aggregate’s data supports concurrency management.
      #### *Transaction boundary*
         1. Since an aggregate’s state can only be modified by its own business logic, the aggregate also acts as a transactional boundary.
         2. All changes to the aggregate’s state should be committed transactionally as one atomic operation. If an aggregate’s state is modified, either all the changes are committed or none of them is.
         3. no system operation can assume a multi-aggregate transaction. A change to an aggregate’s state can only be committed individually, one aggregate per database transaction.
      #### *Hierarchy of entities*
         1. we don’t use entities as an independent pattern, only as part of an aggregate.
         2. There are business scenarios in which multiple objects should share a transactional boundary
         3. DDD prescribes that a system’s design should be driven by its business domain.
         - ![Alt text](img/ximage-8.png)
         4. The hierarchy contains both entities and value objects, and all of them belong to the same aggregate if they are bound by the domain’s business logic.
         5. That’s why the pattern is named “aggregate”: it aggregates business entities and value objects that belong to the same transaction boundary.
         6. To support changes to multiple objects that have to be applied in one atomic transaction, the aggregate pattern resembles a hierarchy of entities, all sharing transactional consistency,
         7. The aggregate ensures that all the conditions are checked against strongly consistent data, and it won’t change after the checks are completed by ensuring that all changes to the aggregate’s data are performed as one atomic transaction.
      #### *Referencing other aggregates*
         1.  Only the information that is required by the aggregate’s business logic to be strongly consistent should be a part of the aggregate. All information that can be eventually consistent should reside outside of the aggregate’s boundary;
         - ![Alt text](img/ximage-9.png)
         2. keep the aggregates as small as possible and include only objects that are required to be in a strongly consistent state by the aggregate’s business logic:
         - ![Alt text](img/ximage-10.png)
         -  the Ticket aggregate references a collection of messages, which belong to the aggregate’s boundary. On the other hand, the customer, the collection of products that are relevant to the ticket, and the assigned agent do not belong to the aggregate and therefore are referenced by its ID.
         - The reasoning behind referencing external aggregates by ID is to reify that these objects do not belong to the aggregate’s boundary, and to ensure that each aggregate has its own transactional boundary
      #### **Aggregate Root**
         1. Since an aggregate represents a hierarchy of entities, only one of them should be designated as the aggregate’s public interface
         2. ![Application Layer, Service Layer access the root and change internal state of the aggregate](img/ximage-11.png)
         ![Message Entity changed by application layer by thourgh aggreagete root Ticket](img/ximage-12.png)
         3. In addition to the aggregate root’s public interface, there is another mechanism through which the outer world can communicate with aggregates: domain events.
      #### **Domain Events**
         1. A domain event is a message describing a significant event that has occurred in the business domain
         2. already happened, their names should be formulated in the past tense
         3. Make sure the names of the domain events succinctly reflect exactly what has happened in the business domain.
         4. Domain events are part of an aggregate’s public interface. An aggregate publishes its domain events.
         5. Other processes, aggregates, or even external systems can subscribe to and execute their own logic in response to the domain events
         6. ![Alt text](img/ximage-13.png)
      #### **Ubiquitous language**
         1. Last but not least, aggregates should reflect the ubiquitous language. The terminology that is used for the aggregate’s name, its data members, its actions, and its domain events all should be formulated in the bounded context’s ubiquitous language
   4. ### **Domain Services**
      1. You may encounter business logic that either doesn’t belong to any aggregate or value object, or that seems to be relevant to multiple aggregates. In such cases, domain-driven design proposes to implement the logic as a domain service.
      2. A domain service is a stateless object that implements the business logic.
      3. such logic orchestrates calls to various components of the system to perform some calculation or analysis.
      4. Domain services make it easy to coordinate the work of multiple aggregates.
      5. it is important to always keep in mind the aggregate pattern’s limitation of modifying only one instance of an aggregate in one database transaction. Domain services are not a loophole around this limitation
      6. Instead, domain services lend themselves to implementing calculation logic that requires reading the data of multiple aggregates
      7.  It is just a stateless object used to host business logic.
   ## Conclusion
      1. Value objects
         Concepts of the business domain that can be identified exclusively by their values and thus do not require an explicit ID field. Since a change in one of the fields semantically creates a new value, value objects are immutable.

         Value objects model not only data, but behavior as well: methods manipulating the values and thus initializing new value objects.

      2. Aggregates
         A hierarchy of entities sharing a transactional boundary. All of the data included in an aggregate’s boundary has to be strongly consistent to implement its business logic.

         The state of the aggregate, and its internal objects, can only be modified through its public interface, by executing the aggregate’s commands. The data fields are read-only for external components for the sake of ensuring that all the business logic related to the aggregate resides in its boundaries.

         The aggregate acts as a transactional boundary. All of its data, including all of its internal objects, has to be committed to the database as one atomic transaction.

         An aggregate can communicate with external entities by publishing domain events—messages describing important business events in the aggregate’s lifecycle. Other components can subscribe to the events and use them to trigger the execution of business logic.

      3. Domain services
         A stateless object that hosts business logic that naturally doesn’t belong to any of the domain model’s aggregates or value objects.

# **Chapter 7. Modeling the Dimension of Time**
 
- The event-sourced domain model pattern is based on the same premise as the domain model pattern
- value objects, aggregates, and domain events.
- The difference between these implementation patterns lies in the way the aggregates’ state is persisted
- The event-sourced domain model uses the **event sourcing pattern** to manage the aggregates’ states

## Event Sourcing
   1. it’s crucial to analyze the data and optimize the process based on the experience
   2.  One of the ways to fill in the missing information(history of a state changes) is to use event sourcing.
   3.  an event sourcing–based system persists events documenting every change in an aggregate’s lifecycle.
   #### Search
   - ![Alt text]img/2image.png)
   - ![History And Search]img/2image-1.png)
   #### Analysis
   - ![And other states]img/2image-2.png)
   - ![Current Status]img/2image-3.png)

   #### Source of Truth
   - For the event sourcing pattern to work, all changes to an object’s state should be represented and persisted as events. These events become the system’s source of truth (hence the name of the pattern). 
   - ![Alt text]img/2image-4.png)
   - The accepted name for the database that is used for persisting events is **event store**.

   #### Event Store
   - The event store should not allow modifying or deleting the events2 since it’s append-only storage. 
   - ![Alt text]img/2image-5.png)
   - The expectedVersion argument in the Append method is needed to implement optimistic concurrency management

## Event-Sourced Domain Model
   -  **event-sourced domain model rather than just event sourcing**. 

   1. The event-sourced domain model uses domain events exclusively for modeling the aggregates’ lifecycles. All changes to an aggregate’s state have to be expressed as domain events.
   2. **Each operation on an event-sourced aggregate follows this script:**
   - Load the aggregate’s domain events.

   - Recreate a state representation—project the events into a state representation that can be used to make business decisions.

   - Execute the aggregate’s command to execute the business logic, and consequently, produce new domain events.

   - Commit the new domain events to the event store.

   1. ![Load Events and Create Ticket]img/2image-6.png)
   2. ![Ticket Aggregate recreate logic in constructor]img/2image-7.png)
   - instantiates an instance of the state projector class, TicketState, and sequentially calls its AppendEvent method for each of the ticket’s events:
   3. ![Passes the events to TicketState, so we can generate current state of ticket, with the overloaded apply method]img/2image-8.png)
   4. ![Escalation Business logic]img/2image-9.png)
   - Business not change the ticket aggregate state instead it creates a TicketEscalated Domain events. We don't alter the aggregate domain model, we send domain events to achieve Event-Source Domain Model.
   5. ![Apply Methods with different Domain Event Type overloaded]img/2image-10.png)

   #### Advantages
   - Compared to the more traditional model, in which the aggregates’ current states are persisted in a database, the event-sourced domain model requires more effort to model the aggregates
   1. Time traveling
      - You can always recreate a past states of the aggregate.
      - Business perpective debugging
   2. Deep insight
      - Event sourcing provides deep insight into the system’s state and behavior.
      - you can always add new projections that will leverage the existing events’ data to provide additional insights.
   3. Audit log
      - The persisted domain events represent a strongly consistent audit log of everything that has happened to the aggregates’ states.
   4. Advanced optimistic concurrency management
      - The classic optimistic concurrency model raises an exception when the read data becomes stale
   #### Disadvantages
   - So far it may seem that the event-sourced domain model is the ultimate pattern for implementing business logic and thus should be used as often as possible.
   1. Learning curve
      - The obvious disadvantage of the pattern is its sharp difference from the traditional techniques of managing data.
   2. Evolving the model
      - Evolving an event-sourced model can be challenging. The strict definition of event sourcing says that events are immutable.
   3. Architectural complexity

   ### Frequently Asked Questions
      1. Performance 
         - In most systems, the performance hit will be noticeable only after 10,000+ events per aggregate.

      2. Deleting Data
         - GPDR This need can be addressed with the forgettable payload pattern: all sensitive information is included in the events in encrypted form
      3. Why Can’t I Just…?

   ### Conclusion
   1. In an event-sourced domain model, all changes to an aggregate’s state are expressed as a series of domain events.
   2. That’s in contrast to the more traditional approaches in which a state change just updates a record in the databases.
   3. The resultant domain events can be used to project the aggregate’s current state. Moreover, the event-based model gives us the flexibility to project the events into multiple representation models, each optimized for a specific task.
   4. This pattern fits cases in which it’s crucial to have deep insight into the system’s data, whether for analysis and optimization or because an audit log is required by law
   5. This chapter completes our exploration of the different ways to model and implement business logic.

# **Chapter 8. Architectural Patterns**

       
