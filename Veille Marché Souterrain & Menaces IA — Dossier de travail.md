# Veille Marché Souterrain & Menaces IA — Dossier de travail

*Audit du rapport « Des Binaires aux Agents », base de connaissances actualisée au 22 novembre 2026, et cahier de lancement d'un projet de veille entreprise (volet développement inclus)*

**Statut** : brouillon de travail v4.0 · **Date** : 22 novembre 2026 · **Diffusion** : TLP:CLEAR (sources ouvertes uniquement) · **Base** : rapport « Version intégrée — recommandations DeepSeek appliquées et validées » (778 lignes) + vérifications web du jour

---

## 0. Mode d'emploi et verdict

### 0.1 Ce que contient ce dossier

| Partie | Contenu | À quoi ça sert |
| --- | --- | --- |
| **1** | Audit du rapport source : 30 affirmations testées, erreurs structurelles, lacunes | Savoir ce qui est solide, ce qui est périmé, ce qui est faux |
| **2** | Base de connaissances actualisée (chronologie, acteurs, familles, forums, démantèlements) | Socle factuel du projet de veille |
| **3** | IA, agents, supply chain : LLMjacking, MCP, skills, TeamPCP, Binance | Le cœur « nouveau » de 2026 |
| **4** | Institutions, statistiques, droit (France/UE) | Ancrages indépendants et cadre de conformité |
| **5** | Projet de veille : charte, besoins en renseignement (PIR), sources, cycle, gouvernance, éthique, KPI | Le cahier de lancement « métier » |
| **6** | Développement : architecture, modèle de données, modules, code d'exemple, backlog, feuille de route | Le cahier de lancement « technique » |
| **7** | Prospective instrumentée : scénarios A–E mesurables par la plateforme | Transformer la prospective en tableau de bord |
| **8** | Exercices, registre des risques, gabarits, glossaire, sources, points à vérifier | Annexes opérationnelles |

### 0.2 Verdict en dix lignes

1. **Le cadre analytique tient** : le malware vendu comme service, la chaîne fragmentée en maillons spécialisés, l'infrastructure comme centre de gravité, les démantèlements qui déplacent plus qu'ils ne suppriment. Les sources institutionnelles (ANSSI, ENISA, Europol, MITRE) le confirment.
2. **Les faits de 2026 sont majoritairement exacts** pour les événements « IA » : Endgame de juin 2026, TeamPCP, ClawHub, MONST3R-AI, rapport Google/Mandiant sur le LLMjacking, GTG-1002.
3. **Mais le rapport contient des erreurs factuelles nettes sur l'écosystème classique** : stealers (Rhadamanthys présenté n°1 alors qu'il a été perturbé en novembre 2025), ransomware (groupes démantelés présentés comme dominants, Qilin absent), forums (XSS.is et BreachForums donnés « actifs ✓ »).
4. **Le marqueur « ✓ vérifié novembre 2026 » n'est pas fiable en l'état** : il a été apposé sur des statuts de forums faux. Il faut le remplacer par un registre de preuves (URL, date, double source).
5. **Les chiffres de parts de marché et de prix sont des ordres de grandeur non sourcés** ; plusieurs sources indépendantes donnent des classements différents selon la métrique (machines infectées, identifiants, activité observée).
6. **La structure est incohérente** : sept « voiles » annoncés, quatre jamais développés (LLM criminels, LLMjacking, Bizarre Bazaar, MCP) ; exercices et glossaire renvoient à des sections absentes.
7. **Des faits majeurs de 2026 manquent** : braquage Binance (≈6,3 Md$, 29 octobre 2026), Bizarre Bazaar, 175 000 hôtes Ollama exposés, phase de novembre 2026 d'Endgame, vote NIS2 du 17 novembre.
8. **La prospective A–E est utilisable** à condition d'être branchée sur des indicateurs mesurables (partie 7).
9. **Pour un projet de veille, le rapport est une bonne note de cadrage, pas une base de connaissances** : il faut un modèle de données, des sources tracées, une revue périodique.
10. **Recommandation** : scinder en une *édition narrative* (pédagogique, conservée) et une *édition de travail* (ce dossier), toutes deux alimentées par un registre de preuves versionné.

### 0.3 Légende et conventions

Les balises du rapport source sont conservées et complétées :

| Balise | Sens |
| --- | --- |
| 🔵 | Observation : ce que les sources rapportent |
| 🟡 | Extrapolation : ce que la tendance suggère |
| 🔪 | Hypothèse prospective |
| ✓ | Vérifié le 22/11/2026 sur au moins une source nommée |
| ✓✓ | Vérifié sur au moins deux sources indépendantes dont une primaire |
| ✗ | Erroné ou périmé dans le rapport source |
| ⚠ | Exact mais à nuancer ou à préciser |
| ? | Non retrouvé / non vérifiable en l'état |
| † | Connaissance antérieure, non revérifiée ce jour |

**Échelle de fiabilité des sources** (utilisée en partie 5) : A = source primaire (autorité, éditeur de la plateforme, décision judiciaire) ; B = analyse d'un acteur reconnu avec méthode explicite ; C = presse spécialisée relayant un tiers ; D = blog/agrégateur/billet non sourcé ; E = rumeur ou revendication d'acteur malveillant.

### 0.4 Limites de cette révision

- Les vérifications du 22/11/2026 reposent sur des recherches web ; plusieurs chiffres proviennent de presse spécialisée relayant un communiqué (fiabilité C). Les PDF intégraux (IOCTA, ENISA, ANSSI) n'ont pas été relus ligne à ligne : toute citation exacte doit être reprise dans la source primaire avant publication externe.
- Les points marqués † ou ? sont regroupés au §8.6.
- Ce dossier est **défensif** : aucune méthode offensive opérationnelle n'y figure.

---

## 1. Audit du rapport source

### 1.1 Méthode

Chaque affirmation vérifiable a reçu un identifiant (V01…V30), un verdict, la preuve ou la correction, et une action. Les affirmations purement interprétatives (métaphores, « Réel Vivant ») ne sont pas auditées : elles relèvent de l'édition narrative.

### 1.2 Verdict global

| Catégorie | Nombre | Lecture |
| --- | --- | --- |
| ✓ / ✓✓ confirmé | 12 | Surtout les événements IA, institutions, TeamPCP, ClawHub, DBSC |
| ⚠ à nuancer | 7 | Chiffres exacts mais lecture incomplète |
| ✗ erroné ou périmé | 7 | Stealers, ransomware, forums, parts de marché |
| ? non retrouvé | 4 | Prix, « Europol \~20 % », −49 % de mentions, 40 % ENISA |

### 1.3 Tableau de validation

| ID | Affirmation du rapport | Verdict | Preuve ou correction | Action |
| --- | --- | --- | --- | --- |
| V01 | Endgame juin 2026 : 326 serveurs, 142 domaines, \~27 M identifiants, présenté comme « revendication d'éditeur non recoupée » | ✓✓ ⚠ | Annonce **Europol du 24 juin 2026** (opération 15–19 juin ; Canada, Danemark, Allemagne, Pays-Bas, Royaume-Uni, États-Unis ; Microsoft, Bitdefender, IBM X-Force, Proofpoint, ESET…) : 326 serveurs, 142 domaines, \~27 M d'identifiants issus de 385 000+ systèmes, >41 M€ de crypto identifiés. Cibles : **SocGholish, Amadey, StealC**. | Retirer la réserve §IV.2 ; ajouter SocGholish |
| V02 | « Les clients de StealC ont migré en quelques jours vers Vidar 2.0 et Rhadamanthys » | ✗ | Rhadamanthys avait été perturbé en **novembre 2025** (Endgame, 1 025 serveurs). Vidar 2.0 était déjà n°1 depuis novembre 2025. Analyse Flare : les démantèlements déplacent les opérateurs plutôt qu'ils ne réduisent le marché. | Réécrire |
| V03 | Parts de marché stealers : Rhadamanthys \~35 %, Vidar \~25 %, StealC \~20 %, Lumma \~15 %, RedLine \~5 % | ✗ | Non sourcées et contredites. **Vidar** : stealer le plus utilisé sur Russian Market depuis nov. 2025 (Intrinsec, avr. 2026) ; Flashpoint le place 1er début 2026 et sur H1 2026 (Vidar, StealC, Lumma). **Lumma** : \~50 % de l'activité observée 2025 (IBM X-Force), \~60 % des identifiants (Specops). Le classement dépend de la **métrique**. | Remplacer par « classement par métrique » |
| V04 | « Lumma en déclin après Operation Magnus » | ✗ | Magnus (oct. 2024) visait RedLine/META. Lumma a été perturbé en **mai 2025** (Microsoft/DOJ/Europol, \~2 300 domaines) puis est **revenu** (Trend Micro : de 1 à 450+ URL de C2 en trois semaines ; Bitdefender, fév. 2026). | Corriger |
| V05 | Groupes ransomware dominants 2026 : LockBit 3.0, RansomHub, BlackSuit, Akira, Play, 8Base, Cl0p | ✗ | **Qilin** n°1 (≈12–13 % des publications 2026 : 1 165 au 11 nov. selon ransomnews ; 1 127 au 25 oct. selon ransomware.live via Stingrai), **The Gentlemen** n°2 (1 084), **Akira** (439), INC, DragonForce, SafePay. **LockBit 5.0** (reprise post-Cronos) sort du top 10 au T3 2026 (Check Point). RansomHub, BlackSuit, 8Base : † arrêtés ou démantelés en 2025. | Refaire le tableau |
| V06 | LockBit 3.0 classé « G5 agentique » | ✗ | Aucune source. RaaS classique. | Supprimer de G5 |
| V07 | Rançons moyennes par groupe (1–5 M$, etc.) | ? | Aucune source. Utiliser médianes et taux de paiement de négociateurs (Coveware, Chainalysis) avec date. | Sourcer |
| V08 | XSS.is « actif ✓ 2019-2026 » | ✗ | Administrateur présumé arrêté à Kiev le **22 juillet 2025** (enquête du parquet de Paris, 4 ans ; SBU ; Europol), domaine saisi ; forum de 50 000+ membres ; escrow ≈7 M€ de gains. Relances/onion rapportés ensuite : statut « perturbé, partiellement relancé ». | Corriger |
| V09 | BreachForums « actif ✓ 2022-2026 » | ✗ | Saisies multiples (dont oct. 2025), fuite de 323 988 comptes (janv. 2026), **aucune continuation authentifiée** ; Europol désigne **DarkForums** comme successeur. | Corriger |
| V10 | Forums listés : Exploit.in, XSS, BreachForums, RaidForums, Russian Market, 2easy, Genesis | ⚠ | Manquent : **RAMP** (saisi par le FBI, janv. 2026), **LeakBase** (saisi, « Operation Leak », mars 2026), **Nulled/Cracked** (« Operation Talent », janv. 2025), DarkForums. Exploit.in : actif, aucune saisie rapportée. Russian Market : actif, aucune action publique. | Compléter |
| V11 | RaidForums fermé « mai 2022 (Operation Tourniquet) » | ⚠ | Saisie par le FBI annoncée le **12 avril 2022**. | Corriger la date |
| V12 | Genesis Market fermé avril 2023 (Cookie Monster) | ✓ † | Conforme aux connaissances antérieures. | Sourcer |
| V13 | ANSSI : 1 366 incidents, 128 rançongiciels, éducation/recherche 34 % | ✓✓ ⚠ | Panorama 2025 publié le **11 mars 2026**. Le 34 % porte sur **tous les incidents**, pas sur les rançongiciels (ambigu au §2.3). Ajouts utiles : 2 209 signalements ; 3 586 événements (−18 %, effet JO 2024) ; 196 vols de données sans chiffrement (130 en 2024) ; **65 % des revendications d'extorsion = bluff ou recyclage** (DG de l'ANSSI) ; collectivités = 11 % des victimes de rançongiciel (17 % en 2024). | Préciser |
| V14 | ENISA 2026 : 73 % entités NIS2 ; admin. publique 32 % ; ransomware = impact court terme le plus élevé | ✓✓ ⚠ | Publié le **22 septembre 2026** ; 8 257 incidents (année 2025) ; 31,8 % admin. publique. Mais **51 % des incidents sont des DDoS de faible impact** et \~82 % des événements visant l'admin. publique sont des **DDoS hacktivistes** : le « 32 % » reflète surtout l'hacktivisme, pas la cybercriminalité. « 40 % des événements financiers = rançongiciel » : ? | Nuancer |
| V15 | Europol IOCTA 2026 (citations, typologie RaaS ouvert/semi-fermé/fermé) | ✓ ⚠ | IOCTA 2026 publié le **28 avril 2026** (11e édition), titre « how encryption, proxies and AI are expanding cybercrime », thème du « velocity gap ». Citations littérales et typologie non relues dans le PDF ; RansomHub cité comme « ouvert » alors qu'arrêté en 2025. | Relire le PDF |
| V16 | GTG-1002 / MITRE C0062 | ✓✓ ⚠ | Campagne C0062 v1.0 existe ; septembre 2025 ; **acteur étatique probable (Chine), espionnage, \~30 cibles, succès limités** : ce n'est pas un acteur du marché criminel. Chiffres 80–90 % / 4–6 décisions issus du rapport Anthropic (nov. 2025). T1136.001 et T1567 confirmées ; T1587.004 : ? (la page cite T1588.002 pour l'obtention d'outils). | Reclasser (hors marché) |
| V17 | Bitsight : MONST3R-AI ; mentions de rançongiciels −49 % | ✓ ? | Billet Bitsight du **15 octobre 2026** : 16 forums, fév.–août 2026, mentions d'agents IA / injection de prompt indirecte / MCP **+67 %** ; MONST3R-AI vendu sur forum restreint, clés volées OpenAI/Anthropic/Google, plafond de budget, aucun modèle hébergé. Chiffres du vendeur (1 072 trouvailles, 199 hôtes) **non vérifiés**. « −49 % » non retrouvé et en tension avec les records de publications sur sites de fuite (2 830 victimes au T3 2026). | Séparer « discussion forum » et « revendications » |
| V18 | Google/Mandiant : LLMjacking, LLM Mixers, factures jusqu'à 1 M$ | ✓✓ | Rapport GTIG/Mandiant du **2 novembre 2026** : \~1 M$ chez une victime ; >100 k$/jour ; Jet Night (>500 000 appels en <8 h, plan « illimité » ≈60 $/mois) ; UNC6993 ; outils new-api et gcli2api ; clés testées par requêtes à 1 token. | Intégrer |
| V19 | Bizarre Bazaar situé en 2025 (voile 5) | ⚠ | Rapport Pillar Security du **28 janvier 2026** (campagne déc. 2025–janv. 2026) ; absent du corps du rapport. | Développer, dater 2026 |
| V20 | TeamPCP : chronologie | ✓✓ ⚠ | Dates majeures conformes. Compléments : 71 clients Europa touchés (42 internes + ≥29 autres entités de l'Union) ; données publiées par ShinyHunters le 28 mars ; volume \~340 Go (CERT-EU/Help Net) vs 92 Go compressées (presse) ; CVE-2026-33634 (Trivy) ; Mandiant : 1 000+ environnements SaaS ; annonce d'un partenariat avec le groupe Vect (Akamai). « Début mars, incident Aqua » : ? | Compléter |
| V21 | ClawHub : 341 skills malveillants | ✓✓ ⚠ | Koi Security, fév. 2026 : 2 857 skills audités, 341 malveillants dont 335 d'une même campagne (« ClawHavoc »), \~12 % du registre ; AMOS (stealer macOS) ; VirusTotal puis NVIDIA (1er juin) ; Unit 42 : 5 skills non bloqués fév.–mai 2026. Billets DEV Community aux chiffres incompatibles : à écarter. | Citer Koi/Unit 42 |
| V22 | DBSC (contre-mesure) | ✓✓ | Disponibilité générale sur Chrome 146 Windows (avril 2026), annonce Workspace du 28 mai 2026 ; macOS à venir ; adoption côté serveurs nécessaire. | Indicateur C |
| V23 | NIS2 : « transposition en cours d'harmonisation (fin 2026) » | ⚠ | Échéance du 17 oct. 2024 manquée ; Sénat 12 mars 2026 ; **saisine de la CJUE par la Commission le 8 juillet 2026** ; **Assemblée nationale 17 nov. 2026 (464 voix contre 64)** ; commission mixte paritaire possible ; texte couvrant NIS2, REC et adaptation DORA ; promulgation non intervenue au 22 nov. | Préciser |
| V24 | DORA (janv. 2025), RGPD art. 33-34, CP art. 323-1/323-3 | ✓ † | Exacts ; ajouter art. 323-2 (entrave) et 323-3-1 (outils, avec réserve « motif légitime ») † ; recel (art. 321-1) † pour la détention de données volées. | Faire valider (juriste) |
| V25 | Coût de développement d'un stealer : 50 000 $ → 500 $ | 🟡 ? | Non sourcé (le rapport le reconnaît). | Garder en 🟡 ou supprimer |
| V26 | Prix : stealer 200–400 $/mois, loader 2 000–10 000 $, IAB 500–50 000 $, RaaS \~20 % « Europol » | ⚠ ? | Points d'ancrage retrouvés : StealC ≈200 $/mois ; log ≈10 $ (Russian Market) ; plan LLM « illimité » ≈60 $/mois ; AMOS 500–1 000 $/mois (Koi). « \~20 % Europol » : non retrouvé. | Sourcer chaque prix |
| V27 | Économie criminelle numérique > 1 900 Md$ (Taylor & Francis) | ? | Non revérifié. | Ordre de grandeur seulement |
| V28 | QakBot « Duck Hunt » : 200 serveurs | ? † | Non revérifié. | Sourcer (DOJ) |
| V29 | Binance (programme et exercice E3 : « post-mortem ») | ✓✓ (absent du corps) | Vol d'environ **6,3 Md$** le 29 octobre 2026 ; post-mortem publié le 3 novembre : chaîne partie de **juin 2026** (installateur Homebrew falsifié pour installer un outil de codage IA, faux entretien d'embauche, outil IA interne, service de signature) ; attribution FBI à la Corée du Nord (rapportée). | Rédiger (§3.6) |
| V30 | Endgame de novembre 2026 (Gootloader, CastleRAT, StealC : 235 serveurs, 257 domaines, 6,4 M d'identifiants) | ✓ (source C) | Communiqué Europol du **12 novembre 2026** rapporté par un agrégateur ; à recouper sur Europol. | Intégrer après recoupement |

### 1.4 Erreurs structurelles (cohérence interne)

| ID | Défaut | Conséquence |
| --- | --- | --- |
| S1 | Sept « voiles » annoncés ; les voiles 3 (LLM criminels), 4 (LLMjacking), 5 (Bizarre Bazaar), 6 (MCP) n'ont **aucune section dédiée** | Le prologue promet ce que le corps ne livre pas |
| S2 | Numérotation en collision : §1.1 existe en Partie I et en Acte I ; Parties I–VII et Actes I–VII s'entremêlent (Actes VI-VII logés dans la Partie II) | Renvois ambigus |
| S3 | Note de révision : trois recommandations ; les titres des Actes VI et VII citent « recommandation 5 » et « recommandation 2 » | Traçabilité brisée |
| S4 | Scénarios S1–S4 (§4.5) puis A–E (Acte VII) : S3 et S4 disparaissent sans explication, S2 devient E | Perte d'information |
| S5 | Exercices et glossaire renvoient à des contenus absents : « cinq surfaces d'accès », post-mortem Binance, modèle de menace MCP, pickle/safetensors, honeytokens détaillés, « triptyque dangereux » | Matériel pédagogique orphelin |
| S6 | Sources commerciales : « voir édition initiale pour les URL » | **Aucune URL** : rien n'est vérifiable depuis ce document |
| S7 | Validation circulaire : les recommandations d'un LLM sont « validées comme pertinentes » par le même type d'outil | Faux sentiment de contrôle |
| S8 | Marqueur ✓ appliqué à des statuts faux (forums) | La balise perd toute valeur |
| S9 | Couches éditoriales (métaphores, annotations, épilogue) mêlées aux faits | Difficile à exploiter en veille |

### 1.5 Lacunes de couverture

| ID | Thème absent ou sous-traité | Pourquoi c'est important | Traité en |
| --- | --- | --- | --- |
| L1 | Hacktivisme / DDoS et convergence étatique-criminelle | 51 % des incidents ENISA ; « brouillard » ANSSI | §4.2 |
| L2 | Extorsion sans chiffrement, groupes d'ingénierie sociale (ShinyHunters) | ANSSI : 196 incidents ; Coveware T2 2026 | §2.5 |
| L3 | RPDC, faux entretiens, outils IA piégés (Binance) | Plus gros vol crypto de l'histoire | §3.6 |
| L4 | Bizarre Bazaar ; 175 000 hôtes Ollama exposés | Surface « IA exposée » | §3.2 |
| L5 | LLM criminels (WormGPT et successeurs) † | Voile 3 non écrit | §3.1 |
| L6 | Skills et serveurs MCP piégés | Chaîne d'approvisionnement agentique | §3.4 |
| L7 | SaaS et dépendances (Vercel avr. 2026, vers npm †) | Rayon d'explosion | §3.5 |
| L8 | ClickFix / faux CAPTCHA, malvertising | Vecteur d'accès des stealers 2026 | §2.4 |
| L9 | Proxys résidentiels, chiffrement, crypto | Thèmes de l'IOCTA 2026 | §4.3 |
| L10 | Vulnérabilités des équipements de bordure, n-day exploités vite | Accès initial dominant côté étatique/criminel | §4.2 |
| L11 | Fraude en ligne (« domaine criminel à plus forte croissance » selon Europol) | Hors périmètre malware mais même économie | §4.3 |
| L12 | Assurance : exclusions du vol de ressources, conditions LOPMI † | Qui paie la facture d'IA | §4.5 |
| L13 | Mesure du marché : forums ≠ sites de fuite ≠ logs | Évite les contresens (−49 % vs record) | §5.4 |
| L14 | Droit de la veille elle-même (détention, collecte, RGPD) | Risque juridique du projet | §5.8 |

### 1.6 Recommandations de réécriture

1. **Deux éditions** : « narrative » (pédagogie, ton conservé) et « de travail » (ce dossier). Seule la seconde sert de base au projet.
2. **Registre de preuves** : une ligne par affirmation (ID, énoncé, source, URL, date de publication, date de relecture, fiabilité A–E, double source oui/non, relecteur). Le marqueur ✓ n'est apposé qu'à partir de la double source.
3. **Remplacer les parts de marché** par des classements **par métrique** et par **source**, datés.
4. **Séparer les trois mesures du marché** : discussions sur forums, revendications sur sites de fuite (non vérifiées), observations de télémétrie (machines, identifiants).
5. **Revue de péremption** : toute entrée du registre porte une durée de vie (forums : 1 mois ; groupes ransomware : 1 mois ; prix : 1 mois ; textes juridiques : 3 mois ; cadre analytique : 12 mois).
6. **Revue croisée humaine** de tout contenu généré ou « validé » par un modèle, avec relecture de la source primaire pour chaque chiffre publié.

---

## 2. Base de connaissances actualisée (état au 22 novembre 2026)

### 2.1 Modèle d'écosystème et « cinq surfaces d'accès »

Le rapport source parle de « cinq surfaces d'accès » dans son programme de formation sans jamais les définir (défaut S5). **Proposition de reconstruction** (🟡 à valider par l'équipe) :

| # | Surface d'accès | Ce que l'attaquant vise | Exemples 2026 documentés |
| --- | --- | --- | --- |
| 1 | **Poste et navigateur** | Logs de stealers : mots de passe, cookies de session, jetons, portefeuilles | Vidar 2.0, Lumma, StealC ; faux CAPTCHA/ClickFix ; installateurs piégés |
| 2 | **Identité et sessions** | Jetons SSO rejouables, OAuth, comptes de service | Rejeu de cookies ; contre-mesure DBSC |
| 3 | **Code, CI et dépendances** | Secrets de pipeline, comptes de publication de paquets | TeamPCP (Trivy, KICS, LiteLLM, Telnyx) ; Homebrew falsifié (Binance) |
| 4 | **Cloud et capacités d'IA** | Clés d'API, jetons IAM, endpoints LLM exposés | LLMjacking (Google/Mandiant) ; Bizarre Bazaar ; 175 000 hôtes Ollama exposés |
| 5 | **Agents et outils** | Droits d'un agent, serveurs MCP, skills | ClawHavoc (341 skills) ; MONST3R-AI ; GTG-1002 |

**Chaîne de valeur** (inchangée sur le fond) : auteur de malware (abonnement) → distributeur (forum, canal Telegram, boutique) → opérateur de logs (tri, qualification) → courtier d'accès → exploitant (ransomware, fraude, espionnage) → blanchiment. **Services transverses** : hébergement « bulletproof », proxys résidentiels, crypters, cash-out. **Institutions invisibles** : réputation, escrow, hébergement tolérant. La saisie de XSS.is (juillet 2025) rappelle que la fonction d'escrow et de confiance, plus que n'importe quel produit, rendait ce forum structurellement important.

### 2.2 Chronologie corrigée 2019 → novembre 2026

| Date | Événement | Statut |
| --- | --- | --- |
| 2019 | Généralisation de la double extorsion (chiffrement + fuite) | 🔵 † |
| 12 avr. 2022 | RaidForums saisi (FBI) | ✓ |
| 2022 | Apparition de LockBit 3.0 | † |
| avr. 2023 | Genesis Market saisi (« Cookie Monster ») | † |
| juil. 2023 | WormGPT puis FraudGPT : premiers « LLM criminels » commerciaux | † |
| août 2023 | QakBot (« Duck Hunt ») | † |
| fév. 2024 | LockBit perturbé (« Cronos ») | † |
| mai 2024 | Endgame, phase 1 (loaders/droppers) ; Sysdig nomme le **LLMjacking** | † / ✓ |
| oct. 2024 | RedLine/META perturbés (« Magnus ») | † |
| janv. 2025 | Nulled et Cracked saisis (« Operation Talent ») | ✓ |
| fév.–avr. 2025 | 8Base, puis RansomHub : fin d'activité | † |
| mai 2025 | **Lumma perturbé** (Microsoft, DOJ, Europol ; \~2 300 domaines) ; retour en quelques semaines | ✓✓ |
| 22 juil. 2025 | **XSS.is** : arrestation présumée de l'administrateur, saisie du domaine | ✓✓ |
| juil. 2025 | BlackSuit démantelé | † |
| sept. 2025 | **GTG-1002** (premier cas documenté d'intrusion largement exécutée par un agent) | ✓✓ |
| oct. 2025 | BreachForums de nouveau saisi | ✓ |
| nov. 2025 | **Endgame, phase 3 : Rhadamanthys** (1 025 serveurs) ; Anthropic publie le rapport GTG-1002 (13 nov.) | ✓✓ |
| déc. 2025–janv. 2026 | **Bizarre Bazaar** (35 000 sessions d'attaque en \~40 jours) ; publication Pillar Security le 28 janv. | ✓✓ |
| janv. 2026 | **RAMP** saisi par le FBI ; fuite de 323 988 comptes BreachForums | ✓ |
| 3 fév. 2026 | **ClawHavoc** : 341 skills malveillants sur ClawHub (Koi Security) | ✓✓ |
| mars 2026 | ANSSI Panorama (11), Sénat NIS2 (12), **LeakBase** saisi, **TeamPCP** (19–27 mars) | ✓✓ |
| avr. 2026 | IOCTA (28) ; DBSC en disponibilité générale sur Chrome 146 Windows (9) ; CERT-EU publie l'analyse de la brèche Commission (2–3) ; Vercel : incident SaaS | ✓ |
| mai 2026 | DBSC GA Workspace (28) ; nouvelles clés Gemini liées à un compte de service et restreintes par défaut (28) | ✓ / C |
| juin 2026 | ClawHub–NVIDIA (1er) ; **Endgame (15–19, annoncé le 24)** : SocGholish, Amadey, StealC ; compromission initiale chez Binance (installateur falsifié) | ✓✓ |
| 8 juil. 2026 | Saisine de la CJUE contre la France (NIS2) | ✓ (C) |
| août 2026 | Mois record de publications sur sites de fuite (1 223) | ✓ (C) |
| 22 sept. 2026 | **ENISA Threat Landscape 2026** | ✓✓ |
| 15 oct. 2026 | **Bitsight : MONST3R-AI** | ✓✓ |
| 29 oct. 2026 | **Vol Binance (\~6,3 Md$)** | ✓✓ |
| 2 nov. 2026 | **Google/Mandiant : LLMjacking et LLM mixers** | ✓✓ |
| 3 nov. 2026 | Post-mortem Binance | ✓✓ |
| 12 nov. 2026 | Endgame (Gootloader, CastleRAT, StealC) : 235 serveurs, 257 domaines | ✓ (C) |
| 17 nov. 2026 | **Assemblée nationale : loi Résilience (NIS2) adoptée en première lecture** | ✓✓ |

### 2.3 Ransomware : le paysage 2026

**Volumes (revendications sur sites de fuite, non vérifiées)** : 9 663 victimes listées du 1er janvier au 11 novembre 2026, soit **+32 %** sur la même période de 2025 (ransomnews). Check Point : record de **2 830 victimes au T3 2026**, **117 marques** actives, les dix premières ne pesant plus que 49 % des victimes (première fois que la majorité est hors du top 10). Mois le plus chargé : août (1 223).

| Groupe | Position 2026 | Données | Remarque |
| --- | --- | --- | --- |
| **Qilin** | n°1 (leak sites) | 1 165 publications, 12,1 % (au 11 nov.) ; 1 058 en 2025 (13,0 %) | Leader depuis 2025 ; 195 revendications industrielles au T3 (Dragos) |
| **The Gentlemen** | n°2, dépasse Qilin certains mois | 1 084 publications ; 383 au T3 selon un suivi trimestriel (Qilin 363) | Scission récente ; surtout industrie |
| **Akira** | n°3 | 439 publications ; **22,6 % des cas attribués** (Sophos) | Premier en réponse à incident ; baisse sur sites de fuite au T3 industriel (129 → 52) |
| **INC Ransom**, **DragonForce**, **SafePay**, **Play** | top 10 | INC 384, DragonForce 329 (ransomware.live, 25 oct.) | Cartel annoncé LockBit + Qilin + DragonForce (sept. 2025) |
| **LockBit 5.0** | en recul | 287 publications (3,0 %) ; sort du top 10 au T3 (Check Point) | Reprise post-Cronos ; fuite de base de données en mai 2025 |
| **Lone Wolf, ShinyHunters** | cas d'IR (Coveware T2 2026) | Lone Wolf 17 %, ShinyHunters 12 %, Akira 9 %, The Gentlemen 8 % | Extorsion sans chiffrement et acteurs isolés sous-représentés sur les sites de fuite |

**Lecture (🟡)** : le classement change selon la **métrique** (publications sur sites de fuite, cas de réponse à incident, plaintes au FBI, victimes industrielles). Le FBI IC3 recense 63 nouvelles variantes en 2025 ; ses dix plus signalées (Akira, Qilin, INC/Lynx/Sinobi, BianLian, Play, RansomHub, LockBit, DragonForce, SafePay, Medusa) = 56,8 % des incidents rapportés. Aucun groupe n'est donc « le » leader : il faut suivre **au moins trois mesures**.

**Groupes du tableau source devenus obsolètes** : RansomHub, BlackSuit, 8Base (†, fin 2025) ; LockBit 3.0 remplacé par 5.0. **Tactiques** : extorsion multiple confirmée (chiffrement, vol, fuite, pression sur clients, DDoS) ; **ENISA** y voit un basculement structurel vers la menace de divulgation.

### 2.4 Stealers, loaders et vecteurs d'accès

| Famille | Rôle | Statut au 22/11/2026 | Source |
| --- | --- | --- | --- |
| **Vidar 2.0** | Stealer | Le plus utilisé depuis nov. 2025 ; réécriture complète, multithread ; cible 200+ extensions de navigateur (source C) | Intrinsec, Flashpoint, secure.com |
| **Lumma (LummaC2)** et clones (Remus) | Stealer | Perturbé mai 2025, revenu à grande échelle ; doxxing et compromission Telegram ont érodé la confiance | Microsoft, Trend Micro, Bitdefender, AhnLab |
| **StealC** | Stealer | Endgame juin 2026 puis nov. 2026 ; 73 grappes de clients identifiées par ESET | Europol, ESET |
| **Rhadamanthys** | Stealer | Perturbé nov. 2025 (Endgame) | Europol |
| **RedLine / META, Raccoon** | Stealers | Perturbés (oct. 2024 †) | † |
| **Acreed, ACRStealer** | Stealers | Poussée brève après les démantèlements | DeepStrike, AhnLab |
| **AMOS** | Stealer macOS | 500–1 000 $/mois ; distribué via skills ClawHub | Koi, Trend Micro |
| **Amadey** | Loader/dropper | Actif depuis oct. 2018 ; 53 grappes d'affiliés ; Endgame juin 2026 | Europol, ESET |
| **SocGholish** | Loader (faux updates) | Endgame juin 2026 ; \~14 971 sites compromis remédiés | Europol (via presse) |
| **Gootloader, CastleRAT** | Loader / RAT | Endgame nov. 2026 (source C) | Europol (via agrégateur) |

**Vecteurs de distribution 2026** : faux CAPTCHA/ClickFix, logiciels piratés hébergés sur des services légitimes (mega.nz, Amazon S3, MediaFire — AhnLab, juil. 2026), fausses mises à jour, installateurs piégés, hameçonnage, **canaux Telegram « cloud »** qui diffusent et vendent des logs (Intrinsec). **Ampleur** : Flashpoint compte 7,4 M d'appareils infectés et 1,7 Md d'identifiants volés au premier semestre 2026 (chiffre relayé, source C). **Prix** : log ≈ 10 $ sur Russian Market (Flare), abonnement StealC ≈ 200 $/mois.

**Anatomie d'un log (version 2026, structure défensive)** : navigateur (mots de passe, cookies, historique, extensions) ; **outils de développement et d'IA** (clés d'API, configurations d'outils en ligne de commande, variables d'environnement, jetons GitHub) ; VPN et messageries professionnelles ; portefeuilles ; métadonnées machine servant à **qualifier** l'accès. Contre-mesure structurelle : liaison des sessions à l'appareil (DBSC) et secrets éphémères.

### 2.5 Extorsion sans chiffrement et ingénierie sociale

- **ANSSI 2025** : 196 incidents de vol de données sans chiffrement (130 en 2024) ; 128 compromissions par rançongiciel ; rançongiciel en léger recul. Le DG de l'ANSSI indique que **65 % des revendications de vol étaient du bluff ou du recyclage** de données déjà publiques.
- **ShinyHunters** : a publié les données de la Commission européenne (28 mars 2026) ; 12 % des cas Coveware au T2 2026 ; a déclaré en avril 2026 qu'aucune version légitime de BreachForums n'existe (source C).
- **Conséquence pour la veille** : chaque revendication doit passer par un **flux de vérification** (§5.9) avant d'être qualifiée d'incident.

### 2.6 Forums et marchés : statuts au 22/11/2026

| Plateforme | Statut | Détail | Fiabilité |
| --- | --- | --- | --- |
| **Exploit.in** | Actif | Aucune saisie rapportée ; cité dans un communiqué DOJ d'août 2026 | B |
| **XSS.is** | Perturbé | Admin présumé arrêté (22 juil. 2025), domaine saisi ; relances/onion rapportés | A/C |
| **BreachForums** | Fragmenté | Saisies multiples (dont oct. 2025) ; clones, usurpations ; aucune version authentifiée | B/C |
| **DarkForums** | Actif | Désigné par Europol comme successeur de BreachForums | B |
| **RAMP** | Saisi | FBI, janv. 2026 | B |
| **LeakBase** | Saisi | « Operation Leak », mars 2026 | C |
| **Nulled, Cracked** | Saisis | « Operation Talent », janv. 2025 | C |
| **Russian Market** | Actif | Aucune action publique ; Vidar y est n°1 | B/C |
| **Dread** | Actif | Plus grand forum encore en ligne selon un suivi d'oct. 2026 | C |
| **Canaux Telegram** | Actifs | Distribution de logs en « cloud » ; migration des utilisateurs depuis XSS/BreachForums vers des espaces plus petits sur invitation (IOCTA 2026) | B |

**Règle de veille** : un statut de forum a une durée de vie d'environ un mois ; il ne s'écrit jamais sans date de contrôle ni source primaire (Europol, DOJ, bannière de saisie archivée).

### 2.7 Opérations de répression et effets observés

| Opération | Date | Cible | Résultat | Effet observé |
| --- | --- | --- | --- | --- |
| Cronos | fév. 2024 | LockBit | † | Retour sous forme de LockBit 5.0, parts érodées |
| Magnus | oct. 2024 | RedLine/META | † | Lumma devient n°1 |
| Talent | janv. 2025 | Nulled, Cracked | Saisies | Migration vers d'autres forums |
| Lumma | mai 2025 | Lumma | \~2 300 domaines | Retour en semaines (C2 : 1 → 450+) |
| Ratatouille | juil. 2025 | XSS.is | Admin arrêté, escrow (≈7 M€) | Migration vers espaces fermés |
| Endgame phase 3 | nov. 2025 | Rhadamanthys, VenomRAT, Elysium | 1 025 serveurs | Vidar 2.0 n°1 ; Flare : +41 % de volume à 90 jours (corrélation prudente) |
| RAMP, Leak | janv.–mars 2026 | Forums | Saisies | Fragmentation |
| **Endgame juin 2026** | 15–19 juin | SocGholish, Amadey, StealC | **326 serveurs, 142 domaines, \~27 M d'identifiants, >41 M€ de crypto** | À mesurer (indicateur §7.4) |
| Endgame nov. 2026 | 12 nov. | Gootloader, CastleRAT, StealC | 235 serveurs, 257 domaines, 6,4 M d'identifiants | Source C |

**Enseignements** (🔵 corroborés par Flare sur \~44 M de logs, juin 2024 – juin 2026) : la plupart des démantèlements **déplacent** les opérateurs vers des familles de remplacement plutôt que de réduire le marché ; la **réputation survit** ; l'**infrastructure** compte plus que le code ; les **données récupérées** (millions d'identifiants) sont le bénéfice défensif le plus tangible — d'où l'intérêt d'une veille sur les listes de remédiation publiées par les autorités.

### 2.8 Prix : points d'ancrage retrouvés

| Produit | Valeur | Source | Fiabilité |
| --- | --- | --- | --- |
| Log individuel (Russian Market) | ≈ 10 $ | Flare via DeepStrike | C |
| Abonnement StealC | ≈ 200 $/mois | Recorded Future via Stingrai | C |
| Stealer macOS AMOS | 500–1 000 $/mois | Koi Security | B |
| Accès LLM « illimité » (Jet Night) | ≈ 60 $/mois | GTIG | B |
| Jetons revendus par mixers | 0,10–25 $ par million | Suivi trimestriel GTIG relayé | C |
| Offres grand public comparables | 100–250 $ | GTIG via SiliconANGLE | C |

Les prix du rapport source (IAB 500–50 000 $, loaders 2 000–10 000 $/mois, commissions 10–30 %) restent des ordres de grandeur commerciaux **non retrouvés** : ne pas les republier sans source.

### 2.9 Lois du marché révisées

1. **Résilience / déplacement** : chaque démantèlement déplace plus qu'il ne supprime (Flare, IOCTA, ENISA).
2. **Spécialisation** : plus le marché grossit, plus les métiers se spécialisent (stealer, trieur, courtier, exploitant, mixer).
3. **Confiance comme infrastructure** : l'escrow et la réputation valent plus que le code (XSS).
4. **Gresham souterrain** : les dumps abondants chassent les données rares (🟡).
5. **Loi de la métrique** *(nouvelle)* : le classement d'un acteur dépend de ce qu'on mesure ; une veille sérieuse publie toujours plusieurs mesures.
6. **Marché de la revendication** *(nouvelle)* : une partie notable des annonces d'extorsion est fausse ou recyclée (ANSSI : 65 % en 2025) ; la crédibilité est un attribut à instruire, pas un acquis.

---

## 3. IA, agents et chaîne d'approvisionnement : le cœur « nouveau » de 2026

### 3.1 Les « LLM criminels » (voile 3, non écrit dans le rapport source)

- 🔵 † **2023** : WormGPT puis FraudGPT sont vendus comme assistants « sans garde-fous ». Les successeurs et clones se multiplient ; la valeur réelle est incertaine.
- 🔵 **2026** : Sophos documente un marché où des personas neufs vendent des clés d'API et des chatbots « illimités », avec un **écart de connaissance** chez les acheteurs et des avis positifs de complaisance. Trellix décrit un outil de simulation d'attaque (« APEX AI ») vendu **comme logiciel** (chaque acheteur l'exécute lui-même), intégrant des sources OSINT (Shodan, DeHashed).
- 🔵 **IOCTA 2026** : l'IA générative aide surtout les fraudeurs à produire des escroqueries plus convaincantes et personnalisées. **Lecture** (🟡) : l'IA accélère et abaisse la barrière sur les techniques existantes ; elle ne crée pas, à ce stade, de nouvelle classe d'attaque.
- 🟡 **« Fausse IA »** : une partie de l'offre est une arnaque entre criminels (ancien scénario S4 du rapport source). À suivre comme indicateur de saturation du marché.
- 🔵 **Bitsight** (16 forums, févr.–août 2026) : mentions d'agents IA, d'injection de prompt indirecte et de MCP **+67 %**.

### 3.2 LLMjacking : de la curiosité à l'industrie

**Définition** : usage frauduleux de capacité d'inférence via des identifiants, clés ou endpoints d'autrui ; la facture arrive à la victime. Le terme a été posé par Sysdig en mai 2024.

| Étape | Ce qui est documenté | Source |
| --- | --- | --- |
| **Acquisition** | Clés dans des dépôts publics, jetons IAM, clés de compte de service, jetons GitHub (PAT), jetons de session actifs, **endpoints LLM exposés** (ports par défaut, API sans authentification, environnements de dev, serveurs MCP) | GTIG/Mandiant ; Pillar Security |
| **Validation** | Requêtes minimales (plafonnées à 1 token) pour tester qu'une clé est vivante | GTIG |
| **Abus** | Activation de services jamais utilisés par la victime (ex. Bedrock, API Gemini), création de clés supplémentaires, instances GPU, relèvement de quotas, schéma **« burn and rotate »** (comptes de service créés puis supprimés) | GTIG ; presse relayant le suivi T4 |
| **Monétisation** | Revente via des **LLM mixers** : passerelles qui agrègent des clés volées (outils open source **new-api**, **gcli2api**), plans « illimités » ≈ 60 USD/mois | GTIG ; DailyAI |
| **Coût victime** | ≈ **1 M USD** chez une victime ; **> 100 000 USD/jour** ; UNC6993 a dépassé 100 000 USD en peu de temps ; Jet Night : **> 500 000 appels en < 8 h** | GTIG/Mandiant (2 nov. 2026) |

**Bizarre Bazaar** (Pillar Security, 28 janv. 2026 ; Sysdig) : campagne attribuée à un acteur (alias *Hecker*, *Sakuya*, *LiveGamer101*), **35 000 sessions d'attaque en \~40 jours** (≈ 972/jour) contre des honeypots, chaîne en trois rôles — **scanner** (recherche de LLM/MCP exposés), **validateur** (silver.inc), **place de marché** (« NeXeonAI », accès unifié à 50+ modèles) — touchant 30+ fournisseurs. **Ollama** : SentinelLABS et Censys recensent \~**175 000 hôtes exposés**. Pillar observe du trafic d'attaque **dans les heures** suivant l'exposition d'un endpoint, puis un glissement de la revente vers la **reconnaissance des outils branchés** (passage vers l'agentique). **Sysdig** : +376 % de vol d'identifiants visant des services d'IA entre T4 2025 et T1 2026 (relayé par la CSA, source C).

**Contre-mesures structurelles côté fournisseurs** (🔵 C) : depuis le 28 mai 2026, les nouvelles clés Google AI Studio sont des **clés d'autorisation liées à un compte de service** et restreintes à l'API Gemini par défaut. **Recommandations GTIG** : plafonds de dépense, clés limitées par modèle et par environnement, surveillance des journaux pour détecter les requêtes à 1 token, MFA résistant à l'hameçonnage, secrets centralisés et tournants.

**Signaux de détection (défensifs) — à implémenter en §6**

| Signal | Donnée source | Logique | Gravité |
| --- | --- | --- | --- |
| Pic de consommation par clé | Facturation fournisseur, journaux d'inférence | Écart robuste à la base saisonnière (>3 MAD) ou ×3 en < 1 h | Haute |
| Service d'IA jamais utilisé soudain activé | Journaux d'audit cloud | Événement d'activation sans changement approuvé | Critique |
| Création en rafale de clés ou comptes de service puis suppression | Journaux IAM | N créations/suppressions en < 1 h | Critique |
| Requêtes à 1 token en série depuis IP inconnues | Journaux de l'API | Rafale de requêtes minimales, plusieurs clés | Haute |
| Clé utilisée depuis un ASN/pays inhabituel | Journaux de l'API | Écart au profil d'usage | Moyenne |
| Endpoint LLM/MCP joignable depuis Internet | Inventaire externe (scan de surface) | Port d'inférence ouvert sans authentification | Haute |
| Honeytoken d'IA utilisé | Clé factice posée sur postes et dépôts | Toute utilisation | Critique |

### 3.3 Agents offensifs : état des preuves

| Cas | Ce qui est établi | Nuance | Statut |
| --- | --- | --- | --- |
| **GTG-1002** (sept. 2025 ; rapport Anthropic du 13 nov. 2025 ; MITRE C0062) | Acteur **étatique probable (Chine)**, espionnage, \~30 cibles, **succès limités** ; l'agent a exécuté l'essentiel du travail tactique (reconnaissance, exploitation, mouvement latéral) avec peu de points de décision humains ; milliers de requêtes à une cadence impossible pour un humain | Anthropic note que des erreurs (informations inventées, identifiants non valides) restent un obstacle à l'autonomie totale ; le rapport source cite des chercheurs divisés sur la nouveauté | ✓✓ |
| **MONST3R-AI** (Bitsight, 15 oct. 2026) | Modèle **« apporte ta clé »** : l'acheteur colle des clés volées, fixe un budget, désigne des cibles ; le vendeur n'héberge aucun modèle | Chiffres de résultats du vendeur non vérifiés | ✓✓ |
| **Pipelines autonomes sur calcul volé** (CSA, juin 2026) | Premier cas où le calcul volé sert de **moteur de raisonnement** d'un pipeline offensif autonome | Note de recherche assistée par IA (source C) | ✓ (C) |
| **Exploitation à partir de correctifs** (suivi GTIG T4 2026, relayé) | Des attaquants transforment des différentiels de correctifs en exploits fonctionnels | Source secondaire, à relire dans le rapport GTIG | ? |

**Lecture (🟡)** : le signal fort n'est pas l'existence d'un « malware IA » mais la **compression de la fenêtre de détection** et le **financement par le vol** (le coût de l'agent est reporté sur la victime d'une clé volée). D'où le couplage direct entre veille sur les marchés de clés et FinOps.

### 3.4 Skills, MCP et agents comme chaîne d'approvisionnement

- **ClawHavoc** (Koi Security, 3 fév. 2026) : audit de **2 857 skills** ; **341 malveillants**, dont **335** issus d'une même campagne ; \~12 % du registre ; usurpation de noms (typosquats), fausses « prérequis » obligeant l'utilisateur à exécuter une commande ; vol de clés d'exchanges et de portefeuilles ; distribution d'**AMOS**. Le nombre a plus que doublé en 15 jours de recherche supplémentaire. Bitdefender estimait \~17 % de skills malveillants parmi ceux analysés les premières semaines. Contre-mesures : VirusTotal, puis NVIDIA (1er juin 2026). Unit 42 a encore trouvé **5 skills non bloqués** entre février et mai 2026.
- **Détournement sémantique** (Unit 42) : un skill malveillant exploite l'interprétation du langage naturel de l'agent et son contexte (système de fichiers, shell, gestionnaires d'identifiants) **sans exploit classique**.
- **Serveurs MCP** : 🔵 † un serveur MCP piégé (« postmark-mcp », 2025) a copié des courriels ; Pillar suit une campagne distincte de **reconnaissance de points d'accès MCP** ; Bitsight note la montée des mentions MCP en forum.
- **« Triptyque dangereux »** (défini ici, absent du rapport source) 🟡 : un agent cumulant (1) accès à des **données privées**, (2) exposition à des **contenus non fiables** (courriels, pages, fichiers), (3) capacité d'**agir ou communiquer vers l'extérieur** est exploitable par injection de prompt. **Règle de conception** : ne jamais réunir les trois dans le même agent ni le même jeton ; sinon, imposer une validation humaine sur les actions irréversibles.

| Contrôle | Objectif | Preuve de conformité |
| --- | --- | --- |
| Inventaire des agents, skills et serveurs MCP | Savoir ce qui tourne et avec quels droits | Registre à jour, propriétaire nommé |
| Compte et jeton **dédiés par agent** | Limiter le rayon d'explosion | Permissions énumérables |
| Permissions **éphémères et minimales** | Réduire la durée d'exposition | Durée de vie < 1 h pour actions sensibles |
| Revue et épinglage des skills | Éviter la mise à jour malveillante | Empreinte de contenu validée |
| Bac à sable | Contenir l'exécution | Isolation fichier/réseau |
| Journalisation des appels d'outils | Détection et forensics | Journaux centralisés |

### 3.5 Chaîne d'approvisionnement logicielle : TeamPCP et au-delà

| Date (2026) | Étape | Statut |
| --- | --- | --- |
| 19 mars | Versions de **Trivy** compromises ; la CI de la Commission les exécute, une clé AWS à droits étendus est exfiltrée (CVE-2026-33634) | ✓✓ |
| 23 mars | **Checkmarx KICS** compromis avec des identifiants volés auparavant | ✓ |
| 24 mars | **LiteLLM** : versions malveillantes sur PyPI (1.82.7 et 1.82.8 †) ; le SOC de la Commission voit des appels AWS anormaux (5 jours après l'accès) | ✓ / † |
| 25 mars | CERT-EU informé ; clé révoquée, accès créés par l'attaquant désactivés | ✓✓ |
| 27 mars | **SDK Telnyx** sur PyPI ; annonce publique de la Commission | ✓ |
| 28 mars | **ShinyHunters** publie les données (≈ 340 Go selon CERT-EU/Help Net ; 92 Go compressées selon la presse) | ✓ |
| 2–3 avril | Billet CERT-EU : **71 clients** du service Europa touchés (42 internes + ≥ 29 autres entités de l'Union) ; Mandiant : **1 000+ environnements SaaS** dans la campagne | ✓✓ |

**Leçons (🔵 recommandations CERT-EU)** : mettre à jour vers une version saine, **faire tourner tous les secrets** (pas seulement révoquer), auditer les versions d'outils en CI, **épingler les actions GitHub par empreinte de commit**. **Leçons (🟡)** : un outil de sécurité exécuté en CI a les droits du déploiement ; une passerelle LLM concentre clés d'IA, clés cloud et secrets d'orchestration ; la latence de détection (5 jours) reste la variable critique. **Compléments** : ransomware **Vect** annoncé comme partenaire de TeamPCP (Akamai) ; **Vercel** (avril 2026) : revendication de vente de données internes (clés, jetons npm/GitHub) pour \~2 M USD, présentée par l'acteur comme tremplin de supply chain (source C, **revendication non vérifiée**) ; † vers npm de 2025.

### 3.6 Le cas Binance (29 octobre 2026) : une chaîne de bout en bout

| Phase | Fait rapporté | Statut |
| --- | --- | --- |
| **Juin 2026** | Un ingénieur DevOps des portefeuilles cherche un **outil de codage IA** sur un moteur de recherche et installe un **installateur Homebrew falsifié** | ✓ (post-mortem Binance, 3 nov.) |
| **Été** | Faux entretien d'embauche ; l'équipe de sécurité détecte une anomalie, remplace l'appareil et fait tourner les identifiants, mais les attaquants **avaient déjà** de quoi poursuivre | ✓ (C) |
| **Automne** | Abus d'un **outil d'IA interne** ; atteinte du **service de signature** des portefeuilles | ✓ |
| **29–30 oct.** | Retraits non autorisés supérieurs à 1 Md USD détectés, puis **≈ 6,3 Md USD** au total (plus gros vol crypto connu ; Bybit : 1,46 Md USD en févr. 2025) ; Binance indique **aucune clé privée de portefeuille divulguée** et couvre les pertes (98 % reconstitués au 4 nov.) | ✓✓ |
| **Attribution** | FBI : Corée du Nord (rapporté) | ✓ (C) |

**Enseignements pour la veille** : (1) les **moteurs de recherche et installateurs d'outils IA** sont un vecteur d'accès initial ; (2) la **rotation après incident** ne suffit pas si l'attaquant a déjà établi d'autres points d'appui ; (3) les **outils d'IA internes** deviennent des chemins de latéralité ; (4) la **séparation des tâches** autour des opérations irréversibles est un contrôle critique — c'est exactement la question 8 du « test express » du rapport source (qui peut valider une transaction irréversible, depuis quel poste ?).

### 3.7 Cartographie des cadres d'analyse (mise à jour)

| Menace | MITRE ATT&CK | OWASP LLM Top 10 (2025) | Contrôle principal |
| --- | --- | --- | --- |
| Vol de cookies de session | T1539 | — | DBSC, sessions courtes |
| Comptes cloud valides (clés volées) | T1078.004 | LLM02 | Rotation, plafonds, IP |
| Détournement de ressources (LLMjacking) | T1496 | LLM10 | Plafonds de dépense, alertes de coût |
| Compromission de la chaîne logicielle | T1195.002 | LLM03 | Épinglage, provenance |
| Autonomie excessive d'un agent | — | LLM06 | Permissions minimales, validation humaine |
| Injection de prompt indirecte | — | LLM01 | Isolation des contenus, triptyque |

---

## 4. Institutions, statistiques et droit

### 4.1 Les ancrages institutionnels (vérifiés le 22/11/2026)

| Source | Publication | Faits clés | Usage veille |
| --- | --- | --- | --- |
| **ANSSI** | Panorama de la cybermenace 2025 (11 mars 2026) | 2 209 signalements ; 1 366 incidents (1 361 en 2024) ; 128 rançongiciels ; 196 vols de données sans chiffrement ; éducation/recherche 34 %, ministères/collectivités 24 %, santé 10 %, télécoms 9 % ; 65 % des revendications = bluff/recyclage ; espionnage en hausse | Base française, calibrage des revendications |
| **ENISA** | Threat Landscape 2026 (22 sept. 2026) | 8 257 incidents (2025) ; 73 % entités NIS2 ; admin. publique 31,8 % ; DDoS 51 % ; \~82 % des événements admin. = DDoS hacktivistes ; rançongiciel = impact court terme le plus élevé ; convergence criminels/hacktivistes/étatiques ; dépendances | Contexte UE, NIS2 |
| **Europol** | IOCTA 2026 (28 avr. 2026) | « Encryption, proxies and AI » ; fraude = domaine à plus forte croissance ; crypto ; « velocity gap » ; migration des forums vers des espaces sur invitation | Tendances, répression |
| **Europol** | Communiqués Endgame (24 juin, 12 nov. 2026) | Chiffres d'infrastructure et d'identifiants | Indicateurs de répression |
| **CERT-EU** | Billet Trivy / Commission (avril 2026) | Chronologie et recommandations | Cas d'école supply chain |
| **MITRE** | Campagne C0062 (v1.0) | Techniques de GTG-1002 | Cartographie |

### 4.2 Hacktivisme, étatique et convergence (lacunes L1 et L10)

ENISA relève que cybercriminels, hacktivistes et acteurs étatiques **s'appuient de plus en plus sur les mêmes outils**, les objectifs restant distincts ; l'ANSSI parle d'un « brouillard » entre acteurs étatiques et cybercriminels. **Implication** : la veille ne doit pas cloisonner « malware criminel » et « menace étatique » (cf. GTG-1002, RPDC/Binance). Pour la France, l'ANSSI note aussi que les compromissions d'équipements exposés et la **gestion des vulnérabilités** occupent largement ses équipes d'intervention.

### 4.3 Europol IOCTA 2026 : lecture

Trois mots structurent le rapport : **chiffrement** (E2EE, protocoles décentralisés), **proxys** (réseaux résidentiels qui compliquent la détection) et **IA** (fraudes plus convaincantes). Europol décrit un « velocity gap » : les criminels avancent plus vite que les capacités de réponse. Les DDoS y sont qualifiés de menace hybride. **À relire dans le PDF** : la typologie RaaS (ouvert / semi-fermé / fermé) et les citations reprises par le rapport source.

### 4.4 Cadre juridique France/UE

| Sujet | État au 22/11/2026 | Statut |
| --- | --- | --- |
| **NIS2 (France)** | Échéance du 17 oct. 2024 manquée ; Sénat 12 mars 2026 ; saisine CJUE 8 juil. 2026 ; **Assemblée 17 nov. 2026 (464–64)** ; CMP possible ; texte « Résilience » couvrant NIS2, REC et adaptation DORA ; \~15 000 organisations concernées (presse) | ✓✓ |
| **NIS2 (obligations)** | Gestion des risques, notification des incidents significatifs (alerte précoce 24 h, notification 72 h, rapport final à un mois †), responsabilité des organes de direction | † |
| **RGPD** | Notification à la CNIL sous 72 h (art. 33) ; information des personnes si risque élevé (art. 34) | ✓ † |
| **DORA** | Applicable depuis janv. 2025 ; délais d'incident spécifiques | ✓ † |
| **Code pénal** | Art. 323-1 (accès/maintien frauduleux), 323-2 (entrave), 323-3 (données), 323-3-1 (outils, avec réserve de « motif légitime, notamment de recherche ou de sécurité informatique »), recel (art. 321-1) | † à faire valider |
| **Cyber Resilience Act** | Obligations de signalement de vulnérabilités applicables à partir de septembre 2026 (†) ; principales obligations en 2027 (†) | † |
| **Responsabilité en cas de clé volée** | Analyse doctrinale : l'usage d'une clé valide est traité comme consommation légitime ; pas de jurisprudence consolidée sur le LLMjacking | 🟡 |

### 4.5 Assurance et conformité

- 🔵 Couverture habituelle : remédiation, perte d'exploitation, rançon (plafonnée ou exclue), sanctions. **Exclusions fréquentes** : vol de ressources sans atteinte aux données (cryptojacking, LLMjacking), actes de guerre, défaut de maintenance.
- 🟡 Un suivi sectoriel (source C) signale des assureurs qui **plafonnent** la facture d'IA ; à vérifier auprès du courtier.
- † **LOPMI (2023)** : remboursement d'une rançon conditionné au dépôt de plainte dans un délai de 72 h.
- 🔵 **ANSSI** recommande de ne pas payer (le paiement ne garantit pas la récupération, finance les attaquants, signale la disposition à payer).
- 🟡 « Paradoxe du phare » (la conformité attirerait les attaques) : **non démontré** ; ENISA 73 % = entités essentielles ou importantes, et l'admin. publique est surtout visée par du DDoS hacktiviste.
- **Action** : faire cartographier par la direction juridique, l'assureur et le DPO les clauses touchant la facture d'IA, avant le premier incident.

---

## 5. Projet de veille : cahier de lancement métier

### 5.1 Finalité et objectifs

**Finalité** : donner à l'entreprise une capacité durable à **anticiper, détecter et réduire** l'impact des menaces issues du marché souterrain (accès vendus, logs, rançongiciels, LLMjacking, supply chain, agents), en transformant l'information ouverte en **décisions** (priorités de correction, arbitrages budgétaires, exercices, communication).

| # | Objectif | Résultat attendu à 6 mois | Mesure |
| --- | --- | --- | --- |
| O1 | **Détecter l'exposition propre** (identifiants, clés, sessions, domaines, dépôts) avant exploitation | Alerte sur 100 % des clés d'IA et secrets de l'entreprise retrouvés hors périmètre | Délai de détection, délai de révocation |
| O2 | **Maîtriser le coût de l'IA comme indicateur de compromission** | Tableau de bord de coûts par clé/projet + alertes | Écart détecté vs facturé |
| O3 | **Surveiller la chaîne d'approvisionnement** (dépendances, CI, skills, MCP) | Inventaire et épinglage des composants critiques | % de composants épinglés |
| O4 | **Produire du renseignement actionnable** (hebdo, mensuel, flash) | Bulletins livrés à l'heure, retours des lecteurs | Taux d'actions déclenchées |
| O5 | **Mesurer la prospective** (scénarios A–E) | Indicateurs alimentés automatiquement | Complétude des indicateurs |
| O6 | **Capitaliser** (base de connaissances et registre de preuves) | Entités et revendications tracées | % d'affirmations sourcées |

### 5.2 Hypothèses de cadrage (à trancher au kick-off)

| Paramètre | Hypothèse par défaut | Alternatives | Impact |
| --- | --- | --- | --- |
| Taille | ETI (500–5 000 salariés) | PME / grand compte | Effectifs, outillage, licences |
| Secteur | Services/industrie, entité NIS2 probable | Santé, finance (DORA), public | Priorités sectorielles, obligations |
| Pile technique | Cloud public + SaaS + développement interne | Hybride, on-premise | Sources de journaux |
| Usage de l'IA | Outils de codage IA, quelques services LLM en production | Aucun / massif | Poids du volet O2/O3 |
| Équipe | 1 analyste CTI, 1–2 développeurs, 0,5 DevSecOps, 0,2 FinOps | Externalisation partielle | Calendrier |
| Veille externe payante | Un fournisseur de logs d'infostealers + un flux commercial | Aucun / plusieurs | Couverture O1 |
| Posture | **Passive et défensive** (aucun achat, aucun contact avec des acteurs) | — | Voir §5.8 |

**Décisions à prendre** : (1) sponsor et rattachement (RSSI recommandé) ; (2) périmètre des actifs (domaines, dépôts, comptes cloud, fournisseurs d'IA) ; (3) tolérance au risque juridique ; (4) budget de licences ; (5) qui est destinataire des alertes critiques 24/7.

### 5.3 Besoins prioritaires en renseignement (PIR)

Un PIR est une question à laquelle la direction a besoin de répondre ; chaque source, collecteur et livrable doit se rattacher à au moins un PIR.

| ID | PIR | Décision soutenue | Indicateurs / signaux | Fréquence |
| --- | --- | --- | --- | --- |
| **PIR-1** | Nos identifiants, sessions, clés ou accès sont-ils en vente ou dans des logs récents ? | Révocation, réinitialisation, notification | Domaines/emails dans logs ; clés retrouvées ; revendications nous citant | Continu |
| **PIR-2** | Une clé ou un endpoint d'IA à nous est-il exposé ou abusé ? | Révocation, plafonds, durcissement | Pics de coût ; endpoints ouverts ; clés dans dépôts publics | Continu |
| **PIR-3** | Une dépendance, une action CI, un skill ou un serveur MCP que nous utilisons est-il compromis ? | Gel, rotation de secrets | Avis TeamPCP-like ; CVE ; signalements de registres | Continu |
| **PIR-4** | Quels groupes ransomware et courtiers ciblent notre secteur, notre pays, nos fournisseurs ? | Priorités de défense, exercices | Publications sur sites de fuite par secteur/pays ; fournisseurs touchés | Hebdo |
| **PIR-5** | Quelles vulnérabilités exploitées activement touchent notre parc ? | Priorisation des correctifs | KEV, EPSS, avis CERT-FR | Quotidien |
| **PIR-6** | Une revendication nous visant est-elle crédible ? | Activation de la cellule de crise ou classement | Voir flux §5.9 | À la demande |
| **PIR-7** | Quelles opérations de répression changent le paysage (déplacement, nouvelles familles) ? | Ajustement des règles de détection | Communiqués Europol/DOJ ; migration de familles | Hebdo |
| **PIR-8** | Quelle évolution réglementaire ou assurantielle nous touche ? | Plan de conformité, clauses | NIS2/CMP, DORA, CRA, clauses d'assurance | Mensuel |
| **PIR-9** | Quel scénario de la prospective (A–E) se réalise ? | Arbitrage d'investissement | Indicateurs §7.4 | Trimestriel |
| **PIR-10** | Nos agents et outils d'IA internes cumulent-ils les trois conditions dangereuses ? | Redesign des droits | Inventaire d'agents, revue des permissions | Trimestriel |

### 5.4 Sources : catalogue de départ

**Principe** : trois mesures du marché à ne jamais confondre — (a) **discussions de forums** (intentions, offres), (b) **revendications de sites de fuite** (non vérifiées), (c) **télémétrie** (machines infectées, identifiants observés). Chaque source reçoit une cote A–E (§0.3) et une durée de vie.

| Famille | Source | Type d'accès | Cote | Usage / remarque |
| --- | --- | --- | --- | --- |
| **Autorités** | CERT-FR / ANSSI (alertes, avis, panoramas) | Flux RSS, web | A | PIR-5, 8 ; référence France |
|  | ENISA (Threat Landscape, NIS360) | Web, PDF | A | Contexte UE |
|  | Europol (IOCTA, communiqués Endgame) | Web | A | PIR-7 |
|  | CERT-EU (billets d'incident) | Web | A | PIR-3 |
|  | DOJ / FBI (communiqués, saisies) | Web | A | PIR-7 |
|  | CISA KEV | Fichier JSON public | A | PIR-5 |
| **Vulnérabilités** | NVD, GitHub Advisory Database, OSV | API publiques | A/B | PIR-3, 5 |
|  | EPSS (FIRST) | API publique | B | Priorisation |
| **Menaces ouvertes** | abuse.ch (URLhaus, MalwareBazaar, ThreatFox) | API (clé gratuite à vérifier) | B | Indicateurs techniques |
|  | ransomware.live (ou équivalent) | API/RSS (conditions à vérifier) | C | PIR-4 ; revendications, pas faits |
|  | MITRE ATT&CK (campagnes, groupes) | STIX public | A | Cartographie |
| **Éditeurs et CTI** | Google GTIG/Mandiant, Unit 42, Sophos, Bitsight, Trend Micro, Akamai, Sysdig, Pillar Security, Koi, Flashpoint, Intrinsec | Blogs, RSS, rapports | B | Traités comme intéressés ; croiser avec A |
| **Exposition propre** | Surveillance d'identifiants (fournisseur commercial ou service de notification de fuite) | API payante | B | PIR-1 ; périmètre = domaines de l'entreprise uniquement |
|  | Analyse de secrets sur dépôts (outils open source de détection de secrets) | Interne | A | PIR-2 |
|  | Inventaire de surface externe (scan de ses propres plages) | Interne | A | PIR-2, 10 |
|  | Facturation cloud et fournisseurs d'IA | Interne | A | PIR-2 |
| **Registres** | PyPI, npm, GitHub Actions marketplace, registres de skills | API/RSS | B | PIR-3 |
| **Droit** | Légifrance, JO, Assemblée nationale, EUR-Lex | Web | A | PIR-8 |
| **Presse** | Presse spécialisée (Next, LeMagIT, BleepingComputer, The Record, SecurityWeek…) | RSS | C | Détection précoce, jamais source unique |

**Règles** : pas de source seule pour un fait publié ; toute valeur chiffrée porte la date de publication et la date de relecture ; les revendications d'acteurs malveillants sont cotées E ; un contenu de forum n'est jamais cité comme fait, seulement comme « annonce ».

### 5.5 Cycle du renseignement du projet

| Étape | Activités | Responsable | Sortie |
| --- | --- | --- | --- |
| **Direction** | Revue trimestrielle des PIR, arbitrages | Sponsor + responsable CTI | PIR validés |
| **Collecte** | Collecteurs automatiques + veille manuelle ; contrôle de santé des sources | Développeurs, analyste | Éléments bruts archivés |
| **Traitement** | Normalisation, déduplication, extraction d'entités, scoring | Plateforme | Éléments structurés |
| **Analyse** | Qualification de crédibilité, corrélation, rattachement aux PIR, hypothèses concurrentes | Analyste | Évaluations |
| **Diffusion** | Alerte, bulletin, flash, tableau de bord | Analyste, plateforme | Livrables §5.6 |
| **Retour** | Utilité, actions déclenchées, erreurs | Lecteurs | Mise à jour des PIR |

### 5.6 Livrables et cadence

| Livrable | Cadence | Destinataires | Contenu |
| --- | --- | --- | --- |
| **Alerte critique** | Immédiat | RSSI, SOC, DevSecOps | Fait, source, action requise, délai |
| **Flash** | < 24 h | RSSI, IT | Événement majeur (ex. compromission de dépendance) |
| **Bulletin hebdomadaire** | Hebdo | Sécurité, IT, développement | Faits marquants, vulnérabilités exploitées, ransomware du secteur, répression |
| **Rapport mensuel** | Mensuel | Direction | Tendances, indicateurs, décisions demandées |
| **Revue trimestrielle des scénarios** | Trimestriel | Comité de direction | Mise à jour A–E, matrice d'hypothèses (§7) |
| **Registre de preuves** | Continu | Équipe | Une ligne par affirmation, relecture périodique |
| **Note de conformité** | À l'événement | Juridique, DPO | Textes, échéances (NIS2/CMP, CRA, DORA) |

Chaque livrable commence par une **conclusion en une phrase** (BLUF), indique le **niveau de confiance** (faible / moyen / élevé), la **cote de source** et la **décision attendue**.

### 5.7 Gouvernance (RACI)

| Activité | Sponsor (RSSI) | Resp. CTI | Analyste | Dév. | DevSecOps | FinOps | Juridique / DPO | SOC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Définir les PIR | A | R | C | I | C | C | C | C |
| Maintenir le registre de preuves | I | A | R | C | I | I | I | I |
| Développer la plateforme | I | A | C | R | C | I | C | I |
| Surveillance du coût d'IA | I | C | C | R | C | A/R | I | C |
| Alertes critiques | A | R | R | I | R | C | I | R |
| Validation des usages sensibles (forums, fournisseurs) | A | R | C | I | I | I | R | I |
| Revue des scénarios | A | R | R | I | C | C | C | C |

R = réalise, A = rend compte, C = consulté, I = informé.

### 5.8 Cadre juridique et éthique de la veille elle-même (lacune L14)

**Posture recommandée** : **observation passive de sources ouvertes et de fournisseurs légitimes** ; aucun achat de données ou de services illicites ; aucune interaction avec des acteurs malveillants ; aucun accès à des espaces fermés par identité fictive sans validation écrite de la direction juridique et de la direction générale.

| Risque | Règle | Statut |
| --- | --- | --- |
| Détention de données volées | Ne stocker que ce qui concerne **ses propres actifs** ; ne pas conserver de listes tierces ; recel (art. 321-1) et STAD (art. 323-x) à faire valider | † |
| Outils offensifs | Ne pas détenir ni exécuter de logiciels malveillants ; l'art. 323-3-1 réserve un « motif légitime » mais le périmètre doit être validé | † |
| RGPD | Les données de fuite contiennent des données personnelles : base légale (intérêt légitime de sécurité), minimisation, durée de conservation, analyse d'impact si traitement à grande échelle, information du DPO | † |
| Conditions d'utilisation | Respect des conditions des API et sites collectés ; pas de contournement | 🔵 |
| Sources non fiables | Contenu collecté traité comme **non fiable** (risque d'injection de prompt lors du traitement par un modèle, §6.6) | 🔵 |
| Attribution et diffamation | Ne jamais nommer une personne sur la base d'une revendication ; formules prudentes (« revendiqué par ») | 🔵 |
| Prestataires | Contrats avec clauses de licéité de la collecte, localisation des données, sous-traitance | 🟡 |
| Journalisation | Qui a accédé à quoi ; revue annuelle | 🟡 |

**Lignes rouges** : acheter des logs ; payer une rançon ou un « service » à un acteur ; ouvrir des identifiants volés pour les tester ; publier des données personnelles issues de fuites.

### 5.9 Flux de vérification d'une revendication (PIR-6)

Contexte : l'ANSSI estime que **65 % des revendications d'extorsion étaient du bluff ou du recyclage** en 2025.

| Étape | Action | Critère de décision |
| --- | --- | --- |
| 1 | Enregistrer la revendication (copie datée, hachage, source, cote E) | Preuve conservée |
| 2 | Vérifier l'**ancienneté** : échantillon déjà public ? présent dans d'anciennes fuites ? | Recyclé → classer, surveiller |
| 3 | Comparer l'**échantillon** à nos données (formats, identifiants internes, jetons factices posés) | Correspondance → incident probable |
| 4 | Vérifier la **cohérence** (dates, volumes, systèmes cités) avec nos journaux | Incohérence → bluff probable |
| 5 | Consulter les **sources externes** (autorités, éditeurs, pairs) pour le même acteur | Acteur connu pour bluffer ou non |
| 6 | Évaluer le **risque résiduel** (exposition de clés, données personnelles) et agir (rotation, notification éventuelle) | RGPD 72 h si violation avérée |
| 7 | Clôturer avec le niveau de confiance et mettre à jour le registre | Enseignements capitalisés |

### 5.10 Indicateurs de performance du projet

| Famille | Indicateur | Cible indicative |
| --- | --- | --- |
| **Réactivité** | Délai entre détection d'une clé exposée et sa révocation | < 1 h (test n°1 du rapport source) |
|  | Délai de qualification d'une alerte critique | < 30 min en heures ouvrées |
| **Qualité** | Part d'alertes jugées utiles par les destinataires | > 70 % à 6 mois |
|  | Taux de faux positifs sur les alertes de coût d'IA | < 20 % |
| **Couverture** | Part des PIR couverts par ≥ 2 sources indépendantes | > 80 % |
|  | Part de dépendances critiques épinglées par empreinte | > 90 % |
|  | Part de clés d'IA avec plafond de dépense dur | 100 % |
| **Preuves** | Part d'affirmations publiées avec double source | > 90 % |
| **Impact** | Décisions ou actions attribuables à un livrable | ≥ 1 par mois |
| **Santé** | Disponibilité des collecteurs ; sources en panne | > 95 % |

### 5.11 Feuille de route métier

| Phase | Durée | Jalons |
| --- | --- | --- |
| **0. Cadrage** | S0–S2 | Sponsor, PIR validés, cadrage juridique, choix de sources, registre de preuves initialisé |
| **1. Socle** | S3–S12 | Bulletin hebdomadaire, surveillance KEV/avis, tableau de bord de coûts d'IA, inventaire de clés et de dépendances, premier tabletop |
| **2. Exposition et supply chain** | M4–M6 | Surveillance d'identifiants, honeytokens, audit d'épinglage CI, scan de surface externe |
| **3. Plateforme et automatisation** | M6–M9 | Corrélation, scoring, synchronisation avec plateformes de renseignement, assistant de synthèse sous contrôle |
| **4. Agents et prospective** | M9–M12 | Inventaire d'agents/MCP, indicateurs des scénarios automatisés, revue trimestrielle |

---

## 6. Développement : cahier de lancement technique

### 6.1 Principes directeurs

1. **Traçabilité d'abord** : aucune affirmation sans lien vers un élément source archivé (le « registre de preuves » du §1.6 devient le cœur du modèle de données).
2. **Contenu collecté = non fiable** : tout texte externe peut contenir des instructions malveillantes ; le traitement par modèle de langage est isolé et sans effet de bord (§6.6).
3. **Manger sa propre soupe** : la plateforme applique les contrôles qu'elle recommande (secrets dans un coffre, plafonds de dépense par clé, honeytokens, épinglage des dépendances, journalisation).
4. **Incrémental** : un MVP utile en 12 semaines, puis enrichissements ; chaque module est autonome et remplaçable.
5. **Standards ouverts** : STIX 2.1, MITRE ATT&CK, SBOM (CycloneDX/SPDX), formats d'export vers des plateformes de renseignement existantes plutôt que de les réinventer.
6. **Acheter / adopter avant de construire** : ne développer que la valeur propre à l'entreprise (corrélation avec ses actifs, coûts d'IA, scénarios).

### 6.2 Architecture de référence

```text
 SOURCES                    COLLECTE              TRAITEMENT                STOCKAGE            RESTITUTION
 ───────                    ────────              ──────────                ────────            ───────────
 Autorités (RSS, KEV)  ─┐                                                   ┌─ PostgreSQL        ┌─ Alertes (chat, courriel,
 Vulnérabilités (OSV…) ─┤   ┌────────────┐   ┌───────────────┐             │  (relationnel +    │   ticket, SIEM)
 Éditeurs / CTI (RSS)  ─┼──▶│ Collecteurs│──▶│ File de        │──▶ Normalisation ──▶│   vecteurs)        ├─ Bulletins (hebdo,
 Sites de fuite (API)  ─┤   │ (pull/push)│   │ messages       │   Déduplication │   ├─ Objets bruts      │   mensuel, flash)
 Fournisseurs identifiants┤   └────────────┘   └───────────────┘   Extraction     │  (stockage objet,  ├─ Tableaux de bord
 Journaux internes     ─┤        ▲                                 d'entités       │   immuable)        ├─ API interne
 (cloud, IAM, facture) ─┘        │ santé / quotas                   Enrichissement  └─ Index plein texte ├─ Export STIX / MISP /
                                  │                                  Scoring              (option)           │   OpenCTI
                             Orchestrateur ◀────────────────────────  Assistant LLM (isolé, sans outils)       └─ Indicateurs de scénarios
                                                       Gouvernance transverse : identités, secrets, journaux d'audit, rétention, TLP
```

### 6.3 Pile technologique proposée

| Couche | Choix par défaut | Alternatives | Justification |
| --- | --- | --- | --- |
| Langage | Python 3.12 (collecte, analytique), TypeScript (interface) | Go pour collecteurs | Écosystème de sécurité et de données |
| API | FastAPI | Flask | Typage, documentation automatique |
| Orchestration | Prefect ou Dagster | Airflow, cron + files | Reprises, observabilité |
| File | Redis Streams ou NATS | RabbitMQ, SQS | Simplicité |
| Base | PostgreSQL + pgvector | SQLite (prototype) | Un seul moteur relationnel et vectoriel |
| Objets bruts | Stockage compatible S3 (MinIO ou cloud) | Disque versionné | Archives immuables, hachage |
| Plateforme de renseignement | **MISP** et/ou **OpenCTI** | Aucune au MVP | Réutiliser STIX, corrélation, partage |
| Détection | Règles **Sigma**, YARA pour artefacts internes | — | Standard ouvert |
| Visualisation | Grafana ou Metabase | Superset | Rapide à livrer |
| Secrets | Coffre (Vault, gestionnaire cloud) | — | Contrôle O1/O2 |
| CI/CD | GitHub Actions ou GitLab CI, actions épinglées par empreinte, analyse statique de workflows | — | Contrôle O3 |
| Qualité | pytest, ruff, mypy ; bandit ; analyse de dépendances | — | Hygiène |

*Les noms d'outils sont des exemples ; leur disponibilité, licence et maintenance sont à vérifier avant adoption.*

### 6.4 Modèle de données canonique

```sql
-- Sources et éléments bruts
CREATE TABLE source (
  id            SERIAL PRIMARY KEY,
  name          TEXT NOT NULL UNIQUE,
  kind          TEXT NOT NULL,          -- authority | vendor | feed | press | internal
  reliability   CHAR(1) NOT NULL CHECK (reliability IN ('A','B','C','D','E')),
  ttl_days      INT NOT NULL DEFAULT 30,
  access        JSONB NOT NULL,         -- url, auth ref (jamais le secret), cadence
  legal_status  TEXT NOT NULL           -- approved | pending | refused
);

CREATE TABLE raw_item (
  id            BIGSERIAL PRIMARY KEY,
  source_id     INT REFERENCES source(id),
  fetched_at    TIMESTAMPTZ NOT NULL,
  published_at  TIMESTAMPTZ,
  url           TEXT,
  sha256        CHAR(64) NOT NULL UNIQUE,   -- déduplication, immuabilité
  object_ref    TEXT NOT NULL,              -- chemin du contenu brut archivé
  lang          TEXT
);

-- Affirmations (le registre de preuves)
CREATE TABLE claim (
  id            BIGSERIAL PRIMARY KEY,
  statement     TEXT NOT NULL,
  claim_type    TEXT NOT NULL,   -- fact | figure | status | attribution | forecast
  value_num     NUMERIC,
  value_unit    TEXT,
  as_of         DATE,            -- date à laquelle le fait est vrai
  epistemic     TEXT NOT NULL CHECK (epistemic IN ('observation','extrapolation','hypothesis')),
  confidence    TEXT NOT NULL CHECK (confidence IN ('low','medium','high')),
  verified      TEXT NOT NULL CHECK (verified IN ('none','single','double','primary')),
  review_due    DATE NOT NULL,   -- péremption
  reviewer      TEXT
);

CREATE TABLE evidence (
  claim_id      BIGINT REFERENCES claim(id),
  raw_item_id   BIGINT REFERENCES raw_item(id),
  span          TEXT,            -- extrait court justifiant l'affirmation
  stance        TEXT NOT NULL CHECK (stance IN ('supports','contradicts','context')),
  PRIMARY KEY (claim_id, raw_item_id)
);

-- Entités et alias (familles de malwares, groupes, forums, outils, CVE, entreprises)
CREATE TABLE entity (
  id            BIGSERIAL PRIMARY KEY,
  kind          TEXT NOT NULL,   -- malware | actor | forum | tool | vuln | org | campaign | operation
  canonical     TEXT NOT NULL,
  status        TEXT,            -- active | disrupted | defunct | unknown
  status_as_of  DATE,
  UNIQUE (kind, canonical)
);
CREATE TABLE entity_alias (entity_id BIGINT REFERENCES entity(id), alias TEXT, PRIMARY KEY (entity_id, alias));
CREATE TABLE claim_entity (claim_id BIGINT REFERENCES claim(id), entity_id BIGINT REFERENCES entity(id), role TEXT, PRIMARY KEY (claim_id, entity_id));

-- Pilotage : besoins, indicateurs, alertes
CREATE TABLE pir (id TEXT PRIMARY KEY, question TEXT NOT NULL, decision TEXT, owner TEXT, cadence TEXT);
CREATE TABLE pir_link (pir_id TEXT REFERENCES pir(id), claim_id BIGINT REFERENCES claim(id), PRIMARY KEY (pir_id, claim_id));

CREATE TABLE indicator (
  id TEXT PRIMARY KEY,            -- ex. IND-A1
  scenario CHAR(1) NOT NULL,      -- A..E
  label TEXT NOT NULL, unit TEXT, threshold NUMERIC, direction TEXT, collection_method TEXT
);
CREATE TABLE indicator_obs (indicator_id TEXT REFERENCES indicator(id), observed_at DATE, value NUMERIC, source_claim BIGINT REFERENCES claim(id), PRIMARY KEY (indicator_id, observed_at));

CREATE TABLE alert (
  id BIGSERIAL PRIMARY KEY, created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  severity TEXT NOT NULL, pir_id TEXT REFERENCES pir(id), title TEXT NOT NULL,
  body TEXT, asset_ref TEXT, status TEXT NOT NULL DEFAULT 'open', acked_at TIMESTAMPTZ, closed_at TIMESTAMPTZ
);
```

**Règle de publication** : un livrable externe ne peut citer qu'une affirmation dont `verified` vaut `double` ou `primary` et dont `review_due` n'est pas dépassée.

### 6.5 Modules fonctionnels

| ID | Module | Entrées | Sorties | PIR | Priorité |
| --- | --- | --- | --- | --- | --- |
| **M1** | **Collecte et registre de sources** (RSS, API, fichiers) avec contrôle de santé | Registre YAML | Éléments bruts archivés | Tous | MVP |
| **M2** | **Normalisation, déduplication, entités** (familles, groupes, forums, CVE) avec table d'alias | Éléments bruts | Éléments structurés | Tous | MVP |
| **M3** | **Registre de preuves et affirmations** (§6.4) avec péremption | Analyste, assistant | Affirmations sourcées | PIR-6, 9 | MVP |
| **M4** | **Veille vulnérabilités** : KEV, EPSS, avis, rapprochement avec l'inventaire | KEV, inventaire logiciel | Liste priorisée | PIR-5 | MVP |
| **M5** | **Coût d'IA et détection d'abus** : ingestion des facturations et journaux, base saisonnière, alertes | Factures, journaux d'inférence/IAM | Alertes, tableau de bord | PIR-2 | MVP |
| **M6** | **Secrets et honeytokens** : détection de secrets dans dépôts, pose/suivi de clés factices | Dépôts, journaux | Alertes critiques | PIR-1, 2 | MVP |
| **M7** | **Supply chain** : inventaire (SBOM), requêtes OSV/avis, audit d'épinglage CI, surveillance des registres | Dépôts, lockfiles, workflows | Rapport, alertes | PIR-3 | Phase 2 |
| **M8** | **Exposition d'identifiants** : intégration d'un fournisseur, rapprochement avec l'annuaire, workflow de remédiation | API fournisseur | Cas de remédiation | PIR-1 | Phase 2 |
| **M9** | **Surface externe IA/MCP** : inventaire de ses endpoints, détection d'API d'inférence ouvertes | Plages IP, DNS | Alertes | PIR-2, 10 | Phase 2 |
| **M10** | **Sites de fuite et répression** : flux de revendications filtrées par secteur/pays/fournisseurs ; calendrier des opérations | API/RSS | Suivi PIR-4/7 | PIR-4, 7 | Phase 2 |
| **M11** | **Tableau de bord des scénarios** : indicateurs A–E (§7.4) | Observations | Courbes, seuils | PIR-9 | Phase 3 |
| **M12** | **Assistant de synthèse sous contrôle** (résumé, traduction, classement, extraction) | Éléments bruts | Brouillons à valider | Tous | Phase 3 |
| **M13** | **Export et intégration** (STIX 2.1, MISP/OpenCTI, SIEM, tickets) | Entités, alertes | Flux sortants | Tous | Phase 3 |
| **M14** | **Inventaire des agents/MCP/skills** et revue des permissions | Dépôts, configurations, registres internes | Registre, écarts | PIR-10 | Phase 4 |

### 6.6 Sécurité de la plateforme et usage des modèles de langage

| Risque | Mesure | Test associé |
| --- | --- | --- |
| **Injection de prompt par contenu collecté** | Contenu délimité et marqué non fiable ; modèle **sans outils, sans accès réseau, sans secrets** ; sortie contrainte à un schéma ; aucune action déclenchée par le texte | Corpus d'attaques synthétiques en CI |
| **Hallucination** | Toute affirmation générée doit citer un extrait ; vérification automatique que l'extrait existe dans la source ; relecture humaine avant publication | Taux d'extraits introuvables = 0 |
| **Coût et abus** | Clé dédiée à la plateforme, plafond de dépense dur, alerte de consommation, rotation | Test d'alerte mensuel |
| **Fuite de données** | Pas d'envoi de données personnelles ni de secrets au modèle ; masquage préalable | Tests de masquage |
| **Secrets** | Coffre ; aucune clé dans le code ni les journaux ; honeytokens | Analyse de secrets en CI |
| **Chaîne logicielle** | Dépendances épinglées par empreinte, SBOM, analyse de workflows, mises à jour revues | Contrôle bloquant en CI |
| **Accès** | SSO, rôles minimaux, journal d'audit, marquage TLP | Revue trimestrielle |
| **Intégrité des données** | Objets bruts immuables, hachage, versionnage des règles | Vérification périodique |

### 6.7 Exemples de code (défensifs, à adapter)

**a) Détection robuste d'un pic de coût par clé (M5)**

```python
from statistics import median

def robust_z(history: list[float], current: float, floor: float = 1e-6) -> float:
    """Score de déviation robuste (médiane/MAD) du coût horaire courant."""
    if len(history) < 24:
        return 0.0  # pas assez d'historique : ne pas alerter
    med = median(history)
    mad = median(abs(x - med) for x in history)
    scale = max(1.4826 * mad, floor, 0.05 * med)  # plancher pour éviter les divisions par ~0
    return (current - med) / scale

def classify(history: list[float], current: float, hard_cap_per_hour: float) -> str:
    if current >= hard_cap_per_hour:
        return "critical"       # plafond dur dépassé : révocation immédiate
    z = robust_z(history, current)
    if z >= 6 or (history and current >= 3 * max(history[-24:])):
        return "high"
    if z >= 3:
        return "medium"
    return "ok"
```

*Variante recommandée* : calculer la base **par heure de la semaine** (profil saisonnier) et par clé ; fusionner les alertes d'une même clé sur 1 heure.

**b) Rapprochement d'une dépendance avec des avis publics (M7)**

```python
import requests

OSV_URL = "https://api.osv.dev/v1/query"

def osv_vulns(ecosystem: str, name: str, version: str) -> list[dict]:
    payload = {"package": {"name": name, "ecosystem": ecosystem}, "version": version}
    r = requests.post(OSV_URL, json=payload, timeout=20)
    r.raise_for_status()
    return r.json().get("vulns", [])

# Exemple : osv_vulns("PyPI", "litellm", "1.82.7")
```

**c) Priorisation par exploitation connue (M4)**

```python
import requests

KEV_URL = "https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json"

def kev_ids() -> set[str]:
    data = requests.get(KEV_URL, timeout=30).json()
    return {v["cveID"] for v in data.get("vulnerabilities", [])}

def prioritise(findings: list[dict], kev: set[str]) -> list[dict]:
    for f in findings:
        f["priority"] = "urgent" if f["cve"] in kev else "normal"
    return sorted(findings, key=lambda f: f["priority"] != "urgent")
```

**d) Contrôle d'une sortie de modèle (M12) : l'extrait cité doit exister**

```python
def verify_citations(summary_items: list[dict], source_text: str) -> list[dict]:
    """Chaque item : {"claim": str, "quote": str}. Rejette si la citation est absente de la source."""
    ok = []
    for it in summary_items:
        if it.get("quote") and it["quote"].strip() in source_text:
            ok.append(it)
    return ok  # les items rejetés partent en revue humaine, jamais en publication
```

**e) Registre de sources (M1) — extrait**

```yaml
sources:
  - name: cert-fr-avis
    kind: authority
    reliability: A
    ttl_days: 365
    access: {type: rss, url: "<à renseigner depuis la page officielle>", every: 1h}
    pir: [PIR-5, PIR-8]
    legal_status: approved
  - name: kev
    kind: authority
    reliability: A
    ttl_days: 3650
    access: {type: json, url: "https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json", every: 6h}
    pir: [PIR-5]
    legal_status: approved
  - name: leaksite-claims
    kind: feed
    reliability: E        # revendications, non vérifiées
    ttl_days: 30
    access: {type: api, url: "<à renseigner ; conditions d'usage à valider>", every: 1h}
    pir: [PIR-4, PIR-6]
    legal_status: pending
```

**f) Score de priorité d'un élément (M2)**

```text
priorité = relevance × actionability × timeliness × credibility
  relevance      : 0 à 1  (correspondance avec nos actifs, secteur, pays, fournisseurs)
  actionability  : 0 à 1  (une action concrète existe-t-elle ?)
  timeliness     : décroissance exponentielle depuis la publication (demi-vie par type)
  credibility    : cote de source (A=1,0 ; B=0,8 ; C=0,6 ; D=0,3 ; E=0,1) × bonus de double source
Seuils : ≥ 0,6 alerte ; 0,3–0,6 bulletin ; < 0,3 archive
```

### 6.8 Stratégie de test et d'évaluation

- **Jeu de référence** : 100 éléments annotés à la main (pertinence, entités, crédibilité) pour mesurer précision/rappel de l'extraction et du scoring ; mis à jour chaque trimestre.
- **Tests de non-régression** des collecteurs avec enregistrements de réponses ; surveillance des changements de format de sources.
- **Tests d'injection de prompt** : corpus d'éléments piégés ; la sortie doit rester conforme au schéma et n'induire aucune action.
- **Tests d'alerte de coût** : rejeu de séries historiques et simulations de pics ; mesure du délai de détection.
- **Exercices** : un honeytoken d'IA posé sur un poste de test doit déclencher une alerte critique en < 15 minutes.
- **Revue d'exactitude** mensuelle : échantillon de 10 affirmations publiées revérifiées à la source primaire (objectif : 0 erreur de fait).

### 6.9 Backlog initial (extraits)

| Epic | Histoire | Critères d'acceptation |
| --- | --- | --- |
| E1 Socle | Enregistrer une source dans le registre et la collecter toutes les heures | Source `approved` seulement ; échec visible dans le tableau de santé ; objet brut haché |
| E1 | Dédupliquer deux éléments identiques publiés par deux sources | Un seul élément structuré, deux preuves liées |
| E2 Registre | Créer une affirmation avec preuves et péremption | Impossible de publier sans `verified` ≥ `double` |
| E2 | Alerter quand une affirmation arrive à échéance de relecture | Notification à J-7 au relecteur |
| E3 Vulnérabilités | Afficher les CVE du KEV présentes dans l'inventaire | Liste triée ; lien vers actifs ; export |
| E4 Coût d'IA | Ingérer la facturation par clé et calculer la base saisonnière | Précision mesurée sur historique ; alerte < 15 min après dépassement |
| E4 | Alerter sur activation d'un service d'IA non utilisé | Alerte critique avec l'identité auteure |
| E5 Secrets | Détecter une clé d'IA dans un dépôt de l'organisation | Blocage de fusion + alerte ; procédure de révocation liée |
| E5 | Poser et suivre des honeytokens | Alerte à la première utilisation, inventaire des emplacements |
| E6 Supply chain | Vérifier que toutes les actions CI sont épinglées par empreinte | Rapport de conformité, build bloquant en cas d'écart |
| E6 | Signaler une dépendance concernée par un avis | Ticket automatique au propriétaire |
| E7 Bulletins | Générer un brouillon de bulletin hebdomadaire sourcé | Chaque phrase liée à une preuve ; relecture humaine obligatoire |
| E8 Scénarios | Saisir et afficher les indicateurs A–E | Courbe, seuil, source de chaque observation |
| E9 Assistant | Résumer un rapport long avec citations vérifiées | Taux de citations introuvables = 0 |
| E10 Agents | Lister les agents et serveurs MCP avec leurs droits | Registre à jour ; écart de permissions signalé |

### 6.10 Plan de sprints (MVP en 12 semaines)

| Sprint | Objectif | Livrables |
| --- | --- | --- |
| S1 (sem. 1–2) | Fondations | Dépôt, CI avec actions épinglées, coffre, base, registre de sources (3 sources) |
| S2 (sem. 3–4) | Collecte et preuves | Collecteurs RSS/JSON, déduplication, modèle claim/evidence, premier registre importé depuis ce dossier |
| S3 (sem. 5–6) | Vulnérabilités et bulletin | M4 (KEV), bulletin hebdomadaire manuel assisté, tableau de santé |
| S4 (sem. 7–8) | Coût d'IA | M5 : ingestion de facturation, alertes, tableau de bord |
| S5 (sem. 9–10) | Secrets | M6 : détection de secrets, honeytokens, procédure de révocation testée |
| S6 (sem. 11–12) | Stabilisation et revue | Tests d'injection, revue d'exactitude, démo à la direction, décision sur la phase 2 |

### 6.11 Exploitation et effort

- **Exploitation** : supervision des collecteurs, rotation des secrets trimestrielle, restauration testée, revue des accès, mise à jour des dépendances avec revue (pas d'auto-mise à jour aveugle).
- **Effort indicatif (🟡)** : MVP ≈ 1 analyste + 1–2 développeurs + 0,5 DevSecOps sur 12 semaines ; phases 2–4 ≈ 1 à 2 développeurs en continu. Le poste de coût dominant est généralement la **licence de renseignement externe** et le **temps d'analyse**, pas l'infrastructure ; chiffrage à faire avec les fournisseurs retenus.

---

## 7. Prospective instrumentée (scénarios A–E 2027-2030)

### 7.1 Méthode

Le rapport source a bien discriminé cinq scénarios (mécanisme, bénéficiaires, indicateur de bascule). Ce dossier ajoute trois exigences pour en faire un **outil de pilotage** et non une lecture d'été :

1. **Hypothèses concurrentes** : on ne cherche pas des preuves du scénario favori, on cherche ce qui **discrimine** (méthode d'analyse des hypothèses concurrentes, ACH) ; un fait compatible avec tous les scénarios n'apporte rien.
2. **Indicateur mesurable, source nommée, base initiale** : chaque indicateur a une méthode de collecte, une fréquence et une valeur de départ à mesurer au T1 2027.
3. **Pas de probabilités chiffrées** au démarrage : pondération qualitative (−, 0, +, ++) et révision trimestrielle ; la revue ne porte que sur les changements.

### 7.2 Scénarios et mesure

| Scénario | Mécanisme causal (rappel) | Signal discriminant | Indicateur | Collecte |
| --- | --- | --- | --- | --- |
| **A. Commodité totale** | Offre abondante, prix marginal tendant vers zéro, différenciation par le service | Prix d'un accès admin < 500 USD ; packs avec garantie de remplacement ; plans « illimités » d'accès LLM | IND-A1 prix médian d'un accès administrateur ; IND-A2 prix du plan LLM mixer | Fournisseur CTI commercial, relevés d'annonces |
| **B. Fracture réglementaire** | Traçabilité (KYC cloud/IA) crée un marché gris de comptes « propres » et proxys | Prix de comptes vérifiés > 2× ; offres de contournement de vérification | IND-B1 prix d'un compte cloud/IA vérifié ; IND-B2 nombre d'offres de contournement | Fournisseur CTI, annonces de fournisseurs d'IA sur la vérification |
| **C. Course défensive gagnante** | Liaison d'appareil, secrets éphémères et plafonds par défaut dévaluent le rejeu | Part de sessions liées à l'appareil > 40 % ; prix des logs de navigateur −50 % | IND-C1 part de sessions liées à l'appareil (interne) ; IND-C2 prix du log | Mesure interne + publications des éditeurs de navigateurs/identités + relevés |
| **D. Crise de confiance supply chain** | Compromissions en cascade : gel et internalisation des dépendances | > 30 % de grandes organisations avec registre interne obligatoire | IND-D1 enquêtes sectorielles ; IND-D2 nombre de compromissions de registres/CI par trimestre | Enquêtes, avis CERT, registres |
| **E. SOC agentique vs attaquants agentiques** | Agents défensifs à la vitesse des agents offensifs | > 50 % de grandes organisations avec agent défensif en production ; incidents à réponse entièrement automatisée | IND-E1 adoption ; IND-E2 campagnes agentiques documentées | Enquêtes, rapports d'éditeurs, MITRE |

**Indicateurs transverses** : IND-T1 coût médian d'un incident de LLMjacking (seuil de confirmation : > 100 000 USD) ; IND-T2 publications de revendications sur sites de fuite par trimestre (**référence 2026 : 9 663 au 11 nov.**) ; IND-T3 part des revendications recyclées (**référence ANSSI 2025 : 65 %**) ; IND-T4 mentions d'agents/MCP en forum (**référence Bitsight : +67 % entre février et août 2026**) ; IND-T5 nombre de marques de ransomware actives (**référence : 117 au T3 2026**).

### 7.3 Matrice de preuves 2026 (ACH, première passe)

Lecture : « + » rend le scénario plus probable, « − » moins probable, « 0 » non discriminant. **À réviser par au moins deux analystes.**

| Fait 2026 | A | B | C | D | E | Remarque |
| --- | --- | --- | --- | --- | --- | --- |
| Plan LLM « illimité » vendu ≈ 60 USD/mois (GTIG) | ++ | 0 | 0 | 0 | 0 | Prix très bas, packaging de service |
| Record de revendications et 117 marques de ransomware | + | 0 | − | 0 | 0 | Fragmentation, barrière basse |
| DBSC en disponibilité générale (Windows) | 0 | 0 | + | 0 | 0 | Adoption côté serveurs encore inconnue |
| Clés d'IA liées à des comptes de service et restreintes par défaut (Google) | 0 | + | + | 0 | 0 | Contrôle côté fournisseur |
| TeamPCP, Vercel, Binance (installateur falsifié) | + | 0 | 0 | ++ | 0 | Cascade de confiance |
| MONST3R-AI, GTG-1002 | + | 0 | 0 | 0 | + | Agents côté offensif ; pas encore de preuve côté défense |
| Proxys résidentiels et chiffrement (IOCTA) | 0 | + | 0 | 0 | 0 | Contournement de la traçabilité |
| Vote NIS2 (17 nov.) | 0 | + | 0 | + | 0 | Pression réglementaire, pas du KYC d'IA en soi |
| Démantèlements répétés sans baisse de volume (Flare) | + | 0 | − | 0 | 0 | Résilience du marché |

**Lecture provisoire (🔪)** : les preuves de 2026 pèsent surtout vers **A** (commodité) et **D** (confiance supply chain) ; **C** progresse sur le plan technique (DBSC, clés restreintes) mais sans preuve d'adoption ; **E** n'a de preuves que côté offensif. Hypothèse de travail : **A + D dominants, C en rattrapage lent, B et E à surveiller**. Ce n'est pas une prédiction.

### 7.4 Calendrier de veille 2027

| Échéance (🟡 estimée) | Événement à suivre | Lien |
| --- | --- | --- |
| Début 2027 | Commission mixte paritaire et promulgation de la loi Résilience (NIS2) | PIR-8 |
| Mars 2027 | Panorama de la cybermenace de l'ANSSI (édition 2026) | PIR-4, IND-T3 |
| Avril–mai 2027 | IOCTA 2027 | PIR-7 |
| Suites d'Endgame | Nouvelles phases ; mesure du déplacement des familles | IND-T, PIR-7 |
| Sept. 2027 | ENISA Threat Landscape 2027 | PIR-4 |
| En continu | Extension de DBSC (macOS) et adoption par les services majeurs | IND-C1 |
| En continu | Mises à jour des politiques de clés/KYC des fournisseurs d'IA | IND-B |

### 7.5 Invariants (ce qui ne bouge pas)

- Tant que les identités numériques sont stockées sous une **forme rejouable**, un marché existe pour les voler (invariant structurel, pas prédiction de forme).
- Les démantèlements **restructurent** plus qu'ils ne suppriment.
- La **confiance** (réputation, escrow) est l'infrastructure critique du marché.
- Le coût d'un abus d'IA retombe sur le **titulaire de la clé**.

### 7.6 Règles de mise à jour

Revue **trimestrielle** ; un scénario n'est modifié que si un indicateur franchit son seuil ou si un fait discriminant apparaît ; toute modification est datée et motivée dans le registre de preuves.

---

## 8. Annexes opérationnelles

### 8.1 Exercices et tabletops

**Reprise des exercices du rapport source (E1–E9)** : inventaire des clés d'IA ; politique de secrets pour outils assistés par IA ; matrice de risque donnée/accès/modèle/prompt/coût/chaîne ; procédure « clé compromise » ; tabletop FinOps/SOC (+15 000 USD/heure un dimanche soir) ; tabletop supply chain ; audit d'un agent relié par MCP ; pré-mortem « 500 000 USD de clé » ; discrimination prospective.

**Exercices ajoutés pour le projet de veille**

| Code | Exercice | Livrable | Lien |
| --- | --- | --- | --- |
| **E10** | Revendication d'extorsion nous visant : appliquer le flux de vérification | Fiche de décision (crédible / recyclé / bluff) | §5.9 |
| **E11** | Injection de prompt par un flux de veille piégé | Rapport de test, correctifs d'isolation | §6.6 |
| **E12** | Honeytoken d'IA utilisé depuis une IP inconnue | Chronologie, délai de révocation mesuré | M6 |
| **E13** | Compromission d'une action CI annoncée à 17 h le vendredi (type TeamPCP) | Liste ordonnée des secrets à faire tourner | M7 |
| **E14** | Cas Binance rejoué : installateur d'outil d'IA falsifié sur un poste d'ingénieur | Cartographie des droits et ruptures de chaîne | §3.6 |
| **E15** | Revue d'exactitude : revérifier 10 affirmations du registre | Taux d'erreur, causes | §6.8 |

### 8.2 Registre des risques du projet

| ID | Risque | Probabilité | Impact | Mesure |
| --- | --- | --- | --- | --- |
| R1 | Information périmée présentée comme actuelle (cf. forums) | Haute | Haut | Péremption, double source, revue mensuelle |
| R2 | Sur-interprétation de revendications | Haute | Haut | Flux §5.9, cote E |
| R3 | Risque juridique de la collecte | Moyenne | Haut | Posture passive, validation juridique, DPO |
| R4 | Injection de prompt via contenu collecté | Moyenne | Haut | Isolation du modèle, schémas, tests |
| R5 | Hallucination dans un livrable | Moyenne | Haut | Citations vérifiées, relecture humaine |
| R6 | Fatigue d'alerte | Haute | Moyen | Seuils, regroupement, retours des lecteurs |
| R7 | Dépendance à un fournisseur unique | Moyenne | Moyen | Deux sources par PIR critique |
| R8 | Plateforme elle-même compromise (secrets, dépendances) | Faible | Très haut | §6.6, honeytokens, épinglage |
| R9 | Surcoût de modèles de langage | Moyenne | Moyen | Plafond dur, alerte, clé dédiée |
| R10 | Périmètre qui dérive vers la chasse offensive | Moyenne | Haut | Lignes rouges §5.8, comité de pilotage |
| R11 | Départ de l'analyste unique | Moyenne | Haut | Documentation, registre, binôme |
| R12 | Biais de confirmation sur les scénarios | Moyenne | Moyen | ACH, relecture croisée |
| R13 | Atteinte à des personnes (nommer à tort) | Faible | Haut | Formules prudentes, validation juridique |
| R14 | Contenu délicat (données de victimes) stocké | Moyenne | Haut | Minimisation, rétention courte, chiffrement |

### 8.3 Gabarits

**Fiche PIR** : identifiant · question · décision soutenue · propriétaire · sources · indicateurs · fréquence · seuil d'alerte · date de revue.

**Fiche source** : nom · type · cote A–E · durée de vie · conditions d'usage (légal) · coût · contact · cadence · date de dernier contrôle · PIR liés.

**Flash (≤ 1 page)** : *Conclusion en une phrase* · *Ce qui s'est passé (faits datés et sourcés)* · *Pourquoi cela nous concerne (actifs, fournisseurs)* · *Niveau de confiance* · *Actions recommandées et délai* · *Ce que nous ne savons pas encore*.

**Fiche hypothèse (ACH)** : hypothèse · mécanisme · faits pour/contre · faits non discriminants · indicateur de bascule · date de revue.

### 8.4 Glossaire étendu

| Terme | Définition |
| --- | --- |
| **ACH** | Analyse des hypothèses concurrentes : comparer des hypothèses sur les preuves qui les discriminent |
| **BLUF** | Conclusion en tête de livrable |
| **Burn and rotate** | Création puis suppression de comptes de service pour échapper aux limites et à la détection |
| **C2** | Infrastructure de commande et de contrôle |
| **ClickFix** | Technique poussant l'utilisateur à exécuter lui-même une commande (faux CAPTCHA) |
| **DBSC** | Cookies courts liés à une clé non exportable de l'appareil |
| **EPSS** | Probabilité estimée d'exploitation d'une vulnérabilité |
| **Escrow** | Séquestre exercé par un forum entre acheteur et vendeur |
| **Honeytoken** | Secret factice dont toute utilisation déclenche une alerte |
| **IAB** | Courtier d'accès initial |
| **Infostealer / log** | Malware qui vole des secrets ; le « log » en est le produit |
| **KEV** | Catalogue des vulnérabilités exploitées connues |
| **LLM mixer** | Passerelle qui mutualise des clés volées et revend un accès unifié |
| **LLMjacking** | Usage frauduleux de capacité d'inférence via des identifiants volés |
| **Loader / dropper** | Malware qui télécharge et exécute un second malware |
| **MAD** | Écart absolu médian, mesure de dispersion robuste |
| **MaaS / RaaS** | Logiciel malveillant / rançongiciel en tant que service |
| **MCP** | Protocole reliant modèles et outils |
| **MISP / OpenCTI** | Plateformes de partage et de gestion du renseignement sur les menaces |
| **PIR** | Besoin prioritaire en renseignement |
| **Proxy résidentiel** | Relais passant par des connexions domestiques, difficile à distinguer du trafic légitime |
| **SBOM** | Nomenclature logicielle |
| **Sigma / YARA** | Formats de règles de détection (journaux / artefacts) |
| **Skill** | Extension d'agent décrite en langage naturel, exécutée avec les droits de l'agent |
| **STIX / TAXII** | Standards d'échange de renseignement sur les menaces |
| **TLP** | Protocole de marquage de la diffusion (CLEAR, GREEN, AMBER, RED) |
| **Triptyque dangereux** | Données privées + contenus non fiables + action externe dans un même agent |
| **UNC** | Identifiant provisoire d'un groupe non encore attribué (Mandiant) |

### 8.5 Sources consultées le 22/11/2026

**Autorités et institutions**

- ANSSI, Panorama de la cybermenace 2025 : https://cyber.gouv.fr/actualites/panorama-de-la-cybermenace-2025/ · PDF CERTFR-2026-CTI-003 : https://cert.ssi.gouv.fr/uploads/CERTFR-2026-CTI-003.pdf
- ENISA Threat Landscape 2026 : https://www.enisa.europa.eu/publications/enisa-threat-landscape-2026
- Europol IOCTA 2026 : https://www.europol.europa.eu/cms/sites/default/files/documents/IOCTA-2026.pdf
- CERT-EU, brèche de la Commission via Trivy : https://cert.europa.eu/blog/european-commission-cloud-breach-trivy-supply-chain
- MITRE ATT&CK, campagne C0062 : https://attack.mitre.org/campaigns/C0062
- Anthropic, rapport GTG-1002 (nov. 2025) : https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf

**Répression et marché**

- Endgame juin 2026 : https://www.bleepingcomputer.com/news/security/amadey-stealc-malware-operations-disrupted-in-operation-endgame-action/ · https://securityaffairs.com/194173/cyber-crime/europol-disrupts-stealc-and-amadey-malware-infrastructure-in-operation-endgame.html
- XSS.is : https://www.scworld.com/brief/major-russian-language-cybercrime-forum-xss-is-shut-down-alleged-admin-arrested · https://securityaffairs.com/194524/security/xss-is-the-forum-that-ran-the-ransomware-supply-chain-is-down-the-market-isnt.html
- Stealers : https://www.intrinsec.com/wp-content/uploads/2026/04/TLP\_CLEAR-20260424-New\_Vidar.pdf · https://www.dexpose.io/infostealer-malware-groups/
- Forums : https://brandefense.io/knowledge-hub/guides/hacker-and-leak-forums-101/ · https://deepstrike.io/blog/exploit-forum
- Ransomware : https://ransomnews.com/ransomtracker/2026/ · https://www.dragos.com/blog/dragos-industrial-ransomware-analysis-q3-2026 · https://www.stingrai.io/blog/ransomware-statistics-2026

**IA, agents, supply chain**

- Google GTIG/Mandiant, LLMjacking : https://cloud.google.com/blog/topics/threat-intelligence/llmjacking-monetizing-compromised-cloud-models
- Bitsight : https://www.bitsight.com/blog/evolution-of-ai-cybercrime-underground-forums
- Bizarre Bazaar (Pillar Security) : https://www.bleepingcomputer.com/news/security/hackers-hijack-exposed-llm-endpoints-in-bizarre-bazaar-operation/
- ClawHub / ClawHavoc : https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html · https://unit42.paloaltonetworks.com/openclaw-ai-supply-chain/
- TeamPCP : https://www.akamai.com/blog/security-research/telnyx-sdk-pypi-2026-teampcp-supply-chain-attacks · https://isc.sans.edu/diary/32864
- Binance, post-mortem : https://www.binance.com/square/post/384314395829022
- DBSC : https://blog.google/security/protecting-cookies-with-device-bound-session-credentials/

**Droit**

- NIS2, vote de l'Assemblée : https://www.leto.legal/news/nis-2-loi-resilience-adoptee-assemblee-464-voix-17-novembre-2026 · état de la transposition : https://www.openkritis.de/eu/eu-nis-2-france.html

### 8.6 Points à vérifier avant diffusion externe

1. Citations exactes et typologie RaaS de l'IOCTA 2026 (lire le PDF).
2. Statuts † : RansomHub, BlackSuit, 8Base, Cronos, Magnus, Duck Hunt, Genesis, WormGPT/FraudGPT, postmark-mcp, vers npm 2025.
3. Chiffres Endgame de novembre 2026 : à recouper sur le communiqué Europol.
4. Chiffres Taylor & Francis (1 900 Md USD) et « Europol \~20 % de commission RaaS ».
5. « −49 % de mentions de ransomware » (Bitsight) : retrouver ou supprimer.
6. « 40 % des événements financiers = rançongiciel » (ENISA) : retrouver ou supprimer.
7. T1587.004 pour GTG-1002 (la page MITRE cite d'autres techniques).
8. Versions LiteLLM 1.82.7/1.82.8 et date de l'accès initial à la Commission (sources divergentes : 10 ou 19 mars ; CERT-EU retient le 19).
9. Volume des données de la Commission (340 Go vs 92 Go compressées).
10. Détails temporels Binance (sources secondaires divergentes) : s'en tenir au post-mortem.
11. Textes juridiques †: art. 321-1, 323-1 à 323-3-1 CP, NIS2 art. 23, LOPMI, calendrier du Cyber Resilience Act.
12. Parts de marché des stealers : à remplacer par un classement par métrique et par source.
13. Statut d'XSS.is après la saisie (relance/onion) et de BreachForums/DarkForums.
14. Contenus du suivi GTIG T4 2026 (exploitation à partir de correctifs) relayés par un site secondaire.
15. Prix et parts : toute valeur republiée doit porter sa date et sa source primaire.

### 8.7 Journal des changements par rapport au rapport source

| Domaine | Rapport source | Ce dossier |
| --- | --- | --- |
| Structure | 7 voiles, Parties/Actes entrelacés | 8 parties numérotées, renvois uniques |
| Preuves | Balises sans URL | Registre de preuves, cotes A–E, double source |
| Stealers | Rhadamanthys n°1 ; parts non sourcées | Vidar n°1 depuis nov. 2025 ; classement par métrique |
| Ransomware | LockBit 3.0, RansomHub, BlackSuit, 8Base | Qilin, The Gentlemen, Akira, INC, DragonForce ; LockBit 5.0 en recul |
| Forums | XSS et BreachForums « actifs » | XSS perturbé, BreachForums fragmenté, RAMP/LeakBase saisis |
| Endgame | Revendication d'éditeur | Chiffres Europol ; phase de nov. 2026 |
| IA | Voiles 3 à 6 non développés | §3.1 à §3.6 : LLM criminels, LLMjacking, Bizarre Bazaar, agents, skills/MCP, TeamPCP, Binance |
| Droit | NIS2 « en cours » | Vote du 17 nov. 2026, CJUE, CMP |
| Prospective | Scénarios A–E | + ACH, indicateurs mesurables, matrice de preuves |
| Usage | Pédagogique | Projet de veille : PIR, sources, gouvernance, développement |

---

*Ce dossier est une carte de travail : datée, sourcée autant que possible, et explicite sur ce qu'elle ne sait pas. L'édition narrative du rapport d'origine (prologue, métaphores, épilogue) reste valable comme pédagogie ; elle ne doit plus servir de base factuelle.*
