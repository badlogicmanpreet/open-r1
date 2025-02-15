1. Create ssh key pair
   ssh-keygen -t ed25519 -C "mghotra81@gmail.com"
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   cat ~/.ssh/id_ed25519.pub
   copy and add to github account

2. git clone 

