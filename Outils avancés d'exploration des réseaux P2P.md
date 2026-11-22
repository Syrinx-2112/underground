# Outils avancés d'exploration des réseaux P2P

Nov 22, 2026 · @Priest of Syrinx

Cette étude prolonge la réponse Grok d'origine (une vingtaine de dépôts classés par réseau) en une base de travail : taxonomie des méthodes, outils vérifiés réseau par réseau, simulation, observation passive, visualisation, biais de mesure et cadre éthique. Les outils décrits d'après une page ou un dépôt ouvert le 22/11/2026 renvoient à leur source ; les autres sont marqués « à confirmer ».

## 1. Cadrage et taxonomie

Explorer un réseau P2P revient à répondre à quatre questions : **qui est présent** (découverte), **qui est relié à qui** (topologie), **que disent les pairs** (protocoles, contenus) et **comment le réseau se comporte** (churn, latence, robustesse). Chaque question appelle une famille d'outils différente.

| Famille | Principe | Outils typiques | Limite principale |
| --- | --- | --- | --- |
| Crawl actif | Interroger récursivement les tables de routage des pairs (k-buckets Kademlia, GETADDR Bitcoin, FINDNODE discv5) | Nebula, `devp2p discv4/discv5 crawl`, crawlers Bitcoin, Bitnodes | Ne voit pas les nœuds derrière un NAT ; l'instantané n'est jamais simultané |
| Nœuds sentinelles | Déployer des nœuds qui reçoivent le trafic légitime du réseau | forks de mldht (BoonTorrent), honeynodes de peer-observer | Biais de position (identifiant, adresses) ; coût d'exploitation |
| Observation passive | Capturer et disséquer le trafic d'un point du réseau | Wireshark et ses dissecteurs, Zeek (à confirmer pour chaque protocole) | Vision locale ; trafic chiffré |
| Simulation et émulation | Reproduire le protocole dans un modèle ou un réseau virtuel | PeerSim, OverSim, ns-3, Testground, Locust, GNUnet | Validité externe : le modèle n'est pas le réseau réel |

Le vocabulaire commun aux réseaux Kademlia (libp2p, Mainline DHT, discv4/discv5, I2P netDb) :

- **Distance XOR** : la proximité entre deux identifiants est le OU exclusif de leurs bits ; elle ne reflète aucune proximité géographique.
- **k-bucket** : liste de pairs groupés par préfixe commun avec l'identifiant local (CPL). Un crawler génère des clés à CPL donné pour « tirer » chaque bucket d'un pair.
- **Session** : période continue pendant laquelle un pair répond ; sa durée alimente les études de churn.
- **Bootstrap** : pairs de départ connus à l'avance, point d'entrée de tout crawl.

## 2. Vue d'ensemble

&#91;embedded content: chaîne d'exploration · 6 étapes dont une validation\]

La chaîne se lit de haut en bas : on découvre les pairs, on les sonde, on stocke les données brutes, on les analyse, puis on les valide avant de les visualiser. L'étape de validation, en couleur, est celle que les projets personnels sautent le plus souvent ; les biais de la section 11 s'y corrigent.

## 3. libp2p, IPFS et Filecoin

Nebula est l'outil de référence pour mesurer un réseau libp2p : il sert de base aux rapports hebdomadaires de ProbeLab sur la DHT Amino d'IPFS. Les autres entrées de la liste d'origine sont plus modestes ou plus anciennes.

| Outil | Rôle | Points vérifiés | Statut |
| --- | --- | --- | --- |
| [Nebula](https://github.com/dennis-tra/nebula) (ProbeLab) | Crawler + moniteur de DHT Kademlia | Supporte IPFS Amino, Filecoin, Ethereum CL, Polkadot, Kusama, Celestia, Pactus ; sorties JSON ou PostgreSQL ; `--neighbors` conserve les k-buckets | Actif, base des rapports ProbeLab ; la branche main n'est pas stable |
| [ipfs-crawler](https://github.com/trudi-group/ipfs-crawler) (ex-wiberlin) | Crawler de la partie Kademlia de libp2p, produit le graphe de voisinage | Enregistre pour chaque nœud l'identifiant, les multiadresses, la version d'agent et les protocoles ; adossé à l'article de Henningsen et al. (2020) ; 69 étoiles | Dépôt déplacé vers trudi-group ; dernière synchro observée en 2025 |
| [p2p-tools](https://github.com/notassigned/p2p-tools) | Boîte à outils CLI libp2p | Sous-commandes `ping`, `dht peer`, `dht content`, `provide`, `pubsub listen/publish` ; Go 1.17+ ; 11 commits | Utilitaire minimal, pratique pour des essais manuels |
| `dht-graph` (aschmahmann) | Crawler de DHT exportant en GraphViz | Introuvable lors de la vérification ; le README de Nebula cite le `basic-crawler` de go-libp2p-kad-dht du même auteur comme source d'inspiration | À confirmer |
| `libp2p-dht-scrape-aas` (alanshaw) | Scraper de DHT « as a service » | Non vérifié | À confirmer |

### Comment Nebula explore la DHT

Le crawler part d'un jeu de pairs d'amorçage, puis suit chaque entrée de leurs tables de routage jusqu'à épuisement des nouveaux identifiants. Comme il connaît l'identifiant d'un pair, il en déduit ses buckets et fabrique des clés aléatoires à préfixe commun donné pour interroger chacun d'eux ; la page ProbeLab indique que les buckets 0 à 15 sont interrogés en parallèle. Le moniteur rappelle ensuite les pairs découverts avec un recul exponentiel pour mesurer disponibilité et churn : il ne fait qu'un appel de transport, sans négociation de protocole.

Un crawl enregistre, pour chaque pair, la liste des protocoles et la chaîne d'agent obtenues lors d'une vraie connexion libp2p. Les durées de session (suite ininterrompue de réponses) servent aux études de churn.

```bash
# Simple passage à blanc : affiche un résumé sans rien écrire
nebula --dry-run crawl

# Autre réseau, limité à 1000 pairs
nebula --dry-run crawl --network FILECOIN --limit 1000

# Sauvegarde JSON avec les k-buckets (graphe de voisinage)
nebula --json-out ./results/ crawl --neighbors
```

Les tables de routage complètes représentaient environ 250 Mo pour la DHT Amino en avril 2023, d'où l'option désactivée par défaut. Le fichier `*_neighbors.json` se convertit en liste d'adjacence avec `jq` pour alimenter Gephi, Graphviz ou NetworkX.

### Cadence et limites

ProbeLab publie ses mesures à partir de crawls toutes les 2 heures ; la durée d'un crawl varie de 1 minute (Filecoin) à environ 20 minutes (Ethereum CL) et 23 minutes (Ethereum EL).

- **Contrainte de compilation** : selon les README consultés, Nebula exige Go 1.19 car il dépend d'un go-libp2p antérieur à 0.30, version qui a retiré le transport QUIC historique. De nombreux pairs IPFS n'écoutant que sur `quic` ou sur TCP, le gain est une meilleure joignabilité, au prix d'une pile logicielle figée. Vérifier la version courante avant d'installer.
- **Cohérence des chiffres** : l'auteur signale que ses chiffres sont cohérents avec ceux d'ipfs-crawler, mais diffèrent de ceux de MigaLabs pour Ethereum CL, vraisemblablement à cause d'une agrégation différente des pairs.
- **Observatoires publics** : les graphiques de [probelab.io](https://probelab.io) et la page « Periodic Measurements of the IPFS Network » de l'institut Weizenbaum donnent des séries déjà calculées, utiles avant de lancer son propre crawl.

### Pistes complémentaires à confirmer

Ces outils sont connus de l'écosystème libp2p mais n'ont pas été rouverts pour cette étude : les commandes de diagnostic de Kubo (`ipfs swarm peers`, `ipfs dht`), le banc d'essai Testground (voir la section simulation) et les implémentations officielles go-libp2p, js-libp2p et rust-libp2p.

## 4. BitTorrent et Mainline DHT

La Mainline DHT est un Kademlia minimaliste (quatre messages de contrôle, identifiants de 160 bits) et l'un des plus grands réseaux DHT en service ; un article de simulation l'évalue à environ 10 millions de pairs. C'est aussi le réseau le mieux outillé pour l'indexation, car le protocole a prévu une extension dédiée.

### Outils vérifiés

| Outil | Langage | Ce qu'il fait | Points à retenir |
| --- | --- | --- | --- |
| [shiyanhui/dht](https://github.com/shiyanhui/dht) | Go | Implémentation du protocole DHT, avec un mode standard et un mode « spider » qui récupère les métadonnées | Environ 2,8 k étoiles ; le mode spider ne suit pas les BEP ; environ 300 Mo de RAM par défaut ; ne fonctionne pas derrière un NAT |
| [the8472/mldht](https://github.com/the8472/mldht) | Java | Nœud « serveur » respectueux du protocole : BEP 5, 32, 33, 44, 45, 51 ; BEP 42 partiel | Composants optionnels : indexation automatique (environ 0,3 torrent/s en mono-adresse), export Redis des couples infohash-IP observés, CLI distante |
| [bitmagnet](https://github.com/bitmagnet-io/bitmagnet) | Go + TypeScript | Indexeur BitTorrent auto-hébergé : crawler DHT, classification, interface web, API GraphQL | Successeur fonctionnel de magnetico, dont l'auteur dit qu'il n'est plus maintenu |
| [pythonTorrentDHT](https://pypi.org/project/pythontorrentdht/) | Python | DHT complète en Python pur, évolution de nitmir/btdht ; BEP 5, 9, 10, 51 | Pratique pour prototyper un échantillonneur en quelques lignes |
| [DHT-Telescope](https://www.github.com/cas1m1r/DHT-Telescope) | Python | Crawler actif et passif, `visualizer.py` place les nœuds sur une carte | Projet personnel de 2025, démonstrateur plutôt qu'outil de mesure |
| [BoonTorrent](https://github.com/huangyingting/boontorrent) | Scala, Python, JS | Surveillance en temps réel du trafic DHT par des nœuds qui écoutent, basés sur un fork de mldht | Projet étudiant de 2018 (Penn), utile comme architecture de référence |
| [Code de Wang et Kangasharju](https://www.cl.cam.ac.uk/~lw525/MLDHT/) | Python | `crawler.py`, `injector.py`, `maintainer.py`, `logparser.py` | Matériel de recherche de 2013 : à lire plus qu'à déployer |

Les crawlers `p2pspider` (thejordanprice), `dhtc` (nbdy) et `magnet-dht` (chenjiandongx) cités dans le document d'origine n'ont pas été rouverts : à confirmer.

### L'extension BEP 51 : échantillonner au lieu d'écouter

La spécification BEP 51 (statut Draft) explique que l'indexation par observation passive des requêtes `get_peers` est possible mais inefficace : elle avantage les indexeurs disposant de nombreuses adresses IP. Elle ajoute donc une requête `sample_infohashes` qui renvoie un échantillon aléatoire d'infohashes (multiples de 20 octets) que le nœud interrogé détient. Le champ `target` permet de parcourir l'espace de clés avec une seule requête par nœud, et la réponse indique `interval` (délai minimal avant de réinterroger) et `num` (estimation du nombre d'infohashes stockés).

```json
// Requête
{"t": "aa", "y": "q", "q": "sample_infohashes",
 "a": {"id": "<20 octets>", "target": "<20 octets>"}}
// Réponse
{"t": "aa", "y": "r",
 "r": {"id": "<20 octets>", "interval": 21600, "num": 1234,
       "samples": "<N x 20 octets>", "nodes": "<compact>"}}
```

Les valeurs ci-dessus sont illustratives ; seule la structure vient de la spécification. Des bibliothèques exposent déjà ce message : le paquet Go `anacrolix/dht`, le paquet de bitmagnet, une crate Rust (`irontide-dht`) et pythonTorrentDHT.

### Méthodologie : le biais des nœuds manquants

Wang et Kangasharju (IEEE P2P 2013) identifient une erreur systématique supérieure à 40 % dans les estimations antérieures de la taille de réseaux DHT (Mainline, KAD, Vuze), due aux nœuds que les crawlers ne voient pas. Ils proposent un **facteur de correction**, stable dans un environnement donné, qui rend les résultats de différents crawlers cohérents. Employer un très grand nombre d'échantillonneurs simultanés dispense du facteur, mais exige davantage de ressources.

### Sécurité observée

Les mêmes auteurs ont montré par des honeypots, dans un article de GLOBECOM 2012, que des attaques Sybil visant les tables de routage et les contenus de la Mainline DHT étaient en cours depuis longtemps et à grande échelle. Le défi pour un outil de mesure est donc double : ne pas être confondu avec un attaquant, et ne pas prendre pour des pairs légitimes des nœuds Sybil injectés.

## 5. Réseaux de blockchains

Les réseaux de cryptomonnaies sont les plus instrumentés, parce que leur santé (diversité des clients, concentration géographique, résistance à l'éclipse) intéresse directement les opérateurs. Deux mécaniques de découverte dominent : le message `GETADDR` de Bitcoin et ses dérivés, et les DHT discv4/discv5 d'Ethereum.

### Bitcoin et dérivés

| Outil | Approche | Points vérifiés |
| --- | --- | --- |
| [Bitnodes](https://github.com/ayeowch/bitnodes) (Addy Yeow) | Crawl actif : `getaddr` récursif depuis des nœuds d'amorçage | Python (gevent) avec Redis ; ignore les protocoles antérieurs à la version 70001 ; API de captures du réseau ; référence historique |
| [Coincrawler](https://github.com/Nutomic/coincrawler) | Fork de Bitnodes | Configurations pour Bitcoin, Bitcoin Cash, Dash, Litecoin |
| [virtu/p2p-crawler](https://github.com/virtu/p2p-crawler) | Crawl actif multi-transport | IPv4, IPv6, Onion et I2P ; déploiement par flake Nix ; options de délais par transport |
| [openbitcoincom/node-crawler](https://github.com/openbitcoincom/node-crawler) | Recensement des nœuds joignables | Poignée de main `version`/`verack`, géolocalisation hors ligne, schéma Postgres, licence MIT ; recensement planifié tous les dix jours |
| [bitcoin-peers-crawler](https://docs.rs/bitcoin-peers-crawler) | Bibliothèque Rust asynchrone | Limites de concurrence, politique de transport (v2 chiffré exigible), version de protocole minimale |
| [AtlasP2P](https://github.com/RaxTzu/AtlasP2P) (RaxTzu) | Plateforme de carte de nœuds, forkable | Crawler Python, GeoLite2, tuiles OpenStreetMap/CARTO, tableau de bord d'administration ; première instance : Dingocoin ; inspirée de Bitnodes |
| [peer-observer](https://github.com/peer-observer/peer-observer) | Honeynodes passifs de Bitcoin Core | Extracteurs eBPF, RPC, P2P, journal et IPC publiant vers NATS (messages protobuf) ; outils de journalisation, d'archivage et de test de connectivité en Rust |
| `crawly` (jochemin) | Cité dans la liste d'origine | Introuvable lors de la vérification : à confirmer |

Trois précisions utiles à qui lance son propre crawler :

- Bitcoin Core ne répond pas immédiatement à `getaddr` : la réponse est mise en file derrière un minuteur de Poisson d'environ 30 secondes selon le README d'openbitcoin, de sorte que le crawler doit garder la connexion ouverte.
- Un crawler ne voit pas les nœuds derrière un NAT. Les travaux antérieurs estiment les nœuds inaccessibles à environ deux tiers du réseau.
- Une étude de 2018 (Ben Mariem et al.) a découvert plus de 200 000 adresses IP pour seulement 9 673 nœuds actifs, d'où l'importance de distinguer « adresse annoncée » et « nœud répondant ».

`peer-observer` mérite une mention à part : c'est de la surveillance défensive. Son auteur ne publie pas les adresses des honeynodes (un attaquant éviterait simplement de les attaquer) et prévient que l'installation est délicate, car les tracepoints exigent des privilèges élevés et des correctifs de Bitcoin Core non encore fusionnés au moment de la lecture du README.

### Ethereum : execution layer et consensus layer

| Outil | Cible | Points vérifiés |
| --- | --- | --- |
| [`devp2p`](https://geth.ethereum.org/docs/tools/devp2p) (Geth) | Couche d'exécution, discv4 et discv5 | `discv4`/`discv5 ping`, `resolve`, `crawl`, `listen` ; `nodeset info`/`filter` (par IP, âge, réseau eth, support snap) ; `enrdump` pour décoder un ENR ; `dns sync` ; suites de tests discv4, discv5 et `rlpx eth-test` |
| [ethereum/node-crawler](https://github.com/ethereum/node-crawler) | Nœuds d'exécution | Backend fondé sur devp2p, API et frontend de visualisation ; 109 étoiles d'après ecosyste.ms |
| [NodeWatch](https://blog.chainsafe.io/nodewatch-the-open-source-node-explorer-for-ethereum-consensus-eth2/) (ChainSafe pour l'Ethereum Foundation) | Nœuds de consensus | Explorateur ouvert fondé sur discv5, mesure la diversité des clients et la part des nœuds hébergés en cloud |
| [armiarma](https://github.com/migalabs/armiarma) (MigaLabs) | Consensus layer, libp2p | Crawler libp2p ; exporte les métriques Prometheus (port 9080 par défaut) ; filtre par `fork-digest` |
| [eth-light-crawler](https://github.com/migalabs/eth-light-crawler) (MigaLabs) | Taille du réseau via discv5 | Interroge des ENR aléatoires, stockage PostgreSQL |
| [ragno](https://github.com/migalabs/ragno) (MigaLabs) | Couche d'exécution | Connexion aux nœuds EL |
| [FastEthereumCrawler](https://ethresear.ch/t/crawling-the-ethereum-discv5-network-fast/20962) | discv5 | Fondé sur nim-eth ; parcourt de grandes portions du réseau en moins d'une minute |
| Nebula | Consensus et exécution | Voir la section 3 ; ethereum.org signale des instantanés hebdomadaires du graphe de pairs EL |

Exemple de pipeline minimal, d'après la documentation de `devp2p` :

```bash
# Cartographier la DHT discv5 dans un fichier JSON de nœuds
devp2p discv5 crawl nodes.json
# Statistiques et filtrage du jeu de nœuds
devp2p nodeset info nodes.json
devp2p nodeset filter nodes.json -eth-network mainnet -snap -limit 20
```

### Études multi-chaînes

Une étude longitudinale récente (arXiv 2511.15388) couvre 36 réseaux de cryptomonnaies : ses auteurs ont déployé 15 crawlers actifs pendant neuf mois à partir de fin 2024 et exploité des données de deux crawlers communautaires. Pour les dérivés de Bitcoin, un crawler fondé sur le client de référence n'a besoin que du « magic » du réseau et de nœuds d'amorçage ; le port par défaut change (8333 pour Bitcoin, 22556 pour Dogecoin, 9333 pour Litecoin). C'est la meilleure base méthodologique actuelle pour comparer les chaînes entre elles.

## 6. Réseaux d'anonymat : Tor et I2P

Ces deux réseaux s'étudient à l'échelle des relais et des routeurs, jamais à celle des utilisateurs : l'objectif est de mesurer la capacité, la diversité et la disponibilité du réseau, pas d'identifier qui s'en sert. Leur architecture explique des outillages très différents. Tor publie un annuaire central, donc la liste des relais est publique et interrogeable. I2P n'a pas d'annuaire centralisé, ce qui rend, selon les auteurs d'une étude présentée à l'IMC 2018, l'obtention d'une vue complète du réseau plus difficile.

### Tor

| Ressource | Rôle | Points vérifiés |
| --- | --- | --- |
| [Onionoo](https://metrics.torproject.org/onionoo.html) | API REST JSON sur les relais et les ponts | Points d'accès `summary`, `details`, `bandwidth`, `weights`, `clients`, `uptime` ; recherche par adresse IP, empreinte, type (relais ou pont) ; conçue comme source de données pour d'autres applications |
| [Tor Metrics](https://metrics.torproject.org/sources.html) | Graphiques officiels et sources | Spécifications publiées : protocole d'annuaire v3, fichiers de bande passante, listes de sorties TorDNSEL, statistiques Snowflake, BridgeDB et Bridgestrap ; page « Reproducible Metrics » pour refaire les calculs |
| Trackers de relais communautaires | Classements, familles, opérateurs | Plusieurs sites alimentés par Onionoo (dont celui de 1AEO, mis à jour toutes les 30 minutes d'après son annonce, qui prolonge le projet Allium arrêté) ; offrent agrégation par famille ou par système autonome et données historiques selon le site |

Un tracker n'est pas un crawler : il s'appuie sur ce que les autorités d'annuaire publient déjà. Pour la plupart des questions de recherche sur Tor (concentration de la bande passante, diversité d'hébergement, âge des relais), Onionoo suffit et évite de produire du trafic additionnel.

### I2P

| Ressource | Rôle | Points vérifiés |
| --- | --- | --- |
| netDb (documentation officielle) | Base distribuée de contacts | Deux types d'objets : RouterInfo (identité, adresses, options, signature) et LeaseSet (points d'entrée de tunnels d'une destination) ; distribuée par « floodfill » ; identifiant de réseau `netId=2` ; capacités publiées (floodfill, tranche de bande passante, joignabilité) |
| Statistiques publiées | Santé du réseau | Les routeurs floodfill publient `netdb.knownRouters` et `netdb.knownLeaseSets` ; des statistiques simplifiées agrégées par des sites comme stats.i2p (taux de construction de tunnels, par exemple) |
| [Hoang et al., IMC 2018](https://arxiv.org/pdf/1809.09086) | Mesure académique | Étude empirique du réseau I2P : population de pairs, churn, répartition ; mesure de la résistance au blocage |
| [Infrastructure VPN de mesure du blocage](https://www.arxiv.org/pdf/1907.07120) | Mesure de la censure | Évalue dans quels pays I2P est bloqué, à l'aide de serveurs VPN de volontaires |

Deux éléments d'actualité à garder en tête. La version 2.4.0 d'I2P a introduit une stratégie d'isolation des sous-bases de la netDb, que la version 2.5.0 a auditée, ce qui change ce qu'un observateur voit depuis un routeur donné. Et un signalement de 2026 sur net4people évoque un outil qui lit la netDb locale d'i2pd pour exporter des listes d'adresses à bloquer au niveau du pare-feu : cette information vient d'un résumé secondaire généré automatiquement, donc à confirmer à la source avant toute utilisation.

### Entrée de liste à confirmer

L'outil `iasds/i2p-network-scanner` (scan passif de la NetDB I2P) mentionné dans le document d'origine n'a pas pu être retrouvé lors de la vérification.

## 7. Autres réseaux

La liste d'origine couvrait surtout libp2p, BitTorrent et Bitcoin. Les réseaux ci-dessous ont une littérature de mesure ou des outils publics ; ils montrent surtout que la même méthode (amorçage, parcours récursif, vérification de cohérence) se transpose d'un protocole à l'autre.

| Réseau | Outils et méthode | Résultats ou points de repère vérifiés |
| --- | --- | --- |
| Monero | Nebula étendu au protocole Levin (poignée de main et liste de pairs de 250 entrées au maximum, `PING` dont la réponse contient l'identifiant du pair) ; outils des articles académiques ; tableaux de bord publics monero.fail et xmrnetscan | Cao et al. (2020) : 21 678 adresses IP vues, environ 20 % actives, 13,2 % des nœuds maintenant 82,86 % des connexions. Kopyciok et al. (AFT 2026) : environ 14,38 % de pairs au comportement anormal, une entité contrôlant au moins 1 582 nœuds. Exposé ProbeLab 2026 : plus de 29 000 nœuds découverts, plus de 16 000 poignées de main réussies |
| Gnutella, eMule/aMule (Kad), Azureus | Crawlers historiques : Blizzard (Steiner et al.), Cruiser (Stutzbach et al., architecture à modules par protocole) | Blizzard explore une zone de préfixe 8 bits en moins de 2,5 secondes, ce qui permet de balayer tout l'espace d'identifiants de Kad ; certains clients changent d'identifiant (« chameleons ») et faussent les décomptes |
| Hyphanet (ex-Freenet) | Mesures sur le mode ouvert (opennet) ; le mode darknet, qui ne relie que des amis, est très difficile à détecter de l'extérieur | Roos et al. (PETS 2014) : topologie, nombre d'utilisateurs, churn, popularité des fichiers ; la topologie réelle diffère beaucoup des modèles |
| Nostr | nostr.watch (relais, NIP-11, NIP-66, événements de type 10002), BigBrotr (six services asynchrones, PostgreSQL 16+, Python 3.11+) | Les relais sont découverts à l'exécution à partir des listes publiées par les utilisateurs ; le dépôt nostr.watch se déclare en alpha et en réécriture |
| Matrix | Étude « A Glimpse of the Matrix » : un robot (DSN Traveller) hébergé sur un serveur privé pour limiter l'interférence ; points d'accès de découverte `/.well-known/matrix/server` et de fédération | Jeu de données de structure anonymisé publié par les auteurs |
| Veilid | Bibliothèque `veilid-core` (Rust) avec configuration DHT | Aucun outil public de mesure retrouvé : à confirmer |

Précisions utiles :

- **Détecter les nœuds hostiles est devenu un sujet à part entière.** Sur Monero, le contrôle de cohérence entre l'identifiant annoncé à la poignée de main et celui renvoyé au `PING` sert à repérer des nœuds de surveillance. Les auteurs de l'étude ProbeLab publient ce résultat comme un instantané neutre et notent que les nœuds derrière un NAT restent invisibles au crawler.
- **Les vieux articles restent la meilleure école de méthode.** Les travaux de l'équipe d'Eurécom sur Kad et Azureus et ceux de Stutzbach sur le churn décrivent les pièges classiques (pannes du crawler, instantanés partiels, redondance des crawlers) que les outils récents ont fini par intégrer.
- **Couverture limitée.** Secure Scuttlebutt, Solana (gossip) et Polkadot ne figurent ici que par ricochet : Nebula supporte Polkadot et Kusama ; je n'ai pas vérifié d'outil dédié aux autres.

## 8. Simulation, émulation et bancs d'essai

Explorer un réseau réel ne permet ni de contrôler les conditions ni de reproduire une expérience. La simulation et l'émulation comblent ce manque : on y valide une méthode de crawl, on mesure l'effet d'un changement de protocole ou d'une attaque sans toucher au réseau public.

| Outil | Type | Points vérifiés |
| --- | --- | --- |
| [Testground](https://github.com/testground/testground) (Protocol Labs) | Banc d'essai de systèmes distribués | Conçu pour tester, évaluer et simuler des systèmes P2P de 2 à 10 000 instances ; plans de test multi-langages (constructeurs `exec:go`, `docker:go` ; exécuteurs `local:exec`, `local:docker`, `cluster:k8s`) ; métriques vers InfluxDB ; plans libp2p (`libp2p/dht`, cas `find-peers`) |
| IPTB et libp2p testlab | Gestion de grappes locales | IPTB gère des grappes d'instances IPFS ; testlab est un déployeur Nomad avec langage de scénarios |
| PeerSim | Simulateur Java à événements discrets | Peut simuler des réseaux de millions de nœuds ; utilisé comme référence pour comparer une stratégie de placement à un DHT Kademlia |
| PeerfactSim.KOM | Simulateur Java modulaire | Couches de recouvrement variées (Chord, CAN, Pastry, Kademlia, Gnutella, etc.), supervision et visualisation intégrée |
| Simulateur Kademlia distribué (KIT) | Simulation parallèle | Exploite jusqu'à 16 processus logiques ; démontre la simulation à pleine échelle de la Mainline DHT (environ 10 millions de pairs) |
| OverSim, ns-3 | Cadres de simulation généralistes | Cités par la littérature comme références ; non rouverts pour cette étude |
| Locust | Cadre d'expérimentation DHT concurrent | Orienté évaluations de sécurité (ICCNS 2020) : modéliser des artefacts d'implémentation que les simulateurs abstraits ignorent |
| QUANTAS | Simulateur abstrait | Implémentations de Chord et Kademlia ; confirme des routes de requête logarithmiques en la taille du réseau |
| `py-DHT` | Module Python | Simulateur de DHT utilisé dans une étude de l'échantillonnage de disponibilité des données pour Ethereum, validée sur le réseau IPFS réel |
| GNUnet (émulation) | Émulation sur grappe | A exécuté une DHT de type Kademlia de 80 000 pairs sur 32 machines, avec communications chiffrées ; autorise des topologies sous-jacentes restreintes (pare-feu, NAT), ce que les autres simulateurs DHT consultés ne permettaient pas |
| `ethp2p` + netviz | Simulateur et visualiseur | Le simulateur écrit des traces `.bctrace`, que netviz rejoue (voir section 10) |

### Quand choisir quoi

- **Valider un crawler** : une topologie connue dans Testground ou une émulation GNUnet, puis comparer ce que le crawler « voit » à la vérité terrain. C'est la réponse directe au problème des nœuds manquants de la section 4.
- **Étudier un protocole** : PeerSim ou PeerfactSim.KOM pour des centaines de milliers de nœuds, avec les limites de réalisme d'un modèle.
- **Évaluer une attaque ou un correctif** : Locust ou Testground, qui exécutent du code de production plutôt qu'un modèle. Les travaux sur les attaques Sybil et d'éclipse dans la DHT d'IPFS illustrent cette approche, avec des contre-mesures comme SR-DHT-Store ou S/Kademlia.

La taille d'une simulation ne remplace jamais la mesure : les artefacts que les modèles abstraient (NAT, pairs hétérogènes, versions logicielles) sont justement ceux qui faussent les crawls.

## 9. Observation passive du trafic

L'observation passive répond à une question que le crawl actif ne peut pas poser : que se passe-t-il autour de *mon* nœud ? Elle ne produit pas de trafic de mesure, mais sa vue est locale.

| Outil | Usage | Points vérifiés |
| --- | --- | --- |
| Wireshark, dissecteur `bt-dht` | Décoder la DHT BitTorrent en bencode | Champs filtrables (`bt-dht.id`, `bt-dht.peers`, `bt-dht.nodes`, `bt-dht.bencoded.string` etc.) ; documente BEP 5 et BEP 42 ; en pratique il faut restreindre à un port UDP (par exemple `udp.port == 51413`) puis décoder |
| Wireshark, dissecteur `vuze-dht` | DHT Vuze (Azureus) | Champs `vuze-dht.action`, `contact`, `dht_size`, etc. |
| [nDPI](https://www.ntop.org/ndpi/) (ntop) | Identification de protocoles applicatifs | Bibliothèque LGPLv3, plus de 450 protocoles ; reconnaît BitTorrent, eDonkey, Gnutella ; ne déchiffre pas ; capture à fournir (libpcap, PF\_RING) |
| mldht, `PassiveRedisIndexer` | Statistiques de pairs vus par infohash | Exporte les couples infohash-IP observés vers Redis |
| peer-observer | Honeynodes Bitcoin Core | Voir section 5 : événements extraits, publiés sur NATS, traités par des outils Rust |
| Zeek | Journaux de connexions et protocoles | Outil courant d'analyse de réseau ; non rouvert pour cette étude, à confirmer pour chaque protocole P2P |

```bash
# Exemple : isoler les paires requête/réponse DHT d'un client BitTorrent
tshark -r capture.pcapng -d udp.port==51413,bt-dht -Y "bt-dht"
```

La commande ci-dessus est illustrative : elle suppose un client BitTorrent écoutant sur le port UDP 51413 et la décode comme DHT ; adaptez le port au client observé.

### Limites de l'observation passive

Le chiffrement (TLS, Noise, transports obfusqués) réduit la visibilité aux métadonnées de flux. L'observation est aussi biaisée par la position : un nœud ne voit que les pairs qui le contactent, ce qui favorise les pairs actifs et les identifiants proches du sien dans l'espace XOR. Enfin, le trafic capturé contient des adresses IP de tiers : voir la section 12.

## 10. Stockage, analyse et visualisation

Un crawl produit des millions de lignes (pairs, adresses, sessions, voisins). La valeur du travail dépend moins du crawler que de la chaîne qui suit : stocker proprement, calculer des indicateurs, puis visualiser.

| Étape | Outils observés | Remarque |
| --- | --- | --- |
| Stockage | PostgreSQL (Nebula, eth-light-crawler, schéma d'openbitcoin) ; Redis (Bitnodes, mldht) ; NATS + protobuf (peer-observer) ; fichiers JSON | Garder les données brutes horodatées en UTC : on recalcule plus tard, on ne re-crawle pas |
| Supervision | Prometheus (armiarma, port 9080 par défaut) ; Grafana (tableaux de bord de Nebula, non fournis dans son dépôt) | Utile pour surveiller le crawler lui-même |
| Analyse de graphe | NetworkX (jusqu'à environ 10 000 nœuds), igraph écrit en C (100 000 nœuds et plus) | Pour un graphe de voisinage d'un million de pairs, privilégier igraph ou un traitement par échantillonnage |
| Exploration visuelle | Gephi (jusqu'à environ 100 000 nœuds et un million d'arêtes selon son site ; import GEXF, GraphML, DOT, CSV) ; Graphviz (format DOT, produit par `dht-graph`) ; PyVis | Chaîne classique : NetworkX écrit un fichier GEXF que Gephi ouvre (tutoriel Bellingcat), la couleur des nœuds suit leur type |
| Visualisation web | netviz (deck.gl, WebGL2, lecture temporelle) ; ccNetViz (WebGL, licence GPLv3) | netviz lit des traces de simulation ; l'adapter à des données de crawl suppose d'écrire un décodeur (extension prévue par l'outil) |
| Géolocalisation | MaxMind GeoLite2 (AtlasP2P) ; base IP-pays hors ligne (openbitcoin) ; tuiles OpenStreetMap et CARTO | La géolocalisation d'IP est approximative et brouillée par l'hébergement cloud, les VPN et Tor |

### Indicateurs à calculer

Ces indicateurs sont une proposition de méthode, pas un résultat tiré d'une source :

- **Taille et disponibilité** : nombre de pairs joignables par crawl, distribution des durées de session, taux de retour.
- **Structure du graphe** : distribution des degrés entrants et sortants, taille de la plus grande composante connexe, coefficient de clustering, centralité d'intermédiarité sur un échantillon.
- **Concentration** : part des N premiers systèmes autonomes ou pays, indice de Herfindahl, nombre minimal d'entités couvrant 50 % des nœuds.
- **Diversité logicielle** : répartition des versions et chaînes d'agent (Nebula les enregistre pour libp2p ; Bitnodes pour Bitcoin).

```python
# Illustratif, non testé : métriques de base sur un graphe lu depuis un CSV src,dst
import csv
import igraph as ig

edges = [(r[0], r[1]) for r in csv.reader(open('edges.csv'))]
g = ig.Graph.TupleList(edges, directed=True)
print('noeuds', g.vcount(), 'aretes', g.ecount())
print('composante faible max', max(g.connected_components(mode='weak').sizes()))
print('degre sortant moyen', sum(g.outdegree()) / g.vcount())
g.write_graphml('graph.graphml')  # pour Gephi
```

## 11. Méthodologie et biais de mesure

Un chiffre de crawl n'est jamais « la taille du réseau » : c'est le nombre de pairs qu'un outil donné a pu joindre, à un moment donné, d'une certaine façon. Les écarts entre outils viennent presque toujours des biais ci-dessous.

| Biais | Symptôme | Parade |
| --- | --- | --- |
| Nœuds injoignables (NAT, pare-feu) | Le crawler ne voit que les pairs acceptant les connexions entrantes ; pour Bitcoin, les travaux antérieurs estiment les nœuds inaccessibles à environ deux tiers du réseau | Annoncer que la mesure porte sur le réseau joignable ; croiser avec une mesure passive si possible |
| Nœuds manquants | Sous-estimation systématique de plus de 40 % dans des travaux antérieurs sur Mainline DHT (Wang et Kangasharju) | Facteur de correction, ou très nombreux échantillonneurs simultanés |
| Instantané non simultané | Un crawl dure de 1 à 23 minutes selon le réseau chez ProbeLab, environ une heure pour un crawler Bitcoin de 2018 : des pairs partent ou arrivent pendant le parcours | Reporter la durée du crawl ; raccourcir par la parallélisation ; croiser avec le moniteur d'uptime |
| Complétude inconnue | Pas de vérité terrain | Liste de nœuds « canaris » connus (passerelles publiques, nœuds à soi) : ipfs-crawler vérifie si ces nœuds apparaissent dans le crawl |
| Churn et sessions | Une période d'observation courte surestime les sessions courtes, une longue estime mal les départs définitifs | Moniteur avec recul exponentiel (Nebula) ; observer sur plusieurs mois pour les sessions longues (Steiner et al.) |
| Identités multiples | Un pair compte plusieurs fois : changement d'identifiant à la reconnexion (Azureus, « chameleons » de Kad), nœuds Sybil (Mainline, Monero) | Compter en adresses IP, identifiants et machines distincts ; chercher les clusters anormaux |
| Dérive logicielle de l'outil | Les chiffres changent sans que le réseau change : le README de Nebula indique qu'après le retrait du transport QUIC historique de go-libp2p, le nombre de pairs injoignables a nettement augmenté | Épingler et publier les versions de l'outil et des bibliothèques |
| Filtrage par protocole | Bitnodes ignore les versions de protocole anciennes ; shiyanhui/dht bloque les IP qui lui semblent mauvaises et peut en bloquer de bonnes | Documenter les filtres ; comparer avec un second outil |
| Effet du crawler lui-même | Le crawler est détecté, limité en débit ou banni ; ses sondages modifient les tables de routage | Comportement poli, identification claire (agent utilisateur), limites de débit |
| Agrégation différente | Nebula et MigaLabs donnent des chiffres différents pour Ethereum CL | Comparer les méthodes d'agrégation avant de comparer les résultats |

### Protocole de reproductibilité

1. Publier la liste des pairs d'amorçage, les versions de l'outil et de ses dépendances, la configuration complète et l'horodatage UTC de chaque crawl.
2. Conserver les données brutes et dériver les agrégats par script, jamais à la main.
3. Exécuter au moins deux outils indépendants sur le même réseau, comme l'ont fait les auteurs de Nebula en se comparant à ipfs-crawler.
4. Valider d'abord en simulation ou dans un banc d'essai (section 8) sur une topologie connue.
5. Écrire séparément ce qui est mesuré, estimé et inféré.

## 12. Éthique, droit et sécurité opérationnelle

Cette section donne des repères de bonne pratique, pas un avis juridique : pour un projet publié ou mené dans un cadre professionnel, faire valider le périmètre par un juriste ou le délégué à la protection des données de l'organisme.

### Données personnelles

- **Les adresses IP sont des données personnelles en pratique.** Un crawl produit des listes d'IP de particuliers. Appliquer la minimisation : ne garder que ce qui sert la question, pseudonymiser (hachage salé) avant tout partage, fixer une durée de conservation, et publier des agrégats plutôt que des listes brutes.
- **Exemples de pratique prudente dans la littérature** : l'étude sur la fédération Matrix a publié une structure de réseau anonymisée et fait tourner son robot depuis un serveur privé dédié pour limiter l'interférence ; l'auteur de peer-observer ne divulgue pas les adresses de ses honeynodes ; l'étude IMC 2018 sur I2P consacre une section à ses principes éthiques de collecte ; les travaux de Wang et Kangasharju sur la Mainline DHT mentionnent, dans la conception de leurs honeypots, la volonté d'éviter les problèmes juridiques.
- **Autorité de référence en France** : la CNIL. Le cadre de recherche du RGPD (minimisation, finalité, analyse d'impact quand le risque est élevé) s'applique à ces collectes.

### Périmètre technique et responsabilité

- **Rester dans le protocole.** Interroger un pair par les messages normaux du protocole (poignée de main, `find_node`, `getaddr`) n'est pas la même chose que sonder des services ou contourner des protections. En droit français, les articles du Code pénal sur l'accès frauduleux à un système de traitement automatisé (à partir de l'article 323-1) sont la limite à ne pas franchir : à faire confirmer pour tout cas limite.
- **Ne pas attaquer un réseau de production.** Les attaques Sybil, d'éclipse ou de saturation se testent en simulation ou sur un banc d'essai isolé (section 8), jamais sur le réseau public, y compris « pour la science ». Les contre-mesures publiées (S/Kademlia, SR-DHT-Store) montrent la marche à suivre : évaluer le correctif, pas exploiter la faille.
- **Divulgation responsable.** Une anomalie découverte (nœuds de surveillance, bug de protocole) se signale d'abord aux mainteneurs du client concerné, comme le font les équipes derrière peer-observer et l'analyse de Monero.
- **Anonymat : mesurer les relais, pas les personnes.** Sur Tor et I2P, utiliser les données publiques d'annuaire (Onionoo, netDb) ; tout travail visant à relier des utilisateurs ou des services cachés à des identités sort de ce document.

### Contenus et indexation

Les crawlers BitTorrent collectent des infohashes et des noms de torrents, qui peuvent désigner des contenus protégés ou illicites. Ne conserver que les métadonnées nécessaires, ne jamais télécharger les contenus, prévoir une procédure de retrait et de purge, et garder à l'esprit que des outils comme bitmagnet sont des moteurs de recherche de ces métadonnées. L'entrée `sci-hub-p2p` de la liste d'origine relève de la distribution de contenus et sort du périmètre de cette étude.

### Hygiène d'exploitation

- Héberger les crawlers sur une machine dédiée, avec une chaîne d'agent explicite et une adresse de contact ; prévoir des plaintes d'abus automatiques (les fournisseurs d'hébergement réagissent aux balayages massifs).
- Respecter les limites de débit, les bannissements et les listes d'exclusion : un crawler poli est aussi un crawler dont les mesures sont moins biaisées.
- Ne jamais lancer d'outil de balayage Internet à grande échelle (type ZMap) sans autorisation et sans avoir mesuré son empreinte réseau.

## 13. Corrections apportées au document d'origine

Sur les entrées de la réponse Grok, 6 ont été confirmées ou précisées, 3 sont introuvables et 12 n'ont pas pu être rouvertes (21 entrées au total). Aucune n'est contredite frontalement, mais plusieurs descriptions sont trop optimistes ou incomplètes.

| Entrée d'origine | Verdict | Précision |
| --- | --- | --- |
| `notassigned/p2p-tools` | Confirmé | CLI Go (1.17+) : `ping`, `dht peer`, `dht content`, `provide`, `pubsub` ; dépôt minuscule (11 commits). Utile comme exemple de code, pas comme outil de recherche |
| `aschmahmann/dht-graph` | Introuvable | Le README de Nebula cite le `basic-crawler` de go-libp2p-kad-dht, du même auteur, comme inspiration |
| `alanshaw/libp2p-dht-scrape-aas` | Non rouvert | À confirmer |
| organisation `libp2p` | Confirmé | Implémentations go, js et rust ; point d'entrée logique pour les outils de test |
| `shiyanhui/dht` (≈ 2,8 k étoiles) | Confirmé, à nuancer | Le mode « spider » ne suit pas les BEP, consomme environ 300 Mo et ne fonctionne pas derrière un NAT ; l'auteur reconnaît qu'un crawler lent s'explique par ces limites |
| `thejordanprice/p2pspider`, `nbdy/dhtc`, `chenjiandongx/magnet-dht`, `0xcaff/dht-crawler`, `mmathys/dht-crawler` | Non rouverts | À confirmer, y compris l'état de maintenance |
| `jochemin/crawly` | Introuvable | Alternatives vérifiées : Bitnodes, virtu/p2p-crawler, openbitcoin node-crawler |
| `RaxTzu/AtlasP2P` | Confirmé | Surtout une plateforme de carte de nœuds forkable pour dérivés de Bitcoin ; crawler en Python ; première instance : Dingocoin |
| `peer-observer` | Confirmé, à mettre à jour | Dépôt désormais sous l'organisation peer-observer ; instrumentation par honeynodes, installation non triviale, accès public limité |
| `nyonson/bitcoin-peers` | Non rouvert | Une crate `bitcoin-peers-crawler` existe sur docs.rs ; le lien avec cette entrée n'est pas établi |
| `ethp2p/netviz` | Confirmé, description corrigée | Moteur de visualisation de simulations de protocoles (traces `.bctrace` du simulateur ethp2p, deck.gl) ; ce n'est pas un visualiseur générique de crawls |
| `NeuronInnovations/neuron-p2p-diagnostics` | Non rouvert | À confirmer |
| ZMap | Non rouvert | Scanner Internet : hors périmètre P2P strict, voir section 12 |
| `awesome-p2p-networks`, `p2p-index` | Non rouverts | Bons points de départ s'ils sont maintenus : à vérifier |
| `iasds/i2p-network-scanner` | Introuvable | Aucun dépôt de ce nom retrouvé ; ne pas citer sans vérification |
| `sci-hub-p2p` | Non rouvert, hors périmètre | Distribution de contenus plutôt que mesure de réseau |

### Absents de la liste d'origine et ajoutés ici

Nebula et ses observatoires ProbeLab, ipfs-crawler (trudi-group), `devp2p` de Geth, Bitnodes et ses dérivés, mldht, bitmagnet, les crawlers Ethereum (node-crawler, armiarma, NodeWatch), Onionoo et Tor Metrics, Testground et les simulateurs de DHT, les dissecteurs Wireshark, nDPI, ainsi que les travaux académiques sur Monero, Kad, Hyphanet, Matrix et Nostr.

## 14. Recommandations et suite de l'étude

Le meilleur ordre de travail est de lire, observer, crawler, valider, puis publier : chaque étape réduit le risque de produire des chiffres faux.

1. **Lire** : l'article Kademlia (Maymounkov et Mazières, 2002), BEP 5 et BEP 51, l'article de Wang et Kangasharju (2013) pour la méthode, la synthèse d'Urdaneta et al. sur la sécurité des DHT.
2. **Observer** : capturer le trafic d'un client BitTorrent local et le décoder avec le dissecteur `bt-dht` ; interroger Onionoo avec `curl` et `jq` pour un premier jeu de données Tor.
3. **Crawler** : `nebula --dry-run crawl` pour libp2p, `devp2p discv5 crawl` pour Ethereum, un échantillonneur BEP 51 pour la Mainline DHT (par exemple avec pythonTorrentDHT en Python).
4. **Valider** : rejouer la méthode dans Testground ou un simulateur sur une topologie connue ; ajouter des nœuds canaris au crawl réel.
5. **Publier** : script reproductible, données brutes archivées, agrégats seuls diffusés, section de limites explicite.

### Pistes de projets

| Projet | Question | Outils |
| --- | --- | --- |
| Deux crawlers face à face | Nebula et ipfs-crawler comptent-ils les mêmes pairs sur une semaine ? | Nebula, ipfs-crawler, PostgreSQL |
| Échantillonneur BEP 51 | Combien d'infohashes un nœud annonce-t-il, quel intervalle de réinterrogation, quelle couverture de l'espace de clés ? | pythonTorrentDHT, `anacrolix/dht` |
| Observatoire de diversité Ethereum | Quelle part des nœuds CL et EL tourne chaque client, et comment évolue-t-elle ? | `devp2p`, armiarma, NodeWatch |
| Effet du NAT sur la complétude | À partir de quelle proportion de pairs injoignables un crawl sous-estime-t-il la taille de plus de 40 % ? | Testground, simulateur Kademlia |
| Santé des relais Nostr | Latence, support des NIP et disponibilité des relais dans le temps | nostr.watch, BigBrotr |
| Honeynode Bitcoin | Quelles anomalies un nœud d'observation voit-il ? | peer-observer (privilèges élevés requis) |

### Questions ouvertes

- Les entrées « à confirmer » de la section 13 et les outils non rouverts (Zeek, OverSim, ns-3, p2pspider, dhtc) sont à vérifier un par un avant citation.
- Domaines non couverts : Solana, Secure Scuttlebutt, Veilid (aucun outil de mesure trouvé), Lightning, Filecoin au-delà de Nebula, analyse de trafic chiffré.
- Les numéros de version, de licence et d'étoiles viennent de pages consultées le 22/11/2026 ou d'agrégateurs (ecosyste.ms) ; les vérifier avant de les reprendre dans une publication.

## 15. Sources

Les pages ci-dessous ont été consultées par recherche le 22/11/2026 (extraits de résultats, pas lecture intégrale) ; les chiffres cités en dépendent.

**libp2p, IPFS, Filecoin**

- [Nebula (README)](https://github.com/dennis-tra/nebula) · [Nebula (pkg.go.dev)](https://pkg.go.dev/github.com/dennis-tra/nebula-crawler) · [ProbeLab : Nebula](https://probelab.io/tools/nebula/) · [ProbeLab : exposé libp2p-day](https://probelab.io/talks/nebula-a-network-agnostic-dht-crawler/)
- [ipfs-crawler (trudi-group), via ecosyste.ms](https://awesome.ecosyste.ms/projects/github.com%2Ftrudi-group%2Fipfs-crawler) · [Crawling the IPFS Network (Networking 2020)](https://dl.ifip.org/index.html/db/conf/networking/networking2020/1570644207.pdf)
- [p2p-tools (notassigned)](https://github.com/notassigned/p2p-tools)

**BitTorrent et Mainline DHT**

- [BEP 51 : DHT Infohash Indexing](https://www.bittorrent.org/beps/bep_0051.html) · [mldht (the8472), via ecosyste.ms](https://awesome.ecosyste.ms/projects/github.com%2Fthe8472%2Fmldht) · [bitmagnet](https://github.com/bitmagnet-io/bitmagnet) · [pythonTorrentDHT](https://pypi.org/project/pythontorrentdht/)
- [shiyanhui/dht (copie consultée)](https://github.com/yann-shi/dht) · [DHT-Telescope](https://www.github.com/cas1m1r/DHT-Telescope) · [BoonTorrent](https://github.com/huangyingting/boontorrent)
- [Wang et Kangasharju, Mainline DHT (2013)](https://www.cl.cam.ac.uk/~lw525/publications/P2P2013_13.pdf) · [page du projet](https://www.cl.cam.ac.uk/~lw525/MLDHT/) · [Real-World Sybil Attacks](https://www.cl.cam.ac.uk/~lw525/publications/security.pdf)

**Blockchains**

- [Bitnodes (wiki)](https://www.github.com/ayeowch/bitnodes/wiki) · [Coincrawler](https://github.com/Nutomic/coincrawler) · [virtu/p2p-crawler](https://github.com/virtu/p2p-crawler) · [openbitcoin node-crawler](https://github.com/openbitcoincom/node-crawler) · [bitcoin-peers-crawler](https://docs.rs/bitcoin-peers-crawler)
- [AtlasP2P, via la carte de Dingocoin](https://github.com/dingocoin/dingocoin-nodes-map) · [peer-observer](https://github.com/peer-observer/peer-observer) · [Delving Bitcoin : peer-observer](https://delvingbitcoin.org/t/peer-observer-a-tool-and-infrastructure-for-monitoring-the-bitcoin-p2p-network-for-attacks-and-anomalies/1988)
- [Ben Mariem et al., réseau Bitcoin (arXiv)](https://arxiv.org/abs/2001.09105) · [CoNEXT 2018](https://orbi.uliege.be/bitstream/2268/229009/1/conext_BTC_2018.pdf) · [36 réseaux de cryptomonnaies (arXiv)](https://arxiv.org/pdf/2511.15388)
- [devp2p (Geth)](https://geth.ethereum.org/docs/tools/devp2p) · [ethereum/node-crawler](https://github.com/ethereum/node-crawler) · [NodeWatch (ChainSafe)](https://blog.chainsafe.io/nodewatch-the-open-source-node-explorer-for-ethereum-consensus-eth2/) · [armiarma](https://github.com/migalabs/armiarma) · [MigaLabs](https://github.com/migalabs) · [FastEthereumCrawler](https://ethresear.ch/t/crawling-the-ethereum-discv5-network-fast/20962) · [ethereum.org : Nebula](https://ethereum.org/developers/tools/crawling-the-ethereum-p2p-network-with-nebula/)

**Tor et I2P**

- [Onionoo](https://metrics.torproject.org/onionoo.html) · [Tor Metrics : sources](https://metrics.torproject.org/sources.html) · [tor-relays : trackers de relais](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/YTCJ4YQVSWD2RNCBGALXDJDJVUIRK2BT)
- [I2P : netDb](https://geti2p.net/en/docs/how/network-database) · [Hoang et al., IMC 2018](https://arxiv.org/pdf/1809.09086) · [blocage d'I2P par VPN](https://www.arxiv.org/pdf/1907.07120) · [I2P 2.5.0](https://github.com/i2p/i2p.i2p/releases/tag/i2p-2.5.0-0) · [résumé secondaire sur un scanner de netDb](https://corpus.lantern.io/findings/2026-thewizard0fbsod-quirky-chinese-i2p__i2p-netdb-cn-scanner-tool/)

**Autres réseaux**

- Monero : [ProbeLab, topologie](https://probelab.io/blog/peering-into-privacy-a-deep-dive-into-the-monero-network-topology/) · [Cao et al.](https://eprint.iacr.org/2019/411) · [Kopyciok et al.](https://arxiv.org/pdf/2509.10214) · [Gao et Piškorec](https://arxiv.org/pdf/2504.15986) · [exposés ProbeLab 2026](https://probelab.io/talks/?year=2026)
- Kad, Azureus, Gnutella : [Steiner et al., Kad à long terme](https://www.eurecom.edu/publication/2885/download/rs-publi-2885.pdf) · [Crawling Azureus](https://www.eurecom.fr/publication/2495/download/ce-steimo-080604.pdf) · [Stutzbach et al., churn](https://www.cs.uoregon.edu/Reports/TR-2005-003.pdf)
- Hyphanet : [Roos et al., PETS 2014](https://www.petsymposium.org/2014/papers/Roos.pdf) · [hyphanet.org](https://www.hyphanet.org/fr/pages/about.html)
- Nostr : [nostr-watch](https://github.com/sandwichfarm/nostr-watch) · [BigBrotr](https://pypi.org/project/bigbrotr/5.3.1/) · Matrix : [A Glimpse of the Matrix](https://ar5iv.labs.arxiv.org/html/1910.06295)

**Simulation, observation, visualisation**

- [Testground](https://github.com/testground/testground) · [PeerfactSim.KOM](https://www.kom.tu-darmstadt.de/papers/SGR+11.pdf) · [simulateur Kademlia distribué](https://eudl.eu/pdf/10.4108/icst.simutools.2014.254609) · [Locust](https://unpaywall.org/10.1145%2F3442520.3442531) · [QUANTAS](https://arxiv.org/pdf/2205.04930) · [simulateur DHT pour l'échantillonnage Ethereum](https://arxiv.org/pdf/2402.09993) · [émulation GNUnet](https://www.academia.edu/28104663/Beyond_Simulation_Large_Scale_Distributed_Emulation_of_P2P_Protocols) · [netviz (miroir du README)](https://github-users.yhajoa.workers.dev/ethp2p/netviz)
- [Wireshark : bt-dht](https://www.wireshark.org/docs/dfref/b/bt-dht.html) · [Wireshark : vuze-dht](https://www.wireshark.org/docs/dfref/v/vuze-dht.html) · [nDPI](https://www.ntop.org/ndpi/) · [Gephi](https://www.gephi.org/features) · [Bellingcat : NetworkX et Gephi](https://www.bellingcat.com/resources/how-tos/2016/09/01/dark-web-osint-python-part-three-visualization) · [ccNetViz](https://npmjs.com/package/ccnetviz)

**Sécurité des DHT**

- [Urdaneta et al., synthèse](https://research.vu.nl/en/publications/a-survey-of-dht-security-techniques/) · [Sybil actif sur la DHT d'IPFS](https://arxiv.org/pdf/2505.01139) · [attaques sur Kad](https://www-users.cs.umn.edu/%7Ehopper/kad_attack_journal.pdf) · [censure de contenu sur IPFS](https://arxiv.org/pdf/2307.12212) · [synthèse récente sur les réseaux P2P sûrs](https://arxiv.org/html/2509.19539v1)
