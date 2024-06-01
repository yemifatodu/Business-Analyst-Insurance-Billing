# Business-Analyst-Insurance-Billing
 ## questionnaire 



| PHYSICIAN_ID | PHYSICIAN_FIRST_NAME | PHYSICIAN_LAST_NAME | CREDENTIALS | NPI_NUMBER  |
|--------------|----------------------|---------------------|-------------|-------------|
| 4501         | Paul                 | Atreides            | NP          | 5540025212  |
| 4502         | Gurney               | Halleck             | MD          | 9083536138  |
| 4503         | Vladimir             | Harkonnen           | MD          | 3472738009  |
| 4504         | Liet                 | Kynes               | MFM         | 2951264553  |
| 4505         | Shadout              | Mapes               | NP          | 1161350857  |
| 4506         | Thufir               | Hawat               | MD          | 5579307246  |
| 4507         | Leto                 | Atreides            | MD          | 7080808397  |
| 4508         | Feyd                 | Rautha              | MD          | 9658650083  |
| 4509         | Alia                 | Atreides            | MS          | 8900676113  |
| 4510         | Ghanima              | Atreides            | MFM         | 9337905023  |
| 4511         | Duncan               | Idaho               | NP          | 9337905123  |







| CLINIC_ID | CLINIC_NAME    | CLINIC_ADDRESS_1      | CLINIC_CITY | CLINIC_ZIP | CLINIC_STATE |
|-----------|----------------|-----------------------|-------------|------------|--------------|
| 1001      | House Atreides | 789 Ocean View Road   | Caladan     | 78644      | TX           |
| 1002      | Loyal to You   | 790 Ocean View Road   | Caladan     | 78644      | TX           |
| 1003      | House Harkonnen| 456 Baron Lane        | Giedi Prime | 78641      | TX           |
| 1004      | Fremen         | 123 Dessert Road      | Arrakis     | 78724      | TX           |






| KITID  | DATE_OF_SERVICE | RECEIVED_DATE | CARRIER | ORDERING_CLINIC_ID | ORDERING_PHYSICIAN_ID |
|--------|-----------------|---------------|---------|--------------------|-----------------------|
| 600000 | 1/1/2021        | 4/1/2021      | FEDEX   | 1001               | 4501                  |
| 600001 | 1/4/2021        | 3/4/2021      | UPS     | 1002               | 4502                  |
| 600002 | 1/7/2021        | 3/7/2021      | FEDEX   | 1003               | 4503                  |
| 600003 | 1/10/2021       | 4/10/2021     | FEDEX   | 1004               | 4504                  |
| 600004 | 1/2/2021        | 5/2/2021      | FEDEX   | 1004               | 4505                  |
| 600005 | 1/5/2021        | 2/5/2021      | UPS     | 1002               | 4506                  |
| 600006 | 1/8/2021        | 4/8/2021      | UPS     | 1001               | 4507                  |
| 600007 | 1/11/2021       | 4/11/2021     | FEDEX   | 1003               | 4508                  |
| 600008 | 1/3/2021        | 3/3/2021      | UPS     | 1001               | 4509                  |
| 600009 | 1/6/2021        | 3/6/2021      | FEDEX   | 1001               | 4510                  |
| 600010 | 1/9/2021        | 4/9/2021      | UPS     | 1002               | 4511                  |






### Utilizing the "Business Analyst, Insurance Billing - CANDIDATE DATASET.xlsx" that was attached to your invite email can you please provide a .docx file back with the answers to the below questions: 

1. Write a query in SQL that will get all records from a given table? 
2. Write a query in SQL that joins the KIT table to the CLINIC table. 
3. Write a query in SQL that joins the KIT table to the PHYSICIAN table. 
4. Write a query in SQL to get count of Kit's by Physician NPI. 
5. Write a query in SQL to get count of Kit's by Clinic. 
6. Write a query in SQL to get kit count of providers and kit count of clinics by Carrier. 
7. Write a query in SQL to get distinct count of physicians who ordered in Q1 of 2021.
8. Write a query in SQL to get number of days between received date and date of service for each kit. 
9. What was the average ship time for each carrier? 
10. What was the average ship time for each clinic for each date of service quarter? 

### SQL solutions to the provided questions:

 Q1 Write a query in SQL that will get all records from a given table.

```sql
SELECT * FROM table_name;```

 Q2 Write a query in SQL that joins the KIT table to the CLINIC table.

```sql
SELECT *
   FROM KIT
   JOIN CLINIC ON KIT.ORDERING_CLINIC_ID = CLINIC.CLINIC_ID;```


 Q3 Write a query in SQL that joins the KIT table to the PHYSICIAN table.

```sql
SELECT *
FROM KIT
JOIN PHYSICIAN ON KIT.ORDERING_PHYSICIAN_ID = PHYSICIAN.PHYSICIAN_ID;```



Q4 Write a query in SQL to get count of Kit's by Physician NPI.

```sql
SELECT PHYSICIAN.NPI_NUMBER, COUNT(KIT.KITID) AS Kit_Count
   FROM KIT
   JOIN PHYSICIAN ON KIT.ORDERING_PHYSICIAN_ID = PHYSICIAN.PHYSICIAN_ID
   GROUP BY PHYSICIAN.NPI_NUMBER;```

Q5 Write a query in SQL to get count of Kit's by Clinic.

```sql
SELECT CLINIC.CLINIC_NAME, COUNT(KIT.KITID) AS Kit_Count
   FROM KIT
   JOIN CLINIC ON KIT.ORDERING_CLINIC_ID = CLINIC.CLINIC_ID
   GROUP BY CLINIC.CLINIC_NAME;```

Q6 Write a query in SQL to get kit count of providers and kit count of clinics by Carrier.

```sql
SELECT 
    KIT.CARRIER,
    COUNT(DISTINCT KIT.ORDERING_PHYSICIAN_ID) AS Provider_Kit_Count,
    COUNT(DISTINCT KIT.ORDERING_CLINIC_ID) AS Clinic_Kit_Count
FROM KIT
GROUP BY KIT.CARRIER;```

Q7 Write a query in SQL to get distinct count of physicians who ordered in Q1 of 2021.

```sql
SELECT COUNT(DISTINCT ORDERING_PHYSICIAN_ID) AS Distinct_Physician_Count
FROM KIT
WHERE DATE_OF_SERVICE BETWEEN '2021-01-01' AND '2021-03-31';```

Q8 Write a query in SQL to get the number of days between received date and date of service for each kit.

```sql
SELECT KITID, DATE_OF_SERVICE, RECEIVED_DATE,
       DATEDIFF(DAY, DATE_OF_SERVICE, RECEIVED_DATE) AS Days_Between
   FROM KIT;```

Q9 What was the average ship time for each carrier?

```sql
SELECT CARRIER, AVG(DATEDIFF(DAY, DATE_OF_SERVICE, RECEIVED_DATE)) AS Average_Ship_Time
   FROM KIT
   GROUP BY CARRIER;```

Q10 What was the average ship time for each clinic for each date of service quarter?

```sql
SELECT CLINIC.CLINIC_NAME, 
       DATEPART(QUARTER, DATE_OF_SERVICE) AS Service_Quarter,
       AVG(DATEDIFF(DAY, DATE_OF_SERVICE, RECEIVED_DATE)) AS Average_Ship_Time
FROM KIT
JOIN CLINIC ON KIT.ORDERING_CLINIC_ID = CLINIC.CLINIC_ID
GROUP BY CLINIC.CLINIC_NAME, DATEPART(QUARTER, DATE_OF_SERVICE);```







