# Dolibar – Intégration de la page d’administration Carto

Ce dépôt contient la page de présentation et de configuration associée au module cartographique. Ce guide explique comment un développeur peut intégrer ou lier la page d’administration Dolibarr suivante à son site web:

- URL cible: https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu=

## Prérequis
- Avoir un site web (WordPress, Next.js/React, PHP, autre) avec accès aux fichiers/templates.
- Vérifier que l’instance Dolibarr/URL est accessible publiquement.
- Noter que certaines politiques de sécurité (X-Frame-Options, Content-Security-Policy, CORS) peuvent empêcher l’intégration directe en iframe. Voir les alternatives ci-dessous.

## Options d’intégration

### 1) Lien simple (recommandé)
Affichez un bouton ou un lien menant vers la page d’administration Dolibarr.

```html
<a href="https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu="
   class="btn" target="_blank" rel="noopener">Ouvrir l’administration Carto</a>
```

Avantages:
- Compatible partout
- Aucun problème de headers de sécurité

### 2) Intégration via iframe (si autorisée)
Si le serveur autorise l’affichage dans un iframe, vous pouvez l’embarquer:

```html
<iframe src="https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu="
        style="width:100%;height:80vh;border:1px solid #e5e7eb;border-radius:12px"></iframe>
```

Attention:
- Si l’iframe ne s’affiche pas, c’est probablement bloqué par X-Frame-Options ou CSP côté Dolibarr.

### 3) Proxy inverse (solution robuste)
Servez la page via votre domaine pour éviter les blocages inter-domaines.

Exemple Nginx:
```nginx
location /carto-admin/ {
  proxy_pass https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu=;
  proxy_set_header Host dolibarr.liger-cd.com;
  proxy_set_header X-Forwarded-Proto $scheme;
}
```

Puis intégrez:
```html
<iframe src="https://votre-domaine.com/carto-admin/" style="width:100%;height:80vh;border:0"></iframe>
```

### 4) WordPress
- Lien simple: ajouter un bouton via l’éditeur.
- Iframe: utiliser un bloc “HTML personnalisé” ou un shortcode.

Shortcode exemple:
```php
function carto_admin_shortcode() {
  return '<iframe src="https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu=" style="width:100%;height:80vh;border:0"></iframe>';
}
add_shortcode('carto_admin', 'carto_admin_shortcode');
```
Puis insérez [carto_admin] dans la page.

### 5) Next.js / React
- Lien simple:
```jsx
import Link from 'next/link';
export default function CartoLink() {
  return <Link href="https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu=" target="_blank">Ouvrir l’administration Carto</Link>;
}
```
- Iframe:
```jsx
export default function CartoIframe() {
  return <iframe src="https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu=" style={{width:'100%',height:'80vh',border:0}} />;
}
```
- Proxy API (si headers bloquent):
```js
// pages/api/carto.js
export default async function handler(req, res) {
  const r = await fetch('https://dolibarr.liger-cd.com/custom/dolicommap/admin/admin.php?idmenu=129&mainmenu=dolicommap&leftmenu=');
  const html = await r.text();
  res.setHeader('Content-Type','text/html; charset=utf-8');
  res.status(200).send(html);
}
```
Puis:
```jsx
export default function CartoProxy() {
  return <iframe src="/api/carto" style={{width:'100%',height:'80vh',border:0}} />;
}
```

## Paramètres de l’URL
- idmenu, mainmenu, leftmenu: utilisés par Dolibarr pour le contexte du menu. Gardez-les tels quels sauf besoin spécifique.

## Sécurité & bonnes pratiques
- Toujours ouvrir les liens externes avec target="_blank" et rel="noopener".
- Si l’intégration iframe échoue, utilisez le lien simple ou un proxy inverse.
- Évitez d’exposer des identifiants/API dans le code public.

## Support
Pour toute question d’intégration ou d’ergonomie, ouvrez une issue sur le dépôt ou contactez l’équipe Liger. 
