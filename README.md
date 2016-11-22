# RubyOnRails - Tutorial - Have fun

Lancer la commande : 

```
\curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles --autolibs=0
```
Copier le script d'installation Ruby : 
```
cp /home/m2iagl/decottis/public/install_ruby.sh .
```
Lancer le script d'installation : 
```
chmod +x install_ruby.sh
./install_ruby.sh
```
Ouvrir un nouveau terminal, puis lancer la commande : 
```
gem install nokogiri -- --use-system-libraries
gem install rails
```
