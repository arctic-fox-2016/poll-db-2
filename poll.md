CREATE TABLE congress_members (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name VARCHAR(64) NOT NULL,
  party VARCHAR(64) NOT NULL,
  location VARCHAR(64) NOT NULL,
  grade_1996 REAL,
  grade_current REAL,
  years_in_congress INTEGER,
  dw1_score REAL
, created_at DATETIME, updated_at DATETIME);
CREATE TABLE voters (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    first_name VARCHAR(64) NOT NULL,
    last_name  VARCHAR(64) NOT NULL,
    gender VARCHAR(64) NOT NULL,
    party VARCHAR(64) NOT NULL,
    party_duration INTEGER,
    age INTEGER,
    married INTEGER,
    children_count INTEGER,
    homeowner INTEGER,
    employed INTEGER,
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
  );
CREATE TABLE votes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    voter_id INTEGER,
    politician_id INTEGER,
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY(voter_id) REFERENCES voters(id),
    FOREIGN KEY(politician_id) REFERENCES congress_members(id)
  );

===============================
========== RELEASE 0 ==========
===============================

1.

= Insert into Voters =
INSERT INTO voters (first_name, last_name, gender, party, party_duration, age, married, children_count, homeowner, employed, created_at, updated_at)
VALUES ("Bos", "Rubi", "male", "libertarian", 40, 20, 1, 2, 1, 1, "2012-10-10 16:25:32 -0700", "2012-10-10 16:25:32 -0700");
INSERT INTO voters (first_name, last_name, gender, party, party_duration, age, married, children_count, homeowner, employed, created_at, updated_at)
VALUES ("Bos", "Haidar", "male", "libertarian", 40, 20, 1, 2, 1, 1, "2012-10-10 16:25:32 -0700", "2012-10-10 16:25:32 -0700");

= Insert into Votes =
INSERT INTO votes (voter_id, politician_id, created_at, updated_at)
VALUES (5001, 499, "2012-10-10 17:25:32 -0700", "2012-10-10 17:25:32 -0700");
INSERT INTO votes (voter_id, politician_id, created_at, updated_at)
VALUES (5001, 492, "2012-10-10 17:25:32 -0700", "2012-10-10 17:25:32 -0700");
INSERT INTO votes (voter_id, politician_id, created_at, updated_at)
VALUES (5002, 499, "2012-10-10 17:25:32 -0700", "2012-10-10 17:25:32 -0700");
INSERT INTO votes (voter_id, politician_id, created_at, updated_at)
VALUES (5002, 492, "2012-10-10 17:25:32 -0700", "2012-10-10 17:25:32 -0700");


2.

INSERT INTO congress_members (name, party, location, grade_1996, grade_current, years_in_congress, dw1_score, created_at, updated_at)
VALUES ("Donald Trump", "R", "TX", 13, 10, 0, 0.3, "2012-10-10 17:25:32 -0700", "2012-10-11 17:25:32 -0700");

DELETE FROM congress_members WHERE id = 504;


===============================
========== RELEASE 1 ==========
===============================

1.

DELETE FROM voters
WHERE party = "republican"
AND party = "democrat"
OR id IN
  (SELECT a.id FROM voters a
  INNER JOIN votes b ON b.voter_id = a.id
  GROUP BY a.id
  HAVING COUNT( * ) = 1);


2.

DELETE FROM voters
WHERE homeowner = 1
AND employed = 1
AND children_count = 0
AND party_duration < 3
AND id IN
  (SELECT a.id FROM voters a
  INNER JOIN votes b ON b.voter_id = a.id
  INNER JOIN congress_members c ON c.id = b.politician_id
  WHERE c.grade_current > 12
  );


===============================
========== RELEASE 2 ==========
===============================

1.

UPDATE votes
SET politician_id = 346
WHERE voter_id IN
  (SELECT id FROM voters
  WHERE age > 80
  AND children_count = 0);


2.

UPDATE votes
SET politician_id =
  (SELECT id FROM congress_members
  ORDER BY grade_current ASC
  LIMIT 1)
WHERE politician_id =
  (SELECT id FROM congress_members
  ORDER BY grade_current DESC
  LIMIT 1);
