# Qu'est-ce que l'Infrastructure as Code avec Terraform ?

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