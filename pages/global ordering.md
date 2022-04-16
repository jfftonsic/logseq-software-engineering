sources:: https://www.cs.princeton.edu/courses/archive/spr18/cos518/docs/L3-strong-consistency.pdf

- Global ordering preserves each client’s own local ordering
- Global ordering preserves real-time guarantee
	- All ops receive global time-stamp using a sync’d clock
	- If timestampOp1(x) < timestampOp2(y), OP1(x) precedes OP2(y) in sequence
- Once write completes, all later reads (by wall-clock start time) should return value of that write or value of later write.
- Once read returns particular value, all later reads should return that value or value of later write.