**Sqllite 3**
```sql
CREATE TABLE rooms
(
     id             INTEGER PRIMARY KEY,
     room_no        TEXT NOT NULL,
     description    TEXT,
     e_unit         DOUBLE DEFAULT 0.0,
     pending_amount DOUBLE DEFAULT 0.0,
     rental_amount  DOUBLE DEFAULT 0.0,
     status         BOOLEAN DEFAULT false,
     created_at     DATETIME DEFAULT cur_timestamp,
     updated_at     DATETIME DEFAULT cur_timestamp
) 
```


