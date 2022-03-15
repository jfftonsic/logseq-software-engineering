alias:: JTA

- can be used to coordinate transactional updates to multiple resource managers.
- [[Global Transactions]
- [[Spring]] documentation quotes:
	- > ... JTA, which is a **cumbersome** API (partly due to its exception model). Furthermore, a JTA UserTransaction normally needs to be sourced from **JNDI**, meaning that you also need to use JNDI in order to use JTA. The use of global transactions limits any potential reuse of application code, as JTA is **normally only available in an application server environment**.