# Installation de Terraform

## Commandes suffisantes pour installer Terraform

```bash
# Si ce n'est pas déjà fait, mettre son système à jour, et installer gnugpg
# Au fait le `-y` est là pour dire "oui" automatiquement à toutes les demandes d'installation, quel que soit l'espace que ça prend
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

# On installe la clé GPG HashiCorp
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

# On vérifie l'empreinte de la clé
gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint

# On ajoute le repo HashiCorp à notre OS, et on télécharge ses informations
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update

# Ce qui nous permet d'installer terraform via un `apt-get install`
sudo apt-get install terraform

# Installation de la complétion par tabulation, pour laquelle on doit juste préalablement vérifier que le fichier de config de mon shell courant existe :
vim ~/.zshrc # si le fichier existe, c'est bon on peut le quitter sans rien toucher
terraform -install-autocomplete
```

## Commandes basiques pour vérifier une bonne installation

On commence par afficher des commandes d'aides :

```bash
terraform -help # affiche les commandes généralement disponibles
terraform -help plan # ajouter une commande après le `-help` détaille cette option en particulier
```

On peut maintenant installer un conteneur Docker contenant une image nginx :

```bash
# On commence par créer le dossier qui va contenir les fichiers de confs qui vont décrire l'infra que Terraform devra gérer
# Tous les plugins, modules et autres information sur l'infra structure que Terraform va gérer vont être stockés dans ce dossier
mkdir learn-terraform-docker-container
cd learn-terraform-docker-container
vim main.tf
```

Dans le nouveau fichier main.tf, on va insérer la configuration Terraform suivante, qui est très similaire à celle de l'exo du chapitre précédent: elle va chercher l'image docker nginx, créer un conteneur et le lancer en une seule commande (c'est un `docker pull nginx` suivi d'un `docker run nginx` en même temps):

```bash
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```

On peut alors lancer les deux mêmes commandes qu'au chapitre précédent :

```bash
terraform init
terraform apply
```

Le lancement du conteneur est à présent visible dans un `docker ps` et on peut bien voir le nginx installé sur `localhost:8000`.

Comme au chapitre précédent, on stoppe le conteneur avec un ̀`terraform destroy`. Il n'est alors bien plus visible dans `docker ps`.