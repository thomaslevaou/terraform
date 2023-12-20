# Variables

Jusqu'à maintenant, on a utilisé des chaînes de caractères en dur dans nos configurations Terraform.
Cette techno permet cependant d'utiliser des variables à la place, ce qui peut faciliter la réutilisabilité de certaines données.

Pour ce faire, on crée à côté du `main.tf`, un fichier `variables.tf` (en vrai on peut appeler le fichier comme on veut, car Terraform charge le contenu de tous les fichiers `.tf` sans regarder leurs noms en dehors du main) dans lequel on définit ce qu'on appelle un **bloc variable** `"instance_name"` :

```JS
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "ExampleAppServerInstance"
}
```

On peut alors modifier le `Name = "ExampleAppServerInstance"`  en `Name = var.instance_name` dans le fichier `main.tf`.

Ce qui est alors applicable avec un `terraform apply`.

On peut aussi utiliser le paramètre `-var` pour modifer une variable en paramètre :

```bash
terraform apply -var "instance_name=YetAnotherName"
```

Attention au fait que terraform ne gardera pas ce nom en mémoire dans ce cas (si je vais un autre changement de variable, je ne retrouverai nulle part ̀`YetAnotherName`).

Pour creuser plus en détail, une doc sur les gestions de variables Terraform détaillée est disponible [sur ce lien](https://developer.hashicorp.com/terraform/tutorials/configuration-language/variables).