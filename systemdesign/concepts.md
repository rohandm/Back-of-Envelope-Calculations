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

## ACID properties of database transactions
### Atomicity
* All or none of the operations in a transaction should succeed.
### Consistency
* Transaction should get the data from one consistenct state to another. For eg. in a finance app, if A with balance 500 transfers 100 to B with balance 500 then the final balances for A and B should be 400 and 600 respectively.
### Isolation
* Concurrent transactions should not be affected by each other and end result should be same as if they were executed sequentially.
### Durability
* Changes made by any transaction should be durable and not affected by failures.

## Problems in DB transactions
### Dirty reads
* Uncommited data from one transaction is read by another transaction.
### Dirty writes
* Transaction uses uncommited data from another transaction to make updates which are persisted even if the other transaction fails or is rolled back.
### Non-repeatable reads
* Data read for same query fired multiple times by a transaction is different due to updates made by another transaction.
### Phantom reads
* Happens mainly if multiple range queries are fired by a transaction while another transaction is inserting/deleting rows which satisfy that range.

## Transaction locks
### Read (Shared) lock
* Allows other transactions to get shared lock.
### Write (Exclusive) lock
* Doesn't allow other transactions to get either shared or exclusive lock.
  
## Transaction isolation levels
## Read uncommited
* Mainly used for read transactions.
## Read commited
* Immediately releases read locks while keeps write locks till end of transaction.
## Repeatable reads
* Keeps both read and write locks until end of transaction one acquired.
## Serializable reads
* Takes shared range lock over the entire range of rows being queried and releases it at end of transaction.
  
| Isolation level | Dirty read/write | Non-repeatable reads | Phanton reads |
|-----------------|------------------|----------------------|---------------|
| Read uncommited | Yes | Yes | Yes |
| Read commited | No | Yes | Yes |
| Repeatable read | No | No | No |
| Serializable | No | No | No |

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

## Availability
* Percentage of time the service is accessible and respond normally to clients.
* Availability = (<Total_time> - <Total_time_when_service_is_down>)/<Total_time>
* Availabilty percentages are normally measured as nines (eg. 99% available)

| Number of nines | Availability% | Downtime per year | Downtime per month | Downtime per week |
|-----------------|---------------|-------------------|--------------------|------------------|
| 1 | 90% | 36.5 days | 72 hours | 16.8 hours |
| 2 | 99% | 3.65 days | 7.2 hours | 1.68 hours |
| 3 | 99.9% | 8.76 hours | 43.8 min | 10.1 min |
| 4 | 99.99% | 52.56 min | 4.32 min | 1.01 min |
| 5 | 99.999% | 5.26 min | 25.9 sec | 6.05 sec |
| 6 | 99.9999% | 31.5 sec | 2.59 sec | 0.6 sec |
