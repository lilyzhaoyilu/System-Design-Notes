# Chapter 5 Replica

**Replication** means keeping a copy of the same data on multiple machines that are connected via a network.

\*\*\*\*

## Reasons to replicate

* To keep data geographically close to your users \(and thus **reduce access latency**\) 
* To allow the system to continue working even if some of its parts have failed \(and thus **increase availability/ Scalabiltiy**\) 
* To scale out the number of machines that can serve read queries \(and thus increase **read throughput/ High Availibity**\)
  * **Throughput吞吐量**: the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size.
  * offer disconnected operation

## Difficulties and Methods

All of the difficulty in replication lies in **handling changes** to replicated data. Synchronous/ Async, how to handle failures

### 1.Single-leader\(active/passive or master–slave replication\)

**Replica**: each node that stores a copy of the database

Every write to the database needs to be processed by every replica.

One leader, many followers.

User reads from any of them, but only writes through the leader. 

#### synchronously model or asynchronously model

|  | Synchronous | Async |
| :--- | :--- | :--- |
| Advantage | followers are consistent with the leader | does not block |
| Disadvantage | if a follower does not respond, the write cannot processed. The leader has to block all other writes and wait for it.  | If the leader is down, all the new writes are gone/ cannot write anything new.  |
| Notes | In reality, synch replication usually means one follower is synchronous and others are async. If one is slow/ does not respond, one of the async will be made to sync. This guarantees to have at least two nodes with up-to-date copies.  _semi-synchronous_  | often |

#### Setting Up New Followers / Scale Up

take snapshots and copy, and then get all the new logs from master. 

### 2.Multi-leader\(master–master or active/active replication\)

Replication still happens in the same way: **each node that processes a write must forward that data change to all the other nodes.**

This works good for multi-datacenter operation where each center has one leader. 

Advantages for multi-datacenter:

* 
<table>
  <thead>
    <tr>
      <th style="text-align:left">&lt;b&gt;&lt;/b&gt;</th>
      <th style="text-align:left"><b> multi-datacenter</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">advantage</td>
      <td style="text-align:left">
        <p></p>
        <ul>
          <li>tolerance of datacenter outage</li>
          <li>tolerance of network problems</li>
          <li>helps performance</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">disadvantage</td>
      <td style="text-align:left">the same data may be concurrently modified in two different datacenters,
        and those write conflicts must be resolved (conflict resolution).</td>
    </tr>
  </tbody>
</table>

### **3.**Leaderless Replication / Dynamo-style

every node can be written and queried. 

Read request often sent to several nodes in parallel and version numbers are used to determine which value is newer. 

Methods to fix stale values: 1\) read repair. When a read request finds out different versions, it gets repaired. 2\)anti-entropy process: a program running background looking for stale data

