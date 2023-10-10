## CAP theorem
### Consistency models
#### Strict consistency or Linearizability
* Each replica node must have same view of data.
* Each read request gets value of most recent write.
* Availability and latency takes backseat.
#### Eventual consistency
* All replicas eventually return the same value but it may not be the latest.
* Ensures high availability.
#### Casual consistency
* Preserves order of casually-related (dependent) operations.
#### Sequential consistency
* Preserves order of operations specified by client.

## Spectrum of failure models
### Fail stop
* Node fails but other nodes can still detect it by communicating with it.
### Crash
* Node fails and other nodes cannot detect it.
### Send omission failures
* Node fails to send messages.
### Receive omission failures.
* Node fails to receive messages.
### Temporal failures
* Node generates correct results but might be late due to be useful due to un-synchronized clocks or bad algorithms.
### Byzantine failures
* Random node behaviour.
