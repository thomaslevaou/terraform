# Build infrastructure

## Installation de la CLI AWS

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

aws --version # si cette commande retourne un résultat sans erreur apparente, on a alors bien réussi l'installation d'AWS

# Pour que Terraform y ait accès, on doit spécifier nos clés d'accès AWS (la standard et la secrète)
# Attention, c'est à refaire à chaque relancement de terminal (ou de PC !)
export AWS_ACCESS_KEY_ID={{ma_cle_aws}}
export AWS_SECRET_ACCESS_KEY={{ma_cle_secrete}}
```

Si un jour je dois mettre à jour ma version d'AWS, la doc pour ce faire est [disponible ici](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

## Configuration Terraform - Présentation

On appelle **configuration Terraform** l'ensemble de fichiers utilisés pour décrire une infrastructure Terraform.
On souhaite maintenant écrire une configuration pour définir une instance AWS EC2.

Une configuration doit avoir son propre dossier, d'où le fait qu'on va en créer un ici :

```bash
mkdir learn-terraform-aws-instance
cd learn-terraform-aws-instance
touch main.tf
```

Dans le fichier `main.tf` créé, on peut alors renseigner (via VS Code) la configuration suivante :

```JS
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

Les _providers_ (les "modules" Terraform comme évoqué dans le chapitre 1, permettant à Terraform de discuter avec l'API associée, ici l'API AWS par exemple) sont listés dans le bloc `terraform`.

Pour chaque provider, l'attribut `source` définit par défaut sa source dans le Registry Terraform (`hashicorp/aws` est un raccourci pour `registry.terraform.io/hashicorp/aws`). La source est paramétrable si besoin.
La `version` est optionnelle : si elle n'est pas précisée, c'est la plus récente qui est installée par défaut.
Si besoin un jour, la doc pour plus de détails à ce sujet [est ici](https://developer.hashicorp.com/terraform/language/providers/requirements).

On appelle `resource` dans cette configuration Terraform un bloc utilisé pour définir un composant de l'infrastructure. Un composant peut être physique ou virtuel, et dans le cadre d'AWS, ça peut être une instance EC2 par exemple.
Les deux chaînes de caractères suivant `resource` correspondent au type de la ressource (`"aws_instance"`) et son nom (`"app_server"`).  Attention au fait que le préfixe du type doit correspondre au nom du provider: ici, le préfixe `aws_` correspond bien au nom du provider qu'est `aws`. Le type de resource et son nom forment un unique ID pour la ressource : ici, l'id de l'instance EC2 est donc `aws_instance.app_server`.
Chaque ressource dispose de sa propre liste d'arguments obligatoire et optionnels, listée [sur ce lien](https://developer.hashicorp.com/terraform/language/providers).
Ici, on utilise l'argument `ami` pour paramétrer une AMI d'une image Ubuntu, l'argument `instance_type` pour avoir un type d'instance `t2.micro`, et l'argument `tags` pour ajouter un tag donnant comme nom à l'instance `ExampleAppServerInstance`.

## Configuration Terraform - Initialisation, formatage et lancement

On peut à présent lancer l'initialisation qui, comme vu précédemment, installe les providers nécessaires au fonctionnement de l'infra (ici le module pour que Terraform interagisse avec AWS concrètement) :

```bash
terraform init
```

L'installation peut prendre quelques minutes. Le module est alors installé dans le répertoire `.terraform` du répertoire courant. Les versions exactes installées sont listées dans le fichier `.terraform.lock.hcl`, ce qui peut toujours être pratique si besoin.

Pour améliorer sa lisibilité, il vaut mieux formater une configuration Terraform avant de passer à la suite, ce qui peut être possible via la commande suivante:

```bash
terraform fmt
```

On peut alors valider la bonne syntaxe de la configuration Terraform via :

```bash
terraform validate
```

Et si c'est bon, on peut alors appliquer la configuration :

```bash
terraform apply
```

Terraform affiche alors dans la fenêtre de commande son **plan d'exécution**, c'est-à-dire la liste des actions que Terraform va réaliser pour changer l'infra afin de correspondre à la configuration du fichier. Il affiche aussi la liste des attributs qui vont être définis, sachant qu'il indique pour pas mal de ces attributs un `(known after apply)`, ce qui veut concrètement dire que la value ne sera connue qu'après la création de la ressource (par exemple l'ARN, car le nom de ressource d'Amazon ne sera donné qu'après validationd e la création de la ressource).

On doit entrer manuellement un `yes` si tout nous paraît ok, et qu'on peut appliquer la configuration (sinon en cas de doute, avorter le process).

Une fois que le process est terminé en fenêtre de commande, on peut aller sur la liste des instances EC2 dans la console AWS de la région us-west-2, et on peut voir que l'instance a bien été créée.

## Configuration Terraform - Informations complémentaires

Après application de la configuration, les ID et propriétés des ressources que Terraform gère, ont été écrites dans le fichier `terraform.tfstate`.
Ce fichier contenant pas mal de données sensibles, ses accès doivent être restreints voire gérés de manière distante, comme indiqué [sur ce lien pour plus de détails](https://developer.hashicorp.com/terraform/tutorials/cloud/cloud-migrate).

On peut voir le state actuel (le contenu actuel du fichier `terraform.tfstate`) à l'aide de la commande `terraform show`. On peut voir ici par exemple, que Terraform a reporté les méta-données sur le provider AWS dans ce fichier.

On peut regarder la liste des ressources de notre projet à l'aide de la commande `terraform state list`. Pour gérer le state terraform de manière avancée, `terraform state` permet aussi de [faire plein d'autres trucs listés ici](https://developer.hashicorp.com/terraform/cli/commands/state), mais c'est hors cadre de ce cours.