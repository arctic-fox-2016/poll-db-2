//Insert Voters
INSERT INTO voters (first_name, last_name, gender, party, created_at, updated_at) VALUES ("Andrew", "Tan", "male", 12, DATE('NOW'),DATE('NOW'));

//Insert Votes
INSERT INTO voters (first_name, last_name, gender, party, created_at, updated_at) VALUES ("Andrew", "Tan", "male", 12, DATE('NOW'),DATE('NOW'));

//Checking
INSERT INTO voters (first_name, last_name, gender, party, created_at, updated_at) VALUES ("Andrew", "Tan", "male", 12, DATE('NOW'),DATE('NOW'));

//Deleting NJ
DELETE FROM congress_members WHERE id = 102;

//Adding Trump
INSERT INTO congress_members (name, party, location) VALUES ("Donald Trump", "Parteeeh", "NJ")

//Delete non Democrat and Republican
DELETE FROM voters WHERE party <> "republican" AND party <> "democrat";
