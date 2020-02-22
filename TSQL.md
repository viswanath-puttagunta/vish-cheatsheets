###Get the city with max len and min len
**Partition By, Table/independent Variable usage **

DECLARE @CLSV TABLE(CITY VARCHAR(256), CITY_LEN INT, RANK INT)


/*Now insert the sorted/ranked table into this variable*/
INSERT INTO @CLSV
SELECT CL.CITY, CL.CITY_LEN, ROW_NUMBER() OVER(PARTITION BY CL.DUMMY ORDER BY CL.CITY_LEN DESC) AS RANK FROM
    (SELECT C.CITY, LEN(C.CITY) AS CITY_LEN, 'A' AS DUMMY
            FROM
                (SELECT DISTINCT(CITY) AS CITY FROM STATION) AS C
     )AS CL;

DECLARE @NumRows int;

-- Initialize the variable.
SELECT @NumRows = COUNT(*) FROM @CLSV;

SELECT CITY, CITY_LEN FROM @CLSV WHERE RANK = 1;
SELECT CITY, CITY_LEN FROM @CLSV WHERE RANK = @NumRows;
