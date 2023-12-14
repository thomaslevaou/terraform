# Build infrastructure

## Installation de la CLI AWS

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

aws --version # si cette commande retourne un résultat sans erreur apparente, on a alors bien réussi l'installation d'AWS

# Pour que Terraform y ait accès, on doit spécifier nos clés d'accès AWS (la standard et la secrète)
export AWS_ACCESS_KEY_ID={{ma_cle_aws}}
export AWS_SECRET_ACCESS_KEY={{ma_cle_secrete}}
```

Si un jour je dois mettre à jour ma version d'AWS, la doc pour ce faire est [disponible ici](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

## Configuration Terraform

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
