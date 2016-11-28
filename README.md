# RubyOnRails - Tutorial - Have fun

## 1. Installation
Lancer la commande : 

```
\curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles --autolibs=0
```
Télécharger le script d'installation Ruby : 
https://drive.google.com/open?id=0BxVEKTB9DMMWalcxYWxvakN0VDg

Lancer le script d'installation : 
```
chmod +x install_ruby.sh
./install_ruby.sh
```
Lancer la commande : 
```
. ~/.bashrc
gem install nokogiri -- --use-system-libraries
gem install rails
```

## 2. Hello World des familles
Rails embarque un système de génération de template qui permet de mettre en place une architecture initiale de son projet.

Pour générer ce projet :
```
rails new helloWorld
```
Vous pouvez lancer votre serveur d'application RoR par la commande suivante :
```
rails server
```
NB : On vous conseille de lancer le serveur dans un autre terminal car les modifications sont prises à la volée.
Le serveur d'application RoR est disponible sous http://localhost:3000 (Allez y jeter un oeil !).

Rails fonctionne avec un système de génération très évolué.
Vous pouvez générer un controller avec rails :
```
bin/rails generate controller [NOM-DU-CONTROLLER] Action1 Action2 ActionN
```
Générer un controller HelloWorld avec comme une action "index"
Cette commande va créér le controleur HelloWorld ainsi que la vue associée. De plus il va ajouter la route entre le controleur et la vue dans le fichier config/routes.rb.
RoR va effectué automatiquement le mapping grâce au nom des différents fichiers.
Vous pouvez modifier le fichier app/views/hello_world/index.html.erb pour y mettre votre HelloWorld.
Votre HelloWorld est disponible sous http://localhost:3000/hello_world/index.

## 3. Mise en bouche : CRUD de tablette de chocolat
Créer un nouveau projet chocolatManager ou vous souhaitez.
N'oubliez pas de tuer le serveur HelloWorld puis relancer votre serveur sous ce projet!
Dans le fichier config/routes.rb, ajouter avant le end :
```
resources:chocolat
```
Lancer la commande bin/rails routes
Cette commande liste toutes les routes que vous avez déclaré dans votre fichier routes. Vous comprenez bien que votre resources:chocolat à créér automatiquement toutes les routes dont on a besoin pour du CRUD :).

### Créer un service de création de tablette de chocolat
Afin de créer des tablettes de chocolat, il faut créer l'entité chocolat en base et en tant que modèle Ruby, le système de génération nous permet ça :
```
bin/rails generate model Chocolat nom:string paysOrigine:string nombreDeCarre:integer description:text 
```
Maintenant, il faut lancer la création de la base dans le Sqlite embarqué : 
```
bin/rails db:migrate
```
Générer le controleur "Chocolat" avec comme action new, create et show 
Dans le dossier views/chocolat, créér le fichier new.html.erb avec le contenu suivant :
```
<html>
<head></head>
<body>
<%= form_for :chocolat, url: chocolat_index_url do |f| %>
  <p>
    <%= f.label :nom %><br>
    <%= f.text_field :nom %>
  </p>

  <p>
    <%= f.label :paysOrigine%><br>
    <%= f.text_field :paysOrigine %>
  </p>
  <p>
    <%= f.label :nombreDeCarre%><br>
    <%= f.text_field :nombreDeCarre %>
  </p>
  <p>
    <%= f.label :Description%><br>
    <%= f.text_field :description %>
  </p>
  <p>
    <%= f.submit %>
  </p>
<body>
</html>
<% end %>
```
Ceci est un formulaire simple, accessible à l'adresse http://localhost:3000/chocolat/new.
Il faut maintenant enregistrer en base de données la tablette de chocolat :
Dans le controleur chocolat, ajouter à la méthode create : 
```
  @chocolat = Chocolat.new(params.require(:chocolat).permit(:nom,:paysOrigine,:nombreDeCarre,:description))
  @chocolat.save
  redirect_to @chocolat
  ```
  
Tester !
### Affichage du chocolaaaaat !
 Oops, il vous manque une page show/1 a priori... RoR redirige par défaut sur cette page à la création d'une ressource. Cette page doit donc afficher les détails de la création. Il faut d'abord afficher capturé l'id dans l'url via le controleur en ajoutant le find dans la méthode show : 
 ```
     @chocolat = Chocolat.find(params[:id])
 ```
 puis créér la vue show.html.erb : 
```
<html>
<head></head>
<body>
  <p>
    <strong>Nom:</strong>
    <%= @chocolat.nom %>
  </p>

  <p>
    <strong>pays d orgine:</strong>
    <%= @chocolat.paysOrigine %>
  </p>
  <p>
    <strong>nombre de carre:</strong>
    <%= @chocolat.nombreDeCarre %>
  </p>
  <p>
    <strong>Description:</strong>
    <%= @chocolat.description %>
  </p>
<body>
</html>
```
Maintenant, nous allons lister les chocolats, sur l'action index ajouter :
```
@chocolat = Chocolat.all
```
Pour la vue, voici les instructions utiles :
- Parcourir un objet
```
<% @chocolats.each do |chocolat| %>
<% end %>
```
- Un lien vers une autre URL :
```
<%= link_to 'Afficher', chocolat_path(chocolat) %>
```

Objectif : faire une liste de tous les chocolats avec un lien vers la description.<br/>
Maintenant, vous pouvez ajouter des liens entre toutes vos pages avec l'instruction link. 
```
<%= link_to 'Chocolat', PREFIXE_DE_ROUTE %>
```
Ruby attend le PREFIX de route de la page + "_path" que vous pouvez obtenir avec la commande 
```
bin/rails routes
```
Objectif : Mettre un lien entre toutes la liste, la création et le détail afin de naviguer
### Mise à jour du chocolat
Maintenant que RoR n'a plus de secret pour vous, on vous donne uniquement la syntaxe du formulaire d'édition :
``` 
<%= form_for :chocolat, url: chocolat_path(@chocolat), method: :patch do |f| %>
  <p>
    <%= f.label :nom %><br>
    <%= f.text_field :nom %>
  </p>
  <p>
    <%= f.label :paysOrigine%><br>
    <%= f.text_field :paysOrigine %>
  </p>
  <p>
    <%= f.label :nombreDeCarre%><br>
    <%= f.text_field :nombreDeCarre %>
  </p>
  <p>
    <%= f.label :description%><br>
    <%= f.text_field :description %>
  </p>
  <p>
    <%= f.submit %>
  </p>
```
Pour le backend, on vous laisse gérer, vous avez tous les élements !
### Supprimer une tablette
Ici, on vous donne uniquement le lien vers la suppression pour la vue : 
```
<%= link_to 'Supprimer', chocolat_path(chocolat),
              method: :delete,
              data: { confirm: 'Vous êtes sur ?' } %>
```
