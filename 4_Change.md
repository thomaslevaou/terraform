# Change infrastructure

Maintenant qu'on sait comment créer une instance AWS avec Terraform, on souhaite à présent pouvoir la modifier.

Si on change une configuration Terraform, ce dernier ne va pas tout refaire, mais changer uniquement ce qui a besoin d'être changé par rapport à l'infra existante.
En production, il est recommandé d'utiliser un système de contrôle de version de fichiers pour gérer ses fichiers de conf, et gérer son state à l'aide de Terraform Cloud ou Terraform Entreprise.

On souhaite maintenant changer l'ID de l'AMI de la configuration du chapitre précédent : à la place de la version d'Ubuntu précédente, on souhaite avoir une image d'Ubuntu 16 ici. On modifie donc l'ID de l'AMI dans la configuration Terraform :

```JS
...
resource "aws_instance" "app_server" {
  ami           = "ami-08d70e59c07c61a3a"
  instance_type = "t2.micro"
...
```

Dans les faits, Terraform va en réalité supprimer l'ancienne image et en créer une nouvelle avec cette nouvelle AMI.

On peut alors directement appliquer ce changement de configuration :

```bash
terraform apply # et valider avec un "yes" si c'est tout bon
```

Comme le montre la fenêtre de confirmation, ce changement implique pas mal de changements en cascade dans le state Terraform.

On peut, comme déjà vu dans le chapitre précédent, voir le résultat sur le nouveau state après application du changement, dans `terraform show`.
