# Fast Recovery - retour arrière rapide


Un objectif clé du déploiement continu est de réduire le risque de publication de logiciels. Contre-intuitivement, un déploiement continu efficace réduit en fait le risque d'une livraison. On souligne quatre principes fondamentaux qui nous amènent à la réduction du risque de livraison:

* Les livraisons à faible risque sont progressifs.
* Découpler le déploiement et la livraison.
* Concentrer sur la réduction de la taille des lots.
* Optimiser pour la résilience.


# Les livraisons à faible risque sont progressifs
Toute organisation de maturité raisonnable aura des systèmes de production composés de plusieurs composants ou services liés entre eux, avec des dépendances entre ces composants. Par exemple, mon application peut dépendre d'un contenu statique, d'une base de données et de certains services fournis par d'autres systèmes. La mise à niveau de tous ces composants dans une version Big-Bang est le moyen le plus risqué de déployer de nouvelles fonctionnalités.

> Déployez plutôt les composants indépendamment.

Les modifications de base de données peuvent également être déployées progressivement. Même les entreprises comme Flickr, qui déploient plusieurs fois par jour, ne déploient pas fréquemment les modifications de bases de données. 

>La règle est que on ne modifie jamais les objets existants d'un seul coup. 

Au lieu de cela, divisez les modifications en étapes réversibles:

1. Avant la sortie de la version, ajoutez à la base de données de nouveaux objets qui seront requis par cette nouvelle version.

2. Libérez la nouvelle version de l'application, qui écrit dans les nouveaux objets mais lit, si nécessaire, à partir des anciens objets afin de migrer les données "lazily". Si on doit revenir en arrière à ce point, on peut le faire sans avoir à annuler les modifications apportées à la base de données.

3. Enfin, une fois que la nouvelle version de l'application est stable et que on ne sera plus obligé de revenir en arrière, appliquez le script de contrat pour terminer la migration des anciennes données et supprimer tous les anciens objets.

# Découpler le déploiement et la livraison.

Le déploiement est ce qui se produit lorsque on installe une version de votre logiciel dans un environnement particulier (l'environnement de production est souvent impliqué). La livraison consiste à rendre un système ou une partie de celui-ci (par exemple, une fonctionnalité) disponible pour les utilisateurs.

# Concentrer sur la réduction de la taille des lots.

Lorsque nous réduisons la taille des lots, nous pouvons déployer plus fréquemment, car réduire la taille des lots entraîne une réduction du temps de cycle. Pourquoi cela réduit-il les risques? Lorsqu'une équipe d'ingénierie de publication passe un week-end dans un centre de données pour déployer le travail des trois derniers mois, la dernière chose qu'ils souhaitent faire est de déployer à nouveau de sitôt. Mais comme l'expliquent Dave Farley et Jez Humble dans le livre "Continuous Delivery: Reliable Software Releases through Build", quand quelque chose fait mal, la solution est de le faire plus souvent et d'exposer la douleur.

![Kibana Dev Tools](/batch-size.png)

Un déploiement en production plus souvent permet de réduire le risque de chaque version pour trois raisons:

* Lorsque on déploie plus souvent en production, on pratique plus souvent le processus de déploiement. Par conséquent, on trouve et résoudre les problèmes plus tôt (et, espérons-le, dans les déploiements dans des environnements de préproduction), et le processus de déploiement lui-même changera moins entre les déploiements.

* Lorsque on déploie plus fréquemment, il est beaucoup plus facile de résoudre les problèmes qui se posent, car les changements sont beaucoup moins importants. Il nous faudra beaucoup de temps pour trouver ce qui ne va pas si on a plusieurs mois de changements accumulés. On annulera probablement la publication si on rencontre un problème critique. Toutefois, si on déploie plusieurs fois par semaine, les changements entre les versions sont minimes et ils constituent probablement un bon point de départ pour trouver les causes de l'incident.

* Enfin, annuler un petit changement est beaucoup plus facile que d’annuler des mois de travail. Sur le plan technique, le nombre de composants affectés est beaucoup plus petit; sur le plan commercial, il est généralement beaucoup plus facile de persuader l'équipe de revenir sur une petite fonctionnalité plutôt que sur une vingtaine de fonctionnalités.

# Optimiser pour la résilience

La résilience est la capacité "d'absorber ou d'éviter les dommages sans subir un échec total" et est obtenue en minimisant le temps moyen de réparation (MTTR) des services. Certaines classes d'échecs ne devraient jamais se produire, certaines échecs sont plus coûteuses que d’autres, et certains systèmes critiques pour la sécurité ne devraient jamais avoir d'échecs, mais en général, les entreprises doivent adhérer à l’avis de John Allspaw selon lequel "il est plus important de pouvoir récupérer rapidement d’une panne que d'avoir des échecs moins souvent ".

# Conclusions

Avec ces techniques, on peut réduire considérablement le risque de livraison pour les utilisateurs. Cependant, ils entraînent des coûts de développement supplémentaires et nécessitent une planification préalable. Ces types de coûts sont souvent difficiles à justifier, en partie parce que les gens ont tendance à sous-estimer une récompense qui existera à l’avenir. (Il s'agit d'un biais comportemental connu sous le nom "emporal discounting"). C'est l'une des raisons pour lesquelles il est important de réduire la taille des lots et, par conséquent, la réduction du délai d'exécution. On obtien également beaucoup plus tôt des commentaires sur les avantages de la modification de notre processus de livraison, ce qui augmente la motivation de l'équipe.

Les équipes opérationnelles résistent souvent au changement, parce que tout changement comporte des risques. Bien que cela soit vrai, on ne devrait pas essayer de réduire la fréquence des changements, car cela conduit à son tour à des déploiements «big bang» à risque elevé. Au lieu de cela, on crée des services plus stables et fiables en renforçant la résilience des systèmes et en s'efforçant de minimiser et d'atténuer les risques liés à chaque changement.

# Références

* https://dzone.com/articles/risks-big-bang-deployments-and
* http://www.informit.com/articles/article.aspx?p=1833567&seqNum=4
* https://www.joetheitguy.com/2017/11/22/5-awkward-questions-production-end-devops-pipeline/
* https://dzone.com/articles/the-value-of-optimising-for-resilience
