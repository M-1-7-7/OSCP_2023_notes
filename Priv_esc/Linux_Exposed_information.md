  # Exposed Sensitive Information

  ## Inspecting User Trails

  user enviroment variables use `env` command to get see if any plain txt creds or other sensitve info is in there hardcoded.

  get more details around hard coded values using the `cat .bashrc` comand

  `sudo -l` may show that you have permisions like ` (ALL : ALL) ALL` which means you can `sudo -i` to become root


  ## Inspecting Server Footprints

  monitor proccessors in search for credentials use this command: `watch -n 1 "ps -aux | grep pass"`

  use tcp dump to monitor core credentials this way: `sudo tcpdump -i lo -A | grep "pass"`

  

  
