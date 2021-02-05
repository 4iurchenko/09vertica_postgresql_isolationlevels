# Default login
Default DB Name - docker
Default User - dbadmin
Default Password (NO PASSWORD)


# SQLs

CREATE schema test;

CREATE TABLE test.games
(
    game_id int,
    game_name varchar(32)
);

drop table test.games

INSERT INTO test.games (game_id, game_name) VALUES (1,'Game1');
COMMIT;

SELECT * FROM test.games;

# Conclusion
Vertica doesn't support read uncommited