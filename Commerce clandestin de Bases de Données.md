# Commerce clandestin des bases de données — Vecteurs, monétisation, détection et investigation

**Dossier technique défensif · Édition du 26 novembre 2026 · TLP:CLEAR**

**Cohérence du dépôt** : ce document complète les dossiers « Économie souterraine des données, des accès et des agents IA », « Outils avancés d'exploration des réseaux P2P », « Veille Marché Souterrain & Menaces IA », « Marché noir des données cryptographiques » et « Commerce clandestin des codes source et des modèles d'IA ». Il reprend leur légende épistémique et leur posture défensive.

**Posture** : analyse d'investigateur orientée détection, preuve et réponse. Les mécanismes sont décrits au niveau utile pour concevoir des défenses et mener une enquête. Aucune procédure d'extraction, aucune charge utile, aucune méthode d'évasion n'est fournie.

**Légende** : 🔵 observation rapportée · 🟡 lecture / extrapolation · 🔪 hypothèse prospective · ✓ vérifié novembre 2026 sur au moins une source · ✓✓ recoupé sur deux sources indépendantes · † connaissance antérieure non revérifiée ce jour.

---

## 0. Thèse

La base de données n'est plus un coffre isolé : c'est un nœud d'un graphe qui s'étend du poste du développeur au data warehouse cloud, en passant par les buckets objet, les pipelines d'ETL, les bases vectorielles de RAG, les entrepôts de modèles et les exports analytiques. La valeur souterraine a migré des **fuites ponctuelles** (un bucket mal configuré) vers les **exfiltrations en cascade** (un jeton OAuth → un data warehouse → des dizaines de clients) et vers les **fuites silencieuses de longue durée** (exposition non détectée pendant des années). Le marché ne vend plus seulement des lignes : il vend des **identités qualifiées** (combos, fullz, logs), des **accès rejouables** (sessions, jetons) et des **capacités d'interrogation** (endpoints exposés, clés d'API). La défense ne protège plus une base : elle protège un graphe d'identités, de flux et de copies.

---

## 1. Périmètre et taxonomie des actifs

### 1.1 Typologie des bases ciblées

| Type | Technologies 2026 | Taille typique | Valeur souterraine | Surface d'exposition |
| --- | --- | --- | --- | --- |
| **Relationnelle** | PostgreSQL, MySQL, Oracle, SQL Server, MariaDB | Go à Po | Élevée (PII, transactions) | Port 5432/3306/1433/1521, JDBC exposé |
| **NoSQL document** | MongoDB, Couchbase, DynamoDB | Go à Po | Élevée (profils, logs applicatifs) | Port 27017, API REST, buckets S3 |
| **NoSQL clé-valeur** | Redis, Memcached, etcd | Mo à Go | Moyenne (sessions, caches) | Port 6379/11211/2379 sans authentification |
| **Colonne / analytique** | ClickHouse, Cassandra, ScyllaDB | To à Po | Élevée (télémétrie, logs) | Port 8123/9042, API HTTP |
| **Data warehouse** | Snowflake, BigQuery, Redshift, Synapse | Po à Eo | Critique (agrégats métier) | Jetons OAuth, clés de service, intégrations SaaS |
| **Data lake / objet** | S3, GCS, Azure Blob, MinIO | Eo | Critique (tout type) | SAS tokens, ACL, URLs présignées |
| **Vectorielle (RAG)** | Pinecone, Weaviate, Milvus, Qdrant, pgvector | Go à To | Élevée (connaissances métier, PII) | API REST, ports 19530/8080 |
| **Graph** | Neo4j, ArangoDB, JanusGraph | Go à To | Moyenne (relations, recommandations) | Ports 7474/7687, API HTTP |
| **Séries temporelles** | InfluxDB, TimescaleDB, Prometheus | Go à To | Moyenne (métriques, IoT) | Ports 8086/8088/9090 |
| **Moteur de recherche** | Elasticsearch, OpenSearch, Solr | To | Élevée (logs, contenus indexés) | Ports 9200/9300/8983 |
| **Messaging / streaming** | Kafka, RabbitMQ, Pulsar | To | Moyenne à élevée (événements, PII transitant) | Ports 9092/5672/6650 |
| **Embarquée / locale** | SQLite, DuckDB, LMDB, LevelDB | Mo à Go | Faible à élevée selon contenu | Fichiers sur postes, buckets, CI |

🟡 **Point d'attention** : les bases vectorielles (RAG) sont une nouvelle classe d'actifs. Elles contiennent souvent des connaissances métier, des documents internes et des PII vectorisés. Leur exposition est souvent négligée car elles sont perçues comme des « caches » plutôt que comme des bases de données.

### 1.2 Ce qui se vend réellement

La valeur ne se mesure pas en lignes mais en **qualité d'identité** et en **capacité de réutilisation** :

| Marchandise | Unité | Prix indicatif 2026 | Source |
| --- | --- | --- | --- |
| Combo (email + mot de passe) | unité | ~0,10–1 $ | Russian Market, Flare † |
| Fullz (identité complète : nom, adresse, SSN/INSEE, date naissance, carte) | unité | 5–50 $ | Flare, SOCRadar † |
| Log d'infostealer (machine complète) | unité | ~10 $ | Russian Market ✓ |
| Accès à base de données (identifiants validés) | unité | 50–50 000 $ selon cible | SOCRadar, Rapid7 † |
| Dump complet (millions d'enregistrements) | lot | 500–50 000 $ | Forums, Telegram † |
| Accès à data warehouse (Snowflake, etc.) | unité | 5 000–500 000 $ | Snowflake/UNC5537 † |
| Capacité d'interrogation (endpoint exposé) | accès | 0 (gratuit) à revente | Bizarre Bazaar, Ollama ✓ |

🟡 **Lecture** : le prix unitaire s'effondre avec le volume (loi de Gresham souterraine), mais la valeur se concentre sur les **accès qualifiés** (privilèges, cible, fraîcheur) et les **capacités d'interrogation** (qui permettent de rejouer la collecte à la demande).

---

## 2. Évolution du marché 2019 → 2026

### 2.1 Cinq âges de la base de données comme marchandise

| Âge | Période | Marchandise dominante | Vecteur principal | Illustration |
| --- | --- | --- | --- | --- |
| **1. Fuites ponctuelles** | 2013–2018 | Dumps SQL, CSV | Erreurs de configuration S3, MongoDB | Collection #1 (2019, compilation de fuites antérieures) † |
| **2. Industrialisation** | 2019–2022 | Dumps massifs, combos | Infostealers, buckets ouverts | Verifications.io (763M), Capital One (106M), Twitch (125 Go) † |
| **3. Accès qualifiés** | 2022–2024 | Logs, sessions, accès brokerés | IAB, marketplaces | Genesis Market (2023), Lapsus$ † |
| **4. Cascades SaaS** | 2024–2025 | Data warehouses, intégrations OAuth | Jetons d'intégration volés | Snowflake/UNC5537 (~165 organisations), Salesloft Drift † |
| **5. IA et RAG** | 2025–2026 | Bases vectorielles, corpus d'entraînement, endpoints LLM/MCP | Expositions par défaut, agents détournés | Ollama (175 000 hôtes), MCP (1 862 sans authentification), DeepSeek ✓ |

### 2.2 Chiffres clés 2026

- **ITRC S1 2026** : 1 803 compromissions, 471,2 M d'avis de victimes (plus que toute l'année 2025) ✓
- **SpyCloud S1 2026** : ~2 000 sources de fuites, plus de 11 Md d'enregistrements, 780 M de mots de passe ✓
- **Jeu AT&T** : 176 M d'enregistrements réapparu en 2026 avec numéros de sécurité sociale déchiffrés ✓
- **Jeu WhatsApp scrapé** : 399,6 M d'enregistrements ✓
- **ShinyHunters fin 2025** : ~1,5 Md d'enregistrements revendiqués ✓
- **Instructure/Canvas (avr.-mai 2026)** : ~275 M revendiqués, ~9 000 établissements ; confirmé : noms, e-mails, identifiants étudiants, messages privés ✓
- **ANSSI 2025** : 196 incidents de vol de données sans chiffrement (130 en 2024) ; 65 % des revendications d'extorsion = bluff ou recyclage ✓✓

---

## 3. Vecteurs d'exfiltration

### 3.1 Les sept portes d'entrée

| # | Porte | Mécanisme | Exemples 2026 |
| --- | --- | --- | --- |
| 1 | **Configuration exposée** | Port ouvert sans authentification, bucket public, SAS token trop permissif | Ollama (175 000 hôtes), MCP (1 862 sans auth), Microsoft Research (38 To) ✓ |
| 2 | **Secrets en clair** | Clés dans `.env`, dépôts, logs CI, variables d'environnement | TeamPCP (LiteLLM), Binance ✓✓ |
| 3 | **Chaîne d'approvisionnement** | Paquet, action CI, skill ou serveur MCP piégé | TeamPCP (Trivy, KICS), ClawHavoc (341 skills) ✓✓ |
| 4 | **Identité humaine compromise** | Infostealer, phishing, vishing, faux recruteur | Snowflake/UNC5537, Binance ✓✓ |
| 5 | **Identité machine compromise** | Jeton OAuth, clé de service, PAT, jeton CI | Salesloft Drift, Commission européenne ✓✓ |
| 6 | **Agent détourné** | Injection indirecte dans un agent ayant accès à la base | GTG-1002, EchoLeak ✓✓ |
| 7 | **Initié ou sous-traitant** | Copie opportuniste, accès large non supervisé | Instructure/Canvas (comptes « free-for-teacher ») ✓ |

### 3.2 Anatomie d'une exfiltration moderne

```
┌─────────────────────────────────────────────────────────────────┐
│  Entrée : secret rejouable (jeton, clé, cookie, mot de passe)   │
│  ou : exposition directe (port ouvert, bucket public)           │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Reconnaissance : lister les bases, schémas, volumes            │
│  (requêtes INFORMATION_SCHEMA, SHOW DATABASES, API /v1/models)  │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Collecte : export paginé, COPY INTO, SELECT * vers S3,         │
│  dump pg_dump/mongodump, export BigQuery vers GCS               │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Sortie : upload vers stockage personnel, partage P2P,          │
│  tunnel DNS, exfiltration par l'agent (curl, scp, API)          │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Monétisation : vente directe, extorsion, CaaS, blanchiment     │
└─────────────────────────────────────────────────────────────────┘
```

### 3.3 Le cas spécifique des bases vectorielles et RAG

Les bases vectorielles présentent des risques spécifiques :

- **Exposition par défaut** : Weaviate, Milvus, Qdrant écoutent souvent sur des ports sans authentification par défaut (8080, 19530, 6333/6334).
- **Contenu sensible** : elles contiennent des embeddings de documents internes, de conversations, de PII.
- **Extraction par similarité** : un attaquant peut interroger la base avec des vecteurs proches de documents sensibles pour les reconstruire partiellement.
- **Absence de journalisation fine** : les logs d'accès sont souvent moins détaillés que pour les bases relationnelles.

🟡 **Lecture** : les bases vectorielles héritent des erreurs du web (port ouvert sans authentification) et ajoutent un risque nouveau : l'extraction sémantique.

---

## 4. Anatomie technique des dumps

### 4.1 Formats courants

| Format | Nature | Implication défensive |
| --- | --- | --- |
| **SQL** (`.sql`) | Instructions DDL + DDL | Lisible, searchable, mais volumineux |
| **CSV / TSV** | Texte délimité | Très compressible, facile à importer |
| **JSON / JSONL** | Documents structurés | Flexible, mais volumineux |
| **Parquet** | Colonne, compressé | Efficace pour l'analytique, moins lisible |
| **Avro** | Ligne, avec schéma | Utilisé dans Kafka, Hadoop |
| **ORC** | Colonne, optimisé Hive | Data lakes Hadoop |
| **Protocol Buffers / Thrift** | Binaire, compact | Difficile à inspecter sans schéma |
| **SQLite / DuckDB** | Fichier unique | Souvent sur postes, buckets, CI |
| **MongoDB BSON / export JSON** | Documents | Volumineux en JSON |
| **Elasticsearch snapshot** | Lucene segments | Nécessite un cluster pour restauration |

### 4.2 Signaux d'exfiltration

| Signal | Source | Logique |
| --- | --- | --- |
| **Requêtes `SELECT *` ou `COPY` massives** | Logs de base | Hors profil d'application |
| **Exports vers stockage externe** | Logs cloud (S3, GCS, Azure Blob) | `COPY INTO s3://...`, `EXPORT DATA` |
| **Connexions depuis IP/ASN inhabituels** | Logs d'authentification | Écart au profil |
| **Création de comptes ou rôles** | Logs IAM de la base | Persistance |
| **Désactivation de l'audit ou des logs** | Logs d'administration | Évasion |
| **Volume de lecture anormal** | Métriques de base | Écart à la baseline |
| **Accès à des schémas/tables jamais lus** | Logs de requêtes | Ciblée |
| **Téléchargement de fichiers `.sql`, `.csv`, `.parquet`** | EDR, proxy, DLP | Sortie de données structurées |

### 4.3 Fingerprinting d'un dump

Pour prouver qu'un dump découvert appartient à votre organisation :

| Technique | Principe | Force | Limite |
| --- | --- | --- | --- |
| **Canaris de données** | Lignes fictives insérées dans le corpus | Preuve forte si présentes | Nécessite planification |
| **Hachage de sous-ensembles** | SHA-256 de plages de lignes | Preuve si intact | Détruit par réordonnancement |
| **Statistiques de colonnes** | Distribution de valeurs, nulls, formats | Indice | Non probant seul |
| **Métadonnées internes** | Noms de tables, colonnes, contraintes | Indice | Souvent publiques |
| **Watermark de lignes** | Modifications subtiles de valeurs (ex: espace insécable) | Preuve | Fragile face à nettoyage |
| **Corrélation avec journaux** | Accès anormal avant la fuite | Preuve circonstancielle | Nécessite logs conservés |

🟡 **Aucune technique n'est suffisante seule**. Un dossier probant combine canaris, hachages, statistiques et preuve d'accès.

---

## 5. Études de cas

### 5.1 Verifications.io (2019) †

- **Mécanisme** : base MongoDB exposée sans authentification sur Internet.
- **Actif** : ~763 M d'adresses e-mail avec données personnelles.
- **Leçon** : une base exposée est une fuite sans intrusion. La défense : inventaire externe, authentification obligatoire, filtrage réseau.

### 5.2 Capital One (juillet 2019) †

- **Mécanisme** : faille SSRF sur un pare-feu applicatif WAF mal configuré, obtention de jetons via les métadonnées de l'instance EC2 (IMDSv1), rôle IAM trop permissif, lecture de compartiments S3.
- **Actif** : ~106 M de personnes (États-Unis, Canada), dont 140 000 numéros de sécurité sociale.
- **Leçon** : mauvaise configuration + privilège excessif = brèche majeure. La défense : IMDSv2 obligatoire, rôles IAM au moindre privilège, WAF durci.

### 5.3 Twitch (octobre 2021) †

- **Mécanisme** : erreur de configuration serveur (bucket S3 mal configuré).
- **Actif** : ~125 Go de données internes dont le code source, les paramètres de configuration, les paiements des streamers.
- **Leçon** : une seule modification de configuration suffit. La défense : revue régulière des ACL, outils de détection de buckets ouverts.

### 5.4 Microsoft Research (septembre 2023) †

- **Mécanisme** : jeton de signature d'accès partagé (SAS) trop permissif sur un dépôt Azure Blob contenant des artefacts de recherche en IA.
- **Actif** : ~38 To de données accessibles, incluant des poids de modèles, des jeux de données d'entraînement et des historiques de conversations.
- **Leçon** : premier grand « open bucket » de l'ère IA. La défense : rotation des SAS, durée de vie bornée, permissions minimales, inventaire externe.

### 5.5 Snowflake / UNC5537 (avril-juin 2024) †

- **Mécanisme** : identifiants volés par infostealers (certains anciens), comptes sans MFA, accès à des instances Snowflake de clients.
- **Actif** : ~165 organisations, dont Ticketmaster, Santander, AT&T.
- **Leçon** : archétype de l'économie des accès. Un jeton volé donne accès à des années de données. La défense : MFA obligatoire, jetons liés à l'appareil, rotation, surveillance des accès.

### 5.6 National Public Data (2024) †

- **Mécanisme** : agrégation de données personnelles (probablement par scraping et compilation).
- **Actif** : milliards d'enregistrements revendiqués.
- **Leçon** : dernier soubresaut des méga-dumps. La valeur unitaire s'effondre, mais le volume explose.

### 5.7 ShinyHunters (fin 2025) ✓

- **Mécanisme** : ingénierie sociale contre des instances Salesforce, exploitation de comptes compromis.
- **Actif** : ~1,5 Md d'enregistrements revendiqués.
- **Leçon** : la donnée sert l'extorsion ; l'accès vient de l'humain.

### 5.8 Instructure / Canvas (avril-mai 2026) ✓

- **Mécanisme** : comptes « free-for-teacher » exploités (mécanisme exact non publié) ; seconde faille le 7 mai ; défigurations de pages de connexion ; extorsion pay-or-leak.
- **Actif** : revendiqué ~275 M d'enregistrements, ~9 000 établissements. Confirmé : noms, e-mails, identifiants étudiants, messages privés. Fenêtre du 30 avril au 7 mai ; accord rapporté le 11 mai.
- **Leçon** : écart entre revendiqué et confirmé ; deuxième brèche ShinyHunters en huit mois. La défense : segmentation des comptes, surveillance des accès, procédure de crise.

### 5.9 Panorama S1 2026 ✓

- **ITRC** : 1 803 compromissions, 471,2 M d'avis de victimes (plus que toute l'année 2025).
- **SpyCloud** : ~2 000 sources de fuites, plus de 11 Md d'enregistrements, 780 M de mots de passe.
- **Jeu AT&T** : 176 M réapparu avec SSN déchiffrés.
- **Jeu WhatsApp** : 399,6 M scrapés.
- **Leçon** : le volume explose, la valeur se concentre sur les accès.

### 5.10 Buckets ouverts et expositions par défaut (2025-2026) ✓

| Date | Événement | Mécanisme | Chiffres |
| --- | --- | --- | --- |
| Janv. 2026 | Instances Ollama | Service sans authentification joignable depuis Internet | ~175 000 instances dans 130 pays ; ~300 000 estimées vulnérables à CVE-2026-7482 |
| 2025-2026 | Serveurs MCP | Serveurs sans authentification, souvent en transport SSE obsolète | Trend Micro : 1 467 exposés ; Knostic : 1 862 sans auth, 119 testés, tous ouverts |
| Mars 2024 | ShadowRay (Oligo) | Clusters Ray exposés sans authentification | Exécution de code, vol de secrets, minage |
| Janv. 2025 | DeepSeek (Wiz) | Base ClickHouse exposée | Historiques de conversation, secrets |

🟡 **Lecture** : les services exposés sont trouvés vite : les honeypots de Pillar voient du trafic hostile en quelques heures.

---

## 6. Marché et chaîne de valeur

### 6.1 Acteurs

| Rôle | Fonction | Exemples 2026 |
| --- | --- | --- |
| **Collecteurs** | Opèrent des infostealers, scannent Internet, exploitent des expositions | Vidar 2.0, Lumma, StealC ✓ |
| **Trieurs / qualificateurs** | Valident les accès, classent par valeur | Checkers sur Russian Market † |
| **Courtiers (IAB)** | Revendent des accès validés avec garantie | Initial Access Brokers † |
| **Exploitants** | Utilisent les accès pour extorquer, espionner, frauder | ShinyHunters, UNC5537, ransomwares ✓ |
| **Revendeurs** | Forums, Telegram, marketplaces | XSS.is (perturbé), BreachForums (fragmenté), DarkForums, RAMP (saisi), LeakBase (saisi) ✓ |
| **Extorqueurs** | Menacent de publier ou vendent la non-publication | Cl0p, Akira, Qilin, The Gentlemen ✓ |
| **Blanchisseurs** | Convertissent les profits en monnaie propre | Mixers, ponts, services OTC ✓ |

### 6.2 Circuits de vente

```
┌─────────────────────────────────────────────────────────────────┐
│  Collecte : infostealers, scans, expositions, initiés           │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Tri : validation, qualification, classification                │
│  (checkers, panels, garantie de remplacement)                   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Vente : forums (XSS, BreachForums, DarkForums, Exploit.in),    │
│  Telegram (canaux « cloud »), marketplaces (Russian Market)     │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Monétisation : extorsion, revente, exploitation directe,       │
│  LLMjacking, fraude, espionnage                                 │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  Blanchiment : crypto, stablecoins, mixers, cash-out P2P        │
└─────────────────────────────────────────────────────────────────┘
```

### 6.3 Prix et dynamique

- **Loi de Gresham souterraine** : les dumps abondants chassent les données rares ; les logs qualifiés chassent les dumps ; les accès à fort privilège chassent les logs ordinaires. 🟡
- **Érosion des prix** : un combo vaut ~0,10 $, un fullz 5–50 $, un accès à Snowflake 5 000–500 000 $.
- **Concentration de la valeur** : la valeur unitaire se concentre sur les accès qualifiés (privilèges, cible, fraîcheur).
- **Garantie de remplacement** : les IAB offrent un remplacement si l'accès est révoqué dans les 24–72 h.

---

## 7. Ingénierie de détection

### 7.1 Télémétrie à collecter

| Source | Événements clés |
| --- | --- |
| **Base de données** | Connexions, requêtes, exports, créations de comptes/rôles, modifications de configuration, désactivation de logs |
| **Cloud (IAM, stockage)** | `GetObject` en masse, `COPY INTO`, génération d'URL présignées, assomption de rôles |
| **Réseau / proxy / DNS** | Volume sortant par identité et destination, destinations jamais vues, services de partage |
| **Poste de travail (EDR)** | Archivage massif, montage de disque externe, usage de clients de transfert, accès à des outils de base |
| **Application** | Logs d'authentification, profils d'usage, accès à des données sensibles |
| **Fournisseur SaaS** | Journaux d'audit (Snowflake, BigQuery, Salesforce), alertes de sécurité |

### 7.2 Logique de détection

1. **Anomalie de volume** : requêtes ou exports supérieurs au baseline (percentile 99 sur 30 jours).
2. **Accès hors profil** : connexion depuis IP/ASN inhabituel, à une heure inhabituelle, avec un rôle inhabituel.
3. **Requêtes sensibles** : `SELECT *` sur tables PII, `COPY INTO` vers stockage externe, accès à `INFORMATION_SCHEMA`.
4. **Création de persistance** : nouveau compte, nouveau rôle, nouvelle clé d'API.
5. **Évasion** : désactivation de l'audit, modification des logs, changement de configuration.
6. **Sortie** : corrélation entre lecture massive et upload vers destination externe.
7. **Honeytoken** : toute utilisation d'un secret leurre ou accès à une table leurre.

🟡 **Principe** : combiner volume, rareté de l'identité, rareté de la destination et heure. Un seuil unique est contournable ; une corrélation multi-signaux l'est beaucoup moins.

### 7.3 Honeytokens et canaris

- **Tables leurres** : tables avec noms attractifs, aucune donnée légitime, tout accès alerte.
- **Lignes leurres** : lignes fictives insérées dans des tables réelles, détectables par hachage ou watermark.
- **Secrets leurres** : fausses clés d'API, faux jetons, faux mots de passe dans `.env`, configs, dépôts.
- **Canaris de données** : séquences rares insérées pour détecter la mémorisation ou la fuite.

🟡 **Gouvernance** : les honeytokens ne doivent pas être utilisables pour accéder à une ressource réelle ; leur déclenchement doit remonter vers un canal à réponse rapide.

### 7.4 DLP et contrôle de sortie

- **Filtrage par allowlist** : destinations autorisées pour les exports, plutôt que blocklist.
- **Détection par empreinte** : hachage de sous-ensembles, formats connus (SQL, CSV, Parquet).
- **Inspection des services de partage** : détection de publication non autorisée sur GitHub, S3, GCS.
- **Agrégation par fenêtre glissante** : prise en compte des transferts fragmentés.

---

## 8. Méthodologie d'investigation

### 8.1 Phases

1. **Qualification** : quelle base, quel périmètre, quelle exposition ? Quelle revendication, quelle preuve ?
2. **Préservation** : gel des journaux (base, cloud, réseau, EDR) avant rotation ; instantanés ; copie des preuves avec hachage et horodatage.
3. **Confinement** : révocation des accès, rotation des secrets, suspension des comptes, restriction des politiques.
4. **Reconstitution** : ligne de temps croisée identité / ressource / réseau ; premier accès, persistance, collecte, sortie.
5. **Évaluation de portée** : quelles tables, quelles lignes, quelles PII, quels secrets ?
6. **Notification et juridique** : RGPD (72 h), NIS2, DORA, secret des affaires.
7. **Retour d'expérience** : correctifs d'architecture, de détection et de processus.

### 8.2 Questions directrices

- La base est-elle complète ou partielle ?
- Qui détenait le droit de lire ? Si la lecture était autorisée, la preuve de l'abus tient à la sortie.
- L'accès initial vient-il d'un jeton volé, d'une exposition, d'un initié ?
- Les journaux couvrent-ils la période ? Combien de temps les politiques de rétention conservent-elles les événements ?
- L'actif a-t-il déjà été revendu, copié, déployé ailleurs ?

### 8.3 Preuve d'appartenance

| Technique | Principe | Force | Limite |
| --- | --- | --- | --- |
| Canaris de données | Lignes fictives insérées | Preuve forte si présentes | Nécessite planification |
| Hachage de sous-ensembles | SHA-256 de plages | Preuve si intact | Détruit par réordonnancement |
| Statistiques de colonnes | Distribution de valeurs | Indice | Non probant seul |
| Watermark de lignes | Modifications subtiles | Preuve | Fragile face à nettoyage |
| Corrélation avec journaux | Accès anormal avant fuite | Preuve circonstancielle | Nécessite logs conservés |

🟡 **Aucune technique n'est suffisante seule**. Un dossier probant combine canaris, hachages, statistiques et preuve d'accès.

---

## 9. Défense : architecture et maturité

### 9.1 Principes d'architecture

- **Moindre privilège** : rôles et comptes au minimum nécessaire ; pas de `root` ou `sa` pour les applications.
- **Segmentation** : bases critiques isolées dans des VPC privés, sans accès Internet direct.
- **Chiffrement** : au repos (TDE, chiffrement de buckets) et en transit (TLS obligatoire).
- **Authentification forte** : MFA pour les accès humains, jetons courts pour les accès machine, pas de mots de passe en clair.
- **Journalisation** : audit de toutes les connexions et requêtes sensibles, rétention longue (90 jours minimum, 1 an recommandé).
- **Inventaire** : cartographie de toutes les bases, y compris shadow IT (SQLite sur postes, buckets oubliés).
- **Rotation** : secrets tournants, révocation immédiate après incident.
- **Sauvegarde** : sauvegardes chiffrées, testées, isolées (immuables si possible).

### 9.2 Trois niveaux de maturité

**Niveau 1 : hygiène**
- Authentification forte sur toutes les bases exposées.
- Pas de secrets en clair dans le code ou les configurations.
- Chiffrement au repos et en transit.
- Inventaire des bases et des buckets.
- Sauvegardes testées.

**Niveau 2 : détection**
- Journalisation de toutes les connexions et requêtes sensibles.
- Détection d'anomalies (volume, accès hors profil, exports).
- Honeytokens et canaris.
- DLP sur les exports.
- Surveillance des buckets et endpoints exposés.

**Niveau 3 : architecture résiliente**
- Segmentation réseau stricte.
- Fédération d'identité pour les accès machine.
- Bases vectorielles et RAG traités comme des actifs critiques.
- Exercices réguliers de simulation d'exfiltration.
- Dossier de preuve d'appartenance prêt.

### 9.3 Test express en huit questions

1. Pouvez-vous lister toutes vos bases de données en moins d'une heure ?
2. Détectez-vous une requête `SELECT *` sur une table PII un dimanche soir ?
3. Vos buckets S3/GCS/Azure Blob sont-ils tous privés ?
4. Les accès à vos data warehouses (Snowflake, BigQuery) ont-ils le MFA ?
5. Avez-vous des honeytokens dans vos bases et buckets ?
6. Pouvez-vous révoquer tous les accès à une base en moins de 15 minutes ?
7. Vos bases vectorielles (RAG) sont-elles inventoriées et protégées ?
8. Savez-vous combien de temps vos journaux de base sont conservés ?

### 9.4 Plan 30-60-90 jours

| Horizon | Actions |
| --- | --- |
| **30 jours** | Inventaire de toutes les bases, buckets et endpoints ; audit des authentifications ; chiffrement des buckets ; révocation des accès orphelins. |
| **60 jours** | Journalisation de toutes les connexions ; détection d'anomalies ; honeytokens ; DLP sur les exports ; scan des buckets exposés. |
| **90 jours** | Segmentation réseau ; fédération d'identité ; exercice de simulation ; dossier de preuve d'appartenance ; revue des bases vectorielles. |

---

## 10. Indicateurs pour un projet de veille

| Indicateur | Définition | Utilité |
| --- | --- | --- |
| **Temps moyen de détection (MTTD)** | Du premier accès anormal à l'alerte | Efficacité de la détection |
| **Temps de confinement (MTTC)** | De l'alerte à la révocation effective | Efficacité de la réponse |
| **Couverture honeytokens** | Part des bases et buckets équipés | Surface surveillée |
| **Part de bases avec MFA** | Bases avec MFA / total | Authentification forte |
| **Part de buckets privés** | Buckets privés / total | Exposition |
| **Rétention effective des journaux** | Jours de journaux disponibles | Capacité d'enquête |
| **Veille externe** | Revendications, ventes ou fuites détectées | Alerte précoce |

---

## 11. Prospective 2027–2030 🔪

| Scénario | Dynamique | Signaux faibles (2026) |
| --- | --- | --- |
| **A. Explosion des bases vectorielles** | Les bases RAG deviennent des cibles majeures, avec des expositions massives | Ollama (175 000 hôtes), MCP (1 862 sans auth) ✓ |
| **B. Cascades SaaS** | Un jeton OAuth compromis donne accès à des dizaines de clients via des intégrations | Snowflake/UNC5537, Salesloft Drift † |
| **C. Fuites silencieuses** | Expositions non détectées pendant des années, découvertes par des chercheurs ou des concurrents | Toyota (10 ans), AT&T (réapparu en 2026) ✓ |
| **D. Extorsion industrialisée** | Groupes comme ShinyHunters accumulent des téraoctets de données et les monétisent par extorsion | Instructure/Canvas, ShinyHunters ✓ |
| **E. IA comme cible et vecteur** | Les bases de données d'entraînement et les modèles deviennent des cibles ; les agents deviennent des vecteurs d'exfiltration | DeepSeek, GTG-1002 ✓✓ |

---

## 12. Cartographie des cadres

| Menace | MITRE ATT&CK | NIST CSF | RGPD | NIS2 |
| --- | --- | --- | --- | --- |
| Exfiltration de données | T1530, T1567, T1041, T1048 | DE.CM, RS.AN | Art. 33-34 | Notification 24/72 h |
| Accès non autorisé | T1078, T1133, T1190 | PR.AC, PR.IP | Art. 32 | Gestion des risques |
| Configuration exposée | T1190, T1562 | PR.IP, DE.CM | Art. 32 | Gestion des risques |
| Secrets en clair | T1552 | PR.AC, PR.IP | Art. 32 | Gestion des risques |
| Chaîne d'approvisionnement | T1195 | PR.IP, PR.MA | Art. 28 | Gestion des tiers |
| Agent détourné | T1059, T1566 | PR.AC, DE.CM | Art. 32 | Gestion des risques |

---

## 13. Glossaire

| Terme | Définition |
| --- | --- |
| **Bucket** | Conteneur de stockage objet (S3, GCS, Azure Blob). |
| **Canari** | Données fictives insérées pour détecter une fuite. |
| **Combo** | Combinaison email + mot de passe. |
| **DLP** | Data Loss Prevention : prévention des fuites de données. |
| **Fullz** | Identité complète (nom, adresse, SSN, date naissance, carte). |
| **Honeytoken** | Secret factice dont toute utilisation déclenche une alerte. |
| **IAB** | Initial Access Broker : courtier d'accès initial. |
| **IMDS** | Instance Metadata Service (AWS) : métadonnées de l'instance. |
| **MFA** | Multi-Factor Authentication : authentification multifacteur. |
| **PII** | Personally Identifiable Information : données personnelles. |
| **RAG** | Retrieval-Augmented Generation : génération augmentée par récupération. |
| **SAS token** | Shared Access Signature : jeton d'accès partagé (Azure). |
| **SSRF** | Server-Side Request Forgery : falsification de requête côté serveur. |
| **TDE** | Transparent Data Encryption : chiffrement transparent des données. |
| **Watermark** | Marque insérée dans les données pour prouver l'origine. |

---

## 14. Réserves méthodologiques

- **Sources** : les faits marqués † proviennent de connaissances antérieures et doivent être recoupés sur les sources primaires avant diffusion externe. Les faits marqués ✓ et ✓✓ ont été vérifiés en novembre 2026.
- **Attribution** : difficile de distinguer vol lucratif, hacktivisme, fuite interne ou espionnage.
- **Prix du marché noir** : volatils et souvent issus de revendications non vérifiées ; les chiffres donnés sont des ordres de grandeur.
- **Revendications vs confirmations** : ANSSI estime que 65 % des revendications d'extorsion étaient du bluff ou du recyclage en 2025. Toute revendication doit passer par un flux de vérification.
- **Cadre légal** : synthèse non exhaustive, à valider par le conseil juridique de chaque juridiction.
- **Posture** : ce document ne détaille ni extraction de données d'un système protégé, ni contournement de DLP ou de détection.

---

## 15. Sources

**Autorités et institutions**
- ANSSI, Panorama de la cybermenace 2025 (11 mars 2026) ✓✓
- ENISA, Threat Landscape 2026 (22 sept. 2026) ✓✓
- Europol, IOCTA 2026 (28 avr. 2026) ✓
- CERT-EU, brèche de la Commission via Trivy (avril 2026) ✓✓
- ITRC, S1 2026 ✓
- SpyCloud, fuites 2026 ✓

**Études de cas**
- Wiz, Microsoft Research SAS token (sept. 2023) †
- Oligo Security, ShadowRay (mars 2024) †
- Wiz, DeepSeek ClickHouse (janv. 2025) †
- Bitdefender, Instructure / Canvas (avr.-mai 2026) ✓
- Cointelegraph, post-mortem Binance ✓
- SOCRadar, Binance ✓

**Marché et vecteurs**
- Pillar Security, Bizarre Bazaar (28 janv. 2026) ✓✓
- Google GTIG / Mandiant, LLMjacking (2 nov. 2026) ✓✓
- Koi Security, ClawHavoc (3 fév. 2026) ✓✓
- Unit 42, ClawHub / OpenClaw ✓✓
- Trend Micro, serveurs MCP exposés ✓
- Akamai, TeamPCP et Telnyx ✓✓
- CERT-EU, TeamPCP ✓✓

**Sources des autres dossiers du dépôt**
- « Économie souterraine des données, des accès et des agents IA » (21 nov. 2026)
- « Outils avancés d'exploration des réseaux P2P » (22 nov. 2026)
- « Veille Marché Souterrain & Menaces IA » (22 nov. 2026)
- « Marché noir des données cryptographiques » (24 nov. 2026)
- « Commerce clandestin des codes source et des modèles d'IA » (25 nov. 2026)

---

**Dossier technique défensif · Édition du 26 novembre 2026 · À actualiser avant toute diffusion externe.**
