# Change infrastructure

Maintenant qu'on sait comment créer une instance AWS avec Terraform, on souhaite à présent pouvoir la modifier.

Si on change une configuration Terraform, ce dernier ne va pas tout refaire, mais changer uniquement ce qui a besoin d'être changé par rapport à l'infra existante.
En production, il est recommandé d'utiliser un système de contrôle de version de fichiers pour gérer ses fichiers de conf, et gérer son state à l'aide de Terraform Cloud ou Terraform Entreprise.

On souhaite maintenant changer l'ID de l'AMI de la configuration du chapitre précédent.