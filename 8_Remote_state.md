# Store remote state

Avec les chapitres précédents, je sais maintenant créer, modifier et détruire des environnements Terraform en local (en gros).
En prod, il vaut bien entendu mieux garder son state sécurisé, chiffré et accessible aux autres membres de l'équipe.
Et c'est à ça que sert **Terraform Cloud**, vers lequel on va migrer le state Terraform construit jusqu'à présent.

Le chapitre ne me semble applicable que pour une entreprise, donc je vais ne faire que lire la doc "théorique" pour ce chapitre.

Une migration du state Terraform semble demander, après création du compte Terraform Cloud, un ajout d'un bloc `cloud` dans le `main.tf`, ressemblant à celui ci-dessous :

```JS
  cloud {
    organization = "organization-name"
    workspaces {
      name = "learn-tfc-aws"
    }
  }
```

On peut alors se connecter au cloud en fenêtre de commande, puis ré-initialiser la configuration Terraform pour que celle-ci migre sur le cloud:

```bash
terraform login # cette commande a l'air d'ouvrir une nouvelle fenêtre, qui affichera une clé API après connexion à conserver, car devra être demandée par le terminal

terraform init
rm terraform.tfstate # le state étant sur le cloud maintenant, le fichier local n'est plus utile
```

Deux manières d'exécuter Terraform en fenêtre de commande :

- En exécution locale, Terraform Cloud va exécuter Terraform en local, et stocker le state file sur le Cloud;
- En exécution à distance (remote), Terraform est exécuté sur le cloud aussi.

Le tuto reste ici dans de l'exécution à distance.

Les clés API AWS doivent être paramétrées sur le cloud (dans l'interface du site), une fois que la config a été initialisée comme ci-dessus.

Ce qui permet ensuite d'appliquer la conf :

```bash
terraform apply
```

Voilà et la config est sur le cloud, ce qui simplifie le travail collaboratif, et sécurise les données sensibles de notre compte.

On peut à présent détruire notre conf (pour éviter d'avoir à payer AWS), avec un `terraform destroy`.