#Backgroup:
  there are 3 tables: Group, account and groupmember table.
  group can contains account and  another group as member
#Requirement:
 given a gropu id, given all the memenber including nested groups. 
# Risk
there could be cross nested group, group A -> Group B- > group c- Group A, it will cause ifinite loop

Solution 1,  use SQL CTE to implement recuisre.  very slow, based on my test against prd, it run to 1 mintues.
Solution 2,  loop.  super fast, it runs in less than 1 second.

so always Avoid recuisre in Azure SQL!

```sql

CREATE PROC dbo.GetsubWorkers1(@root int)


AS


CREATE TABLE #subWorkers (


  EmployeeID INT NOT NULL ,


  lvl   INT NOT NULL)


 DECLARE @lvl INT;


 SET @lvl = 0;  


 INSERT INTO #subWorkers(EmployeeID, lvl)


  SELECT EmployeeID, @lvl FROM dbo.Employees WHERE EmployeeID = @root;


  WHILE @@rowcount > 0        


  BEGIN


    SET @lvl = @lvl + 1;   


     INSERT INTO #subWorkers(EmployeeID, lvl)


      SELECT C.EmployeeID, @lvl


      FROM #subWorkers  S         


      INNER JOIN dbo.Employees  C


      ON S.lvl = @lvl – 1  AND C.BossID = S.EmployeeID;


  END


  SELECT * FROM #subWorkers


GO
```
(http://blogs.microsoft.co.il/srldba/2008/09/01/recursive-queries-performance-iterative-loop-vs-recursive-cte/)
