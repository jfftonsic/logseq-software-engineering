- On your `/.bashrc` or `/etc/bash.bashrc`
	- ```bash
	  export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
	  ```
- install a X server for windows, for example VcXsrv
- if you installed VcXsrv
	- run XLaunch
	- there are programs that work with the option `Multiple windows`, like Intellij, but others fail, like jetbrains toolbox, in the case of jetbrains toolbox, it worked with `One large window`
	- display number can be left -1
	- `Start no client`, next
	- Check `Disable access control`
- now if you run a GUI app on the WSL ubuntu shell, it will open the window of that app.