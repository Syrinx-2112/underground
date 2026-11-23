# Marché noir des données cryptographiques — Vecteurs et monétisation

**Dossier défensif · Édition du 24 novembre 2026**
**Statut** : TLP:CLEAR · **Base** : trois documents du dépôt (P2P, économie souterraine 2019→2026, veille marché souterrain)
**Posture** : pédagogique et défensive. Les mécanismes sont décrits au niveau utile pour concevoir des défenses — aucune procédure d'attaque, aucune charge utile, aucune méthode d'évasion.

**Légende** : 🔵 observation rapportée · 🟡 extrapolation / lecture · 🔪 hypothèse prospective · ✓ vérifié novembre 2026 sur au moins une source · ✓✓ recoupé sur deux sources indépendantes · † connaissance antérieure non revérifiée ce jour.

---

## 0. Thèse en une phrase

La valeur cryptographique ne se vole plus dans les coffres-forts numériques : elle se capte en amont de la signature, sur le poste du développeur, dans la chaîne d'approvisionnement, dans un faux entretien d'embauche ou dans un installateur d'outil d'IA falsifié — puis elle s'évapore en quelques dizaines de minutes à travers des ponts, des mixers et des services de blanchiment, avant que les points d'étranglement du système financier (gels de stablecoins) n'aient le temps de se refermer.

---

## 1. Cadrage

### 1.1 Ce qui a changé entre 2019 et 2026

| Période | Marchandise | Vecteur dominant | Exemple |
| --- | --- | --- | --- |
| 2019–2021 | Clés privées, phrases de récupération | Exploits de contrats, fuites d'exchange | Poly Network (611 M$), Wormhole (320 M$) † |
| 2022–2024 | Validateurs, multisigs | Ingénierie sociale ciblée | Ronin (625 M$, Lazarus) † |
| 2025 | Chaîne développeur → signature | Postes compromis, JavaScript altéré | Bybit (≈1,5 Md$, TraderTraitor) ✓ |
| 2026 | Outils d'IA + service de signature | Installateurs piégés, faux recruteurs, MPC | Binance (≈6,3 Md$) ✓✓ |

La migration est nette : **du coffre au pipeline**. L'actif cryptographique n'est plus la cible directe ; c'est tout ce qui, en amont de la signature, donne le droit de signer.

### 1.2 Chiffres clés 2026

- **Pertes déclarées 2026** : 7,4 Md$ au 30 octobre (TRM Labs) ✓
- **Part de la Corée du Nord** : ~89 % (Chainalysis, 30 oct. 2026) ; ~85 % (TRM) ✓
- **Plus gros vol crypto connu** : Binance, 29 octobre 2026, ≈6,3 Md$ ✓✓
- **Fonds gelés ou interceptés** : 2,85 Md$ annoncés par Binance au 31 octobre ; Tether >1,5 Md USDT, Circle >800 M USDC ✓
- **S1 2026 (Blockaid)** : >1 Md$ ; Drift 285 M$ et KelpDAO 292 M$ attribués à la RPDC ✓

### 1.3 Invariants structurels

Quatre rails sous-tendent le marché, identiques à ceux de l'économie souterraine générale (section 2.4 du rapport « économie souterraine ») :

1. **Réputation** : pseudonymes stables, retours d'acheteurs.
2. **Séquestre** : escrow des places de marché.
3. **Hébergement résilient** : bulletproof hosting.
4. **Monnaie** : cryptomonnaies, stablecoins, PayPal.

Chaque rail est aussi un **point d'étranglement défensif** : le gel de stablecoins par Tether et Circle lors de l'affaire Binance en est la démonstration la plus nette de 2026 ✓.

---

## 2. Vecteurs d'accès aux actifs cryptographiques

### 2.1 Les cinq surfaces d'accès spécifiques au crypto

| Surface | Ce que l'attaquant vise | Vecteurs 2026 |
| --- | --- | --- |
| **Poste du développeur** | Clés de déploiement, accès au service de signature | Installateurs Homebrew falsifiés, outils d'IA piégés ✓ |
| **Ingénierie sociale** | Accès initiaux, exercices de code | Faux recruteurs, vishing ✓ |
| **Chaîne d'approvisionnement** | Secrets de pipeline, paquets, actions CI | TeamPCP, paquets npm/PyPI piégés ✓ |
| **Interface de signature** | JavaScript altéré, approbations trompeuses | Drainers, Bybit ✓ |
| **Infrastructures exposées** | Nœuds, endpoints, clés en clair | Ollama, MCP, S3 † |

### 2.2 Le poste du développeur : nouveau centre de gravité

L'affaire Binance (section 4.1) cristallise une tendance : **la chaîne développeur → CI → signature** est la cible, pas le portefeuille.

| Étape | Ce qui est rapporté |
| --- | --- |
| Juin 2026 | Ingénieur télécharge un paquet altéré en configurant un outil de codage IA ; le poste est remplacé mais des informations sur l'équipe portefeuilles ont déjà été collectées ✓ |
| Septembre 2026 | Faux recruteurs ciblent un autre ingénieur avec un « exercice de code » piégé ; le poste passe sous contrôle ✓ |
| 20 octobre 2026 | Depuis ce poste, un outil d'IA interne est abusé pour atteindre deux autres ingénieurs et obtenir des identifiants de déploiement ✓ |
| 28 octobre 2026 | Préparation : portefeuille d'opérateur alimenté par de petits retraits Tornado Cash ; au moins 36 h de tests ✓ |
| 29 octobre, ~20 h 05 UTC | Déploiement d'un logiciel malveillant dans l'infrastructure des portefeuilles, accès au service de signature, sorties sur 10 blockchains en 20–31 minutes ✓ |

**Leçon** : l'outil de codage IA est devenu un maillon de confiance — installation piégée, puis outil interne utilisé pour pivoter. La campagne a duré quatre mois avant l'incident visible.

### 2.3 Faux recrutements et ingénierie sociale

| Cas | Mécanisme | Leçon |
| --- | --- | --- |
| Ronin (mars 2022) | Faux entretien, clés de validateurs compromises | Le faux recrutement est un vecteur de premier ordre † |
| Binance (sept. 2026) | Exercice de code piégé ciblant un ingénieur | Même mode opératoire, répété à grande échelle ✓ |
| Blockaid (S1 2026) | Schéma LinkedIn → compromission d'un signataire multisig | Deux des quatre plus gros incidents >1 Md$ ✓ |

🟡 **Lecture** : le vecteur humain n'est pas un « maillon faible » — c'est la porte d'entrée privilégiée quand la cible est architecturée pour résister aux attaques techniques directes.

### 2.4 Drainers et phishing de signature

Les drainers as-a-Service (Inferno, Angel…) représentent l'ancêtre économique des « LLM mixers » : même modèle de commission (~20 % du butin), même logique de plateforme.

| Famille | Mécanisme | Période |
| --- | --- | --- |
| Inferno, Angel | Phishing + signatures trompeuses ; développeurs à la commission | 2023–2025 † |
| Ledger Connect Kit (déc. 2023) | Bibliothèque JS compromise via un compte de publication ; code drainer injecté | Exposition large, pertes limitées † |
| Bybit (fév. 2025) | Environnement d'un développeur de Safe{Wallet} compromis, fichier JS modifié, signataires trompés | ≈1,5 Md$, attribué à TraderTraitor ✓ |

### 2.5 Failles de contrats et ponts

| Cas | Mécanisme | Chiffre |
| --- | --- | --- |
| Poly Network (août 2021) | Faille de contrat intelligent | ≈611 M$ (restitués en grande partie) † |
| Wormhole (fév. 2022) | Faille de vérification de signatures d'un pont | ≈320 M$ † |

🟡 Les ponts concentrent le risque : ils cumulent valeur et complexité, et leur compromission ne dépend d'aucun facteur humain — ce qui les rend à la fois plus rares et plus spectaculaires.

---

## 3. Anatomie technique défensive

### 3.1 Le chemin de signature

Bybit et Binance montrent que **la sécurité des clés dépend de tout ce qui précède la signature** :

```
Poste développeur ──► CI ──► Déploiement ──► Interface ──► Service de signature
       │                │         │              │                │
       └─── chaque maillon est une surface d'attaque ─────────────┘
```

### 3.2 Contrôles à placer avant la signature

| Contrôle | Objectif |
| --- | --- |
| Listes de destinations autorisées | Réduire le rayon d'explosion |
| Plafonds de débit et délais temporisés pour gros montants | Ralentir l'exfiltration |
| Simulation de transaction | Détecter les anomalies avant exécution |
| Vérification hors bande | Contourner un poste compromis |
| Séparation des identités de build et de déploiement | Isoler les rayons d'explosion |
| Aucun jeton de déploiement permanent sur les postes | Supprimer les secrets rejouables |

### 3.3 Délégation de compte (EIP-7702)

Binance indique que le mécanisme **EIP-7702** d'Ethereum a permis de poursuivre les transferts après la suspension des retraits ✓.

🟡 **Conséquence défensive** : surveiller et révoquer les délégations fait partie de la réponse à incident. Un contrôle qui « arrête les retraits » peut être contourné par une délégation antérieure.

### 3.4 MPC et révocation

Le système MPC (Multi-Party Computation) complique la révocation : plusieurs parts de clé sont détenues par plusieurs parties. En cas de compromission d'une part, la révocation exige une coordination qui prend des heures — pendant lesquelles l'attaquant continue de signer.

---

## 4. Monétisation et blanchiment

### 4.1 La chaîne de blanchiment crypto

```
Vol ──► Agrégation ──► Ponts ──► Mixers ──► Services de blanchiment ──► Cash-out
         (chaînes)     (cross-chain)  (Tornado,    (services en langue    (P2P,
                                      THORChain,    chinoise, OTC)         exchanges
                                      Chainflip)                           non-KYC)
```

### 4.2 Les outils de monétisation

| Outil | Rôle | Observations 2026 |
| --- | --- | --- |
| **Tornado Cash** | Mixer Ethereum | Utilisé pour préparer l'attaque Binance (petits retraits) ✓ |
| **THORChain, Chainflip** | Ponts décentralisés | Réponse aux gels de stablecoins ✓ |
| **Services en langue chinoise** | Blanchiment OTC | Chainalysis 2025 : canal principal de la RPDC † |
| **Stablecoins (USDT, USDC)** | Véhicule de transfert | Cibles des gels Tether/Circle ✓ |

### 4.3 Les gels de stablecoins : point d'étranglement défensif

| Acteur | Montants gelés (affaire Binance) |
| --- | --- |
| Tether | >1,5 Md USDT ✓ |
| Circle | >800 M USDC ✓ |
| Binance (annonce) | 2,85 Md$ gelés ou interceptés ✓ |

🟡 **Lecture** : les points d'étranglement du système financier fonctionnent — mais seulement **après** l'exfiltration initiale. La fenêtre critique se mesure en dizaines de minutes (20–31 min pour Binance). La défense doit donc être **amont** (empêcher la signature) autant qu'**aval** (geler).

### 4.4 LLM mixers : la crypto de l'inférence

Les « LLM mixers » (Jet Night, new-api, gcli2api) jouent pour l'inférence le rôle des mixers pour les fonds : ils mutualisent des clés volées et revendent un accès unifié.

| Produit | Prix | Source |
| --- | --- | --- |
| Plan « illimité » Jet Night | ≈60 $/mois (vs 100–250 $ légitime) | GTIG ✓ |
| Jetons revendus | 0,10–25 $ par million | Suivi GTIG relayé ✓ |

🟡 Les services d'IA souterrains acceptent crypto et PayPal ✓ : la frontière entre vol de crypto et vol de capacité d'inférence est poreuse.

---

## 5. Acteurs et attribution

### 5.1 La Corée du Nord : acteur dominant

| Source | Part de la RPDC dans les pertes 2026 |
| --- | --- |
| Chainalysis (30 oct. 2026) | ≈89 % ✓ |
| TRM Labs | ≈85 % ; 7,4 Md$ volés à ce jour ✓ |
| FBI (affaire Binance) | Attribution à la Corée du Nord (rapportée) ✓ |

Clusters identifiés : **Lazarus**, **TraderTraitor** ✓.

### 5.2 Opérations de répression

| Opération | Date | Cible | Résultat |
| --- | --- | --- | --- |
| Endgame (juin 2026) | 15–19 juin | SocGholish, Amadey, StealC | 326 serveurs, 142 domaines, ~27 M d'identifiants ✓ |
| Endgame (nov. 2026) | 12 nov. | Gootloader, CastleRAT, StealC | 235 serveurs, 257 domaines, 6,4 M d'identifiants ✓ (C) |

🟡 Les démantèlements **déplacent** plus qu'ils ne suppriment (Flare, IOCTA, ENISA) : la réputation survit, l'infrastructure compte plus que le code.

### 5.3 Drainers as-a-Service

| Modèle | Commission | Période |
| --- | --- | --- |
| Inferno, Angel | ~20 % du butin | 2023–2025 † |
| Drift, KelpDAO (attribués RPDC) | — | S1 2026, >1 Md$ ✓ |

---

## 6. Études de cas

### 6.1 Binance — 29 octobre 2026 ✓✓

| Phase | Fait |
| --- | --- |
| Juin 2026 | Installateur Homebrew falsifié pour un outil de codage IA |
| Été | Faux entretien, poste compromis, rotation partielle |
| Automne | Abus d'un outil d'IA interne, atteinte du service de signature |
| 29 oct., ~20 h 05 UTC | Sorties sur 10 blockchains en 20–31 min, ≈6,3 Md$ |
| Réponse | Tether gèle >1,5 Md USDT, Circle >800 M USDC ; Binance absorbe la perte, 98 % reconstitués au 31 oct. |
| Attribution | FBI : Corée du Nord ; Chainalysis/TRM : TraderTraitor |

**Leçons** :
1. La chaîne développeur → CI → signature est la cible.
2. L'outil de codage IA est un maillon de confiance.
3. Campagne lente de quatre mois.
4. Les points d'étranglement financiers fonctionnent.
5. Binance dit avoir bloqué 57 tentatives liées à la RPDC en 2026 : un seul échec a suffi.

### 6.2 Bybit — février 2025 ✓

Environnement d'un développeur de Safe{Wallet} compromis, fichier JavaScript modifié, signataires trompés. ≈1,5 Md$. Attribué à TraderTraitor.

**Leçon** : l'interface de signature elle-même peut être altérée.

### 6.3 Ronin — mars 2022 †

Faux entretien d'embauche, clés de validateurs compromises. ≈625 M$. Attribué à Lazarus.

**Leçon** : le faux recrutement est un vecteur de premier ordre contre les infrastructures à haute valeur.

---

## 7. Défense

### 7.1 Trois niveaux de maturité

**Niveau 1 — Hygiène**
- Postes d'entretien et de test de code isolés des postes de production.
- Aucun jeton de déploiement permanent sur les postes.
- Épinglage des actions CI par empreinte de commit.
- Rotation des secrets après tout incident.

**Niveau 2 — Détection**
- Surveillance des délégations EIP-7702.
- Honeytokens dans les configurations d'outils de développement.
- Alertes sur activation de services de signature jamais utilisés.
- Tableau de bord des transactions par seuil et par destinataire.

**Niveau 3 — Architecture**
- Séparation stricte des identités de build et de signature.
- Délais temporisés pour les gros montants.
- Vérification hors bande pour les transactions irréversibles.
- Fédération d'identité de charge de travail à la place des clés statiques.

### 7.2 Test express en huit questions

1. Combien de temps pour révoquer toutes les clés de signature ? (>1 h : priorité)
2. Détecteriez-vous une transaction anormale un dimanche soir ?
3. Chaque signataire a-t-il une identité propre et des permissions énumérables ?
4. Un plafond de débit dur existe-t-il sur chaque service de signature ?
5. Savez-vous quels outils d'IA sont installés sur les postes d'ingénieurs ?
6. Vos actions CI et dépendances critiques sont-elles épinglées par empreinte ?
7. Une clé factice déposée sur un poste déclencherait-elle une alerte ?
8. Qui peut valider une transaction irréversible, et depuis quel poste ?

### 7.3 Plan 30-60-90 jours

| Horizon | Actions |
| --- | --- |
| 30 jours | Inventaire des clés de signature et jetons de déploiement ; plafonds partout ; révocation d'urgence testée ; isolation des postes d'entretien |
| 60 jours | Tableau de bord des transactions ; honeytokens ; épinglage CI ; surveillance des délégations EIP-7702 |
| 90 jours | Fédération d'identité pour les services critiques ; exercice de crise (simulation Binance) ; revue du service de signature comme actif critique |

---

## 8. Prospective 2027–2030 🔪

| Scénario | Dynamique | Signaux 2026 |
| --- | --- | --- |
| **A. Signature agentique** | Les agents signent des transactions avec des permissions éphémères | GTG-1002 ✓ |
| **B. Gel systémique** | Les émetteurs de stablecoins gèlent préventivement sur alerte | Gels Binance ✓ |
| **C. Post-MPC** | La MPC devient la norme, compliquant la révocation mais aussi le vol | Binance ✓ |
| **D. Supply chain IA-crypto** | Les outils d'IA deviennent le vecteur principal contre les infrastructures crypto | Binance, TeamPCP ✓ |
| **E. Attribution claire** | La RPDC est systématiquement attribuée, les sanctions s'alourdissent | Chainalysis, TRM ✓ |

🟡 **Lecture** : le scénario D est déjà en cours. Les scénarios B et C progressent. Le scénario A émerge.

---

## 9. Cartographie des cadres d'analyse

| Menace | MITRE ATT&CK | OWASP |
| --- | --- | --- |
| Vol de clés de signature | T1552.004 (Private Keys) † | — |
| Compromission de la chaîne logicielle | T1195.002 | LLM03 |
| Faux recruteur | T1566.004 (Spearphishing Voice) † | — |
| Drainer de signature | T1059.007 (JavaScript) † | — |
| Blanchiment par mixers | T1590 (Gather Victim Network Information) † | — |

---

## 10. Glossaire spécifique

| Terme | Définition |
| --- | --- |
| **Drainer** | Kit de phishing qui vide des portefeuilles contre commission |
| **EIP-7702** | Mécanisme Ethereum de délégation de permissions d'un compte à un contrat ✓ |
| **Hot wallet / MPC** | Portefeuille connecté en ligne / signature répartie entre plusieurs parties |
| **LLM mixer** | Service qui mutualise des clés volées et revend un accès unifié |
| **TraderTraitor** | Cluster d'intrusion lié à la Corée du Nord, désigné par Chainalysis et TRM pour Binance ✓ |
| **Chemin de signature** | Ensemble des maillons (poste, CI, déploiement, interface) en amont de la signature effective |

---

## 11. Réserves méthodologiques

- **Sources commerciales** : Chainalysis, TRM Labs, Blockaid vendent de la surveillance ; chiffres directionnellement utiles, structurellement orientés.
- **Attribution** : FBI, TRM et Chainalysis d'un côté, démentis de Pyongyang de l'autre — les attributions sont des jugements, pas des faits établis.
- **Post-mortem auto-déclarés** : Binance publie son propre post-mortem ; à croiser avec les analyses indépendantes.
- **Chiffres gelés** : les montants annoncés par Binance (2,85 Md$) et par les émetteurs (Tether, Circle) ne se recoupent pas parfaitement ; l'écart reflète des périmètres différents.
- **Posture défensive** : aucun mécanisme d'attaque opérationnel n'est fourni.

---

## 12. Sources principales

**Autorités et analyses**
- Chainalysis, *Crypto Hack Report 2026* (30 oct. 2026) ✓
- TRM Labs, suivi 2026 ✓
- FBI, attribution Binance (rapportée) ✓
- Blockaid, *S1 2026 Crypto Hacks* ✓

**Études de cas**
- Binance, post-mortem (3 nov. 2026) ✓✓ — https://www.binance.com/square/post/384314395829022
- Cointelegraph, Binance ✓
- SOCRadar, Binance ✓
- Akamai, TeamPCP ✓
- CERT-EU, Commission / Trivy ✓

**Vecteurs et monétisation**
- Google GTIG / Mandiant, *LLMJacking* (2 nov. 2026) ✓✓
- Pillar Security, *Bizarre Bazaar* (28 janv. 2026) ✓✓
- Europol, *Endgame* (24 juin et 12 nov. 2026) ✓

**Cadres**
- MITRE ATT&CK, campagne C0062 (GTG-1002) ✓
- OWASP LLM Top 10 (2025) †

---

**Document défensif · Édition du 24 novembre 2026 · À actualiser avant toute diffusion externe.**
