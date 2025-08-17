# Database Schema Design
1. Booking.com
2. Twitter

## How to approach for designing using RDBMS ?
1. Try to ensure that there is as less `data duplicacy` as possible | Normalization.
2. RDBMS is a normalised fashion to keep data
3. data in RDBMS is stored across tables, and relations between tables are established.
4. Make sure `data redundancy` is as less as possbile
5. Understand that 2 tables can be related and hence the queries will also be impacted.

# Design Booking.com 
## Requirement analysis:
- Owners of hotels should be able to list property and rooms on platform.
- Users can book a room of a hotel for a range of dates.
- Users might want to bookmark some hotels.
- Review support system.

```
Many to Many relations
- 1 User has given many reviews.
- 1 hotel got many reviews.

In RDBMS, Table A and Table B has many-to-many relation between them,
that relation is facilitated by Table C
Table C is called JOIN Table or THROUGH Table
JOIN table contains primary key of both tables A and B
```

```
1 to many relation
Table A and Table B
A has many B's, but a B belongs to only single A.

ex: Hotel A has many rooms like room1, room2, room3.... etc
but room1 belong to Hotel A only.
```

<img width="956" height="1371" alt="image" src="https://github.com/user-attachments/assets/0e7f5efa-6017-4172-be21-1292c39c8413" />

### Should we calculate average rating of user on the go or store it elsewhere ?
- If scale of our data is low, less users are using our app, then computing on the go is not that heavy
- Storing it elsewhere is `Benificial` because it will save compute time.
- Amout of time to fetch rating decreases, latency decreases, time of calculation of rating decreases.

- When 2 people add review at same time, then there can be `concurrency` issues with average rating.
- average rating need not be instantly updated, with time it can be updated eventually.


# Design Twitter
1. Users should be able to create tweets.
2. Users should be able to like tweets
3. Users should be able to comment on tweets
4. Users should be able to like a comment
5. Users should be able to reply on a comment
6. Users should be able to follow other users
7. Users should be able to follow other trends/topics


<img width="1200" height="993" alt="image" src="https://github.com/user-attachments/assets/516a873b-9c47-4524-945f-1d26c87a69dd" />
