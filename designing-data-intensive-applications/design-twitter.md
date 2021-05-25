# Design Twitter

### Guidance

4S analysis

* Scenario - ask features
* Service - split, application, module
* Storage - Schema, data, file system
* Scale - sharding, optimize, special case

### Scenario 

1. brainstorm all possible
2. choose the main features
3. analysis and predict

   1. concurrent user - 并发用户

      1. daily active
      2. peak ~= average concurrent user \* 3
      3. is it fast growing?

      2. Read QPS - query per second

      3. Write QPS

      4. Write more or read more?

### Service - can draw to illustrate

split the system into small services

1. user service - log in, register
2. tweet service - post, news feed..
3. media service - upload image, video
4. friendship service - follow, unfollow

### Storage - can expand the storage beneath the service

1. **select suitable storage for each service**
2. **figure out schema**

for 1 we choose

* SQL for user table
* NoSQL for tweets and social graphs
* File system for media files

for 2 to design schema



### Bonus question : the design of news feed

#### Pull model - real time, still faster

When a user wants to check his/her news feed, they pull from the database and merge the propriate tweets. 

user \(get news feed\) -&gt; web server -&gt; \(fetch following list\) -&gt; webserver-&gt; \(fetch each person's tweet\) -&gt; merge and return to user

Time complicity: N\(following\) N \* readDB + merging\(really quickly compared to database read\)

disadvantage: it is slow 

#### Push Model - not really time senstive

Have a list to store users' tweets. Whenever a person on the list tweets, the list got updated\( fan-out\). Whenever the reader wants to read, just get the top 100 from the list

Time complicity: 1 DB read, write:  N \(fan number\) \* DB write \(can async\)

disadvantage: DB has to write more, and an unlucky follower who follows JB might need hours to see the tweet \(waiting for fan out\)

