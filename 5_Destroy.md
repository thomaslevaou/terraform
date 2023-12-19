# Destroy infrastructure

La commande `terraform destroy` détruit toutes les ressources gérées par Terraform et listées dans le `main.tf`, ce qui en fait donc l'inverse de `terraform apply`.

On peut dès à présent l'appliquer :

```bash
terraform destroy
```

Bien entendu, dans un cas avec plusieurs dépendances entre resources, Terraform les détruit dans un ordre de respect des dépendances.
