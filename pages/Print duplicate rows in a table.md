- Examples:
	- ```sql
	  --The Best approach is to use GROUP BY and HAVING condition. It is more effective and faster then previous.
	  SELECT name, section FROM tbl
	  GROUP BY name, section
	  HAVING COUNT(*) > 1;
	  
	  select NAME
	  from Person
	  group by NAME
	  having count(NAME) > 1;
	  
	  -- other ways
	  select NAME, count(NAME) as num
	  from Person
	  group by NAME;
	  
	  select NAME from
	  (
	    select NAME, count(NAME) as num
	    from Person
	    group by NAME
	  ) as statistic
	  where num > 1;
	  ```