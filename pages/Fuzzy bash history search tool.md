-
- ```bash
  git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
  
  # remove \r from file...
  sed -i.bak 's/\r$//g' ~/.fzf/install
  
  # run installation
  ~/.fzf/install
  
  # remove \r from auxiliary scripts...
  sed -i.bak 's/\r$//g' ~/.fzf/shell/key-bindings.bash
  sed -i.bak 's/\r$//g' ~/.fzf/shell/completion.bash
  
  # if you are using other shells you need to do that for the other scripts in that folder
  ```