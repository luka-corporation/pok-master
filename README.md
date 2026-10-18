# PokéMasters

Le WikiMasters des Pokémon : un site de cartes à collectionner 100 % gratuit, en français.
Collectez les 1025 Pokémon sous forme de cartes, ouvrez des paquets, échangez, enchérissez
et rejoignez une guilde.

## Stack

- **Next.js 16** (App Router, React 19, Turbopack) + **Tailwind CSS 4**
- **Prisma 7** + SQLite (`dev.db`) via `@libsql/client`
- **Auth** : JWT signé (jose) dans un cookie httpOnly, mots de passe bcryptjs
- **Temps réel** : polling SWR (enchères, messages, notifications)
- **Données** : import PokéAPI (1025 Pokémon)

## Démarrage

```bash
npm install
npx prisma migrate dev     # crée dev.db
npm run db:seed            # import PokéAPI (cache .cache/pokeapi/, ~1 min)
npm run dev                # http://localhost:3000
```

## Commandes utiles

| Commande             | Rôle                                          |
| -------------------- | --------------------------------------------- |
| `npm run dev`        | Serveur de développement                      |
| `npm run build`      | Build de production                           |
| `npm run lint`       | ESLint                                         |
| `npm run db:seed`    | (Ré)importe les Pokémon et les succès         |
| `npm run db:reset`   | Remet la base à zéro (puis relancer le seed)  |
| `npm run generate`   | Régénère le client Prisma                     |

## Règles du jeu

- **Paquets** : 5 cartes toutes les 10 min (max 10 en stock). Nouveau joueur : 500 Pièces + 1 paquet.
  Défausser une carte rapporte 1 Pièce.
- **Raretés** (basées sur le total des stats) : Commun < 300, Peu commun < 350, Rare < 400,
  Super rare < 500, Ultra rare < 600, Légendaire ≥ 600 (ou Pokémon légendaire/mythique).
  Variante brillante : 1 % sur les Légendaires.
- **Échanges** : proposez/cartes contre cartes ; l'acceptation transfère les deux côtés.
- **Enchères** : déduction immédiate, remboursement du surenchérisseur dépassé,
  anti-snipe (+2 min dans les 2 dernières minutes), règlement automatique à l'expiration.
- **Cartes engagées** : une carte mise en enchère ou proposée en échange est verrouillée
  jusqu'au règlement.

## Structure

```
prisma/schema.prisma     15 models (User, Pokemon, Card, Trade, Auction, Guild…)
src/lib/                 db, auth, game (paquets/raretés), auctions, achievements…
src/app/api/             routes REST (auth, packs, collection, trades, auctions…)
src/app/(app)/           pages protégées (paquets, collection, échanges…)
src/components/          PokemonCard, Nav, Toaster, CardActions
scripts/import.ts        pipeline d'import PokéAPI
```

---

Pokémon est une marque de Nintendo / Creatures Inc. / GAME FREAK inc. Ce projet est un
hommage non officiel et gratuit, sans monétisation.
