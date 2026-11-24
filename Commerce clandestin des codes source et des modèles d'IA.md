# Commerce clandestin des codes source et des modèles d'IA (ML/LLM) — Vecteurs, monétisation, détection et investigation

**Dossier technique défensif · Édition consolidée du 25 novembre 2026 · TLP:CLEAR**

**Cohérence du dépôt** : ce document complète les dossiers « Économie souterraine des données, des accès et des agents IA », « Outils avancés d'exploration des réseaux P2P », « Veille Marché Souterrain & Menaces IA » et « Marché noir des données cryptographiques ». Il reprend leur légende épistémique et leur posture défensive.

**Posture** : analyse d'investigateur orientée détection, preuve et réponse. Les mécanismes sont décrits au niveau utile pour concevoir des défenses et mener une enquête. Aucune procédure d'exfiltration, aucune charge utile, aucune méthode d'évasion n'est fournie.

**Légende** : 🔵 observation rapportée · 🟡 lecture / extrapolation · 🔪 hypothèse prospective · ✓ vérifié novembre 2026 sur au moins une source · ✓✓ recoupé sur deux sources indépendantes · † connaissance antérieure non revérifiée ce jour.

---

## 0. Thèse

La valeur dérobée a changé de nature. Le dépôt Git reste une cible, mais l'actif rare est désormais le **graphe poids + données d'entraînement + pipeline de production + accès API** : c'est lui qui représente des millions de dollars de calcul, des mois de curation et un avantage concurrentiel directement exploitable. Le vol se fait par trois portes — l'initié, le compte de service (CI/CD, registres, clés cloud) et la chaîne d'approvisionnement — et s'accélère depuis 2025 avec l'émergence des agents IA comme vecteurs d'exfiltration et comme cibles elles-mêmes.

La revente prend cinq formes : vente d'artefact, clone-as-a-service (CaaS), extorsion (parfois triple : chiffrement + publication + notification RGPD), empoisonnement commandité, et **blanchiment de modèles** (usage d'un modèle volé pour générer des données synthétiques et entraîner un dérivé « propre »).

**Conséquence opérationnelle** : on ne protège plus un « fichier », on protège un graphe d'identités, de pipelines et de flux de gros volumes. La détection se fait sur l'accès et le transfert, la preuve sur la provenance, la prévention sur l'identité et l'isolation. La preuve de propriété elle-même devient un défi technique.

---

## 1. Périmètre et taxonomie des actifs

| Actif | Forme typique | Taille ordre de grandeur | Valeur marchande | Difficulté de protection |
| --- | --- | --- | --- | --- |
| Code applicatif / infra-as-code | Dépôts Git, monorepos, mirrors | Mo à Go | Moyenne ; forte si secrets ou 0-day | Moyenne (très distribué) |
| Secrets de build | Jetons CI, clés cloud, certificats de signature | Ko | Disproportionnée : porte d'entrée vers tout le reste | Élevée (éphémères mais omniprésents) |
| Poids de modèle | `.safetensors`, `.gguf`, checkpoints framework, shards | 1 Go à plusieurs To | Critique | Élevée (volume élevé mais point unique de valeur) |
| Données d'entraînement / fine-tuning | Corpus, datasets annotés, préférences RLHF | Go à To | Critique (PII, secrets, IP) | Très élevée (copies multiples, sous-traitants) |
| Recettes d'entraînement | Hyperparamètres, mixtures de données, scripts de filtrage | Ko à Mo | Élevée (savoir-faire) | Moyenne |
| Prompts système, index RAG, mémoire d'agent | Textes, vecteurs, bases documentaires | Mo à Go | Élevée (logique métier, données clients) | Moyenne |
| Adaptateurs (LoRA) | Petits deltas de poids | Mo à Go | Moyenne à élevée, faciles à déplacer | Élevée (taille discrète) |
| Capacité d'inférence | Accès API, quotas, clés | n/a | Élevée (LLMjacking) | Moyenne |

🟡 **Point d'attention** : les adaptateurs LoRA et les checkpoints intermédiaires échappent aux seuils DLP calibrés sur « gros fichier ». Une règle fondée uniquement sur le volume les manquera (voir §3.6).

---

## 2. Marché et chaîne de valeur

### 2.1 Évolution 2019 → 2026

| Période | Marchandise principale | Vecteur dominant | Valeur pour l'attaquant |
| --- | --- | --- | --- |
| 2019–2021 | Code applicatif, bases de données | Buckets S3 ouverts, accès Git exposés, initiés | Revente de vulnérabilités, extorsion, concurrence déloyale † |
| 2022–2024 | Poids « fuités » (cas LLaMA 2023) | Diffusion P2P, forums spécialisés † | Économie de R&D, clones sans garde-fous |
| 2025–2026 | Pipelines CI/CD, poids propriétaires, données RAG, accès API | Chaîne d'approvisionnement, agents détournés ✓✓ | Revente d'accès, extorsion ciblée, espionnage industriel |

### 2.2 Rôles dans l'écosystème 🟡

- **Collecteurs d'accès** : opérateurs d'infostealers, courtiers d'accès initial (IAB). Ils vendent des sessions, jetons et configurations d'outils de développement.
- **Acteurs d'intrusion** : exploitent l'accès pour parcourir dépôts, registres, buckets de modèles.
- **Initiés et sous-traitants** : copie opportuniste ou commanditée, souvent à l'approche d'un départ.
- **Revendeurs / courtiers** : forums fermés, canaux de messagerie chiffrée, salons privés.
- **Hébergeurs de clones** : exploitent un modèle volé comme service payant.
- **Extorqueurs** : monétisent la menace de publication (code, données d'entraînement, PII).
- **Commanditaires étatiques ou concurrents** 🔪 : acquisition de capacité plutôt que revente.

### 2.3 Modèles de monétisation

1. **Vente directe d'un artefact** (poids, dépôt, corpus). Le prix dépend de la rareté, de la fraîcheur et de la preuve d'authenticité fournie par le vendeur (échantillon, hachage, démonstration d'inférence).
2. **Clone-as-a-Service (CaaS)** : le modèle n'est pas vendu mais exploité ; l'accès API est revendu à prix cassé, souvent à des opérateurs d'hameçonnage ou de fraude.
3. **Extorsion double ou triple** : chiffrement éventuel, menace de publication, menace de notification aux régulateurs ou aux clients. La publication d'un corpus d'entraînement ajoute un risque RGPD et de secret des affaires.
4. **Empoisonnement commandité** : le revenu vient de l'insertion discrète d'un comportement dans un pipeline cible (cyber-mercenariat) 🔪.
5. **LLMjacking (adjacent)** : détournement de clés d'API et de quotas d'inférence ; marché de comptes et de clés volés.
6. **Blanchiment de modèle** : usage d'un modèle volé pour générer des données synthétiques et entraîner un dérivé « propre » 🔪.

🟡 **Lecture économique** : la vente d'un artefact est un actif non rival, copiable à coût nul : le vendeur peut le vendre plusieurs fois, ce qui érode vite son prix et incite à préférer le CaaS ou l'extorsion, où la valeur reste rivale.

---

## 3. Vecteurs d'accès et d'exfiltration

### 3.1 Chaîne d'approvisionnement et CI/CD

Le pipeline de build détient les secrets, lit le code, écrit les artefacts signés : c'est un environnement de production.

- **Dépendances et actions compromises** : paquets npm/PyPI malveillants ou détournés, actions GitHub référencées par tag mutable. Une action épinglée par tag ou branche peut changer de contenu sans changement de configuration côté victime.
- **Jetons à longue durée de vie** : PAT, clés d'accès cloud statiques stockées en variables CI. Leur vol équivaut à un accès durable.
- **Caches et artefacts de build partagés** entre branches ou entre dépôts (risque d'empoisonnement de cache et de lecture transversale).
- **Runners auto-hébergés persistants** : état résiduel entre jobs, réseau interne accessible.
- **Fédération OIDC mal bornée** : trust policy trop large (par organisation plutôt que par dépôt/branche/environnement), qui permet à n'importe quel workflow de la plateforme d'assumer un rôle cloud.

**Cas documenté — TeamPCP (mars 2026) ✓✓** : compromission en cascade de Trivy → KICS → LiteLLM → Telnyx, puis brèche de la Commission européenne. Les actions CI de scan de sécurité (Trivy) ont été détournées via un jeton GitHub resté valide, exfiltrant des clés AWS à droits étendus. Le CERT-EU a confirmé 71 clients du service Europa touchés (42 internes + ≥29 autres entités de l'Union) et ~340 Go de données publiées par ShinyHunters. Leçons : un outil de sécurité exécuté en CI a les mêmes droits que l'outil de déploiement ; une passerelle LLM (LiteLLM) concentre clés d'IA, clés cloud et secrets Kubernetes. *(Sources : CERT-EU avril 2026, Akamai, Trend Micro)*

### 3.2 Registres de modèles et stockage objet

- Buckets, registres Docker, espaces de registre de modèles dont la lecture est publique ou partagée par lien non expirant.
- Comptes de service avec droit de lecture sur l'intégralité du stockage de modèles alors que seul un sous-ensemble est requis.
- URL présignées émises avec une durée excessive, journalisées sans corrélation avec l'identité demandeuse.
- Dépôts de modèles tiers téléchargés sans vérification d'intégrité, qui créent le risque inverse : entrée d'artefact piégé (voir §4.2).

### 3.3 Agents IA et outils (MCP, skills, plugins)

Le schéma qui structure l'analyse est le **triptyque dangereux** : (1) accès à des données sensibles, (2) exposition à du contenu non fiable, (3) capacité d'action vers l'extérieur. Un agent qui réunit les trois permet une exfiltration par simple instruction injectée dans un ticket, une page ou un document.

- **Détournement sémantique** : l'agent utilise ses propres outils légitimes ; aucune vulnérabilité logicielle n'est nécessaire, ce qui rend les signatures classiques inefficaces.
- **Extensions malveillantes** : skills, serveurs MCP ou plugins qui copient le contexte (extraits de code, variables d'environnement, clés) vers un tiers.
- **Confusion de privilège** : l'agent s'exécute avec l'identité du développeur, donc avec l'ensemble de ses droits.

**Cas documentés ✓✓** :
- **ClawHavoc / ClawHub (fév. 2026)** : Koi Security a audité 2 857 skills et identifié 341 malveillants, dont 335 issus d'une même campagne. Distribution du stealer macOS AMOS. Unit 42 a encore trouvé 5 skills non bloqués entre février et mai 2026.
- **GTG-1002 (sept. 2025, rapport Anthropic 13 nov. 2025)** : premier cas documenté d'intrusion largement exécutée par un agent, MITRE C0062. Acteur étatique chinois, ~30 cibles, succès limités. L'agent a exécuté l'essentiel du travail tactique avec peu de points de décision humains.
- **postmark-mcp (2025)** : premier serveur MCP malveillant observé en conditions réelles, a copié des courriels. ~1 500 téléchargements/semaine, ~300 organisations.

### 3.4 Menace interne et poste de développement

- Copie vers disque externe, cloud personnel, dépôt privé personnel, messagerie, partage de fichiers.
- Clonage massif de dépôts ou pull de modèles hors du rythme habituel du rôle.
- Sous-traitants et prestataires ML avec accès large et peu supervisé.
- **Infostealers** : journaux contenant `.env`, historiques shell, jetons de fournisseurs Git, cookies de session du registre de modèles, configurations d'outils d'IA locaux. Le jeton vole souvent la session après MFA : le MFA seul ne suffit pas, il faut lier la session à l'appareil et raccourcir sa durée.

### 3.5 Extraction par l'interface d'inférence

Sans accéder au stockage, un adversaire peut approcher la capacité d'un modèle par interrogation massive de son API (distillation non autorisée, extraction de comportement). 🟡 Il ne s'agit pas du vol d'un fichier mais d'un transfert de valeur ; le signal est comportemental : volume, diversité systématique des requêtes, couverture de l'espace d'entrée, comptes multiples coordonnés, faible usage « produit » et forte densité « sondage » (voir §6.5).

### 3.6 Le cas spécifique des adaptateurs LoRA 🟡

Les adaptateurs LoRA (Low-Rank Adaptation) représentent un angle mort important de la défense classique :

- **Taille discrète** : quelques Mo à quelques Go, contre des To pour les poids complets. Ils échappent aux seuils DLP calibrés sur « gros transfert ».
- **Valeur élevée** : ils incarnent le savoir-faire de fine-tuning (données, hyperparamètres, alignement) souvent plus précieux que le modèle de base lui-même.
- **Portabilité** : un LoRA se greffe sur n'importe quel modèle de base compatible, ce qui multiplie les surfaces de réutilisation illicite.
- **Multiplicité** : une organisation peut avoir des centaines de LoRA spécialisés, chacun représentant un actif distinct.

**Signaux défensifs** :
- Corréler les lectures d'adaptateurs avec les écritures vers des destinations externes.
- Inventorier les LoRA comme des actifs à part entière, avec propriétaire nommé et classification.
- Épingler chaque LoRA par empreinte dans un registre interne.
- Appliquer les mêmes contrôles d'accès aux LoRA qu'aux poids complets.

---

## 4. Anatomie technique des artefacts et conséquences défensives

### 4.1 Formats

| Format | Nature | Implication défensive |
| --- | --- | --- |
| `.safetensors` | Tenseurs + en-tête JSON, sans exécution de code à la désérialisation | À privilégier ; hachable, signable ; l'en-tête permet d'inventorier les tenseurs |
| `.gguf` | Format d'inférence quantifié, métadonnées intégrées | Versions quantifiées = copies dérivées plus petites, donc plus faciles à transporter |
| Checkpoints pickle (`.bin`, `.pt`, `.ckpt`) | Sérialisation Python | Peut exécuter du code au chargement : entrée d'artefacts tiers à traiter comme du code non fiable |
| Shards (`model-0000x-of-0000y`) | Découpage en fichiers de taille fixe | Tailles de transfert régulières : signal exploitable en détection |
| Adaptateurs LoRA | Deltas de poids | Petite taille, forte valeur ; hors seuils de volume classiques |

### 4.2 Deux sens du risque

- **Sortie** : vol d'artefacts internes (sections 3 et 6).
- **Entrée** : introduction d'un artefact piégé ou empoisonné dans la chaîne (désérialisation exécutant du code, backdoor comportementale, dépendance de parseur vulnérable). Règle : tout artefact entrant est haché, scanné, chargé en environnement isolé et jamais directement en production.

### 4.3 Propriétés exploitables pour la défense

- **Empreinte stable** : un hachage de contenu (SHA-256) par fichier et par shard, consigné dans un registre interne, permet de prouver ensuite qu'un fichier découvert est bien le vôtre.
- **Inventaire de tenseurs** : noms, formes, dtypes lus depuis l'en-tête suffisent à une comparaison d'architecture sans manipuler les valeurs.
- **Régularité des transferts** : lecture séquentielle complète de tous les shards, depuis une seule identité, en un temps court, est un motif très peu courant hors entraînement/déploiement planifiés.
- **Fingerprint comportemental** : des réponses à des invites de contrôle confidentielles (voir §7.3) distinguent un clone d'un modèle indépendant.

---

## 5. Études de cas

### 5.1 Fuite des poids LLaMA (février 2023) †

- **Mécanisme** : accès restreint à des chercheurs, puis diffusion publique via P2P (BitTorrent).
- **Leçon** : un fichier de plusieurs Go diffusé en P2P ne se rappelle pas. La défense porte sur la distribution initiale : liste de bénéficiaires, traçabilité par exemplaire (watermark par destinataire), clauses contractuelles, délais de réaction.

### 5.2 Microsoft Research — SAS token trop permissif (septembre 2023) †

- **Mécanisme** : Wiz a identifié un SAS (Shared Access Signature) token trop permissif sur un dépôt Azure Blob contenant des artefacts de recherche en IA.
- **Actif** : ~38 To de données accessibles, incluant des poids de modèles, des jeux de données d'entraînement et des historiques de conversations.
- **Leçon** : premier grand « open bucket » de l'ère IA. Un jeton de signature mal configuré équivaut à une exposition publique. La défense : rotation des SAS, durée de vie bornée, permissions minimales, inventaire externe régulier.

### 5.3 ShadowRay — Clusters Ray exposés (mars 2024) †

- **Mécanisme** : Oligo Security a identifié des clusters Ray (calcul distribué pour l'IA) exposés sans authentification sur Internet.
- **Actif** : exécution de code à distance, vol de secrets, minage de cryptomonnaie.
- **Leçon** : un orchestrateur d'IA exposé est un Jenkins ouvert. Les outils de calcul distribués héritent des erreurs de configuration du web. La défense : authentification obligatoire, filtrage réseau, isolation des clusters.

### 5.4 DeepSeek — Base ClickHouse exposée (janvier 2025) †

- **Mécanisme** : Wiz a identifié une base ClickHouse exposée sans authentification.
- **Actif** : historiques de conversations, secrets, potentiellement des données d'entraînement.
- **Leçon** : un fournisseur d'IA n'est pas immunisé. Les bases de données analytiques utilisées pour l'IA sont des cibles de choix.

### 5.5 Ultralytics — Compromission de la chaîne de publication (décembre 2024) †

- **Mécanisme** : ReversingLabs a documenté une compromission du pipeline GitHub Actions d'Ultralytics, menant à la publication de versions PyPI piégées (cryptomineur).
- **Actif** : pipeline de publication de modèles et de paquets.
- **Leçon** : le pipeline de publication est la cible. L'épinglage par SHA de commit et la vérification des signatures sont indispensables.

### 5.6 Hugging Face — Centaines de modèles malveillants (mai 2026) ✓

- **Mécanisme** : The Next Web rapporte la détection de centaines de modèles malveillants sur Hugging Face, utilisant le format pickle pour exécuter du code au chargement.
- **Actif** : registre de modèles public.
- **Leçon** : charger un modèle pickle revient à exécuter du code non fiable. La recherche académique (USENIX Security 2026, « Hide and Seek ») montre que ~95 % des modèles malveillants observés sur Hugging Face sont au format pickle, et que les scanners par signature sont contournables. La défense : privilégier `safetensors`, chargement en bac à sable, analyse comportementale à l'exécution.

### 5.7 Vercel — Revendication de vente de données internes (avril 2026) ⚠

- **Mécanisme rapporté** : un acteur a revendiqué la vente de données internes de Vercel (clés, jetons npm/GitHub) pour ~2 M USD, présentées comme tremplin de supply chain.
- **Statut** : source C, revendication non vérifiée à ce jour.
- **Leçon** : les plateformes SaaS hébergeant du code et des pipelines sont des cibles de choix. Toute revendication doit passer par le flux de vérification (§7 du dossier « Veille Marché Souterrain »).

### 5.8 TeamPCP — Compromission en cascade (mars 2026) ✓✓

- **Mécanisme** : compromission des actions CI Trivy, puis KICS, LiteLLM, Telnyx. Clé AWS à droits étendus exfiltrée à la Commission européenne.
- **Actif** : secrets de pipeline, clés cloud, accès aux dépôts et environnements de build.
- **Leçon** : le pipeline est la surface la plus privilégiée de l'organisation. Épinglage par SHA, secrets éphémères, sortie réseau restreinte des runners.

### 5.9 Exfiltration par agent détourné (2025–2026) ✓✓

- **Mécanisme** : agent avec lecture du dépôt et outil d'envoi, manipulé par contenu non fiable (GTG-1002, ClawHavoc).
- **Actif** : code source, clés, contexte RAG.
- **Leçon** : retirer l'un des trois termes du triptyque, de préférence la capacité de sortie ou l'accès large.

---

## 6. Ingénierie de détection

### 6.1 Télémétrie à collecter

| Source | Événements clés |
| --- | --- |
| Plateforme Git (audit log) | Clones/fetch massifs, création de jetons, changement de visibilité, ajout de clés SSH, accès depuis IP/ASN inhabituels |
| CI/CD | Création/modification de workflows, appels de secrets, changements de références d'actions, sortie réseau des runners |
| Cloud (IAM, stockage objet) | `GetObject` en masse, génération d'URL présignées, assomption de rôles, modifications de politiques de bucket |
| Registre de modèles | Téléchargements par identité, pulls de tous les shards, création de jetons, partages publics |
| Réseau / proxy / DNS | Volume sortant par identité et destination, destinations jamais vues, services de partage personnels |
| Poste de travail (EDR) | Archivage massif, montage de disque externe, usage de clients de transfert, accès aux fichiers de configuration d'outils d'IA |
| Passerelle IA / agents | Appels d'outils, destinations d'upload, taille de contexte envoyée, cohérence entre tâche demandée et outils appelés |
| API d'inférence | Profil de requêtes par clé : volume, diversité, régularité |

### 6.2 Logique de détection (conceptuelle, indépendante du SIEM)

1. **Anomalie de lecture de modèle** : une identité lit un nombre de shards/volumes supérieur à son baseline (percentile 99 sur 30 jours) hors des fenêtres de déploiement planifiées.
2. **Lecture suivie de sortie** : corrélation entre une lecture massive dans le stockage de modèles et, dans les N minutes, un volume sortant inhabituel depuis la même identité ou le même hôte.
3. **Création de jeton ou de clé suivie d'un téléchargement massif** par la même identité (typique de la persistance puis collecte).
4. **Changement de visibilité ou de politique** d'un registre/bucket de modèles (public, accès croisé de compte, lien non expirant).
5. **Modification de workflow CI** introduisant un nouveau hôte de sortie, une référence d'action non épinglée, une lecture de secrets hors périmètre.
6. **Agent hors-gabarit** : appel d'un outil de sortie dont la destination n'est pas dans la liste autorisée, ou envoi d'un contexte dont la taille dépasse la distribution normale.
7. **Honeytoken déclenché** : toute utilisation d'un secret leurre est une alerte de haute fidélité sans seuil statistique.
8. **Sondage d'API** : une clé dont la diversité de requêtes et la régularité dépassent le profil produit, ou un groupe de comptes aux motifs synchronisés.

🟡 **Principe** : combiner volume, rareté de l'identité, rareté de la destination et heure. Un seuil unique (« > 1 Go ») est contournable ; une corrélation multi-signaux l'est beaucoup moins.

### 6.3 Honeytokens et canaris

- **Secrets leurres** : fausses clés cloud, faux PAT, faux jetons de registre dans dépôts, wikis, notebooks et bases RAG, chacun unique et mappé à son emplacement pour localiser la source de la fuite.
- **Documents canaris dans le RAG** : passage connu, unique, qui ne devrait jamais être cité en dehors de requêtes de test ; sa réapparition signale une fuite d'index ou un prompt d'extraction.
- **Modèles et jeux de données leurres** dans le registre : noms attractifs, aucun usage légitime ; tout accès alerte.
- **Canaris dans les données d'entraînement** : séquences rares insérées en petit nombre pour détecter ensuite leur mémorisation par un modèle suspect (voir §7.3).
- **Gouvernance** : les honeytokens ne doivent pas être utilisables pour accéder à une ressource réelle ; leur déclenchement doit remonter vers un canal à réponse rapide.

### 6.4 DLP et contrôle de sortie adaptés à l'IA

- Filtrage de sortie par **allowlist** de destinations pour runners, environnements d'entraînement et passerelles d'agents, plutôt que blocklist.
- Détection de contenu par **empreinte** (hachage de shards, en-têtes safetensors, formats connus) en plus du volume.
- Inspection des services de partage personnels et des dépôts publics (détection de publication non autorisée).
- Prise en compte des **transferts fragmentés** : agrégation par identité et par fenêtre glissante, pas par flux.

### 6.5 Détection de la distillation par API 🟡

La distillation non autorisée est une forme d'extraction de valeur qui ne vole pas de fichier mais transfère la capacité d'un modèle vers un clone. Les signaux sont comportementaux :

- **Volume élevé** : nombre de requêtes très supérieur au profil d'usage produit.
- **Diversité systématique** : couverture méthodique de l'espace d'entrée, plutôt que requêtes métier.
- **Régularité suspecte** : motifs synchronisés, cadence constante inhabituelle.
- **Comptes multiples coordonnés** : plusieurs clés partageant des motifs de requêtes similaires.
- **Faible usage « produit »** : peu de sessions utilisateur légitimes, forte densité de « sondage ».

**Contre-mesures** :
- Limites de débit par clé et par compte.
- Détection de motifs (clustering de requêtes, analyse de similarité).
- Watermarking des sorties (biais statistique détectable dans le texte généré).
- Analyse des profils d'usage par clé et détection d'anomalies.

---

## 7. Méthodologie d'investigation

### 7.1 Phases

1. **Qualification** : quel actif, quelle version, quelle exposition ? Quelle revendication, quelle preuve fournie par l'acteur ?
2. **Préservation** : gel des journaux (cloud, Git, CI, proxy, EDR) avant rotation ; instantanés de stockage et de runners ; copie des preuves avec hachage et horodatage ; chaîne de conservation tenue.
3. **Confinement** : révocation des jetons, rotation des clés, suspension des comptes de service, restriction de politiques ; sans détruire l'état utile à l'enquête.
4. **Reconstitution** : ligne de temps croisée identité / ressource / réseau ; premier accès, persistance, collecte, sortie.
5. **Évaluation de portée** : quels fichiers, quelles versions, quelles données personnelles contenues dans les corpus ?
6. **Notification et juridique** : voir §7.4.
7. **Retour d'expérience** : correctifs d'architecture, de détection et de processus.

### 7.2 Questions directrices de l'investigateur

- Le vol est-il complet (tous les shards) ou partiel (un adaptateur, un checkpoint intermédiaire) ?
- Qui détenait déjà le droit de lire ? Si la lecture était autorisée, la preuve de l'abus tient à la sortie et à la destination.
- L'accès initial vient-il d'un jeton volé (infostealer), d'une dépendance compromise, d'un initié ?
- Les journaux couvrent-ils la période ? Combien de temps les politiques de rétention conservent-elles les événements de stockage objet ?
- L'actif a-t-il déjà été revendu, copié, déployé ailleurs ?

### 7.3 Preuve de provenance et d'appartenance

C'est la partie la plus spécifique du vol de modèle : prouver qu'un modèle ou un dépôt découvert ailleurs dérive du vôtre.

| Technique | Principe | Force | Limite |
| --- | --- | --- | --- |
| Hachage de fichiers | Identité bit à bit | Preuve forte si copie intacte | Détruit par quantification, fusion ou fine-tuning |
| Empreinte d'architecture | Noms, formes, dtypes de tenseurs | Indice utile | Architectures souvent publiques |
| Similarité de poids | Mesure de proximité entre tenseurs de deux modèles | Fort sur copie ou fine-tuning léger | Dégradé par fusion ou réentraînement lourd |
| Fingerprint comportemental | Réponses spécifiques à des invites de contrôle secrètes | Fort pour clones fonctionnels | Peut être perdu après réalignement |
| Watermark des poids | Marque insérée à l'entraînement ou après | Prouve l'origine | Robustesse variable face à compression et fine-tuning |
| Watermark des sorties | Biais statistique détectable dans le texte généré | Indique l'usage d'un modèle marqué | Moins utile pour prouver la copie des poids |
| Canaris de données | Séquences rares dans le corpus, test de mémorisation | Prouve l'usage du corpus | Nécessite d'avoir planifié en amont |
| Marquage par destinataire | Variante unique par bénéficiaire autorisé | Identifie la source d'une fuite | Coût de production par variante |

🟡 **Aucune technique n'est suffisante seule**. Un dossier probant combine un hachage (si possible), une similarité de poids, un fingerprint comportemental et une preuve d'accès (journaux). Les résultats doivent être validés par un expert indépendant avant usage contentieux.

### 7.4 Cadre juridique et notification (à valider par le conseil juridique)

- **Secret des affaires** : directive (UE) 2016/943 et transpositions nationales ; la protection suppose des mesures raisonnables de confidentialité, d'où l'importance de documenter contrôles d'accès, chiffrement et clauses.
- **RGPD** : si un corpus d'entraînement contient des données personnelles, notification à l'autorité dans les 72 heures après prise de connaissance d'une violation, et information des personnes si le risque est élevé.
- **NIS2** : entités essentielles et importantes, alerte précoce et notification d'incident significatif selon les délais de la directive et de sa transposition (loi Résilience adoptée à l'Assemblée le 17 novembre 2026 ✓✓).
- **Règlement IA (UE)** : obligations de cybersécurité pour les modèles à usage général à risque systémique ; ces dispositions et leur calendrier d'application sont à revérifier.
- **Plateformes et hébergeurs** : procédures de retrait (DMCA, signalement pour atteinte au droit d'auteur ou au secret des affaires), à préparer avant l'incident.
- **Extorsion** : tout paiement ou contact avec un acteur d'extorsion relève d'une décision de direction, avec conseil juridique et, selon les pays, autorités.

---

## 8. Défense : architecture et maturité

### 8.1 Principes d'architecture de référence

- **Identité d'abord** : jetons de courte durée, fédération OIDC bornée par dépôt, branche et environnement ; pas de clés statiques en CI.
- **Séparation des zones** : entraînement, évaluation, registre de production et inférence dans des zones distinctes avec accès par rôle ; pas de réseau sortant libre depuis l'entraînement.
- **Chiffrement avec gestion de clés** : poids et corpus chiffrés au repos, clés dans un KMS/HSM, journalisation de chaque déchiffrement, séparation des rôles entre administrateur de stockage et détenteur de clé.
- **Provenance et signature** : signature des artefacts et des commits (Sigstore ou équivalent), attestations de build (niveaux SLSA), SBOM étendu aux modèles et datasets (AIBOM), vérification à l'admission en production.
- **Moindre privilège pour les agents** : outils limités par tâche, listes de destinations autorisées, humain dans la boucle pour toute action de sortie sur données sensibles, isolation du contenu non fiable.
- **Informatique confidentielle (TEE)** : pour l'inférence ou l'entraînement sur actifs critiques, afin que les poids ne soient lisibles ni par l'hébergeur ni par un administrateur compromis ; l'attestation distante conditionne la libération des clés.
- **Rétention et traçabilité** : journaux de stockage objet et d'audit conservés assez longtemps pour une enquête (les durées par défaut sont souvent trop courtes).
- **Gestion des départs** : révocation immédiate, revue des accès aux registres et aux dépôts personnels, rappel des obligations de confidentialité.

### 8.2 Trois niveaux de maturité

**Niveau 1 : hygiène**
- Scan de secrets en pré-commit et en CI ; MFA résistant à l'hameçonnage pour Git, CI et registres ; RBAC.
- Comptes de service au moindre privilège ; suppression des clés statiques inutilisées.
- Actions CI épinglées par SHA de commit.
- Inventaire des dépôts, registres et buckets, et de leur visibilité.

**Niveau 2 : détection et contrôle des flux**
- Télémétrie du tableau 6.1 centralisée, règles 6.2 en production.
- Honeytokens et canaris (§6.3) déployés.
- Filtrage de sortie par allowlist ; DLP à empreintes.
- Signature des artefacts, hachage consigné des modèles.

**Niveau 3 : architecture résiliente**
- Zones cloisonnées sans sortie directe ; TEE pour actifs critiques.
- Watermarking et marquage par destinataire ; dossier de preuve de provenance prêt.
- Exercices réguliers de simulation d'exfiltration (équipe rouge encadrée) et d'extorsion.
- Mesure de la durée de détection et de confinement (§10).

---

## 9. Test express et plan 30-60-90

### 9.1 Huit questions

1. Détectez-vous en moins de 15 minutes une lecture complète de votre stockage de modèles suivie d'une sortie vers une destination inconnue ?
2. Vos pipelines CI ont-ils des droits d'écriture sur la production ou seulement de lecture ? Pouvez-vous le prouver ?
3. Les poids propriétaires sont-ils chiffrés au repos avec des clés distinctes de l'accès au stockage, et chaque déchiffrement est-il journalisé ?
4. Disposez-vous de honeytokens uniques par emplacement dans dépôts et bases RAG ?
5. Toutes les actions CI critiques sont-elles épinglées par SHA ?
6. L'accès d'un salarié ou d'un prestataire sortant est-il révoqué le jour même, et son activité des 30 derniers jours revue ?
7. Vos modèles et agents internes sont-ils testés contre les injections visant à leur faire divulguer prompts, données ou code ?
8. Pouvez-vous révoquer en une action tous les jetons liés à un dépôt ou à un registre ?

### 9.2 Plan 30-60-90

| Horizon | Actions |
| --- | --- |
| **30 jours** | Inventaire et audit des permissions (Git, CI, stockage objet, registres) ; activation et rétention des journaux d'audit ; premières règles de sortie. |
| **60 jours** | Honeytokens ; épinglage des actions et dépendances ; règles de corrélation §6.2 (règles 1, 2, 4, 7) ; fédération OIDC resserrée. |
| **90 jours** | Signature des artefacts et registre de hachages ; premier exercice de simulation ; dossier de preuve de provenance pour les modèles critiques ; revue des agents selon le triptyque. |

---

## 10. Indicateurs pour un projet de veille

| Indicateur | Définition | Utilité |
| --- | --- | --- |
| Temps moyen de détection (MTTD) | Du premier accès anormal à l'alerte | Efficacité de la détection |
| Temps de confinement (MTTC) | De l'alerte à la révocation effective | Efficacité de la réponse |
| Couverture honeytokens | Part des dépôts, registres et bases RAG équipés | Surface surveillée |
| Part d'actions épinglées | Actions CI épinglées par SHA / total | Réduction de surface supply chain |
| Part d'artefacts signés | Modèles et datasets signés et vérifiés à l'admission | Intégrité |
| Identités avec accès modèles | Nombre d'identités pouvant lire les poids de production | Moindre privilège |
| Rétention effective des journaux | Jours de journaux disponibles pour la reconstitution | Capacité d'enquête |
| Veille externe | Revendications, ventes ou fuites détectées concernant l'organisation | Alerte précoce |

**Veille externe** : surveillance de forums, canaux et dépôts publics pour des mentions de la marque, de noms de modèles internes, de noms de fichiers ou de hachages connus ; collecte conforme au droit et à la déontologie, avec validation juridique et sans achat auprès des acteurs.

---

## 11. Prospective 2027–2030 🔪

| Scénario | Dynamique | Signaux faibles (2026) |
| --- | --- | --- |
| **A. Blanchiment de modèles** | Modèle volé utilisé pour générer des données synthétiques et entraîner un dérivé propre ; contestation de la filiation | Discussions de forums sur l'augmentation de données avec modèles « fuités » |
| **B. Marché de l'empoisonnement** | Prestation d'insertion de comportements dans des pipelines cibles | CVE sur les dépendances de pipelines ML, parseurs de poids |
| **C. Extraction industrialisée par API** | Distillation à grande échelle via réseaux de comptes | Hausse de la fraude à l'inscription, garde-fous de débit contournés |
| **D. Agents comme initiés** | Agents autonomes dotés de droits larges, cibles privilégiées d'injection | Multiplication de connecteurs et d'extensions tierces |
| **E. Défense par TEE et provenance** | Adoption massive de l'informatique confidentielle et de la signature | Offres cloud TEE pour GPU, standards de signature de modèles |
| **F. Judiciarisation** | Contentieux sur preuve de filiation entre modèles, expertises techniques | Premières décisions sur le secret des affaires et les modèles |

---

## 12. Cartographie des cadres

| Menace | MITRE ATT&CK | MITRE ATLAS † | OWASP LLM Top 10 (2025) † |
| --- | --- | --- | --- |
| Exfiltration de poids, code, corpus | T1530, T1567, T1041, T1048 | AML.T0035 (collecte d'artefacts ML) | LLM06 (Sensitive Information Disclosure) |
| Archivage préalable | T1560, T1074 | n/a | n/a |
| Compromission de la chaîne d'approvisionnement | T1195.002 | AML.T0010 | LLM05 (Supply Chain Vulnerabilities) |
| Comptes valides / jetons volés | T1078, T1552 | n/a | LLM06 |
| Agent détourné | T1059 | n/a | LLM08 (Excessive Agency) |
| Fuite de prompt système | n/a | n/a | LLM06 |
| Empoisonnement de données ou de modèle | n/a | n/a | LLM03 (Training Data Poisoning) |
| Extraction par API (distillation) | n/a | AML.T0024 | LLM10 (Unbounded Consumption) |

† **Les identifiants ATLAS et OWASP doivent être revérifiés sur les référentiels en vigueur avant diffusion externe** ; ils évoluent d'une version à l'autre. Les numéros OWASP ci-dessus correspondent à l'édition 2025 du Top 10.

---

## 13. Glossaire

| Terme | Définition |
| --- | --- |
| **Poids (weights)** | Paramètres numériques appris lors de l'entraînement. |
| **Checkpoint** | Sauvegarde de l'état d'un modèle en cours d'entraînement. |
| **Safetensors** | Format de sérialisation de tenseurs sans exécution de code au chargement. |
| **LoRA** | Adaptation légère d'un modèle par de petites matrices de delta. |
| **RAG** | Génération augmentée par récupération dans une base externe. |
| **Honeytoken** | Faux identifiant ou donnée leurre signalant tout usage. |
| **Canari de données** | Séquence rare insérée pour tester ensuite la mémorisation ou la fuite. |
| **IAB** | Courtier d'accès initial. |
| **LLMjacking** | Détournement de clés et de quotas d'inférence. |
| **TEE** | Environnement d'exécution de confiance, avec attestation distante. |
| **SLSA / SBOM / AIBOM** | Cadre d'intégrité de la chaîne de build, inventaire logiciel, inventaire étendu aux modèles et jeux de données. |
| **Model laundering** | Usage d'un modèle volé pour produire un dérivé difficile à rattacher. |
| **Triptyque dangereux** | Données privées + contenus non fiables + action externe dans un même agent. |
| **Distillation** | Extraction de la capacité d'un modèle par interrogation massive de son API. |

---

## 14. Réserves méthodologiques

- **Sources** : les faits marqués † proviennent de connaissances antérieures et doivent être recoupés sur les sources primaires avant diffusion externe. Les faits marqués ✓ et ✓✓ ont été vérifiés en novembre 2026.
- **Attribution** : difficile de distinguer vol lucratif, hacktivisme, fuite interne ou espionnage.
- **Prix du marché noir** : volatils et souvent issus de revendications non vérifiées ; aucun chiffre n'est donné ici faute de source fiable.
- **Efficacité des protections** : le watermarking et les fingerprints ont des robustesses variables et évoluent ; la preuve juridique exige une expertise indépendante.
- **Cadre légal** : synthèse non exhaustive, à valider par le conseil juridique de chaque juridiction.
- **Posture** : ce document ne détaille ni extraction de poids d'un système protégé, ni contournement de DLP ou de détection.

---

## 15. Sources

**Autorités et institutions**
- CERT-EU, brèche de la Commission via Trivy (avril 2026) : https://cert.europa.eu/blog/european-commission-cloud-breach-trivy-supply-chain ✓✓
- ANSSI, Panorama de la cybermenace 2025 (11 mars 2026) : https://cyber.gouv.fr/actualites/panorama-de-la-cybermenace-2025/ ✓✓
- ENISA, Threat Landscape 2026 (22 sept. 2026) : https://www.enisa.europa.eu/publications/enisa-threat-landscape-2026 ✓✓
- Anthropic, rapport GTG-1002 (13 nov. 2025) : https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf ✓✓
- MITRE ATT&CK, campagne C0062 : https://attack.mitre.org/campaigns/C0062 ✓✓

**Chaîne d'approvisionnement et IA**
- Akamai, TeamPCP et Telnyx : https://www.akamai.com/blog/security-research/telnyx-sdk-pypi-2026-teampcp-supply-chain-attacks ✓✓
- Trend Micro, compromission de LiteLLM : https://www.trendmicro.com/es_mx/research/26/c/inside-litellm-supply-chain-compromise.html ✓
- Koi Security, ClawHavoc (3 fév. 2026) : https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html ✓✓
- Unit 42, ClawHub / OpenClaw : https://unit42.paloaltonetworks.com/openclaw-ai-supply-chain/ ✓✓
- The Next Web, Hugging Face et modèles malveillants (mai 2026) : https://thenextweb.com/news/hugging-face-clawhub-malware-ai-supply-chain ✓

**Infrastructures exposées**
- Wiz, Microsoft Research SAS token (sept. 2023) †
- Oligo Security, ShadowRay (mars 2024) †
- Wiz, DeepSeek ClickHouse (janv. 2025) †
- ReversingLabs, Ultralytics (déc. 2024) †

**Recherche académique**
- « The Art of Hide and Seek » (USENIX Security 2026, arXiv 2508.19774) : https://arxiv.org/pdf/2508.19774 ✓
- SafePickle (arXiv 2602.19818) : https://arxiv.org/abs/2602.19818 ✓

**Droit**
- NIS2, vote de l'Assemblée nationale (17 nov. 2026) : https://www.leto.legal/news/nis-2-loi-resilience-adoptee-assemblee-464-voix-17-novembre-2026 ✓✓

**Sources des autres dossiers du dépôt**
- « Économie souterraine des données, des accès et des agents IA » (21 nov. 2026)
- « Outils avancés d'exploration des réseaux P2P » (22 nov. 2026)
- « Veille Marché Souterrain & Menaces IA » (22 nov. 2026)
- « Marché noir des données cryptographiques » (24 nov. 2026)

---

**Dossier technique défensif · Édition consolidée du 25 novembre 2026 · À actualiser avant toute diffusion externe.**
