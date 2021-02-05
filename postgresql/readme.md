# 1 Non repeatable read
Isolation level = READ COMMITED
#### Connection1
 - create table revenue (game_name varchar(32), revenue int);
 - insert into revenue values ('game1', 10);
 - commit;
 - select * from revenue;
 - Result: game1 10

#### Connection2
 - select * from revenue;
 - Result: game1 10
 
#### Connection1
 - update revenue set revenue = 15 where game_name = 'game1'
 - commit
#### Connection2
 - select * from revenue;
 - Result: game1 15

# 2 Dirty read
Isolation level = READ UNCOMMITED
#### Connection 1
 - create table revenue1 (game_name varchar(32), revenue int);
 - insert into revenue1 values ('game1', 10);
 - select * from revenue1;
 - Result: game1, 10
 
#### Connection 2
  - select * from revenue1;
  - Result: table doesn't exist
#### Connection 1
  - COMMIT;
#### Connection 2:
 - select * from revenue1;
 - Result: game1, 10
 
## Dirty read - Conclustion
  - read uncommited works as read commited, we can't reproduce dirty read
  - It described here https://www.postgresql.org/docs/9.5/transaction-iso.html
  
# Lost update
#### Connection 1
```
delete from revenue;
insert into revenue values ('game1', 10);
insert into revenue values ('game1', 9);
commit;

select * from revenue;
--game1	10
--game1	9

BEGIN;
UPDATE revenue SET revenue = revenue + 1;
```

#### Connection 2
```DELETE FROM revenue WHERE revenue = 10;```

#### Connection1 
commit;

#### Connection2
```select * from revenue;
--game1	11
--game1	10
```

##Lost update
So here we have a case when we lost update for the another transaction :(
When I did it usint Serializable mode, I have got an issue from the connection 2 when I was trying delete

 


 
 
  
  
  

 