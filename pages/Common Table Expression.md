alias:: CTE

- sources:: [Transactional Data Operations in PostgreSQL Using Common Table Expressions](https://rob.conery.io/2018/08/13/transactional-data-operations-in-postgresql-using-common-table-expressions/)
-
- [[CTE]]'s execute within a single transaction
- When you insert a new record with PostgreSQL, you can ask for it right back with returning *.
- If you just want the id, you can add returning id.
- example:
	- ```sql
	  with new_order as(
	    insert into orders(email, total) 
	    values ('rob@bigmachine.io',100.00) 
	    returning *
	  ), new_items as (
	    insert into order_items(order_id, sku, price)
	    select new_order.id, 'imposter-single',30.00
	    from new_order
	    returning *
	  )
	  select * from new_items;
	  ```