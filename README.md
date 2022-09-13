# Cook it - test

## Consignes

- Planifier la mise en place d'un kafka via AWS, pousser du contenu sur un topic et le consommer.
- Documenter où tu es rendu, ce qui t'a bloqué éventuellement et quel serait ton plan pour le finaliser.

## Approche et Planification

AWS propose un service Apache Kafka entièrement géré et hautement disponible du nom de [AWS MSK](https://aws.amazon.com/msk/).

Le gros de ce travail se résume donc à l'automatisation du déploiment MSK via Terraform.

j'ai opté pour Terraform pour bénéficier de tous les avantages de l'approche IaC.

Il existe un module open source pour terraform permettant de déployer un cluster MSK sur AWS. Ce module est disponible ici [terraform-aws-msk-kafka-cluster](https://github.com/clowdhaus/terraform-aws-msk-kafka-cluster/tree/v1.2.0).

Ce travail propose un [exemple introductif](https://github.com/clowdhaus/terraform-aws-msk-kafka-cluster/tree/v1.2.0/examples/basic) sur lequelle je me suis basé.

## Niveau de progression

A ce stade j'ai préparé les fichiers nécéssaires mais aucun test n'a été fait sur un compte AWS.

Pour tester il faudrait : 

1 - editer le fichier main.tf et y définir les valeurs suivantes du pour se connecter au compte AWS :

```bash
provider "aws" {
    ...
shared_config_files      = ["/Users/tf_user/.aws/conf"]
shared_credentials_files = ["/Users/tf_user/.aws/creds"]
...
}
```
Ou (approche non recommandé dans la pratique)

```bash
provider "aws" {
    ...
  access_key = "my-access-key"
  secret_key = "my-secret-key"
...
}

2 - Puis, exécuter les commandes suivantes :

```bash
$ terraform init
$ terraform plan
$ terraform apply
```

## Perpectives pour la suite

Pour la suite il faudrait addresser les points suivants :

- un véritable dimessionnement du cluster kafka (nombres de brokers, nombre de partitions et replication par topic)

- Définir la politique de sécurisation  et d'accès au cluster Kafka. Ici plusieurs pistes sont envisagles: les controles d'accès aux VPC, des comptes IAM avec des roles spécifiques; l'utilisation de kafka security pour gérer les connexion autorisées sur le cluster et bien d'autres pistes. Une bonne documentation sur le sujet proposé par AWS est [disponible ici](https://docs.aws.amazon.com/msk/latest/developerguide/security.html).

- la creation d'un Topic de test et l'utilisation de producers et consumers sur ce topic.

- Opter pour un stockage distant et partagé pour le state de Terraform.

