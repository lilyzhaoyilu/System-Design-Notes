# Chapter 6 Partitioning

partition, sharding...

**Goal**: **to spread the data and the query load evenly across nodes**. 

If partition is unfair, some partitions have more data or queries than others, we call it **skewed**.

**Hot spot**: a partition with disproportionately high load

#### Partitioning by key range

the downside of key range partitioning is that certain access patterns can lead to hot spots.

#### Partitioning by Hash of Key

The partition boundaries can be evenly spaced, or they can be chosen pseudorandomly \(in which case the technique is sometimes known as consistent hashing\).

It solves the skewed and hotspot problem better but **lose the ability to do efficient range queries**. 

Cassandra achieves a compromise between the two partitioning strategies \[11, 12, 13\]. A table in Cassandra can be declared with a compound primary key consisting of several columns. Only the first part of that key is hashed to determine the partition, but the other columns are used as a concatenated index for sorting the data in Cassandra’s SSTables.

**Partitioning and Secondary Indexes**

A secondary index usually doesn’t identify a record uniquely but rather is a way of searching for occurrences of a particular value

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">local Index</th>
      <th style="text-align:left">Global Index / term partition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">ad</td>
      <td style="text-align:left">
        <p>each partition maintains its own secondary indexes, covering only the
          documents in that partition.</p>
        <p></p>
      </td>
      <td style="text-align:left">client only needs to make a request to the partition containing the term
        that it wants</td>
    </tr>
    <tr>
      <td style="text-align:left">dis</td>
      <td style="text-align:left">If user performs a search, the query is sent to all partitions, and combined
        results are sent back</td>
      <td style="text-align:left">writes are slower and more complicated, because write a single document
        may affect multiple partitions of index(every term in the document might
        be on a different partition, on a different node)</td>
    </tr>
  </tbody>
</table>

Techniques for routing queries



