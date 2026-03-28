# Antigone - Répétition Mobile 🎭

Application interactive pour la répétition théâtrale — **Le Garde** (Antigone de Jean Anouilh)

## Technologie
- Single HTML file
- React 18 (CDN UMD) + Babel standalone + Tailwind CSS
- Web Speech API pour la synthèse vocale
- localStorage pour la persistance des réglages

---

## Interaction avec les répliques

### Répliques normales (Créon, Antigone, etc.)
Lecture automatique en séquence avec synchronisation des personnages

### Répliques du Garde (votre rôle)
Gestion avancée du texte et de la lecture avec possibilité de:
- **Masquer/révéler** le texte à la demande
- **Lire** avec pause/reprise par chunk
- **Réinitialiser** la position de lecture

#### Séquence de taps - Single-click (mode normal)

| Tap | État avant | Action |
|-----|-----------|--------|
| 1er | texte caché | → révèle le texte (reste révélé) |
| 2ème | `idle` | → démarre lecture (état `'playing'`) |
| 3ème+ | `playing` | → pause à position actuelle (état `'paused'`) |
| Follow | `paused` | → reprend depuis la pause |

#### Double-tap (< 300ms) — Reset sans lancer

```
Double-tap surface du texte:
  ✓ Annule toute lecture en cours (cancel + clearTimeout)
  ✓ Réinitialise chunks et index à 0
  ✓ Définit état à 'paused' (texte visible, prêt mais pas lancé)
  ✓ Tableau de bord: "▶ Cliquez pour reprendre" (prêt au play)
```

### Mode Souffleur (📝)
Révélation progressive du texte:
- **État caché** → "..." (3s countdown)
- **État indice** → "Deux premiers mots ..."
- **État texte complet** → Texte intégral
- **État audio** → Lecture en cours

---

## Contrôles

| Bouton | Fonction |
|--------|----------|
| ↻ | Recommencer la scène |
| ◀ | Réplique précédente |
| ▶ | Réplique suivante |
| ⚙ | Réglages (vitesse, voix) |
| 📌 | Marquer/démarquer réplique |
| ☰ | Voir tous les marque-pages |
| 📝 | Activer/désactiver le souffleur |

---

## Gestes tactiles

| Geste | Action |
|-------|--------|
| Swipe gauche | Suivant |
| Swipe droite | Précédent |
| Click texte (Garde) | Play/Pause/Reprendre (voir section Interaction) |
| Double-tap (Garde) | Reset position de lecture sans lancer |

---

## Réglages

### Vitesse de lecture
- Plage: 0.8× à 1.5× (par défaut: 1.15×)

### Pauses de ponctuation
- **Virgule/Point-virgule**: 0–0.5s (défaut: 0.25s)
- **Point/Exclamation/Interrogation**: 0–1.5s (défaut: 0.75s)

### Voix
- Sélection par personnage
- Filtrage automatique des voix françaises (French)
- Priorisation des voix premium (Google, Microsoft)

---

## Persistance des données

Tous les réglages/marque-pages sont sauvegardés automatiquement en `localStorage`:
- `antigoneTRAE_settings` → Vitesse, pauses, voix
- `antigoneTRAE_bookmarks` → Marque-pages
- `antigoneTRAE_prompter` → Réglages souffleur
- `antigoneTRAE_swipeHintSeen` → État de l'indicator swipe