- Followed the Manual instalation steps from:
  
  https://docs.microsoft.com/en-us/windows/wsl/install-win10 
  
  Went to https://aka.ms/wslstore and installed ubuntu
  
  Tried to run, go the following error:
  
  ```
  wls ubuntu WslRegisterDistribution failed with error: 0xc03a001a
  Error: 0xc03a001a The requested operation could not be completed due to a virtual disk system limitation.  Virtual hard disk files must be uncompressed and unencrypted and must not be sparse.
  ```
  
  Then I followed from 
  https://www.thewindowsclub.com/wslregisterdistribution-failed-with-error-0xc03a001a-2 
  only the 'Uncheck Compress Contents for Ubuntu directory' part. Which in summary is:
  
  Go to explorer, folder "%localappdata%\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc", right-click, properties, advanced, leave unchecked: compress and encrypt. It will ask if you want to also apply that for sub-folders, click yes.
  
  Now 'ubuntu' from command line works.
  
  Create user/pass, then `sudo apt update`, then `sudo apt upgrade`. While doing this, the linux vm is taking 1.2G ram.