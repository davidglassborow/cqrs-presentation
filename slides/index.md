- title : Event Sources
- description : CQRS, Event Sourcing
- author : Dave Glassborow
- theme : night
- transition : default

***

## Acronym Soup 

### 3 slightly different things

- CQRS: Command Query Responsibilty Seperation
- ES: Event Sources
- FRP: Functional Reactive Programming

***

### CQRS

- Seperate Reads (Queries) from Writes (Commands)
- Reads vs Writes to system rarely the same rate, decouple them
- Also called [SQS - Command-query seperation](https://en.wikipedia.org/wiki/Command–query_separation)
- Queries: return state and are pure
- Command: change state and returns unit

    
    type Command = 'a -> ()
    

---

### Advantages

- Optimise reads via quries, database projections, search engines, etc.
- Security e.g. read only connection string for queries
- Use Domain Model only for writes, joins for reads, seperate types

---

### Disadvantages

- Complexity over simple SQL data access
- What about things like `Stack.pop()`` ?


***

### ES

![ES Diagram](images/turtle-event-source.png)

- Immutable append only store
- Snapshops for performance
- Define changes in business terms (e.g. ChangeOfAddress) not SQL
- Java [LMAX disrupter](https://lmax-exchange.github.io/disruptor/)


---

### Advantages

* All code is stateless, easy to test.
* Advantages of append only stores:
   - Replication
   - Audit
   - History
   - Replay

---

### Disadvantages

* Can be more complex to implement than a CRUD approach
* Too much logic in command handler
* Versioning of events

***

### FRP

- Stream processing
- Like ES, but with more than one command processor
- Command handler does minimal work, downstream processors
- Combine, project, fold streams

    IObservable

---


![Diagram](images/turtle-frp.png)

![Diagram](images/turtle-stream-processor.png)


---

- EventStore javascript projections
- Eventually consistent reads (e.g. via database)

---

### Advantages

- Decouples stateful logic from other non-intrinsic logic.
- Add and remove domain logic without affecting the core command handler
- Multiple stores
- Lambda architectures 

---


### Disadvantages

- Enterprise-wide versioning issues
- Even more complicated
