# Fiche de conformité — Application mobile Anfa

## Finalité du traitement
Les données collectées (position GPS, historique de paiements, numéro de téléphone) sont traitées dans le but de proposer aux passagers le trajet de bus le plus proche et de gérer leur abonnement mobile money. La finalité est déterminée, explicite et légitime.

## Sensibilité des données
Les données de localisation GPS sont des données personnelles sensibles car elles permettent de reconstituer les déplacements quotidiens d'une personne. L'historique de paiements et le numéro de téléphone sont également des données personnelles à protéger. Leur combinaison crée un profil détaillé du passager.

## Base légale (loi togolaise)
Au Togo, la loi n°2019-014 relative à la protection des données à caractère personnel encadre ce type de traitement. La base légale retenue est le consentement explicite de l'utilisateur lors de l'inscription à l'application, conformément à l'article 5 de cette loi.

## Durée de conservation
Les données de position GPS sont conservées 30 jours. L'historique de paiements est conservé 5 ans (obligation comptable). Le numéro de téléphone est conservé pendant toute la durée de l'abonnement actif puis supprimé dans les 30 jours suivant la résiliation.

## Souveraineté des données
Les données des passagers togolais doivent être hébergées sur le territoire togolais ou dans un pays offrant un niveau de protection équivalent. L'utilisation de MinIO déployé localement répond à cette exigence de souveraineté numérique.

## Droits des personnes
Les passagers disposent des droits suivants : droit d'accès, de rectification, d'effacement, de portabilité et d'opposition. Un formulaire de contact doit être disponible dans l'application pour exercer ces droits. Anfa doit répondre dans un délai maximum de 30 jours.

## Risques identifiés et mesures
Risque principal : fuite des données de localisation permettant de reconstituer les habitudes de déplacement. Mesures : chiffrement des données en transit (HTTPS) et au repos, accès restreint aux données par rôle, journalisation des accès, et audit de sécurité annuel.
