# Guide rapide — Ajouter des icônes au menu (avec couleurs et code)

## Palette de couleurs utilisée
- Bleu marque (fond icône/tooltip): `#273d5b`
- Autres couleurs vues dans le projet (pour repère):
  - Bleu gradient (UI): `#3b82f6`
  - Vert gradient (UI): `#22c55e`
  - Doré (accents): `#f59e0b`

## Composant icône du menu
L’icône est un conteneur carré arrondi qui accueille un SVG en ligne:

```html
<div class="iconAcc">
</div>
```

Styles associés (déjà présents dans les pages):
- Normal: `width:40px; height:40px; border-radius:12px; background:#273d5b;`
- SVG interne: `width:22px; height:22px; stroke:#fff`
- Menu replié: `width:36px; height:36px`

## Ajouter un bouton de menu avec icône + tooltip
1) Dupliquez un lien existant et adaptez le libellé:
```html
<a href="votre-page.html" class="flex items-center gap-3 px-3 h-11 hover:bg-slate-50 transition"
   data-tip="Texte du tooltip">
  <div class="iconAcc">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
      <circle cx="12" cy="12" r="3"></circle>
      <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 1 1-4 0v-.09a1.65 1.65 0 0 0-1-1.51 1.65 1.65 0 0 0-1.82.33l-.06.06A2 2 0 1 1 3.4 17l.06-.06A1.65 1.65 0 0 0 3 15.4a1.65 1.65 0 0 0-1.51-1H1a2 2 0 1 1 0-4h.09a1.65 1.65 0 0 0 1.51-1 1.65 1.65 0 0 0-.33-1.82l-.06-.06A2 2 0 1 1 5.04 3.4l.06.06A1.65 1.65 0 0 0 6.92 3a1.65 1.65 0 0 0 1-1.51V1a2 2 0 1 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06A2 2 0 1 1 20.6 6l-.06.06A1.65 1.65 0 0 0 20.99 8c.02.48.18.94.48 1.32.32.41.53.92.53 1.46s-.21 1.05-.53 1.46c-.3.38-.46.84-.48 1.32z"/>
    </svg>
  </div>
  <span class="text-sm font-semibold navLabel">Mon module</span>
</a>
```

2) Le tooltip s’affiche automatiquement quand le menu est replié grâce à:
```css
.menuCollapsed aside nav a[data-tip]::after {
  content: attr(data-tip);
  position: absolute;
  left: 74px;
  top: 50%;
  transform: translateY(-50%);
  background: #273d5b;
  color: #fff;
  font-size: 12px;
  padding: 6px 8px;
  border-radius: 8px;
  white-space: nowrap;
  box-shadow: 0 8px 24px rgba(15,23,42,.18);
  opacity: 0;
  pointer-events: none;
  transition: opacity .15s ease;
  z-index: 9999;
}
.menuCollapsed aside nav a:hover::after { opacity: 1; }
```

3) Conditions pour que le tooltip soit visible:
- Le body contient la classe `menuCollapsed` (gérée par le script de bascule).
- Les wrappers proches n’ont pas `overflow: hidden` côté menu; utilisez `overflow-visible` si besoin.
- La sidebar a un `position: relative` et un `z-index` suffisant (ex. `z-index: 50`).

## Réutiliser le style d’icône
Vous pouvez copier n’importe quel SVG simple (pictos type Feather) et le coller dans `<div class="iconAcc">...</div>`. Il sera automatiquement:
- Centré,
- Redimensionné (22×22),
- Coloré en blanc (`stroke:#fff`) sur fond bleu `#273d5b`.

## Bibliothèque d’icônes conseillée
- Style: Feather (traits simples, 24×24, arrondis).
- Attributs recommandés sur le SVG: `viewBox="0 0 24 24"`, `fill="none"`, `stroke="currentColor"`, `stroke-width="2"`, `stroke-linecap="round"`, `stroke-linejoin="round"`.

## Exemples prêts à copier/coller
Accueil (DOLIcomMAP):
```html
<a href="index.html" class="flex items-center gap-3 px-3 h-11 hover:bg-slate-50 transition" data-tip="Accueil">
  <div class="iconAcc">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M3 9l9-7 9 7"/>
      <path d="M9 22V12H15V22"/>
      <path d="M21 22H3"/>
    </svg>
  </div>
  <span class="text-sm font-semibold navLabel">DOLIcomMAP</span>
  </a>
```

Carte des tiers:
```html
<a href="map.html" class="flex items-center gap-3 px-3 h-11 hover:bg-slate-50 transition" data-tip="Carte des tiers">
  <div class="iconAcc">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <polygon points="1 6 8 3 16 6 23 3 23 17 16 20 8 17 1 20 1 6"/>
      <line x1="8" y1="17" x2="8" y2="3"/>
      <line x1="16" y1="20" x2="16" y2="6"/>
    </svg>
  </div>
  <span class="text-sm font-semibold navLabel">Carte des tiers</span>
</a>
```

Analyses (bar-chart-2):
```html
<a href="#" class="flex items-center gap-3 px-3 h-11 hover:bg-slate-50 transition" data-tip="Analyses">
  <div class="iconAcc">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <line x1="18" y1="20" x2="18" y2="10"></line>
      <line x1="12" y1="20" x2="12" y2="4"></line>
      <line x1="6" y1="20" x2="6" y2="14"></line>
    </svg>
  </div>
  <span class="text-sm font-semibold navLabel">Analyses</span>
</a>
```

Configuration (sliders):
```html
<a href="#parametres-carte" class="flex items-center gap-3 px-3 h-11 hover:bg-slate-50 transition" data-tip="Configuration">
  <div class="iconAcc">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <line x1="4" y1="21" x2="4" y2="14"></line>
      <line x1="4" y1="10" x2="4" y2="3"></line>
      <line x1="12" y1="21" x2="12" y2="12"></line>
      <line x1="12" y1="8" x2="12" y2="3"></line>
      <line x1="20" y1="21" x2="20" y2="16"></line>
      <line x1="20" y1="12" x2="20" y2="3"></line>
      <line x1="1" y1="14" x2="7" y2="14"></line>
      <line x1="9" y1="8" x2="15" y2="8"></line>
      <line x1="17" y1="16" x2="23" y2="16"></line>
    </svg>
  </div>
  <span class="text-sm font-semibold navLabel">Configuration</span>
</a>
```

## Références dans le projet
- Styles/usage côté accueil: `index.html`
- Styles/usage côté carte: `map.html`

Ouvrir ces fichiers pour voir des exemples complets de liens de menu avec icônes et tooltips.
