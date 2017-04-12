- title : Event Sources
- description : CQRS, Event Sourcing
- author : Dave Glassborow
- theme : night
- transition : default

***

## Acronym Soup 

### 3 slightly different things

- CQRS: Command Query Responsibilty Separation
- ES: Event Sources
- FRP: Functional Reactive Programming

***

### CQRS

- Separate Reads (Queries) from Writes (Commands)
- Reads vs Writes to system rarely the same rate, decouple them
- Also called [SQS - Command-query separation](https://en.wikipedia.org/wiki/Command–query_separation)
- Queries: return state and are pure
- Command: change state and returns unit

    
    type Command = 'a -> ()
    

---

### Advantages

- Optimise reads via queries, database projections, search engines, etc.
- Security e.g. read only connection string for queries
- Use Domain Model only for writes, SQL joins for reads, separate types

---

### Disadvantages

- Complexity over simple SQL data access
- What about things like `Stack.pop()` ?


***

### ES

![ES Diagram](images/turtle-event-source.png)

- Immutable append only store
- Define changes in business terms (e.g. ChangeOfAddress) not SQL
- Snapshops for performance
- Java [LMAX disruptor](https://lmax-exchange.github.io/disruptor/)


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

---


![Diagram](images/turtle-frp.png)

![Diagram](images/turtle-stream-processor.png)


---

- EventStore javascript projections
- Eventually consistent reads (e.g. via database)
- Rx, RxJava, RxJS
- Enumerable dual, sync pull vs async push
	
	
	eventSouce
	|> Observable.filter ((>) 2)
	|> Observable.map (fun x -> x*2)
	|> Observable.merge otherEvents
	|> Observable.subscribe


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
