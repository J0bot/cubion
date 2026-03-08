# Cubion — Équipe & Contribution

Cubion est 100% open source, 100% bénévole.
Pas de hiérarchie. Pas de salaire. Pas de bullshit.
Juste des gens qui veulent construire quelque chose d'open source qui durera des milliers d'années.

---

## Structure actuelle

### José (j0bot) — Fondateur & Architecte Vision
- Roadmap long terme
- Design de l'écosystème cube
- Miniaturisation et form factors
- Prototypage physique (fablab HEIG-VD)
- Communication et visibilité du projet

---

## Rôles recherchés

### Spécialiste Cybersécurité
**Ce qu'on cherche :**
- Quelqu'un qui pense "comment casser ça" avant "comment construire ça"
- Expérience en cryptographie appliquée (clés, chiffrement de flux, PKI)
- Idéalement : connaissance des protocoles embarqués, side-channel attacks

**Ce que tu ferais :**
- Définir le modèle de menace de Cubion
- Spécifier la couche crypto de CubionLink
- Concevoir le Cubion Personnel (cube identitaire + clé GPG physique)
- Proposer un standard de sécurité évolutif sur 100+ ans

---

### Développeur Firmware
**Ce qu'on cherche :**
- À l'aise en C/Rust embarqué
- Connaissance des protocoles (RS-485, 10BASE-T1S, ou volonté d'apprendre)
- Capable de travailler sur ESP32-S3, puis sur hardware custom

**Ce que tu ferais :**
- Implémentation de CubionLink (discovery, routing, resource sharing)
- Drivers pour les couches physiques
- Tests de communication inter-cubes
- Maintenance du firmware de référence

---

### Mainteneur / Dev OS / Software
**Ce qu'on cherche :**
- Linux systems, drivers, ou OS-level
- Ou : développeur qui veut apprendre ça

**Ce que tu ferais :**
- Intégration Linux de CubionLink (driver kernel ou userspace)
- Abstraction "ressource mesh" pour les applications
- Tooling CLI pour debug et config du mesh

---

### Designer Hardware / PCB
**Ce qu'on cherche :**
- KiCad ou équivalent
- Notion de contraintes mécaniques (50×50mm c'est petit)

**Ce que tu ferais :**
- Concevoir le PCB de la face CFC
- Layout du cube CPU v0.1
- Travailler avec José sur les form factors

---

## Comment rejoindre

Ouvre une issue sur GitHub avec le tag `[team]`.
Décris ce que tu sais faire et ce que tu veux faire.
Pas de CV requis. Pas d'entretien. On regarde le code et les idées.

---

## Valeurs de l'équipe

- **Liberté radicale** : chaque contributeur est souverain de son travail
- **Transparence totale** : tout est public, tout est documenté
- **Long terme** : on ne fait pas de compromis qui cassent la compat future
- **Pas de politique** : on build, on ne gère pas d'ego

---

> "On construit quelque chose qui doit exister dans 500 ans.
> Agis en conséquence."
