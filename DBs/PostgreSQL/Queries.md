### Filter

```sql
select distinct on (public."AspNetUsers"."Email") public."Locations"."AuthorId", public."Locations"."CreateAt", public."AspNetUsers"."Email", public."AspNetUsers"."Name", public."AspNetUsers"."CompanyName" from public."Locations"

right join public."AspNetUsers" on public."Locations"."AuthorId" = public."AspNetUsers"."Id";
```

### Select rows with column name

```sql
select * from public."ObjectLines_ad4cf764-be1b-4761-ab39-24fdb6f4d85e" where "ClassId" = 'person'

select * from public."AggregatedObjects_7161836f-2e70-4805-bd98-fe148b416160" where "ClassId" = 'person'

select * from public."AggregatedCountingLines_7161836f-2e70-4805-bd98-fe148b416160" where "ClassId" = 'person'
```

### Select all

```sql
SELECT * FROM public."VideoProcessInfos"
ORDER BY "Id" ASC 
```

### Clear whole table

```sql
# remove all rows
DELETE FROM public."VideoProcessInfos"