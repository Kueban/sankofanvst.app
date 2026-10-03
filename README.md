# sankofanvst.app

Site statique du domaine `sankofanvst.app` (B-49, B-70). Il est publié par GitHub Pages depuis le dépôt **public** dédié [Kueban/sankofanvst.app](https://github.com/Kueban/sankofanvst.app). Pages n'est gratuit que sur un dépôt public, et `sankofa-nvst` est privé.

La source fait foi dans `sankofa-nvst/site/sankofanvst.app/`. Le dépôt public n'en est qu'une copie, sans l'historique du dépôt privé.

## Contenu

- `index.html` : page d'accueil.
- `404.html` : sert les liens d'invitation `https://sankofanvst.app/join/<code>`. Le code est lu dans l'adresse, affiché comme texte, jamais interprété.
- `.well-known/assetlinks.json` : App Links Android de `com.philbank.SankofaTest`. Il porte l'empreinte SHA-256 du certificat de signature EAS.
- `CNAME` : domaine personnalisé. `.nojekyll` : publication telle quelle, y compris `.well-known/`.

Le bouton « Ouvrir dans Sankofa » vise `sankofa://join?code=…` (schéma déclaré dans `app.config.js`).

## Republier

Une fois la modification commitée dans `sankofa-nvst`, lancer depuis un terminal :

```sh
SRC="$HOME/Documents/SANKOFA NVST/sankofa-nvst/site/sankofanvst.app"
TMP=$(mktemp -d)
git clone -q https://github.com/Kueban/sankofanvst.app.git "$TMP/site"
rsync -a --delete --exclude .git "$SRC/" "$TMP/site/"
cd "$TMP/site" && git add -A && git commit -m "Mise à jour du site" && git push
```

GitHub Pages republie en une à deux minutes. Pour suivre la publication :

```sh
gh run list -R Kueban/sankofanvst.app --limit 1
```

Contrôles après publication (codes attendus : 200, 301 vers `https://sankofanvst.app/`, 200 en JSON, 404 avec la page d'invitation) :

```sh
for u in https://sankofanvst.app/ https://www.sankofanvst.app/ \
         https://sankofanvst.app/.well-known/assetlinks.json https://sankofanvst.app/join/TEST1234; do
  curl -sS -o /dev/null -w "$u → %{http_code} %{content_type} %{redirect_url}\n" "$u"
done
```

Une page d'invitation répond 404, c'est voulu : GitHub Pages sert `404.html` pour tout chemin inconnu, et le navigateur l'affiche normalement.

## DNS (Namecheap, domaine `sankofanvst.app`)

| Type | Hôte | Valeur |
|---|---|---|
| A | `@` | 185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153 |
| AAAA | `@` | 2606:50c0:8000::153, 2606:50c0:8001::153, 2606:50c0:8002::153, 2606:50c0:8003::153 |
| CNAME | `www` | kueban.github.io |

La section « Mail Settings » reste sur « Email Forwarding ».
