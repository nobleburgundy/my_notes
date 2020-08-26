# SQL Notes
## Some Examples
## Some helpful SQLs for MassMail
### Compare # of emails in the system
```sql
DECLARE @deleteIfOlderDate DATETIME = DATEADD(DAY, -90, GETDATE())
SELECT COUNT(*)
FROM MassMail.dbo.Email E
INNER JOIN MassMail.dbo.EmailJob EJ
ON EJ.jobid = E.jobid
WHERE EJ.createdon < @deleteIfOlderDate
```
### Search for the previously created and deleted job
```sql
SELECT *
FROM [MassMail].[dbo].[EmailJob]
WHERE  [jobid] = '{JobId}'
```
### EMAIL BATCH
```sql
DECLARE @deleteIfOlderDate DATETIME = DATEADD(DAY, -90, GETDATE())

SELECT Count(*)
FROM [MassMail].[dbo].[EmailBatch]
where [startedon] < @deleteIfOlderDate
```
### Show info for email batches after dateAfter
```sql
DECLARE @dateAfter DATETIME = DATEADD(DAY, -1, GETDATE())
SELECT DISTINCT TOP 10 *
FROM MassMail.dbo.Email E
INNER JOIN MassMail.dbo.EmailJob EJ
ON EJ.jobid = E.jobid
WHERE EJ.createdon > @dateAfter
```
### Count of emails with 'simulator.amazonses.com' domain in past 1000 days
```sql
DECLARE @queryStart DATETIME = DATEADD(DAY, -1000, GETDATE())
SELECT COUNT(*)
FROM MassMail.dbo.Email E
INNER JOIN MassMail.dbo.EmailJob EJ
ON EJ.jobid = E.jobid
WHERE EJ.createdon > @queryStart AND E.domainname='simulator.amazonses.com'
