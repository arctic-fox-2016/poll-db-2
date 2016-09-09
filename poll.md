1.

= Insert into Voters =
INSERT INTO VOTERS(FIRST_NAME, LAST_NAME, GENDER, PARTY, PARTY_DURATION, AGE, MARRIED, CHILDREN_COUNT, HOMEOWNER, EMPLOYED,CREATED_AT,UPDATED_AT)
VALUES ("Ari","Adiprana","male","democrat",40,20,1,2,1,1,DATE('NOW'),DATE('NOW'));

= Insert into Votes =
INSERT INTO votes (voter_id, politician_id, created_at, updated_at) VALUES (5001, 499, DATE('NOW'), DATE('NOW'));
INSERT INTO votes (voter_id, politician_id, created_at, updated_at) VALUES (5001, 492, DATE('NOW'), DATE('NOW'));
INSERT INTO votes (voter_id, politician_id, created_at, updated_at) VALUES (5002, 499, DATE('NOW'), DATE('NOW'));
INSERT INTO votes (voter_id, politician_id, created_at, updated_at) VALUES (5002, 492, DATE('NOW'), DATE('NOW'));

2.
--buat politisi baru
INSERT INTO congress_members (name, party, location, grade_1996, grade_current, years_in_congress, dw1_score, created_at, updated_at)
VALUES ("Donald Trump", "R", "TX", 13, 10, 0, 0.3, DATE('NOW'),DATE('NOW'));

--politisi yang mau diganti :  ID 102
--mengganti votes
UPDATE VOTES SET POLITICIAN_ID = '531' WHERE POLITICIAN_ID='102';
--MENGHAPUS ID 102 DARI DUNIA VOTE INI
DELETE FROM CONGRESS_MEMBERS WHERE ID = '102';


========== RELEASE 1 ==========

1.DELETE FROM VOTERS
    WHERE ID IN(
        SELECT VOTERS.ID
        FROM VOTERS
        INNER JOIN VOTES
          ON VOTERS.ID=VOTES.VOTER_ID
        WHERE PARTY NOT IN ('republican','democrat')
          AND VOTERS.ID IN (
            SELECT a.id
            FROM voters a
            INNER JOIN votes b
              ON b.voter_id = a.id
            GROUP BY a.id HAVING COUNT( * ) = 1)
      );

2.
DELETE FROM VOTES WHERE ID IN(SELECT VOTES.ID FROM VOTERS INNER JOIN VOTES ON VOTERS.ID=VOTES.VOTER_ID, CONGRESS_MEMBERS CM ON CM.ID=VOTES.POLITICIAN_ID WHERE VOTERS.HOMEOWNER=1 AND VOTERS.EMPLOYED=1 AND VOTERS.CHILDREN_COUNT=0 AND VOTERS.PARTY_DURATION>=3 AND CM.GRADE_CURRENT>12);

DELETE FROM VOTERS WHERE ID IN(SELECT VOTES.ID FROM VOTERS INNER JOIN VOTES ON VOTERS.ID=VOTES.VOTER_ID, CONGRESS_MEMBERS CM ON CM.ID=VOTES.POLITICIAN_ID WHERE VOTERS.HOMEOWNER=1 AND VOTERS.EMPLOYED=1 AND VOTERS.CHILDREN_COUNT=0 AND VOTERS.PARTY_DURATION>=3 AND CM.GRADE_CURRENT>12);


========== RELEASE 2 ==========

1.
UPDATE VOTES SET politician_id = 346 WHERE voter_id IN (SELECT id FROM voters WHERE age > 80 AND children_count = 0);

2.
UPDATE votes SET politician_id = (SELECT id FROM congress_members ORDER BY grade_current ASC LIMIT 1) WHERE politician_id = (SELECT id FROM congress_members ORDER BY grade_current DESC LIMIT 1);
