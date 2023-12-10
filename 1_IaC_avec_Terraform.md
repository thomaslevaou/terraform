# Qu'est-ce que l'Infrastructure as Code avec Terraform ?

## Présentation générale

On a vu dans le tuto sur AWS qu'on gérait notre infra avec les interfaces du site d'AWS.
Avec les outils d''**Infrastructure as Code** (IaC), on va pouvoir gérer une infrastructure avec des fichiers de configuration, au lieu de devoir passer par l'interface d'AWS. Ces outils permettent ainsi d'avoir une gestion de l'infra plus sereine et reproductible avec le temps, qui peut ainsi être versionnée, partagée, etc.

**Terraform** est le nom de l'outil d'IaC de l'enterprise HashiCorp. Par rapport à un management "à la main" de l'infra, Terraform permet de :

- gérer une infra sur plusieurs clouds en même temps;
- avoir un langage de programmation facile à comprendre pour un être humain;
- tracer les changements de ressources pendant les déploiements;
- gérer un contrôle de versions (commits) sur l'infra.

Terraform dispose de modules (_plugins_), appelés **providers** ("fournisseurs"), permettant à Terraform d'interagir avec les API des plateformes de cloud comme AWS, GCP, Azure, etc. Ils sont au besoin tous disponible [sur ce lien](https://registry.terraform.io/browse/providers).

Avec Terraform, on peut en gros assembler comme on veut différentes unités d'infra (une instance, un réseau, etc) qu'on appelle **ressources**. On peut créer nos propres modules en combinant les ressources un peu comme on veut.

Pour déployer une infrastructure avec Terraform, il est donc nécessaire :

- d'identifier l'infrastructure du projet, qu'on appelle le **scope**;
- d'écrire la configuration de l'infrastructure (on appelle ceci **l'author**);
- d'installer les plugins Terraform nécessaires pour manager l'infrastructure (**initialisation**);
- de prévisualiser les changement que Terraform va faire pour correspondre à ma configuration (**preview**);
- de faire les changements planifiés (**apply**).

Je suis bien curieux de savoir pourquoi Terraform s'organise de cette manière, mais je suppose que je le saurai en lisant la suite du tuto.

Terraform garde en mémoire la "vraie" infra dans un **state file**, qui sert de source de vérité pour l'environnement. Terraform utilise ce state file pour déterminer les changements à faire pour que l'infra matche la configuration souhaitée (je serai bien curieux de voir ça appliqué concrètement).

On peut collaborer à plusieurs avec Terraform, et on peut connecter Terraform Cloud avec GitHub, GitLab ou autre (qu'on appelle VCS pour "Version Control System") (hâte de voir ça appliqué en vrai).

## Premier exemple d'application

On peut écrire une configuration Terraform dans un fichier `main.tf` (c'est donc l'`author` résultant du `scope` ?), comme ci-dessous (appliqué dans l'éditeur interatif du site de Terraform):

```bash
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.15.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

J'ai l'impression qu'on a une construction d'image Docker nginx en local, puis un lancement sur les ports 8000:80. C'est comme si on scriptait un contenu de Dockerfile, puis qu'on faisait tout de suite un `docker build` puis un `docker run` avec les noms indiqués dans le fichier. Ça crée l'image ET le conteneur en même temps, et lance le conteneur.

Son exécution fait appel aux commandes suivantes :

- Une initialisation, qui installe le plugin nécessaire pour que Terraform puisse interagir avec Docker : `terraform init`;
- Une application du contenu du fichier, càd ici une installation du conteneur avec le serveur nginx : `terraform apply`.

Une fois le apply effectué, on voit bien le conteneur créé ET lancé via un `docker ps`.

La destruction du conteneur et ses ressources se fait via un `terraform destroy`.