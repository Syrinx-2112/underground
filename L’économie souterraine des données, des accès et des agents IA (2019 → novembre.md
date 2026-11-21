# L’économie souterraine des données, des accès et des agents IA (2019 → novembre 2026)

*Synthèse enrichie — édition du 21 novembre 2026. Fusion critique de cinq versions d’un même rapport, complétée par une vérification ciblée des événements récents.*

> **Posture.** Document défensif et pédagogique. Les mécanismes sont décrits au niveau utile pour concevoir des défenses, jamais pour reproduire une attaque : pas de procédure, pas de charge utile, pas de méthode d’évasion.

**Légende.** 🔵 observation (fait rapporté par une source identifiable) · 🟡 extrapolation (tendance déduite) · 🔪 hypothèse prospective · ✓ vérifié lors de cette édition (recherche de novembre 2026) · † repris de connaissances antérieures, non revérifié ici.

**Plan.** 0 Lecture rapide · 1 Analyse des cinq versions · 2 Cadre d’analyse · 3 Chronique des événements · 4 Anatomie technique · 5 Économie souterraine · 6 Droit, assurance, FinOps · 7 Prospective 2027-2030 · 8 Matrice de synthèse · 9 Défense · 10 Cartographie des cadres · 11 Réserves méthodologiques · 12 Pédagogie · 13 Glossaire · 14 Sources.

---

## 0. Lecture rapide

**Thèse en une phrase.** La valeur souterraine a migré de ce qui est *stocké* (les données) vers ce qui *ouvre une porte* (accès, jetons, clés), puis vers ce qui *consomme en continu* (la facture d’inférence) et enfin vers ce qui *agit* (les agents et leurs outils). La porte d’entrée la plus rentable reste un **secret rejouable**, obtenu par un poste infecté, une chaîne d’approvisionnement compromise ou une personne trompée.

### Ce que novembre 2026 ajoute aux cinq versions

| Fait récent | Pourquoi c’est important |
| --- | --- |
| **Binance, 29 oct. 2026** : \~6,3 Md$ drainés en une trentaine de minutes sur 10 chaînes ✓ | Plus grand vol crypto connu. Le post-mortem de l’exchange relie un paquet altéré lors de l’installation d’un outil de codage IA, de faux recruteurs, un outil IA interne et le service de signature : crypto, IA et chaîne d’approvisionnement dans un seul incident |
| **TeamPCP, mars 2026** : Trivy → KICS → LiteLLM → Telnyx, puis brèche de la Commission européenne ✓ | Les passerelles LLM concentrent clés cloud et clés d’IA ; un outil de sécurité est devenu vecteur de vol de secrets |
| **Rapport GTIG/Mandiant du 2 nov. 2026** ✓ | Le LLMjacking s’organise en deux métiers (accès initial / revente via « LLM mixers »), avec une faille dans une passerelle de routage (CVE-2026-40217) |
| **Anthropic, 31 août 2026** ✓ | Des sessions Claude volées par infostealers ont servi à consommer l’usage des victimes ; le fournisseur a déconnecté les sessions, retiré les cartes enregistrées et remboursé |
| **Bleeding Llama (CVE-2026-7482), mai 2026** ✓ | Fuite de mémoire non authentifiée dans Ollama ; \~300 000 instances exposées estimées. Les « open buckets » de l’ère IA |
| **ClawHavoc / ClawHub, 2026** ✓ | Une place de marché de « skills » d’agents devient le nouveau npm, avec 341 skills malveillants documentés par Koi Security |
| **Instructure / Canvas, avril-mai 2026** ✓ | Extorsion pay-or-leak à très grande échelle ; fort écart entre chiffres revendiqués et confirmés |
| **Opération Endgame, juin 2026** ✓ | Démantèlement de StealC et Amadey : les familles d’infostealers se remplacent les unes les autres |

### Dix idées à retenir

1. **Le secret rejouable est le centre de gravité.** Tant qu’un cookie, une clé ou un jeton suffit à agir comme la victime, un marché existera, quelle que soit la marchandise du moment.
2. **L’identité machine est la nouvelle classe privilégiée** : clés d’API, clés de comptes de service, jetons CI, jetons OAuth d’intégrations. Longue durée de vie, pas de MFA, rarement liés à un appareil.
3. **La chaîne d’approvisionnement vole désormais des identifiants**, au-delà du simple malware : un paquet, une action CI, un modèle ou un skill piégé lit les secrets de l’environnement qui l’exécute.
4. **Les mêmes erreurs de configuration reviennent à chaque vague** : S3, MongoDB, Elasticsearch, puis Ollama et MCP.
5. **Le « rogue model » est surtout un mythe marketing ; le « rogue artifact » est une réalité** : modèle, skill, extension ou serveur MCP piégés.
6. **Le prompt est la balle, l’accès est l’arme.**
7. **La facture est un IOC, et le plafond de dépense est un contrôle de sécurité.**
8. **Les passerelles concentrent les secrets** (LLM gateway, MCP, CI/CD) : ce sont des cibles de haute valeur, pas de la simple plomberie.
9. **L’humain reste le premier maillon** : faux recruteurs, vishing, instructions à copier-coller.
10. **Les démantèlements déplacent plus qu’ils ne suppriment** : le marché se réorganise en semaines.

---

## 1. Analyse des cinq versions

### 1.1 Comparatif

| Version | Gabarit et ton | Apport principal | Limite | Traitement ici |
| --- | --- | --- | --- | --- |
| **prospective** (v1) | \~20 Ko, pédagogique | Socle en quatre actes, sources groupées | Peu de cas concrets, dimension financière sous-exploitée | Squelette conservé |
| **EXPANDU** (v2) | \~55 Ko, rigoureux | Posture épistémique, Actes V-VI, niveaux de maturité défensive, glossaire, programme de 7 h | Dense, peu d’événements nommés | Cadre épistémique et défense conservés |
| **v3 « fun »** | \~54 Ko, narratif | Métaphores (sept voiles, protéine repliée, parasite élégant), loi de Gresham souterraine, tableau de bord de coûts | Redondant avec v2, peu de faits nouveaux | Métaphores clés et tableau de bord conservés |
| **synthèse enrichie** (v4) | \~41 Ko | Galerie d’événements (Collection #1, Capital One, Snowflake, Ronin, Bybit, drainers…), quatre âges, section juridique et assurantielle | Imprécisions (voir 1.2), technique peu approfondie | Galerie reprise, corrigée et étendue |
| **Synthese-Grok** (v5) | \~13 Ko, exécutif | Concision ; Bizarre Bazaar, Jet Night, UNC6993 | Perd ateliers et taxonomie | Format « lecture rapide » |

**Socle commun aux cinq versions** : migration données → accès → facture → agents ; trois institutions stables (réputation, séquestre, hébergement résilient) ; LLMjacking comme cryptojacking à plafond illimité ; wrappers dominants parmi les « modèles criminels » ; défense centrée sur identité, clés et coûts.

### 1.2 Points de vigilance et corrections

Les écarts ci-dessous ont été relevés en croisant les cinq versions avec les sources récupérées en novembre 2026.

1. **Bizarre Bazaar.** Les versions 2 et 4 évoquent l’exploitation de RCE dans des frameworks web obsolètes et citent vLLM. Les articles consultés (Pillar via SecurityWeek, BleepingComputer, CSA) décrivent surtout l’abus d’endpoints exposés ou non authentifiés : Ollama (port 11434), API compatibles OpenAI (port 8000), serveurs MCP. Durée observée : 40 jours, plus de 35 000 sessions (\~972/jour). Une campagne distincte de reconnaissance MCP représentait 60 % du trafic des honeypots fin janvier, sans lien établi avec Bizarre Bazaar. Le volet RCE est à vérifier dans le rapport Sysdig original. ✓
2. **Deux montants à ne pas confondre.** Pour GTIG, *près de 1 M$* de coûts de jetons revient à une victime alimentant Jet Night (plus de 500 000 appels en moins de 8 h) ; les *plus de 100 000 $ en un jour* concernent UNC6993. ✓ De son côté, Okta cite 25 000 $ (un architecte logiciel) et près de 1 M$ (une organisation). Le chiffre de 600 000 $ attribué à METR n’a pas été retrouvé. ✓
3. **Bleeding Llama.** Découverte et baptême : Cyera (non la CSA, source secondaire). CVE-2026-7482, Ollama avant 0.17.1, CVSS 9.1 (cve.org, Red Hat) ou 8.8 en CVSS 4.0 (CNA Echo). Chronologie : signalement le 2 février, correctif le 25 février, CVE attribué le 28 avril, car MITRE n’avait pas répondu. Les notes de version du correctif ne signalaient pas la faille, d’où une adoption lente. ✓
4. **Collection #1 à #5.** Collection #1 : \~773 M adresses uniques ; les collections #2 à #5 portent le total à environ 2,2 Md d’enregistrements. Le chiffre de 2,2 Md n’est donc pas celui de Collection #1 seule. †
5. **Lapsus$ et Uber.** L’intrusion Uber (sept. 2022) est le fait d’un acteur se réclamant de Lapsus$ ; l’attribution est moins nette que pour Nvidia, Samsung ou Okta. †
6. **Barème de prix des logs (1× / 3-5× / 10-20× / 50-100×).** Présent dans la v3, sans source : à lire comme échelle illustrative, pas comme mesure. SOCRadar et Rapid7 indiquent seulement que taille de cible et privilège tirent les prix vers le haut.
7. **Le chiffre de 64 % (SOCRadar)** est une part de l’*activité suivie*, pas de la *valeur* : la valeur unitaire se concentre sur les accès.
8. **Place de marché Abacus** : citée comme active en 2026 par la v1 ; plusieurs sources signalaient un exit scam en 2025. À revérifier. †
9. **Dump Okta du 2 août 2026** : 5 871 machines, 162 pays, 7 Go, \~18 % de JWT contenant des données personnelles en clair, jetons et clés d’API valides en nombre. Les valeurs exactes (44 791 JWT, 555 liés à l’IA, 24 clés) n’ont pas été retrouvées mot pour mot ; l’ordre de grandeur est cohérent. ✓
10. **Dates.** « Février 2026 » pour Bizarre Bazaar est la date de publication ; la campagne court de décembre 2025 à janvier 2026.

### 1.3 Ce que cette synthèse ajoute

- Une chronique d’événements par famille (fuites, buckets ouverts, crypto, LLM, modèles et chaîne d’approvisionnement, contre-offensives), datée, sourcée et marquée ✓ ou †.
- Une anatomie technique défensive : sessions, identités machines, détection de secrets, LLMjacking côté cloud, infrastructures d’IA exposées, artefacts de modèles, agents et MCP, signature crypto.
- De nouvelles sections : économie détaillée, droit et assurance (France/UE), cartographie ATT&CK / OWASP, plan 30-60-90 jours, tabletop exercises.
- Un traitement explicite des **conflits d’intérêts** (sources commerciales, auteur de la synthèse).

---

## 2. Cadre d’analyse

### 2.1 Cinq âges de la valeur souterraine

| Âge | Période | Ce qui se vend | Métrique | Limite | Illustration 2026 |
| --- | --- | --- | --- | --- | --- |
| 1. La matière | jusqu’à 2021 | Dumps, combos, code source, CVV | Volume | Surabondance, péremption | Jeu de données AT&T de 176 M d’enregistrements réapparu en 2026 avec numéros de sécurité sociale déchiffrés ✓ |
| 2. La porte | 2021-2024 | Logs d’infostealers, sessions, VPN/RDP, accès brokerés | Privilège | Révocation, MFA, jetons liés à l’appareil | Dump Okta ✓ ; Snowflake † |
| 3. La facture | 2024-2026 | Capacité d’inférence volée | $/jour | Plafonds, alertes de coût | Jet Night, UNC6993 ✓ |
| 4. L’action | 2025 → | Agents détournés, outils MCP, skills | Périmètre d’exécution | Identités non humaines gouvernées, sandbox | GTG-1002, ClawHub ✓ |
| 5. La chaîne (transversal) | 2020 → | Secrets de pipelines, paquets et modèles piégés | Rayon d’explosion | Épinglage, provenance, secrets éphémères | TeamPCP, Binance ✓ |

> *Métaphore des versions précédentes, conservée :* un dump est un acide aminé libre, un accès validé est une protéine repliée. Le marché a payé de plus en plus cher le *travail déjà fait pour l’acheteur*.

### 2.2 Cinq surfaces d’accès (ajout)

| Surface | Secret visé | Vecteurs dominants | Exemples | Signal défensif |
| --- | --- | --- | --- | --- |
| **Identité humaine** | Cookie ou jeton de session, mot de passe | Infostealer, phishing de jeton, vishing | Dump Okta ✓, sessions Claude ✓ | Rejeu (IP, empreinte), jeton lié à l’appareil |
| **Identité machine** | Clé d’API, clé de compte de service, PAT, jeton OAuth, jeton CI | Dépôts, fichiers .env, logs CI, intégrations tierces | Salesloft Drift †, UNC6993 ✓ | Usage hors profil, coût |
| **Infrastructure exposée** | Aucun (pas d’authentification) | Scan d’Internet, ports et réglages par défaut | Ollama ✓, MCP ✓, S3 † | Inventaire externe, honeypots |
| **Chaîne d’approvisionnement** | Secrets d’un pipeline, confiance dans un artefact | Paquet, action CI, modèle, skill, extension | TeamPCP ✓, ClawHavoc ✓ | Épinglage, provenance, filtrage sortant |
| **Facteur humain** | L’employé lui-même | Faux recruteurs, vishing, instructions à copier-coller | Binance ✓, Ronin † | Exercices, isolation des postes |

### 2.3 Quatre rails qui n’ont pas changé

1. **Réputation** : pseudonymes stables, retours d’acheteurs. 2. **Séquestre** : escrow de la place de marché. 3. **Hébergement résilient** : *bulletproof hosting* (Bizarre Bazaar : infrastructure aux Pays-Bas, vente via Discord et Telegram ✓). 4. **Monnaie** : cryptomonnaies, PayPal, stablecoins. Chaque rail est aussi un **point d’étranglement** pour la défense : le gel de stablecoins par Tether et Circle lors de l’affaire Binance en est l’exemple le plus net ✓.

### 2.4 Loi de continuité : les mêmes erreurs à chaque vague

| Vague | Erreur récurrente | Exemple |
| --- | --- | --- |
| Cloud (2017 →) | Stockage ouvert, rôle trop permissif | Capital One † |
| Bases NoSQL | Service exposé sans authentification | Verifications.io † |
| SaaS et OAuth | Jeton d’intégration non borné, absence de MFA | Snowflake †, Drift † |
| IA (2024 →) | Port par défaut exposé, clé en clair, passerelle non authentifiée | Ollama ✓, MCP ✓ |
| Agents (2025 →) | Confiance dans un outil ou un skill tiers | ClawHub ✓, postmark-mcp ✓ |

🟡 *Seuls les logos changent : secret en clair, défaut exposé, confiance transitive, jeton porteur rejouable.*

---

## 3. Chronique des événements (2019 → novembre 2026)

Chaque famille a son tableau. Les chiffres sont ceux des sources citées ; ceux que revendiquent les attaquants sont signalés comme tels. La dernière colonne indique ✓ (revérifié en novembre 2026) ou † (connaissance antérieure, à recouper avant publication externe).

### 3.1 Fuites de données, extorsion et vol d’accès

| Date | Événement | Mécanisme (niveau défensif) | Chiffres clés | Leçon |  |
| --- | --- | --- | --- | --- | --- |
| janv. 2019 | Collection #1 (puis #2 à #5) | Compilation de fuites antérieures, utilisée pour le credential stuffing | \~773 M d’adresses uniques (#1) ; \~2,2 Md d’enregistrements (#2 à #5) | Surabondance : la valeur unitaire des combos s’effondre | † |
| 2021-2022 | Lapsus$ | Achat ou recrutement d’accès d’employés, ingénierie sociale, détournement de sessions ; Nvidia, Samsung, Okta (via un prestataire), Microsoft | — | Un accès humain acheté vaut mieux qu’un exploit | † |
| mai-juin 2023 | MOVEit / Cl0p | Exploitation de masse d’un 0-day dans un outil de transfert de fichiers | Plus de 2 000 organisations, des dizaines de millions de personnes | Contre-exemple : l’exploit de masse existe toujours | † |
| oct. 2023 | Okta — support client | Fichiers HAR de diagnostic contenant des jetons de session valides | Un petit nombre de clients visés | Les artefacts de diagnostic sont des coffres de secrets | † |
| fév. 2024 | Change Healthcare | Portail d’accès distant sans MFA, puis rançongiciel | \~190 M de personnes (estimation révisée à la hausse) | Un seul portail sans MFA suffit | † |
| avr.-juin 2024 | Snowflake / UNC5537 | Identifiants volés par infostealers (certains anciens), comptes sans MFA | \~165 organisations ; Ticketmaster, Santander, AT&T | Archétype de l’économie des accès | † |
| 2024 | National Public Data | Agrégation de données personnelles | Milliards d’enregistrements revendiqués | Dernier soubresaut des méga-dumps | † |
| août 2025 | Salesloft Drift (UNC6395) | Jetons OAuth d’une intégration tierce volés ; données Salesforce aspirées, puis secrets recherchés dans les tickets | Plusieurs centaines d’organisations | Un jeton d’intégration est une identité non humaine à privilèges | † |
| fin 2025 | Campagnes ShinyHunters | Ingénierie sociale contre des instances Salesforce | \~1,5 Md d’enregistrements revendiqués | La donnée sert l’extorsion ; l’accès vient de l’humain | ✓ |
| avr.-mai 2026 | Instructure / Canvas | Comptes « free-for-teacher » exploités (mécanisme exact non publié) ; seconde faille le 7 mai ; défigurations de pages de connexion ; extorsion pay-or-leak | Revendiqué : \~275 M d’enregistrements, \~9 000 établissements. Confirmé : noms, e-mails, identifiants étudiants, messages privés. Fenêtre du 30 avril au 7 mai ; accord rapporté le 11 mai | Écart entre revendiqué et confirmé ; deuxième brèche ShinyHunters en huit mois | ✓ |
| S1 2026 | Panorama | ITRC : 1 803 compromissions et 471,2 M d’avis de victimes (plus que toute l’année 2025). SpyCloud : \~2 000 sources de fuites, plus de 11 Md d’enregistrements, 780 M de mots de passe. Jeu AT&T de 176 M réapparu, jeu scrapé WhatsApp de 399,6 M | Voir colonne précédente | Le volume explose, la valeur se concentre sur les accès | ✓ |

### 3.2 Buckets ouverts et expositions par défaut

| Date | Événement | Mécanisme (niveau défensif) | Chiffres clés | Leçon |  |
| --- | --- | --- | --- | --- | --- |
| 2019 | Verifications.io | Base MongoDB exposée sans authentification | \~763 M d’adresses | Une base exposée est une fuite sans intrusion | † |
| juil. 2019 | Capital One | Faille SSRF sur un pare-feu applicatif mal configuré, jetons obtenus via les métadonnées de l’instance, rôle IAM trop permissif, lecture de compartiments S3 | \~106 M de personnes (États-Unis, Canada) | Mauvaise configuration + privilège excessif = brèche majeure | † |
| oct. 2021 | Twitch | Erreur de configuration serveur | \~125 Go de données internes dont le code source | Une seule modification de configuration suffit | † |
| automne 2022 | Microsoft / « BlueBleed » (SOCRadar) | Point de terminaison Azure Blob mal configuré ; Microsoft conteste l’ampleur | Revendiqué par le découvreur | Quand le découvreur vend du renseignement, l’ampleur est discutable | † |
| 2023 | Toyota (Japon) | Données de véhicules accessibles publiquement pendant \~10 ans | \~2,15 M de clients | Exposition silencieuse de longue durée | † |
| sept. 2023 | Microsoft (recherche en IA) | Jeton de signature d’accès partagé trop permissif sur un dépôt de modèles (Wiz) | \~38 To accessibles | Premier grand « open bucket » de l’ère IA | † |
| mars 2024 | ShadowRay (Oligo) | Clusters Ray (calcul distribué pour l’IA) exposés : exécution de code, vol de secrets, minage | Des clusters actifs compromis | Un orchestrateur d’IA exposé est un Jenkins ouvert | † |
| janv. 2025 | DeepSeek (Wiz) | Base ClickHouse exposée : historiques de conversation, secrets | — | Un fournisseur d’IA n’est pas immunisé | † |
| janv. 2026 | Instances Ollama | Service sans authentification joignable depuis Internet | \~175 000 instances dans 130 pays (The Hacker News) ; \~300 000 estimées vulnérables à la publication de CVE-2026-7482 | Les « open buckets » de l’IA | ✓ |
| 2025-2026 | Serveurs MCP exposés | Serveurs sans authentification, souvent en transport SSE obsolète | Trend Micro : 1 467 serveurs exposés (près de ×3 en quelques mois), dont 1 227 en SSE. Knostic : 1 862 sans authentification, 119 testés, tous ouverts | Le protocole des agents hérite des erreurs du web | ✓ |

🟡 *Les services exposés sont trouvés vite : les honeypots de Pillar voient du trafic hostile en quelques heures (synthèse CSA).* ✓

### 3.3 Crypto : de la clé privée au poste du développeur

| Date | Événement | Mécanisme (niveau défensif) | Chiffres clés | Leçon |  |
| --- | --- | --- | --- | --- | --- |
| août 2021 | Poly Network | Faille de contrat intelligent | \~611 M$ (en grande partie restitués) | Le butin n’est pas toujours monétisable | † |
| fév. 2022 | Wormhole | Faille de vérification de signatures d’un pont | \~320 M$ | Les ponts concentrent le risque | † |
| mars 2022 | Ronin (Axie) | Clés de validateurs compromises après un faux entretien d’embauche ; attribué à Lazarus | \~625 M$ | Le faux recrutement est un vecteur de premier ordre | † |
| déc. 2023 | Ledger Connect Kit | Bibliothèque JavaScript compromise via un compte de publication ; code « drainer » injecté dans des applications | Pertes limitées, exposition large | La dépendance front-end est une chaîne d’approvisionnement | † |
| 2023-2025 | Drainers as-a-Service (Inferno, Angel…) | Phishing et signatures trompeuses ; développeurs rémunérés à la commission | \~20 % de commission | Modèle économique ancêtre des « LLM mixers » | † |
| fév. 2025 | Bybit | Environnement d’un développeur de Safe{Wallet} compromis, fichier JavaScript modifié, signataires trompés ; attribué à la Corée du Nord (TraderTraitor) | \~1,5 Md$ | Le chemin de signature est le joyau | ✓ |
| 2025 | Bilan Chainalysis | Infiltration de faux informaticiens, blanchiment par services en langue chinoise, ponts, mixers | 3,4 Md$ volés ; Corée du Nord ≥ 2,02 Md$ | Peu d’attaques, très gros impact | ✓ |
| S1 2026 | Blockaid | Schéma LinkedIn → compromission d’un signataire multisig, à l’origine de deux des quatre plus gros incidents | > 1 Md$ ; Drift 285 M$ et KelpDAO 292 M$ (attribués RPDC) | Même mode opératoire, répété | ✓ |
| sept. 2026 | Bitget | Portefeuilles chauds et tièdes | \~388 M$ | Fonds mélangés plus tard avec ceux de Binance | ✓ |
| 29 oct. 2026 | **Binance** | Voir l’étude de cas ci-dessous | \~6,3 Md$ (TRM : \~6,36 Md$) | Plus grand vol crypto connu | ✓ |

**Part de la Corée du Nord** (Chainalysis, 30 oct. 2026) : près de 89 % des pertes déclarées de 2026 contre 15 % en 2021 ; TRM Labs estime \~85 % et 7,4 Md$ volés en 2026 à ce jour. ✓

#### Étude de cas : Binance, 29 octobre 2026 ✓

Source principale : post-mortem publié par Binance le 3 novembre (donc auto-déclaré), recoupé avec TRM Labs, Chainalysis et la presse spécialisée. L’enquête n’a pas établi toute la cause à ce jour.

| Étape | Ce que rapporte le post-mortem |
| --- | --- |
| Juin | Un ingénieur télécharge un paquet altéré en configurant un outil de codage IA. Le logiciel malveillant est détecté et le poste remplacé, mais des informations sur l’équipe des portefeuilles ont déjà été collectées |
| Septembre | De faux recruteurs ciblent un autre ingénieur avec un « exercice de code » piégé ; son poste est compromis et passe sous les protections |
| 20 octobre | Depuis ce poste, et avec un outil IA interne, les attaquants atteignent deux autres ingénieurs et obtiennent des identifiants de déploiement |
| 28 octobre | Préparation : un portefeuille d’opérateur est alimenté par de petits retraits Tornado Cash ; au moins 36 h de tests avant l’attaque (TRM) |
| 29 octobre, \~20 h 05 UTC | Déploiement d’un logiciel malveillant dans l’infrastructure des portefeuilles, accès au service de signature interne, sorties de fonds sur 10 blockchains en \~20 à 31 minutes |
| Pendant et après | Le mécanisme EIP-7702 d’Ethereum permet de continuer les transferts après la suspension des retraits ; le système MPC complique la révocation |
| Réponse | Tether gèle plus de 1,5 Md USDT, Circle plus de 800 M USDC ; Binance annonce 2,85 Md$ gelés ou interceptés, absorbe la perte et dit avoir reconstitué 98 % des actifs au 31 octobre |
| Attribution | FBI : Corée du Nord ; Chainalysis et TRM : TraderTraitor avec un haut niveau de confiance ; Pyongyang dément |

**Leçons.** (1) La chaîne développeur → CI → signature est la cible, pas le portefeuille. (2) L’outil de codage IA est devenu un maillon de confiance : installation piégée, puis outil interne utilisé pour pivoter. (3) Campagne lente de quatre mois, très en amont de l’incident visible. (4) Les points d’étranglement du système financier (gel de stablecoins) fonctionnent. (5) Binance dit avoir bloqué 57 tentatives liées à la Corée du Nord en 2026 : un seul échec a suffi.

### 3.4 Attaques contre et avec les LLM, LLMjacking, agents

| Date | Événement | Mécanisme (niveau défensif) | Chiffres clés / leçon |  |
| --- | --- | --- | --- | --- |
| 2023 | Fuites via assistants grand public ; comptes d’IA dans des logs de stealers | Employés collant du code interne (Samsung) ; \~100 000 identifiants ChatGPT repérés dans des logs (Group-IB) | L’IA est à la fois un canal de fuite et une cible de vol | † |
| mai 2024 | Sysdig nomme le LLMjacking | Clés cloud volées, appels à des modèles hébergés (Bedrock) | Coût potentiel \~46 000 $/jour, puis plus de 100 000 $/jour avec des modèles plus chers | † |
| janv.-fév. 2025 | Storm-2139 (Microsoft) | Identifiants Azure OpenAI volés, accès revendus, contournement des garde-fous ; action en justice | Précurseur des « LLM mixers » | † |
| juin 2025 | EchoLeak (Microsoft 365 Copilot, CVE-2025-32711) | Injection indirecte sans clic par e-mail, détournant le contexte de l’assistant pour exfiltrer des données | Première exfiltration zéro-clic marquante par injection | † |
| juil. 2025 | Extension Amazon Q Developer | Invite malveillante glissée dans une version publiée | La chaîne de publication d’un outil IA est une cible | † |
| août 2025 | s1ngularity (paquets Nx) | Paquets npm piégés qui détournent des CLI d’IA locales pour inventorier fichiers et secrets | L’IA installée sur le poste devient un outil de reconnaissance | † |
| août 2025 | GTG-2002 (rapport Anthropic) | Extorsion de données à grande échelle menée avec un agent de code | Les agents abaissent le coût de l’extorsion | † |
| sept. 2025 | Shai-Hulud (npm) | Ver qui vole des jetons de mainteneurs et se republie | Les jetons de publication sont des clés du royaume | † |
| mi-sept. 2025 (rapport 13 nov.) | **GTG-1002** | Groupe étatique chinois ; Claude Code intégré à un cadre d’attaque autonome via MCP ; \~30 cibles, quelques succès ; 80 à 90 % du travail par l’IA, 4 à 6 décisions humaines par campagne ; milliers de requêtes, souvent plusieurs par seconde ; tâches fractionnées pour paraître anodines | Premier cas documenté d’intrusion largement exécutée par un agent. Source : Anthropic, éditeur du modèle, donc source intéressée ✓ |  |
| déc. 2025-janv. 2026 | **Bizarre Bazaar** (Pillar, Sysdig) | Trois rôles liés : scanner, validateur, place de marché (silver.inc, NeXeonAI) ; revente d’accès à plus de 30 fournisseurs via Discord et Telegram, paiements crypto et PayPal ; attribué à l’alias Hecker (Sakuya, LiveGamer101) | Plus de 35 000 sessions en 40 jours ; infrastructure aux Pays-Bas ✓ |  |
| 2 août 2026 (publié sept.) | Okta : dump de logs et fausses inscriptions | Sessions et clés valides dans un dump Telegram de 7 Go (5 871 machines, 162 pays). Second schéma : inscriptions en masse pour récolter crédits gratuits et bonus cloud | Plus de 105 000 tentatives d’inscription depuis 251 IP en un mois ; service « Poison Claude » : \~900 utilisateurs ; rabais de 70 à 90 % ✓ |  |
| 31 août 2026 | Sessions Claude volées (Anthropic) | Infostealers (Vidar, Lumma, StealC, RedLine, Acreed ; AMOS sur macOS) ; limites d’usage qui se remplissent puis se vident sans action de l’utilisateur | Déconnexion forcée, cartes retirées, remboursements ; déconnecter ne supprime pas le malware ✓ |  |
| 2 nov. 2026 | Rapport GTIG / Mandiant | UNC6993 : clé de compte de service mal configurée, génération de clés supplémentaires, activation de services, abonnements pay-as-you-go à des modèles premium ; page de phishing de jetons actifs ; outils de mutualisation de clés (new-api, gcli2api) ; porte dérobée en conteneur qui demande à un modèle de raisonner puis exécute les commandes ; faille CVE-2026-40217 dans une passerelle LiteLLM | UNC6993 : plus de 100 000 $ en un jour. Jet Night : \~60 $/mois pour un accès « illimité » contre 100 à 250 $ en offre légitime ; pannes annoncées sur Telegram, interprétées comme des révocations de clés. Victimes en Amérique, Asie-Pacifique : finance, commerce, technologie, transport ✓ |  |

### 3.5 « Rogue ML models » et chaîne d’approvisionnement de l’IA

| Date | Événement | Mécanisme (niveau défensif) | Leçon |  |
| --- | --- | --- | --- | --- |
| déc. 2022 | PyTorch-nightly (torchtriton) | Confusion de dépendances : paquet malveillant sur PyPI | Les écosystèmes ML héritent des attaques de paquets | † |
| mars 2023 | Fuite des poids LLaMA | Poids diffusés par torrent après une diffusion restreinte | Provenance et licence des poids ; ouvre la voie aux versions modifiées | † |
| févr. 2024 | JFrog : \~100 modèles malveillants sur Hugging Face | Désérialisation pickle exécutant du code au chargement | Charger un modèle revient à exécuter du code | † |
| déc. 2024 | Ultralytics | Compromission du pipeline GitHub Actions, versions PyPI piégées (cryptomineur) | Le pipeline de publication est la cible | † |
| févr. 2025 | nullifAI (ReversingLabs) | Fichiers pickle volontairement « cassés » échappant au scanner de la plateforme | Les scanners par liste sont contournables | † |
| 2025-2026 | Recherche académique | \~95 % des modèles malveillants observés sur Hugging Face sont au format pickle ; contournements de scanners (« Hide and Seek », USENIX Security 2026) ; détection à l’exécution proposée plutôt que par signature | Juger le comportement, pas l’apparence | ✓ |
| mai 2026 | Hugging Face (presse) | The Next Web : des centaines de modèles malveillants, source secondaire | À recouper avant citation externe | ✓ |
| janv.-mai 2026 | **ClawHub / OpenClaw** | 14 skills malveillants en trois jours fin janvier ; Bitdefender : \~17 % des skills analysés début février ; ClawHavoc (Koi) : 341 skills, diffusant le voleur AMOS ; Unit 42 (fév.-mai) : cinq skills évasifs, dont une charge utile suivie de 22 Mo de remplissage pour dépasser les limites d’analyse ; leurre « prérequis » à exécuter par l’agent | Le « ClickFix » s’adresse désormais aux agents ; filtrage par VirusTotal, ClawScan et NVIDIA ✓ |  |
| sept. 2025 | postmark-mcp | Premier serveur MCP malveillant observé en conditions réelles : copie des e-mails traités ; \~1 500 téléchargements par semaine, \~300 organisations (Koi) | Un MCP est un paquet comme un autre ✓ |  |
| 2025-2026 | CVE MCP | mcp-remote CVE-2025-6514 (CVSS 9,6 ; 437 000 téléchargements) ; MCP Inspector CVE-2025-49596 (9,4) ; trois CVE dans mcp-server-git (janv. 2026) ; Apache Pinot MCP CVE-2026-49257 (10,0, Akamai) ; CSA (mai 2026) : au moins sept CVE hautes ou critiques sur des plateformes majeures | Défauts de conception récurrents : exécution de commandes, absence d’authentification ✓ |  |

#### Étude de cas : TeamPCP, mars 2026 ✓

| Date | Étape |
| --- | --- |
| début mars | Premier incident chez Aqua Security ; la rotation des identifiants est incomplète |
| 19 mars | Des commits malveillants sont poussés sur les tags de trivy-action et setup-trivy avec un jeton resté valide (volé \~3 semaines plus tôt via un workflow GitHub Actions mal configuré) ; images Trivy 0.69.4 à 0.69.6 piégées. Charge : voleur d’identifiants lisant la mémoire du processus du runner CI, exfiltration chiffrée vers un domaine typosquatté |
| 19-24 mars | Clé AWS à droits étendus volée à la Commission européenne ; alertes le 24 mars, soit 5 jours après l’intrusion |
| 23 mars | Compromission de Checkmarx KICS avec les identifiants volés |
| 24 mars | LiteLLM 1.82.7 et 1.82.8 publiées sur PyPI avec des jetons issus de la CI du projet ; retirées en \~3 h ; \~3,4 M de téléchargements par jour. Charge en trois étapes : collecte de plus de 50 catégories de secrets, mouvement latéral Kubernetes, porte dérobée persistante ; la 1.82.8 s’exécute à chaque lancement de Python via un fichier .pth |
| 27 mars | Paquet Telnyx sur PyPI ; ver npm « CanisterWorm » (66 paquets ou plus) |
| avril | CERT-EU confirme la brèche de la Commission (42 clients internes et au moins 29 autres entités, \~340 Go) ; ShinyHunters publie les données ; Cisco également touché ; le groupe de rançongiciel Vect annonce un partenariat avec TeamPCP |

**Leçons.** Un outil de sécurité exécuté dans la CI a les mêmes droits que l’outil de déploiement. Une passerelle LLM concentre clés d’IA, clés cloud et secrets Kubernetes. Épingler les actions CI par empreinte de commit, faire tourner les secrets après tout incident et réduire les secrets disponibles dans les runners limitent le rayon d’explosion (recommandations CERT-EU).

### 3.6 Contre-offensives et turbulences du marché

| Date | Événement | Enseignement |  |
| --- | --- | --- | --- |
| avr. 2023 | Genesis Market (opération Cookie Monster) | Place de marché de « bots » : identifiants, empreintes de navigateur et cookies pour usurper des sessions. Précurseur direct de l’économie du rejeu | † |
| oct. 2024 | RedLine / META (opération Magnus) | Retrait d’une famille majeure de stealers | † |
| mai 2025 | Lumma (Microsoft et justice américaine) | \~2 300 domaines saisis ; reprise rapide de l’activité | † |
| sept.-oct. 2025 | Doxxing de membres présumés de Lumma | Déclin de Lumma, migration des clients vers Vidar et StealC ; Rhadamanthys devient le plus gros par volume à l’automne | ✓ |
| oct. 2025 | Vidar 2.0 | Réécriture complète, devient la famille de choix | ✓ |
| juin 2026 | Opération Endgame : StealC et Amadey | 326 serveurs et 142 domaines visés, \~27 M d’identifiants récupérés. SpyCloud : StealC était déjà plus de 90 % sous son pic de janvier (\~14 000 victimes par jour) | ✓ |
| S1 2026 | Flashpoint, Trend Micro | Vidar, StealC et Lumma (et clones comme Remus) dominent ; les panneaux de revente de logs remplacent un stealer démantelé par un autre : on passe d’un modèle de volume à un modèle de flux de travail | ✓ |
| oct. 2026 | Gels de stablecoins | Tether et Circle gèlent des fonds volés en quelques heures | ✓ |

### 3.7 Ce que la chronique enseigne

1. **L’entrée est presque toujours un secret ou une personne**, rarement un exploit inédit (Snowflake, Binance, TeamPCP, Claude).
2. **La détection est lente** : 5 jours à la Commission européenne, quatre mois de campagne chez Binance, trois mois entre signalement et CVE pour Bleeding Llama.
3. **Le rayon d’explosion suit la concentration de secrets** : passerelle LLM, CI, service de signature, intégration OAuth.
4. **Les attributions sont des jugements** : FBI, TRM et Chainalysis d’un côté, démentis de l’autre ; les revendications d’extorsion ne valent pas confirmation.
5. **Les contrôles qui ont fonctionné sont structurels** : gels aux points d’étranglement, rotation des secrets, épinglage des dépendances, plafonds de dépense, liaison des jetons à l’appareil.

---

## 4. Anatomie technique (niveau défensif)

Cette section explique *pourquoi* les attaques de la chronique fonctionnent et *où* placer les contrôles. Elle ne contient ni exploit, ni charge utile, ni méthode d’évasion.

### 4.1 Le vol de session : pourquoi le MFA ne suffit pas

**Principe.** Après l’authentification, le serveur délivre un jeton porteur (cookie, JWT, jeton OAuth) et fait confiance à quiconque le présente. Un infostealer lit les bases de cookies et de mots de passe du navigateur sur le poste ; l’attaquant rejoue ensuite le jeton, souvent depuis un navigateur « anti-détect » et un proxy résidentiel qui imitent l’empreinte de la victime. Le mot de passe et le second facteur ne sont jamais présentés : ils l’ont été *avant* le vol.

```
Authentification (mot de passe + MFA)  →  jeton porteur émis
        │                                      │
   protégé par MFA                       NON protégé : valable pour
                                         quiconque le présente
```

**Ce qui relève la barre, et ses limites.**

| Contrôle | Empêche | N’empêche pas |
| --- | --- | --- |
| Chiffrement des cookies lié à l’application (navigateurs récents) † | Lecture triviale de la base de cookies par un malware générique | Contournements rapides par les familles de stealers, qui s’adaptent en semaines |
| Passkeys / WebAuthn | Phishing et vol de mot de passe lors de la *connexion* | Vol du cookie de session *après* la connexion |
| Liaison du jeton à l’appareil (DBSC et équivalents) | Rejeu depuis une autre machine : un cookie court doit être renouvelé par une preuve de possession d’une clé non exportable (TPM) | Utilisation du jeton *sur* la machine infectée tant que le malware y est |
| Durée de vie courte, jetons de rafraîchissement rotatifs | Réduit la fenêtre d’exploitation | Rejeu immédiat après le vol |
| Évaluation continue du contexte (IP, appareil, comportement) | Rejeu depuis un contexte incohérent | Rejeu depuis un contexte proche (même pays, même fournisseur d’accès) |
| Déconnexion globale + nettoyage du poste | Coupe les sessions volées | Ne supprime pas le malware : la session suivante sera volée aussi (message d’Anthropic aux utilisateurs concernés ✓) |

🟡 *Les passkeys ferment la porte de connexion ; seule la liaison du jeton à l’appareil ferme la fenêtre ouverte par le cookie. Les deux sont complémentaires. Le déploiement de la liaison d’appareil est en cours et la couverture est inégale selon navigateurs et services.*

### 4.2 Identités machines : le maillon faible

| Type de secret | Durée de vie typique | MFA possible ? | Où il fuit | Alternative moins risquée |
| --- | --- | --- | --- | --- |
| Clé d’API d’un fournisseur d’IA | Jusqu’à révocation | Non | Fichiers .env, dépôts, logs CI, logs de stealers | Clé à portée limitée, plafond, liste d’IP, passerelle avec clés virtuelles courtes |
| Clé JSON de compte de service cloud | Années | Non | Dépôts, images de conteneurs, postes | Fédération d’identité de charge de travail (jetons courts, sans clé stockée) |
| Jeton d’accès personnel (GitHub, PyPI, npm) | Mois à années | Parfois | CI, postes, historiques de shell | Publication de confiance (OIDC), jetons à granularité fine |
| Jeton OAuth d’intégration SaaS | Long, rafraîchi | Non | Éditeur tiers compromis (Drift †) | Périmètres minimaux, révocation centralisée, restriction d’IP |
| Jeton de déploiement CI | Variable | Non | Mémoire du runner, variables d’environnement | Secrets éphémères injectés à la demande, runners isolés |

**Trois propriétés expliquent l’attrait des identités machines** : elles sont *porteuses* (qui les a, les utilise), *durables* et *rarement surveillées* (pas de propriétaire humain qui remarque une anomalie). Les versions précédentes disaient « l’agent est le nouveau compte privilégié » ; la chronique montre que c’est déjà vrai de la clé et du jeton CI.

**Effet de concentration.** Les outils qui *voient* beaucoup de secrets deviennent des cibles : scanner de sécurité dans la CI (Trivy), passerelle LLM (LiteLLM), service de signature (Binance). Leur compromission donne un rayon d’explosion disproportionné.

### 4.3 Détecter et neutraliser les secrets exposés

Les formats de clés sont reconnaissables (préfixes de fournisseurs, longueur, jeu de caractères) ; c’est ce qui permet aux défenseurs *comme aux voleurs* de les repérer en masse. Une chaîne défensive complète :

1. **Prévention** : pas de secrets en clair dans le code ; coffre de secrets ; injection à l’exécution.
2. **Blocage au plus tôt** : hooks de pré-commit, *push protection* du dépôt, scan en CI.
3. **Historique** : scan de tout l’historique Git et des artefacts de build (images, paquets publiés), pas seulement de la branche courante.
4. **Validation** : tester si une clé trouvée est *encore valide* (sans l’utiliser) pour prioriser la révocation.
5. **Révocation rapide** : procédure automatisée ; la vitesse de révocation est une métrique de sécurité.
6. **Pièges (honeytokens)** : clés factices déposées dans les endroits que les stealers fouillent (fichiers .env, configs d’outils d’IA, dépôts privés). Toute utilisation déclenche une alerte : c’est un détecteur d’infection très fiable et peu coûteux. 🟡
7. **Veille externe** : surveiller les logs de stealers et les fuites pour ses domaines, comptes et clés.

🟡 *Les stealers récents embarquent des règles dédiées aux formats de clés d’IA et aux fichiers de configuration d’outils de développement assistés par IA (versions 1 à 4). Les honeytokens retournent cette automatisation contre eux.*

### 4.4 LLMjacking vu du cloud : chaîne d’événements et détection

**Chaîne générique** (d’après GTIG, Sysdig, Pillar ✓) :

```
1. Obtenir un identifiant   → log de stealer, dépôt, endpoint exposé,
                              CI compromise, phishing de jeton actif
2. Valider discrètement     → appels à faible volume pour tester la clé
3. Reconnaître / préparer   → lister les modèles, activer des services,
                              créer des clés, souscrire au pay-as-you-go
4. Réduire la visibilité    → (observé en 2024 †) désactiver ou contourner
                              la journalisation des appels de modèles
5. Mutualiser et revendre   → proxys de routage, « LLM mixers »
6. Consommer                → pic de coût, quelques heures
```

**Signaux de détection à instrumenter** (noms d’événements indicatifs † : à vérifier dans la documentation actuelle de chaque fournisseur) :

| Signal | Source de log | Idée de règle |
| --- | --- | --- |
| Appel de modèle par un principal, une IP ou une région jamais vus | Journaux d’audit cloud (par ex. InvokeModel, Converse sur Bedrock ; appels aux API d’IA sur Azure et GCP) | Nouveauté + coût du modèle appelé |
| Salves de requêtes de découverte (liste des modèles, disponibilité) | Mêmes journaux | Rafale de lectures par un compte qui n’en fait jamais |
| Activation d’accès à des modèles ou de services d’IA jamais utilisés | Journaux d’administration | Toute activation hors demande de changement |
| Modification de la configuration de journalisation des appels | Journaux d’administration | Alerte critique immédiate |
| Création de clés de compte de service ou de clés d’API | Journaux IAM | Création hors pipeline approuvé |
| Appels à faible volume suivis d’un pic | Métriques d’usage | Validation puis consommation (GTIG recommande de surveiller ces appels de validation ✓) |
| Consommation nocturne ou de week-end sur une clé interactive | Métriques d’usage | Écart au profil horaire |
| Changement soudain de modèle appelé vers un modèle premium | Métriques d’usage | Liste de modèles autorisés par clé |

```
-- Pseudo-règle (à adapter à votre SIEM / outil FinOps)
POUR chaque clé k, par heure h :
  SI tokens(k,h) > 3 × médiane(tokens(k, même heure de la semaine))
  OU (coût(k,h) > seuil_projet ET heure locale hors 7h-20h ET k.type = interactif)
  ALORS alerte P1 + limitation automatique + revue humaine
```

**Pièges pratiques.** La facture arrive avec des heures ou des jours de retard : détecter sur les *journaux et API d’usage*, pas sur la facture. Les plafonds doivent être posés *chez le fournisseur* (quotas, budgets avec action automatique, liste de modèles autorisés, liste d’IP), pas seulement dans un tableau de bord.

### 4.5 Infrastructures d’IA exposées : les buckets ouverts de 2026

**Pourquoi ça arrive.** Les outils d’inférence locale sont conçus pour un poste de développeur : par défaut Ollama écoute sur 127.0.0.1, mais le réglage documenté qui l’ouvre à tout le réseau est très répandu, et le service n’a pas d’authentification native ✓. Un service de développement passe en production sans changer ses réglages.

| Service (exemples) | Port courant † | Authentification par défaut | Risque principal |
| --- | --- | --- | --- |
| Ollama | 11434 ✓ | Aucune | Calcul volé, fuite de mémoire, création de modèles |
| Serveurs compatibles OpenAI (vLLM, etc.) | 8000 ✓ | Clé optionnelle | Calcul volé, fuite de prompts |
| Passerelle LiteLLM | 4000 | Clé maître | Escalade vers l’administration (CVE-2026-40217 ✓) |
| Ray (tableau de bord) | 8265 | Aucune par défaut | Exécution de code (ShadowRay †) |
| Serveurs MCP | Variable | Souvent aucune | Accès aux outils, aux fichiers et aux bases ✓ |

**Mécanisme de Bleeding Llama (CVE-2026-7482) ✓, niveau conceptuel.** Le chargeur de fichiers de modèles (format GGUF) fait confiance à des dimensions déclarées dans le fichier ; si elles dépassent la taille réelle, le serveur lit au-delà de son tampon. La mémoire du processus contient variables d’environnement, clés d’API, invites système et conversations d’autres utilisateurs ; le résultat peut ensuite être poussé hors du serveur vers un registre distant, car les points d’accès de création et de publication n’exigent pas d’authentification. 🟡 *Analogie avec Heartbleed : un parseur qui lit trop, dans un processus qui détient tout.* **Leçons de conception** : (a) ne pas mettre de secrets dans le processus qui parse des entrées non fiables ; (b) filtrer le trafic *sortant* du serveur d’inférence ; (c) mettre un proxy authentifiant devant tout service sans authentification native ; (d) mettre à jour : le correctif existait depuis février, mais ses notes ne mentionnaient pas la faille.

### 4.6 « Rogue ML models » : artefacts de modèles et chaîne d’approvisionnement

**Typologie élargie** (les versions précédentes distinguaient cinq archétypes de *services* criminels ; il manquait les *artefacts* piégés) :

| Type | Description | Exemple | Contrôle principal |
| --- | --- | --- | --- |
| Artefact exécutable | Fichier de modèle dont le chargement exécute du code (sérialisation non sûre) | Modèles pickle malveillants ✓ † | Formats sans code (safetensors), chargement restreint, bac à sable |
| Poids empoisonnés ou à porte dérobée | Comportement piégé déclenché par un motif précis | Recherche : un nombre faible et constant de documents (de l’ordre de 250) peut suffire à implanter une porte dérobée, quelle que soit la taille du modèle † | Provenance, évaluations de sécurité, modèles de source connue |
| Usurpation de dépôt | Dépôt qui imite un éditeur ou un nom connu | Typosquatting de modèles et de paquets † | Épinglage par empreinte, liste d’éditeurs autorisés |
| Dérivés altérés | Quantification ou fine-tuning « sans censure » diffusé comme original | Dérivés de poids fuités † | Vérification des signatures et des empreintes |
| Poids fuités | Modèle diffusé hors licence | LLaMA, mars 2023 † | Politique de provenance et de licences |
| Skill, plugin, serveur MCP piégé | Extension qui s’exécute avec les droits de l’agent | ClawHavoc ✓, postmark-mcp ✓ | Revue, épinglage, bac à sable (voir 4.7) |
| Service « modèle criminel » | Wrapper, jailbreak-as-a-service… | WormGPT et clones (versions 1 à 5) | Peu de contrôles directs : surveiller les accès volés qui les alimentent |

**Formats et risque d’exécution** (niveau conceptuel) :

| Format | Exécute du code au chargement ? | Remarque |
| --- | --- | --- |
| Pickle / PyTorch .bin, .pt, joblib | Oui, par conception | \~95 % des modèles malveillants observés sur Hugging Face ✓ |
| Keras / HDF5 avec couches personnalisées, SavedModel TensorFlow | Selon la configuration | Opérations personnalisées possibles † |
| ONNX | Rarement | Opérateurs personnalisés à surveiller † |
| GGUF | Non par conception | Bogues mémoire des parseurs (Bleeding Llama ✓) |
| safetensors | Non | Tenseurs seulement ; à privilégier † |
| Dépôts exigeant l’exécution de code distant à l’import (option « trust remote code » des bibliothèques) | Oui | À refuser sauf dépôt audité † |

**Contrôles** : préférer safetensors ; charger les formats risqués en mode restreint ; épingler chaque modèle par empreinte de commit ; passer par un registre interne ; charger dans un bac à sable sans réseau ni secrets ; **ne pas se fier au seul scan par signatures** (contournements publiés ✓) mais ajouter une analyse comportementale à l’exécution ; tenir une nomenclature (ML-BOM) ; signer les modèles produits en interne †.

### 4.7 Agents et MCP : modèle de menace

**Le triptyque dangereux** (formulé par Simon Willison en 2025 †) : un agent qui réunit (1) l’accès à des données privées, (2) l’exposition à du contenu non fiable (e-mail, page web, ticket, document) et (3) un moyen d’agir vers l’extérieur (envoi, requête, écriture) est exploitable par injection indirecte. Cas publiés : serveur GitHub MCP détourné par un ticket piégé (Invariant Labs ✓), EchoLeak †. Meta a proposé une « règle de deux » (ne jamais réunir les trois sans validation humaine) †.

| Classe d’attaque | Principe | Contrôle |
| --- | --- | --- |
| Injection indirecte | Instructions cachées dans un contenu lu par l’agent | Séparer contenu et instructions, réduire les droits, valider les actions sensibles |
| Empoisonnement d’outil | Description d’outil piégée | Épingler les descriptions, revoir les changements |
| « Rug pull » | Outil légitime modifié après approbation | Épinglage par empreinte, alerte sur changement |
| Masquage entre serveurs | Un serveur malveillant influence l’usage d’un autre (démontré par Invariant ✓) | Isolation par serveur, listes d’outils autorisés |
| Confused deputy / passage de jeton | L’agent agit avec des droits plus larges que ceux de l’utilisateur | Identité propre par agent, jetons à portée minimale |
| Injection de commande côté serveur | Entrées passées à un shell ou à une requête SQL (CVE nombreuses ✓) | Validation, requêtes paramétrées, bac à sable |
| Exposition réseau | Serveur MCP joignable sans authentification ✓ | Écoute locale par défaut, OAuth, filtrage réseau |
| Chaîne d’approvisionnement | Paquet, skill ou serveur piégé ✓ | Épinglage, provenance, revue, egress filtré |

**Gouvernance minimale** : inventaire des agents et de leurs outils ; identité non humaine dédiée par agent ; permissions éphémères et minimales ; validation humaine pour les actions irréversibles ; journalisation du graphe d’appels d’outils ; interrupteur d’urgence. 🟡 *Le journal d’appels d’outils devient pour les agents ce que le journal d’authentification est pour les humains.*

### 4.8 Crypto : aspects techniques utiles à la défense

- **Chemin de signature.** Bybit (interface de signature altérée) et Binance (service de signature atteint depuis un poste d’ingénieur) montrent que la sécurité des clés dépend de tout ce qui *précède* la signature : poste du développeur, CI, déploiement, interface.
- **Contrôles de politique avant signature** : listes de destinations autorisées, plafonds de débit, délais temporisés pour les gros montants, simulation de transaction, vérification hors bande, séparation des identités de build et de déploiement, aucun jeton de déploiement permanent sur les postes.
- **Délégation de compte (EIP-7702).** Binance indique que ce mécanisme a permis de poursuivre des transferts après la suspension des retraits ✓ : surveiller et révoquer les délégations fait partie de la réponse à incident.
- **Points d’étranglement.** Gel de stablecoins, listes de sanctions, analyse de chaîne ; les attaquants répondent par la vitesse, les ponts, THORChain, Chainflip et des mixers ✓.
- **Particuliers** : phrase de récupération jamais dans le presse-papiers, un cloud ou une capture ; portefeuille matériel ; révoquer les approbations inutiles ; méfiance envers les « skills » et extensions qui demandent des portefeuilles ✓.
- **Pont avec l’IA** : les services d’IA souterrains acceptent crypto et PayPal ✓ ; les « mixers de LLM » jouent pour l’inférence le rôle des mixers pour les fonds. 🟡

### 4.9 Le facteur humain à l’ère de l’IA

| Technique | Exemple | Contre-mesure |
| --- | --- | --- |
| Faux recruteur et exercice de code | Binance ✓, Ronin †, Blockaid ✓ | Postes d’entretien isolés, jamais de code externe sur un poste de production |
| Vishing du support, fatigue MFA | Uber †, ShinyHunters ✓ | Vérification hors bande, MFA résistant au phishing |
| Instructions à copier-coller (ClickFix) | Leurre « prérequis » de ClawHub, destiné à l’agent ✓ | Ne jamais laisser un agent exécuter une commande issue d’un contenu externe |
| Deepfake audio et vidéo | Arup, Hong Kong, 2024 † | Procédures de rappel et double validation |
| Faux informaticiens infiltrés | Corée du Nord (Chainalysis ✓) | Vérification d’identité renforcée, accès minimal |

---

## 5. Économie souterraine : prix, chaîne de valeur, incitations

### 5.1 Repères de prix

« Versions » = chiffre repris des cinq versions sans nouvelle vérification ; ✓ = revérifié en novembre 2026.

| Produit | Repère | Source |
| --- | --- | --- |
| Accès IA volé ou reconditionné | Rabais de 70 à 90 % sur les abonnements officiels | Okta ✓ |
| « LLM mixer » Jet Night | \~60 $/mois pour un accès « illimité », contre 100 à 250 $ en offre légitime | GTIG ✓ |
| Cadre de jailbreak par abonnement | 50 à 200 $/mois | Group-IB (versions) |
| Prompt à l’unité | Dès \~0,32 $ | Halcyon (versions) |
| « Modèles criminels » | \~300 $/mois (Xanthorox) ; 35, 55 ou 75 $/mois (Luciferus, non vérifié par Sophos) | Unit 42, Trend Micro, Sophos (versions) |
| Annonces d’outils IA sur les forums | De 38 (déc. 2025) à 1 486 (fév. 2026) | Halcyon (versions) |
| Mentions d’agents, d’injection indirecte et de MCP | +67 % entre février et août 2026 | Bitsight (versions) |
| Vol de credentials ciblant les services d’IA | +376 % entre T4 2025 et T1 2026 | Sysdig (versions) |
| Prix moyen des comptes d’IA volés | Plus que doublé en 2026 ; plus de 400 comptes vendus par jour (étude antérieure) | GTIG, eSentire (versions) |
| Drainers crypto | \~20 % du butin | Sources sectorielles † |

### 5.2 Chaîne de valeur actualisée

```
Auteur du stealer ─► Opérateurs de logs ─► Courtiers / checkers ─► Mixers & passerelles ─► Clients
(abonnement MaaS)    (panneaux, trieurs)   (validation, garantie)   (routage, pool de clés)  (fraude, contournement
       ▲                                                                                      de restrictions
       │                                                                                      régionales ✓)
Fermes d’inscription ──► crédits gratuits ─────────────────────────┘
(comptes synthétiques)   (startup, bonus cloud ✓)

Rails : Telegram / Discord · hébergement résilient · crypto et PayPal ✓
Victimes : propriétaires de la clé ou de la session, qui paient la facture
```

**Deux sources d’approvisionnement coexistent** : le *vol* (logs, clés, endpoints exposés) et la *fraude à l’inscription* (comptes synthétiques pour récolter crédits gratuits et bonus de startup ; Okta ✓). Le second n’est pas dans les cinq versions et ne laisse aucune victime individuelle : il s’attaque aux politiques commerciales des fournisseurs. Okta signale aussi qu’un accès d’IA volé *complique l’attribution* d’autres activités malveillantes ✓.

### 5.3 Pourquoi le marché fonctionne : réduction d’incertitude

À chaque maillon, la valeur ajoutée vient d’une **réduction d’incertitude** : le checker confirme la validité, la garantie de remplacement couvre l’échec, le tri qualifie la cible. Les pannes annoncées de Jet Night sur Telegram ✓ montrent le même phénomène côté consommateur : le client achète une disponibilité, et subit la révocation des clés volées.

### 5.4 Loi de Gresham souterraine (v3, conservée)

Les dumps abondants ont chassé les données rares ; les logs qualifiés ont chassé les dumps ; les clés d’IA à fort plafond chassent les logs ordinaires. 🟡 *Le marché se raffine à chaque étape : plus sélectif, plus industriel.*

---

## 6. Droit, assurance, FinOps

*Cette section est une aide à la réflexion, pas un avis juridique : faire valider par un juriste ou le courtier d’assurance.*

| Question | Éléments de réponse | Niveau |
| --- | --- | --- |
| Qui paie une facture d’inférence frauduleuse ? | En règle générale, l’usage d’une clé valide est traité comme une consommation du client (responsabilité partagée). Les gestes commerciaux sont discrétionnaires. Anthropic a remboursé des charges liées aux sessions volées ✓ ; ce n’est pas un droit général | 🟡 |
| Qualification pénale en France | Accès ou maintien frauduleux dans un système de traitement automatisé de données (art. 323-1 du Code pénal), extraction ou modification de données (art. 323-3) † ; selon les cas, escroquerie ou recel | † |
| Obligations de notification | Violation de données personnelles : notification à la CNIL dans les 72 h (art. 33 du RGPD) † ; selon le secteur, NIS2 ou DORA peuvent ajouter des obligations d’incident. Vérifier l’état de la transposition en vigueur | † |
| Assurance cyber | Beaucoup de contrats couvrent remédiation, perte d’exploitation et sanctions, mais limitent ou excluent le « vol de ressources informatiques » sans atteinte aux données ni indisponibilité (cryptojacking, cloud jacking). À vérifier contrat par contrat | 🟡 |
| Contrats fournisseurs | Clauses de responsabilité, plafonds de dépense, procédure de contestation, délai de signalement | 🟡 |

**Impératif FinOps-Sec.** L’absence de plafond de dépense dur sur un compte cloud ou d’IA est une vulnérabilité au même titre qu’un port ouvert. Aligner finance, sécurité et achats : alertes de budget avec action automatique, propriétaire nommé pour chaque clé, procédure de contestation préparée *avant* l’incident.

---

## 7. Prospective 2027-2030 🔪

Méthode : prolonger les courbes observées (industrialisation, déplacement vers les agents, externalisation du coût, concentration des secrets) et chercher les signaux faibles déjà présents en novembre 2026. Ce sont des hypothèses de travail, pas des prédictions.

| Scénario | Dynamique | Signaux déjà visibles ✓ | Implication défensive |
| --- | --- | --- | --- |
| **A. Commodité totale** | Accès et agents détournés vendus comme du SaaS avec garanties | Mixers à tarif fixe, pannes annoncées comme des incidents de service, partenariat rançongiciel-TeamPCP | Identité et quotas deviennent le cœur de la défense ; l’EDR seul ne suffit plus |
| **B. Fracture réglementaire** | Traçabilité exigée (vérification d’identité des clés à fort volume, journalisation) ; marché gris de proxys | Politiques de crédits gratuits resserrées, vérifications d’inscription | La conformité crée de nouvelles cibles : les comptes légitimes à forte confiance |
| **C. Course défensive gagnante** | Jetons liés à l’appareil, secrets éphémères, plafonds par défaut, bac à sable MCP | Liaison d’appareil en déploiement, épinglage d’actions CI recommandé par CERT-EU | Le marché des wrappers s’assèche ; retour aux modèles locaux et à l’ingénierie sociale |
| **D. Crise de confiance dans la chaîne** (ajout) | Après plusieurs compromissions en cascade, les organisations gèlent ou internalisent leurs dépendances | TeamPCP, ClawHavoc, CVE MCP | Registres internes, provenance vérifiée, mises à jour retardées volontairement |
| **E. SOC agentique contre attaquants agentiques** (ajout) | Détection et réponse par agents, à la vitesse des attaquants | GTG-1002 : milliers de requêtes, souvent plusieurs par seconde ✓ | Les agents défensifs deviennent eux-mêmes des identités privilégiées à protéger |

**Invariant des cinq scénarios** : le vecteur principal ne disparaît dans aucun d’eux, le **secret rejouable**. C’est la conclusion la plus robuste du rapport.

**Indicateurs à suivre en 2027** : part des sessions liées à l’appareil ; délai médian entre fuite et révocation ; nombre d’instances Ollama et de serveurs MCP exposés ; prix des comptes d’IA volés ; part des incidents dont l’entrée est un outil d’IA ou de développement ; volume de skills et de serveurs MCP retirés des registres ; part de l’Asie dans les clients des mixers.

---

## 8. Matrice de synthèse (2019 → 2023 → 2026 → 2028)

| Dimension | 2019 | 2023 | 2026 | Tendance 2028 🔪 |
| --- | --- | --- | --- | --- |
| Marchandise phare | Dumps, code, clés crypto | Logs d’infostealers | Logs, jetons, clés d’IA, sessions de services d’IA | Accès aux capacités (agents, quotas) |
| Intermédiaires | Vendeurs de dumps | Marchés de logs, premiers IAB | IAB, checkers, mixers, fermes d’inscription | Courtiers d’identités non humaines |
| Contournement du MFA | Rare | Rejeu de cookies en hausse | Standard, anti-detect browsers | Liaison d’appareil si adoption |
| Infrastructure exposée | S3, MongoDB, Elasticsearch | Idem + SaaS mal configurés | Ollama, MCP, passerelles LLM | Agents et outils internes exposés |
| Chaîne d’approvisionnement | Rare (cas isolés) | Paquets piégés (npm, PyPI) | CI, scanners, passerelles LLM, skills, serveurs MCP | Registres internes, provenance exigée |
| Modèles | Aucun marché | WormGPT, FraudGPT | Wrappers, jailbreaks, artefacts piégés | Agents clés en main |
| Crypto | Vol de clés, hacks de plateformes | Drainers, ponts | Chaîne développeur → signature ; Corée du Nord \~85 à 89 % des pertes | Signature à politiques et délais |
| Barrière à l’entrée | Compétence et capital | En baisse | 10 à 300 $/mois | Quasi nulle |
| Coût pour la victime | Fuite | Fuite + rançon | Fuite + rançon + facture d’inférence | Fuite + facture + actions d’agent |
| Signal de détection | Hash, IOC, C2 | IOC + comportement | + identité, graphe d’API, métriques de coût | Comportement d’agent, graphe d’outils |

---

## 9. Défense

### 9.1 Trois niveaux de maturité

**Niveau 1 : hygiène**

- Plafonds d’usage et budgets chez chaque fournisseur d’IA ; une clé par usage ; restriction par IP ou domaine ; liste de modèles autorisés.
- Interdiction des secrets en clair ; coffre de secrets ; scan de secrets en pré-commit et en CI ; rotation automatisée.
- Lutte contre les infostealers : EDR, navigateurs à jour, limitation des extensions, pas de mots de passe critiques enregistrés dans le navigateur.
- Aucun service d’inférence ou MCP exposé sans authentification ; mises à jour des serveurs d’IA.
- Épinglage des actions CI et des dépendances par empreinte de commit ; secrets de CI réduits au strict nécessaire.

**Niveau 2 : détection**

- Tableau de bord de coûts par clé et par projet avec alertes sur écarts (voir pseudo-règle en 4.4) ; le coût est un IOC.
- Honeytokens dans les fichiers de configuration des postes de développement.
- Scan régulier de la surface externe (serveurs d’IA, MCP, buckets).
- Veille sur ses propres domaines, comptes et clés dans les logs de stealers et marchés ; révocation proactive.
- Détection de rejeu de session (contexte incohérent, deux empreintes pour un même jeton).
- Surveillance des modifications de journalisation et des créations de clés.

**Niveau 3 : architecture et gouvernance**

- Jetons liés à l’appareil et passkeys ; fédération d’identité de charge de travail à la place des clés statiques.
- Passerelle LLM *durcie et isolée* (clés virtuelles courtes, secrets minimaux, mises à jour surveillées) : c’est un joyau.
- Agents : identité dédiée, permissions éphémères, validation humaine pour les actions irréversibles, bac à sable des outils, journal d’appels d’outils, interrupteur d’urgence.
- Chaîne ML : formats sans exécution de code, registre interne, épinglage, chargement en bac à sable.
- Postes d’entretien et de test de code isolés des postes de production.
- Résilience contractuelle et assurantielle revue (section 6).

### 9.2 Plan 30-60-90 jours

| Horizon | Actions concrètes |
| --- | --- |
| **30 jours** | Inventaire de toutes les clés d’IA, des comptes de service et des jetons CI avec propriétaire ; plafonds de dépense partout ; révocation d’urgence documentée et testée ; mise à jour des serveurs d’IA ; vérification qu’aucun service d’IA n’est exposé |
| **60 jours** | Tableau de bord de coûts avec alertes ; honeytokens en place ; épinglage des actions CI ; scan de secrets sur l’historique ; liste d’agents et d’outils MCP autorisés |
| **90 jours** | Fédération d’identité de charge de travail pour les services critiques ; jetons liés à l’appareil sur les applications sensibles ; exercice de crise FinOps/SOC (voir 12.3) ; revue de la passerelle LLM et de la CI comme actifs critiques |

### 9.3 Test express en huit questions

1. Combien de temps pour révoquer *toutes* les clés d’IA ? (plus d’une heure : priorité)
2. Détecteriez-vous une hausse de 300 % sur une clé un dimanche ?
3. Chaque agent a-t-il une identité propre et des permissions énumérables ?
4. Un plafond de dépense dur existe-t-il chez *chaque* fournisseur ?
5. Savez-vous quels services d’IA et serveurs MCP sont joignables depuis Internet ?
6. Vos actions CI et dépendances critiques sont-elles épinglées par empreinte ?
7. Une clé factice déposée sur un poste de développeur déclencherait-elle une alerte ?
8. Qui, dans l’organisation, peut valider une transaction ou un déploiement irréversible, et depuis quel poste ?

---

## 10. Cartographie des cadres d’analyse

| Phénomène | MITRE ATT&CK | OWASP LLM Top 10 (2025) † | Note |
| --- | --- | --- | --- |
| Rejeu de cookie ou jeton volé | T1539 Steal Web Session Cookie †, T1528 Steal Application Access Token † | — | Surface identité humaine |
| Clés en clair, fichiers de configuration | T1552.001 Credentials in Files † | LLM02 Sensitive Information Disclosure | Cible des stealers |
| Compte cloud valide | T1078.004 Valid Accounts: Cloud Accounts ✓ | — | LLMjacking, Commission européenne |
| Consommation de ressources volée | T1496 Resource Hijacking † | LLM10 Unbounded Consumption | Cryptojacking → LLMjacking |
| Paquet, action CI, modèle ou skill piégé | T1195.002 Supply Chain Compromise ✓ | LLM03 Supply Chain, LLM04 Data and Model Poisoning | TeamPCP, ClawHub, modèles pickle |
| Injection de prompt directe ou indirecte | — | LLM01 Prompt Injection | EchoLeak, GitHub MCP |
| Agent aux droits excessifs | — | LLM06 Excessive Agency | GTG-1002, ClawHub |

**Autres cadres.** *Kill chain* : l’accès initial reste le maillon n°1, l’IA comprime la chaîne en heures. *STRIDE* : Spoofing = rejeu de jeton ; Tampering = injection dans le contexte d’un agent ; Elevation of privilege = abus des outils MCP. *Défense en profondeur* : endpoint, identité, capacité, pipeline, coût, chaîne d’approvisionnement. MITRE ATLAS complète ATT&CK pour les techniques propres à l’IA †.

---

## 11. Réserves méthodologiques

1. **Sources majoritairement commerciales.** Éditeurs de détection, de threat intelligence et analystes de chaîne vendent de la surveillance : chiffres directionnellement utiles, structurellement orientés.
2. **Conflit d’intérêts de l’auteur.** Cette synthèse a été rédigée par Claude, un modèle d’Anthropic. Deux éléments concernent Anthropic (GTG-1002 ; sessions Claude volées) et reposent sur des communications de l’entreprise, rapportées ensuite par la presse. Ils sont traités comme les autres sources intéressées : à recouper.
3. **Revendiqué contre confirmé.** Instructure (275 M revendiqués), Binance (cause non entièrement établie, post-mortem auto-déclaré), attributions à la Corée du Nord (FBI, TRM, Chainalysis contre démenti de Pyongyang).
4. **Sources secondaires faibles.** Certaines données MCP et Hugging Face proviennent de synthèses de presse ou de blogs communautaires ; elles sont signalées comme telles et à remplacer par les sources primaires avant publication.
5. **Vérification partielle.** Les éléments marqués ✓ ont été recoupés en novembre 2026 sur une ou plusieurs sources ; ceux marqués † viennent de connaissances antérieures, et les chiffres « versions » des cinq documents. Aucun n’a été audité de façon indépendante.
6. **Échantillons limités.** Le dump Okta de 5 871 machines illustre un mécanisme, il ne mesure pas une prévalence.
7. **Effet de survie.** Les opérations les plus abouties sont celles qui n’apparaissent pas dans les rapports.
8. **Marché instable.** Démantèlements, exit scams et migrations : tout chiffre a une demi-vie de quelques semaines.
9. **Prospective.** Les scénarios sont des hypothèses balisées 🔪, pas des prédictions.
10. **Posture défensive.** Les mécanismes sont décrits à un niveau conceptuel ; aucune procédure d’attaque n’est fournie.

---

## 12. Pédagogie

### 12.1 Programme d’une journée (7 h)

| Horaire | Module | Contenu |
| --- | --- | --- |
| 09:00 | Ouverture | Thèse, légende 🔵 🟡 🔪 ✓ †, cinq surfaces d’accès |
| 09:30 | Atelier 1 | 2019 reconstitué : classer 20 annonces fictives par rayon |
| 10:30 | Acte « Accès » | Chaîne stealer → log → courtier → exploitation ; jeu de rôles en quatre stations |
| 11:30 | Atelier 2 | Anatomie d’un log (structure défensive) et ce qui le qualifie |
| 13:30 | Acte « Facture » | LLMjacking : suivre l’argent, construire le tableau de bord de coûts (4.4) |
| 14:30 | Atelier 3 | Lecture commentée du post-mortem Binance et de la chronologie TeamPCP |
| 15:30 | Acte « Action » | Modèle de menace MCP et agents (4.7) ; débat jailbreak ou exploit |
| 16:30 | Atelier 4 | Test express en huit questions (9.3) |
| 17:15 | Clôture | Scénarios A à E : vote et justification |

### 12.2 Questions de discussion

1. Un jeton de session volé est-il plus dangereux qu’un mot de passe volé ?
2. Qu’est-ce qui distingue un « rogue model » d’un jailbreak revendu, et d’un artefact de modèle piégé ?
3. Les passkeys suffisent-ils contre le vol de session ? Pourquoi ?
4. Une passerelle LLM est-elle un composant d’infrastructure ou un actif critique ?
5. Le LLMjacking est-il une fraude, un vol de ressource, ou les deux ? Qui est responsable ?
6. Dans l’affaire Binance, quel contrôle unique aurait le plus réduit le dommage ?
7. Un MCP est-il un « service exposé » au sens de la surface externe ?
8. Les gels de stablecoins sont-ils un modèle pour d’autres points d’étranglement ?
9. Dans quel scénario (A à E) votre organisation est-elle la plus vulnérable ?
10. Que changerait un mélange de modèles locaux et de passerelle centrale pour votre exposition ?

### 12.3 Exercices et tabletops

| Code | Exercice | Livrable |
| --- | --- | --- |
| E1 | Inventorier les clés d’IA d’une organisation fictive | Tableau : clé, propriétaire, usage, plafond, rotation, risque |
| E2 | Rédiger une politique de secrets pour les outils de développement assistés par IA | Politique : stockage, rotation, interdictions, scan |
| E3 | Matrice de risque donnée, accès, modèle, prompt, coût, chaîne | Signaux de détection et contre-mesures niveaux 1 à 3 |
| E4 | Procédure d’urgence « clé compromise » | Étapes, délais, responsables, critères de révocation massive |
| E5 | Recalculer les pertes d’un scénario de LLMjacking | Estimation de coût, effet d’un plafond |
| **E6** | **Tabletop FinOps/SOC** : alerte de +15 000 $/heure sur un compte d’IA un dimanche soir | Chronologie de confinement sans couper la production légitime |
| **E7** | **Tabletop chaîne d’approvisionnement** : une dépendance de CI est annoncée comme compromise | Liste des secrets à faire tourner, ordre, communication |
| **E8** | **Audit d’un agent** relié par MCP à une messagerie et une base de données | Cartographie des droits, rupture du triptyque dangereux |
| **E9** | **Pré-mortem** : « dans un an, une clé d’IA a coûté 500 000 $ » | Dix causes plausibles classées par probabilité et coût de prévention |

---

## 13. Glossaire étendu

| Terme | Définition courte |
| --- | --- |
| **IAB** | Initial Access Broker : revendeur d’accès déjà obtenu, testé et garanti |
| **Infostealer / log** | Malware qui aspire les secrets d’une machine ; le « log » en est le produit |
| **Jeton porteur** | Jeton valable pour quiconque le présente (cookie, JWT, clé d’API) |
| **DBSC** | Device Bound Session Credentials : cookies courts renouvelés par preuve de possession d’une clé non exportable de l’appareil |
| **Passkey** | Identifiant WebAuthn à clé privée non exportable |
| **Fédération d’identité de charge de travail** | Jetons courts obtenus par une charge de travail sans clé statique stockée |
| **Honeytoken** | Secret factice dont toute utilisation déclenche une alerte |
| **LLMjacking** | Usage frauduleux de capacité d’inférence via des identifiants volés, facturé à la victime (terme de Sysdig, 2024) |
| **LLM mixer** | Service qui mutualise des clés volées et revend un accès unifié |
| **Denial of wallet** | Épuisement du budget d’une victime par consommation de ressources facturées à l’usage |
| **Pay-as-you-go (PAYG)** | Facturation à l’usage, que des attaquants activent sur des comptes compromis ✓ |
| **Jailbreak** | Faire produire à un modèle des sorties que ses garde-fous interdisent |
| **Injection de prompt (indirecte)** | Instructions cachées dans un contenu lu par un agent |
| **MCP** | Model Context Protocol : protocole reliant modèles et outils |
| **Skill** | Extension d’agent décrite en langage naturel et exécutée avec les droits de l’agent |
| **Triptyque dangereux** | Données privées + contenu non fiable + canal de sortie dans un même agent † |
| **Empoisonnement d’outil / rug pull** | Description d’outil piégée / outil modifié après approbation |
| **Pickle** | Format de sérialisation Python exécutant du code au chargement |
| **Safetensors** | Format de poids qui ne contient que des données |
| **GGUF** | Format de modèles pour l’inférence locale |
| **trust remote code** | Option de chargement qui exécute du code fourni par le dépôt du modèle † |
| **Slopsquatting** | Enregistrement de noms de paquets inventés par des assistants de code † |
| **ClickFix** | Leurre demandant à la victime, ou à son agent, d’exécuter une commande collée |
| **Drainer** | Kit de phishing qui vide des portefeuilles contre commission |
| **Hot wallet / MPC** | Portefeuille connecté en ligne / signature répartie entre plusieurs parties |
| **EIP-7702** | Mécanisme Ethereum de délégation de permissions d’un compte à un contrat ✓ |
| **TraderTraitor** | Cluster d’intrusion lié à la Corée du Nord, désigné par Chainalysis et TRM pour Binance ✓ |
| **Exit scam** | Fermeture d’un service avec confiscation des fonds des clients |

---

## 14. Sources

**Récupérées et recoupées en novembre 2026 (✓)**

*LLMjacking et clés d’IA*

- Pillar Security via SecurityWeek, « LLMs Hijacked, Monetized in Operation Bizarre Bazaar » : https://www.securityweek.com/llms-hijacked-monetized-in-operation-bizarre-bazaar/
- BleepingComputer, Bizarre Bazaar : https://www.bleepingcomputer.com/news/security/hackers-hijack-exposed-llm-endpoints-in-bizarre-bazaar-operation/amp/
- Cloud Security Alliance, « LLMjacking: AI Model Hijacking Reaches Black Market Scale » : https://labs.cloudsecurityalliance.org/research/csa-research-note-llmjacking-black-market-ai-model-hijacking/
- Google Cloud / GTIG-Mandiant, « The Rise of LLMJacking » (2 nov. 2026) : https://cloud.google.com/blog/topics/threat-intelligence/llmjacking-monetizing-compromised-cloud-models
- Help Net Security, résumé du rapport GTIG : https://www.helpnetsecurity.com/2026/11/03/llmjacking-stolen-ai-access-cloud-bills/
- Okta Threat Intelligence, « Inside the underground market for AI access » : https://www.okta.com/newsroom/articles/inside-the-underground-market-for-ai-access/
- Malwarebytes et SecurityWeek, sessions Claude volées : https://www.malwarebytes.com/blog/news/2026/09/infostealers-are-hijacking-claude-accounts-at-users-expense et https://www.securityweek.com/anthropic-warns-claude-users-of-infostealer-malware-infections/
- Anthropic, GTG-1002 : https://www.anthropic.com/news/disrupting-AI-espionage

*Infrastructures exposées, MCP, modèles*

- NVD / Red Hat, CVE-2026-7482 : https://nvd.nist.gov/vuln/detail/cve-2026-7482 et https://access.redhat.com/security/cve/CVE-2026-7482
- The Hacker News, Bleeding Llama (Cyera) : https://thehackernews.com/2026/05/ollama-out-of-bounds-read-vulnerability.html
- Trend Micro, serveurs MCP exposés : https://www.trendmicro.com/vinfo/fr/security/news/vulnerabilities-and-exploits/update-on-exposed-mcp-servers-the-threat-widens-to-the-cloud
- CSA, « MCP Security Crisis » : https://labs.cloudsecurityalliance.org/research/csa-research-note-mcp-security-crisis/
- Akamai, vulnérabilités MCP back-end : https://www.akamai.com/blog/security-research/one-fluke-3-pattern-mcp-back-end-vulnerabilities
- Unit 42, ClawHub / OpenClaw : https://unit42.paloaltonetworks.com/openclaw-ai-supply-chain/
- SafePickle (arXiv 2602.19818) : https://arxiv.org/abs/2602.19818 ; « The Art of Hide and Seek » (arXiv 2508.19774) : https://arxiv.org/pdf/2508.19774
- The Next Web, Hugging Face et ClawHub : https://thenextweb.com/news/hugging-face-clawhub-malware-ai-supply-chain

*Chaîne d’approvisionnement et fuites*

- CERT-EU, brèche de la Commission via Trivy : https://cert.europa.eu/blog/european-commission-cloud-breach-trivy-supply-chain
- Akamai, TeamPCP et Telnyx : https://www.akamai.com/blog/security-research/telnyx-sdk-pypi-2026-teampcp-supply-chain-attacks
- Trend Micro, compromission de LiteLLM : https://www.trendmicro.com/es\_mx/research/26/c/inside-litellm-supply-chain-compromise.html
- Bitdefender, avis technique Instructure / Canvas : https://bitdefender.com/en-us/blog/businessinsights/technical-advisory-shinyhunters-breach-instructure-canvas-lms
- SpyCloud, fuites 2026 : https://spycloud.com/blog/data-breaches-2026/
- SpyCloud, StealC et Endgame : https://spycloud.com/blog/stealc-malware-takedown-spycloud-data/
- Bitdefender, Operation Endgame (StealC, Amadey) : https://www.bitdefender.com/en-us/blog/hotforsecurity/operation-endgame-stealc-amadey-malware-takedown
- Flashpoint, rapport mi-2026 : https://flashpoint.io/resources/report/information-stealing-malware-threats/

*Crypto*

- Cointelegraph, post-mortem Binance : https://cointelegraph.com/news/binance-fake-job-interviews-compromised-ai-tool-hack
- SOCRadar, Binance : https://socradar.io/blog/binance-hack-6-3b-hot-wallet-theft/
- Crypto Briefing, Chainalysis 2026 : https://cryptobriefing.com/chainalysis-north-korea-hacks-89-percent-losses/
- The Block, Blockaid S1 2026 : https://theblock.co/post/409944/crypto-hacks-hit-record-high-in-h1-2026-as-losses-top-1-billion-blockaid-says
- The Block, Chainalysis 2025 : https://www.theblock.co/amp/post/382477/crypto-hack-2025-chainalysis

**Sources des cinq versions** (Rapid7, Okta, Sysdig, Trend Micro, Unit 42, Sophos CTU, Group-IB, Halcyon, Bitsight, Flashpoint, SOCRadar, Cyble, Malwarebytes, The Record, etc.) : voir les listes de liens des versions « prospective » et « EXPANDU ». Les chiffres qui en proviennent sont signalés « versions ».

**Connaissances antérieures (†)** : à recouper dans les sources primaires (rapports de Troy Hunt, Wiz, JFrog, ReversingLabs, Mandiant, Microsoft, Anthropic, CERT-UA, Chainalysis, etc.) avant toute publication externe.

---

*Document pédagogique et défensif. Ce marché évolue en semaines : actualiser avant chaque session. Édition du 21 novembre 2026.*
