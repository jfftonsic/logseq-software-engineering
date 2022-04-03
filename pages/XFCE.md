sources:: [Using VcXsrv Windows X Server](https://sourceforge.net/p/vcxsrv/wiki/Using%20VcXsrv%20Windows%20X%20Server/), [Xfce Keyboard Shortcuts](http://xahlee.info/linux/linux_xfce_keyboard_shortcuts.html)

- Hotkeys
	- The config file is at `~/.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-keyboard-shortcuts.xml`
	- Easiest way to change hotkeys
		- Go to Menu `Applications`->`Settings`->`Window Manager`. Select tab `Keyboard`.
- Using XFCE with [[WSL]] and [[VcXSrv]]
	- Run [[VcXSrv]] with `One large window`, and the check box to disable auth checked.
		- You may or may not want to use `-keyhook` at the XLaunch text box for extra arguments.
			- That makes keys, such as Alt+TAB etc, be exclusively applied to the x server window while that window is in focus. So if you want to change window in Windows, you need to click with the mouse in another Windows-hosted window first.
			- I preferred to change XFCE's hotkey to cycle windows to other hotkey (and also the reverse cycle windows). (ALT+'  , in the case)
	- #+BEGIN_CAUTION
	  Documentations suggest using LIBGL_ALWAYS_INDIRECT=1 with the -wgl VcXSrv argument, **but in my environment this caused the XFCE become impossibly slow**. Then I removed the LIBGL_ALWAYS_INDIRECT=1 and kept the -wgl and everything runs smoothly.
	  #+END_CAUTION
	- #+BEGIN_IMPORTANT
	  When leaving the xfce without interaction for sometime, a lock-screen/screensaver  was triggering. That was causing black screen that was impossible to go back from.
	  So, to disable that, I went to `Applications`->`Settings`->`Light Locker Settings`.
	  **ONLY THIS WAS NOT ENOUGH**
	  Then I've then went to `Applications`->`Settings`->`Session and Startup`. Disabled `Lock screen before sleep` and disabled `Screen Locker` from `Application Auto-start`
	  Let's see if this will do it... and it did
	  #+END_IMPORTANT
		-