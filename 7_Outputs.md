# Query data with outputs

On peut définir des **outputs Terraform** pour qu'une commande affiche à l'écran certaines informations que ce dernier ne peut connaître qu'après application d'une configuration, par exemple.

Pour ce faire, on doit créer à côté du `main.tf`, un fichier `outputs.tf`, pour indiquer qu'on va vouloir afficher l'ID de l'instance EC2, et son adresse IP publique ici :

```JS
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.app_server.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.app_server.public_ip
}
```

Pour que les outputs soient utilisables, on doit d'abord appliquer la configuration :

```bash
terraform apply
```

La commande affiche alors à la fin des deux outputs, qu'on peut réafficher si besoin avec la commande suivante :

```bash
terraform output
```

En prod, les outputs servent surtout à connecter des infra entre eux, voire des projets Terraform entre eux. Pour voir ça plus en détail si besoin, c'est [sur cette doc](https://developer.hashicorp.com/terraform/tutorials/configuration-language/outputs).
