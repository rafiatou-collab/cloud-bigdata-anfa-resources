# Rendu Séance 7

**Nom et prénom :** KAMBIA Rafiatou
**Identifiant GitHub :** rafiatou-collab

## Résumé de la séance

J'ai déployé un cluster Kafka à 3 brokers en mode KRaft (sans Zookeeper), créé un topic partitionné et répliqué, écrit un producer et un consumer Python simples, simulé une flotte de 100 bus Anfa envoyant leurs positions GPS en continu, observé la tolérance aux pannes en tuant un broker, puis consommé le flux avec Spark Structured Streaming pour calculer des agrégats en fenêtres temporelles et écrire les résultats en Parquet dans MinIO.

## Étapes principales

1. Déploiement du cluster Kafka (3 brokers KRaft) + Kafka UI + MinIO + Spark via Docker Compose.
2. Création du topic anfa-positions-bus (3 partitions, replication factor 3).
3. Test producer/consumer Python simples pour comprendre la mécanique de base.
4. Lancement du simulateur de flotte (100 bus, ~100 messages/seconde).
5. Démonstration de tolérance aux pannes : arrêt de kafka-2, cluster toujours actif avec 2 brokers.
6. Job Spark Structured Streaming lecture_flux_console.py : affichage des positions en micro-batchs.
7. Job Spark Structured Streaming agregation_streaming.py : agrégation en fenêtres de 30 secondes, écriture Parquet dans MinIO.

## Captures d'écran

### Kafka UI - 3 brokers actifs
![Kafka UI Brokers](captures/kafka-ui-brokers.png)

### Kafka UI - Débit de messages en augmentation
![Kafka UI Débit](captures/kafka-ui-debit.png)

### Kafka UI - 2 brokers actifs après arrêt de kafka-2
![Kafka UI 2 Brokers](captures/kafka-ui-2-brokers.png)

### Spark Structured Streaming - Micro-batchs en console
![Spark Streaming Console](captures/spark-streaming-console.png)

### MinIO - Agrégats en Parquet dans anfa-streaming
![MinIO Agrégats](captures/minio-agregats.png)

## Réflexion

Kafka apporte une capacité temps réel que ni Spark batch ni Airflow ne peuvent offrir seuls : les données de position des bus sont disponibles en quelques secondes, pas le lendemain matin. La combinaison Kafka + Spark Structured Streaming est particulièrement puissante - Kafka garantit la durabilité et la tolérance aux pannes des messages, tandis que Spark apporte la puissance de calcul distribué pour les agrégations. La démonstration de tolérance aux pannes (arrêt de kafka-2 sans interruption du flux) illustre concrètement pourquoi la réplication à 3 est indispensable en production.

## Difficultés rencontrées

- Conflits de noms de conteneurs avec les séances précédentes (anfa-minio, anfa-spark-master). Résolution : docker rm -f sur les anciens conteneurs avant de lancer la stack.
- Le worker Spark monopolisait toute la RAM avec d'anciennes applications non tuées. Résolution : kill des applications via l'UI Spark (http://localhost:8091) puis docker restart anfa-spark-worker.
- Le job Spark ne recevait pas de données malgré le simulateur actif. Résolution : attendre que le worker ait ses ressources libres après avoir tué toutes les applications en cours.
