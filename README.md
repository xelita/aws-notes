# Amazon Elastic Block Store (EBS)

## type de stockage
EBS = stockage persistant même si instance est stoppée
pour stockage racine, cela n'est pas la cas par défaut, il faut le spécifier
Automatiquement répliqué dans multi-A-Z
SLA 99,999 %

Amazon EBS propose quatre types de volume de génération actuelle :
SSD Provisioned IOPS (io1), SSD à usage général (gp2), Throughput Optimized HDD (st1) et Cold HDD (sc1).

Volume SSD Provisioned IOPS (io1) EBS
Volume SSD haute performance conçu pour les charges de travail transactionnelles sensibles au temps de latence
Bases de données NoSQL et relationnelles à fort taux d'E/S
max IOPs: 20 000
4 Go-16 To

Volume SSD General Purpose EBS (gp2)
Volume SSD à usage général offrant un bon rapport prix/performances pour un large éventail de charges de travail transactionnelles
Volumes de démarrage, applications interactives à faible latence, développement et test
max IOPs: 10 000
1 Go-16 To

Volume HDD Throughput Optimized (st1)
Volume HDD économique conçu pour les charges de travail à débit élevé fréquemment consultée
Big Data, entrepôts de données, traitement des journaux
max IOPs: 500
500 Go-16 To

Volume Cold HDD (sc1)
Volume HDD le plus abordable, conçu pour les charges de travail les moins consultées
Données froides nécessitant un nombre réduit d'analyses chaque jour
max IOPs: 250
500 Go-16 To

## snapshots
Stockage dans S3 de manière incrémentale

### Instantanés (snapshots) Amazon EBS
Une fois qu'un volume est créé à partir d'un instantané (snapshot), il n'est pas nécessaire d'attendre que toutes les données soient transférées d'Amazon S3 vers votre volume Amazon EBS pour que l'instance associée puisse commencer à accéder au volume.
Les instantanés Amazon EBS utilisent un mode de chargement progressif ou « lazy loading » pour que vous puissiez les utiliser aussitôt.

Le partage d'instantanés Amazon EBS vous permet de mettre facilement en commun des données avec vos collègues ou d'autres membres de la communauté AWS.

La possibilité de copier des instantanés (snapshots) entre différentes régions AWS permet de tirer plus facilement parti des nombreuses régions AWS à des fins d'expansion géographique, de migration des centres de données et de reprise après sinistre.

### Disponibilité et durabilité des volumes Amazon EBS
Les volumes Amazon EBS sont conçus pour être hautement disponibles et fiables. Les données d'un volume Amazon EBS sont répliquées sans coût supplémentaire sur plusieurs serveurs au sein d'une zone de disponibilité, afin de prévenir toute perte des données due à la défaillance d'un seul composant.
Taux de défaillance annuel (AFR) compris entre 0,1 et 0,2 %

### Amazon EBS Encryption et AWS Identity and Access Management
Amazon EBS Encryption offre un chiffrement aisé des volumes de données, des volumes de démarrage et des instantanés (snapshots) EBS, sans qu'il soit nécessaire de créer ni de gérer d'infrastructure de gestion sécurisée des clés.
Impossible de chiffrer automatiquement le root volume. Utiliser des outils tiers pour cela

## FAQ sur Amazon EBS

### Questions d'ordre général

Q : Qu'est-ce qui arrive à mes données lorsqu'une instance Amazon EC2 se termine ?
Contrairement aux données enregistrées sur un stockage d'instance local (qui persistent uniquement tant que cette instance est active), les données stockées sur un volume Amazon EBS persistent indépendamment de la durée de vie de l'instance. De ce fait, nous vous recommandons d'utiliser le stockage d'instance local uniquement pour les données temporaires. Pour les données nécessitant une durabilité supérieure, nous vous recommandons d'utiliser les volumes Amazon EBS ou de sauvegarder vos données sur Amazon S3. Si vous utilisez un volume Amazon EBS en tant que partition racine, définissez l'indicateur « Delete on termination » sur « No » pour que votre volume Amazon EBS persiste indépendamment de la durée de vie de l'instance.

Q : Comment migrer un volume EBS existant vers un volume SSD à usage général (gp2) EBS ?
La migration de données d'un autre type de volume vers un volume SSD à usage général (gp2) se fait très facilement. Il vous suffit de prendre un instantané (snapshot) du volume existant et de créer un volume SSD à usage général (gp2) à partir de cet instantané (snapshot).

### Instantanés (snapshots)

Q : Sera-t-il possible d'accéder aux instantanés en utilisant l'API Amazon S3 habituelle ?
Non, les instantanés sont uniquement disponibles via l'API Amazon EC2.

Q : Est-ce que les volumes ont besoin d'être démontés pour prendre un instantané ?
Non, les instantanés peuvent être pris en temps réel alors que le volume est attaché et en utilisation. Toutefois, les instantanés ne capturent que les données qui ont été écrites sur votre volume Amazon EBS, toute donnée ayant été cachée localement par votre application ou votre système d'exploitation pouvant être exclue. Pour obtenir des instantanés cohérents sur les volumes associés à une instance, nous recommandons de dissocier correctement le volume, d'exécuter la commande de génération d'instantanés, puis de réassocier le volume. Pour les volumes Amazon EBS qui font office de périphériques racine, nous recommandons d'éteindre la machine pour effectuer un instantané précis.

Q : La création d'un instantané (snapshot) de l'intégralité d'un volume de 16 To dure-t-elle plus longtemps que celle de l'intégralité d'un volume d'un To ?
Non, la création d'un instantané EBS de l'intégralité d'un volume de 16 To ne dure pas plus longtemps que celle de l'intégralité d'un volume d'un To.

Q : EBS Encryption prend-il en charge les volumes de démarrage ?
Oui.
