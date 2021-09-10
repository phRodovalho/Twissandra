# Task 2
## Comandos Cassandra - criando  Twisssandra;

![twissandra_model](C:\Users\pheli\Desktop\2 P 2021\TopEspBD\Laboratorio3\twissandra_model.png)

```cassandra
CREATE KEYSPACE twissandra WITH replication =  {'class': 'SimpleStrategy', 'replication_factor' : 1};

Use twissandra;

create table followers (
    username text, 
    follower text, 
    since timestamp, 
    primary key (username, follower)
);

create table friends (
    username text, 
    friend text, 
    since timestamp, 
    primary key (username, friend)
);

create table users (
    password  text,
    username text,
    primary key (username)
);

create table tweets (
    tweet_id uuid,
    username text,
    body text,
    primary key (tweet_id)
);

create table timeline (
    username text, 
    time timeuuid, 
    tweet_id uuid, 
    primary key (username, time)
) with clustering order by (time desc);

create table userline (
    username text, 
    time timeuuid, 
    tweet_id uuid, 
    primary key (username, time)
) with clustering order by (time desc);
```
### 1 ao 4 - Inserindo Usuários
```cassandra
insert into users (username, password) values ('joao', '12345');
insert into users (username, password) values ('maria', '123');
insert into users (username, password) values ('jose', '1234');
insert into users (username, password) values ('rodrigo', '65432');
```

### 5  - Maria segue Joao
```cassandra
insert into followers (username, follower, since) values ('joao', 'maria', dateof(now()));
insert into friends (username, friend, since) values ('maria', 'joao', dateof(now()));
```

### 6 - Jose segue Joao
```cassandra
insert into followers (username, follower, since) values ('joao', 'jose', dateof(now()));
insert into friends (username, friend, since) values ('jose', 'joao', dateof(now()));
```

### 7 - Rodrigo segue Joao

```cassandra
insert into followers (username, follower, since) values ('joao', 'rodrigo', dateof(now()));
insert into friends (username, friend, since) values ('rodrigo', 'joao', dateof(now()));
```

### 8 - Primeiro tt

```cassandra
insert into tweets (tweet_id, username, body) values (uuid(), 'joao', 'este eh meu primeiro tweet');
insert into userline (username, time, tweet_id) values ('joao', now(), 62659c5c-666a-4be2-a175-2ff8007ee362);

insert into timeline (username, time, tweet_id) values ('jose', now(), 62659c5c-666a-4be2-a175-2ff8007ee362);
insert into timeline (username, time, tweet_id) values ('maria', now(), 62659c5c-666a-4be2-a175-2ff8007ee362);
insert into timeline (username, time, tweet_id) values ('rodrigo', now(), 62659c5c-666a-4be2-a175-2ff8007ee362);
```

### 9 - Tweet rodrigo

```cassandra
insert into tweets (tweet_id, username, body) values (uuid(), 'rodrigo', 'o Cassandra eh muito complicado!');

insert into userline (username, time, tweet_id) values ('rodrigo', now(), 010694aa-fa63-429c-bdd3-c0b867dab208);
```

### 10 - Selecionar o usuário com  username ‘joao’

```cassandra
select * from users where username = 'joao';
```

### 11 - Selecionar as pessoas que o user ‘joao’ está seguindo

	select * from friends where username = 'joao';

### 12 - Selecionar os seguidores do user ‘joao’

	select * from followers where username = 'joao';

### 13 - Selecionar os tweets das pessoas que o user ‘joao’ segue (timeline de ‘joao’)

	select * from timeline where username= 'joao';

### 14 - Selecionar os tweets do user ‘joao’ (userline de ‘joao’)

	select * from userline where username = 'joao';

### 15 - Fazer ‘jose’ deixar de seguir ‘joao’

	delete from followers where username='joao' and follower='jose';
	delete from friends where username='jose' and friend='joao';
