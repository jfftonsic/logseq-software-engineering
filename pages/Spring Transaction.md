- The `isolation` parameter at the `@Transactional` **only applies to newly started transactions**. Switch the `validateExistingTransactions` flag to `true` on your transaction manager if you'd like isolation level declarations to get **rejected** when **participating in an existing transaction with a different isolation level.**
- Spring Transaction Abstractions
	- `TransactionDefinition`: propagation, isolation, timeout, read-only flag.
	- `PlatformTransactionManager`: commit, rollback
- {{renderer code_diagram,nomnoml}}
	- ```nomnoml
	  [Pirate|eyeCount: Int|raid();pillage()|
	    [beard]--[parrot]
	    [beard]-:>[foul mouth]
	  ]
	  [<table>mischief 
	  | bawlfd sf sadf abawlfd sf sadf abawlbawlfd sf sadf abawlfd sf sadf abawlfd sf sadf afd sf sadf a | bawlfd sf sadf abawlfd sf sadf abawlfd sf sadf abawlfd sf sadf abawlfd sf sadf abawlfd sf sadf a ||
	  yell | drink
	  ]
	  ```
- {{renderer :tables_npozuslk}}
	- data nosum
		- bawl ea sdfagidf asdfg aisudf asiuhf ashdf asidfh aius
			- dfasdf
		- dfasdf df auihdf aihdf usduif hasdfhua suhd faiui
			- dsfasdfas
		- dfasdf df auihdf aihdf usduif hasdfhua suhd faiui
			- dsfasdfas
		- dfasdf df auihdf aihdf usduif hasdfhua suhd faiui
			- dsfasdfas
	-