# Build infrastructure

## Installation de la CLI AWS

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

aws --version # si cette commande retourne un résultat sans erreur apparente, on a alors bien réussi l'installation d'AWS
```

Si un jour je dois mettre à jour ma version d'AWS, la doc pour ce faire est [disponible ici](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

