sources:: https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html

- A thread often acts in response to the action of another thread.
- If the other thread's action is also a response to the action of another thread, then livelock may result.
- Livelocked threads are unable to make further progress.
- However, <span style="color: yellow; background-color: black">the threads are not blocked</span> — they are simply too busy responding to each other to resume work.
- This is comparable to two people attempting to pass each other in a corridor: Alphonse moves to his left to let Gaston pass, while Gaston moves to his right to let Alphonse pass. Seeing that they are still blocking each other, Alphone moves to his right, while Gaston moves to his left. They're still blocking each other, so...
-