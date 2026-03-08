# Cubion — Vision Complète de l'Écosystème

> Ce document est la vision long terme. Pas un cahier des charges.
> Certaines choses ici seront réalisées dans 5 ans. D'autres dans 500 ans.

---

## Principe fondamental

**Tout est cube. Chaque cube est un centre.**

Il n'y a pas de cube "principal". Pas de serveur central. Pas de hub obligatoire.
Chaque cube est souverain. Les cubes se connectent, forment un mesh, se séparent.
Le mesh existe tant que les cubes sont ensemble. Il disparaît proprement quand ils se séparent.

---

## Catalogue de cubes

### Computing
- **Cube CPU** — processeur + RAM + stockage minimal. Le cube de base.
- **Cube GPU** — accélération graphique et compute parallèle
- **Cube RAM** — mémoire supplémentaire partagée sur le mesh
- **Cube Stockage** — SSD/NVMe dédié, exposé comme ressource réseau
- **Cube FPGA** — logique reconfigurable pour cas d'usage spécialisés
- **Cube Quantique** *(futur lointain)* — nœud de computing quantique dans le mesh classique

### Interface
- **Cube Écran** — un cube = un tile. Plusieurs cubes écran = écran de n'importe quelle taille et forme
- **Cube Caméra** — capture vidéo/photo
- **Cube Micro** — entrée audio
- **Cube Speaker** — sortie audio, peut former un système multi-point avec plusieurs cubes
- **Cube Clavier** / **Cube Trackpad**
- **Cube LED** — feedback visuel, état du mesh, notifications

### Connectivité
- **Cube WiFi** — accès réseau sans fil
- **Cube Bluetooth** — périphériques BT
- **Cube RJ45** — Ethernet filaire
- **Cube USB-A / USB-C** — connexion périphériques legacy
- **Cube HDMI / DisplayPort** — output vidéo vers écrans externes
- **Cube GPIO** — pour makers, capteurs, robots
- **Cube NFC / RFID** — paiement, identification
- **Cube SDR** *(pour les fous)* — radio définie par logiciel

### Énergie
- **Cube Batterie** — batterie lithium dédiée, alimente tout le mesh via CFC
- **Cube Secteur** — prise murale → CFC, charge tout le mesh depuis un seul point
- **Cube Solaire** — panneau solaire intégré, recharge passive
- **Cube Clock** — horloge atomique de référence. Synchronise tout le mesh. Jamais éteint.
- **Cube Atomique** *(futur)* — source d'énergie longue durée. Chaque cube toujours légèrement alimenté.

### Personnel
- **Cubion Personnel** — ton cube identitaire. Clé GPG physique. Tes données chiffrées. Toujours sur toi.
  - Format : collier, bracelet, accroché à une coque
  - Pose-le sur n'importe quel mesh Cubion → tu t'authentifies, tes ressources sont disponibles
  - Retire-le → tout se ferme proprement

### Adaptateurs (ponts vers l'existant)
Tous les ports existants deviennent accessibles via un cube adaptateur.
Côté CFC : cube normal. Côté sortie : port legacy.
Le mesh voit le port comme une ressource : "USB3.0 x2 disponible sur CubeUID:0x4A2F..."

---

## Form factors

### Coque
La coque ne compute rien. Elle tient les cubes en place dans une forme utile.
N'importe qui peut designer une coque. Open source aussi.

- **Coque Téléphone** — 6-8 cubes. Écran + CPU + batterie + caméra + connectivité = smartphone.
- **Coque Laptop** — ~20 cubes. Clavier intégré + cubes libres configurables.
- **Coque PC Bureau** — de 10 à 1000 cubes selon les besoins.
- **Coque Serveur** — racks de cubes pour infrastructure.
- **Coque Wearable** — collier, bracelet, vêtement avec poches cube.
- **Coque Véhicule** — cubes moteur, roues, capteurs = robot ou véhicule.

### Fabrication
- L'enclos est imprimable en 3D chez soi.
- Le connecteur CFC est la seule pièce critique — commandable séparément.
- Les PCB internes sont open source — fabriquables via JLCPCB, PCBWay, etc.
- Résultat : quelqu'un avec une imprimante 3D et ~50 CHF peut faire son premier cube.

---

## Énergie distribuée

Chaque cube contient une micro-batterie interne (assez pour maintenir la clock et l'identité).
Les cubes batterie dédiés alimentent le mesh via les faces CFC.

**Règle de charge :** une seule prise secteur sur n'importe quel cube du mesh charge tout le réseau.
L'énergie circule de face en face. Le cube qui a le plus de jus partage avec ses voisins.

**Mur de maison :** tes cubes sont posés dans des encoches murales (comme des prises électriques, mais CFC). Tu prends des cubes batterie le matin, ils sont chargés. Tu les poses le soir, ils se rechargent.

---

## Sécurité — le pilier le plus important

> Ton cube personnel = ta clé. Si tu l'as, tu peux tout déchiffrer. Si tu ne l'as pas, personne ne peut.

- Chiffrement de bout en bout natif dans le protocole CubionLink. Pas optionnel.
- Chaque cube a un identifiant cryptographique unique (pas juste un UID ROM).
- Le Cubion Personnel contient la clé GPG maître de l'utilisateur.
- Les données qui transitent entre cubes sont chiffrées par défaut.
- Standard de sécurité conçu pour être **upgradeable sans casser la compat** — les algorithmes vieillissent, le mécanisme de négociation reste.
- Objectif : un standard de sécurité viable pour les 1000-2000 prochaines années.

**Ce que ça signifie concrètement :**
- Dans le train → personne ne peut lire tes données même s'il a un cube identique
- Paiement → ton cube NFC + ta clé GPG = transaction signée localement, jamais sur un serveur
- Vie privée → pas de cloud obligatoire, pas de compte, pas de télémétrie

---

## Décentralisation radicale

Pas de centre. Chaque personne est son propre centre.

Les groupes de cubes partagés (famille, entreprise, coloc) sont possibles — mais opt-in explicite.
La valeur par défaut est : **tes cubes t'appartiennent, à toi seul.**

---

## Roadmap de miniaturisation

| Génération | Taille | Connecteur | Contenu typique |
|---|---|---|---|
| Gen 0 (prototype) | 50×50×50mm | CFC Standard | ESP32-S3, batterie 18650 |
| Gen 1 | 50×50×50mm | CFC Standard v1.0 figé | SBC custom, LiPo |
| Gen 2 | 30×30×30mm | CFC Mini | Module compute dédié |
| Gen 3 | 20×20×20mm | CFC Nano | Chip-on-board |
| Gen N *(futur)* | miniaturisation infinie | Adaptateurs compat | Ce qu'on ne peut pas encore imaginer |

Chaque génération est compatible avec la précédente via adaptateur.
La miniaturisation concerne l'intérieur et le form factor — jamais le standard de connexion.

---

## Vision maximale

Des cubes dans ta poche. Des cubes sur ton corps. Des cubes dans ta maison.
Des cubes qui forment un téléphone. Des cubes qui forment un PC.
Des cubes qui forment un robot. Des cubes qui forment un véhicule.
Des cubes qui forment un datacenter dans une cave.
Des cubes qui forment une station spatiale.

Un cube de 2027 qui fonctionne encore en 2527.

**Tout est cube. Chaque cube dure éternellement.**
