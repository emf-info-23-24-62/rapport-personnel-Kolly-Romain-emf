<h1>🤔 RP - 323 - Programmation fonctionnelle</h1>

>[!TIP]
>**Référence Javascript:** <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference>  
>**Tester du code JS** : <https://runjs.app/play>  
>**Convertir en PDF** : <https://marketplace.visualstudio.com/items?itemName=manuth.markdown-converter>

<h1>Table des matières</h1>

- [Introduction](#introduction)
- [Opérateurs javascript super-cooool 😎](#opérateurs-javascript-super-cooool-)
  - [opérateur `?:`](#opérateur-)
  - [opérateur `??`](#opérateur--1)
  - [opérateur `??=`](#opérateur--2)
  - [opérateur de décomposition 'spread' `...`](#opérateur-de-décomposition-spread-)
  - [Déstructuration](#déstructuration)
- [Date et Heure](#date-et-heure)
  - [Obtenir la date et/ou heure actuelle](#obtenir-la-date-etou-heure-actuelle)
- [Math](#math)
  - [`Math.PI` - la constante π](#mathpi---la-constante-π)
  - [`Math.abs()` - la |valeur absolue| d'un nombre](#mathabs---la-valeur-absolue-dun-nombre)
  - [`Math.pow()` - élever à une puissance](#mathpow---élever-à-une-puissance)
  - [`Math.min()` - plus petite valeur](#mathmin---plus-petite-valeur)
  - [`Math.max()` - plus grande valeur](#mathmax---plus-grande-valeur)
  - [`Math.ceil()` - arrondir à la prochaine valeur entière la plus proche](#mathceil---arrondir-à-la-prochaine-valeur-entière-la-plus-proche)
  - [`Math.floor()` - arrondir à la précédente valeur entière la plus proche](#mathfloor---arrondir-à-la-précédente-valeur-entière-la-plus-proche)
  - [`Math.round()` - arrondir à la valeur entière la plus proche](#mathround---arrondir-à-la-valeur-entière-la-plus-proche)
  - [`Math.trunc()` - supprime la virgule et retourne la partie entière d'un nombre](#mathtrunc---supprime-la-virgule-et-retourne-la-partie-entière-dun-nombre)
  - [`Math.sqrt()` - la raçine carrée d'un nombre](#mathsqrt---la-raçine-carrée-dun-nombre)
  - [`Math.random()` - générer un nombre aléatoire entre 0.0 (compris) et 1.0 (non compris)](#mathrandom---générer-un-nombre-aléatoire-entre-00-compris-et-10-non-compris)
- [JSON](#json)
  - [`JSON.stringify()` - transformer un objet Javascript en JSON](#jsonstringify---transformer-un-objet-javascript-en-json)
  - [`JSON.parse()` - transformer du JSON en objet Javascript](#jsonparse---transformer-du-json-en-objet-javascript)
- [Chaînes de caractères](#chaînes-de-caractères)
  - [`split()` - un ciseau qui coupe une chaîne là où un caractère apparaît et produit un tableau](#split---un-ciseau-qui-coupe-une-chaîne-là-où-un-caractère-apparaît-et-produit-un-tableau)
  - [`trim()`, `trimStart()` et `trimEnd()` - épuration des espaces en trop dans une chaîne (trimming)](#trim-trimstart-et-trimend---épuration-des-espaces-en-trop-dans-une-chaîne-trimming)
  - [`padStart()` et `padEnd()` - aligner le contenu dans une chaîne de caractères](#padstart-et-padend---aligner-le-contenu-dans-une-chaîne-de-caractères)
- [Console](#console)
  - [`console.log()` - Afficher un message sur la console](#consolelog---afficher-un-message-sur-la-console)
  - [`console.info()`, `warn()` et `error()` - Afficher un message sur la console (filtrables)](#consoleinfo-warn-et-error---afficher-un-message-sur-la-console-filtrables)
  - [`console.table()` - Afficher tout un tableau ou un objet sur la console](#consoletable---afficher-tout-un-tableau-ou-un-objet-sur-la-console)
  - [`console.time()`, `timeLog()` et `timeEnd()` - Chronométrer une durée d'exécution](#consoletime-timelog-et-timeend---chronométrer-une-durée-dexécution)
- [Tableaux](#tableaux)
  - [`forEach` - parcourir les éléments d'un tableau](#foreach---parcourir-les-éléments-dun-tableau)
  - [`entries()` - parcourir les couples index/valeurs d'un tableau](#entries---parcourir-les-couples-indexvaleurs-dun-tableau)
  - [`in` - parcourir les clés d'un tableau](#in---parcourir-les-clés-dun-tableau)
  - [`of` - parcourir les valeurs d'un tableau](#of---parcourir-les-valeurs-dun-tableau)
  - [`find()` - premier élément qui satisfait une condition](#find---premier-élément-qui-satisfait-une-condition)
  - [`findIndex()` - premier index qui satisfait une condition](#findindex---premier-index-qui-satisfait-une-condition)
  - [`indexOf()` et `lastIndexOf()` - premier/dernier élément qui correspond](#indexof-et-lastindexof---premierdernier-élément-qui-correspond)
  - [`push()`, `pop()`, `shift()` et `unshift()` - ajouter/supprime au début/fin dans un tableau](#push-pop-shift-et-unshift---ajoutersupprime-au-débutfin-dans-un-tableau)
  - [`slice()` - ne conserver que certaines lignes d'un tableau](#slice---ne-conserver-que-certaines-lignes-dun-tableau)
  - [`splice()` - supprimer/insérer/remplacer des valeurs dans un tableau](#splice---supprimerinsérerremplacer-des-valeurs-dans-un-tableau)
  - [`concat()` - joindre deux tableaux](#concat---joindre-deux-tableaux)
  - [`join()` - joindre des chaînes de caractères](#join---joindre-des-chaînes-de-caractères)
  - [`keys()` et `values()` - les clés/valeurs d'un objet](#keys-et-values---les-clésvaleurs-dun-objet)
  - [`includes()` - vérifier si une valeur est présente dans un tableau](#includes---vérifier-si-une-valeur-est-présente-dans-un-tableau)
  - [`every()` et `some()` - vérifier si plusieurs valeurs sont toutes/quelques présentes dans un tableau](#every-et-some---vérifier-si-plusieurs-valeurs-sont-toutesquelques-présentes-dans-un-tableau)
  - [`fill()` - remplir un tableau avec des valeurs](#fill---remplir-un-tableau-avec-des-valeurs)
  - [`flat()` - aplatir un tableau](#flat---aplatir-un-tableau)
  - [`sort()` - pour trier un tableau](#sort---pour-trier-un-tableau)
  - [`map()` - tableau avec les résultats d'une fonction](#map---tableau-avec-les-résultats-dune-fonction)
  - [`filter()` - tableau avec les éléments passant un test](#filter---tableau-avec-les-éléments-passant-un-test)
  - [`groupBy()` - regroupe les éléments d'un tableau selon un règle](#groupby---regroupe-les-éléments-dun-tableau-selon-un-règle)
  - [`flatMap()` - chaînage de map() et flat()](#flatmap---chaînage-de-map-et-flat)
  - [`reduce()` et `reduceRight()` - réduire un tableau à une seule valeur](#reduce-et-reduceright---réduire-un-tableau-à-une-seule-valeur)
  - [`reverse()` - inverser l'ordre du tableau](#reverse---inverser-lordre-du-tableau)
- [Techniques](#techniques)
  - [``(backticks) - pour des expressions intelligentes](#backticks---pour-des-expressions-intelligentes)
  - [`new Set()` - pour supprimer les doublons](#new-set---pour-supprimer-les-doublons)
- [Fonctions](#fonctions)
  - [Déclaration de fonction](#déclaration-de-fonction)
  - [Fonctions immédiatement invoquées (IIFE)](#fonctions-immédiatement-invoquées-iife)
- [Conclusion](#conclusion)

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Introduction

Ce module m’a permis de découvrir la programmation fonctionnelle en JavaScript à travers des cas concrets, notamment l’analyse d’un gros jeu de données de notes scolaires (`jsonData`).  
Les objectifs principaux pour moi :

- Comprendre la différence entre approche impérative et fonctionnelle.
- Manipuler les tableaux avec `map`, `filter`, `reduce`, etc.
- Limiter les effets de bord en privilégiant des fonctions pures.
- Travailler avec des données réelles (notes, élèves, branches) et en extraire des infos utiles.
- Gagner en lisibilité, maintenabilité et réutilisabilité dans mon code.

L’idée générale : prendre des données comme celles de `jsonData.evaluations` et, grâce aux outils Javascript modernes, construire rapidement des statistiques, tris, regroupements et filtrages sans tout recoder à la main avec des boucles partout.

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Opérateurs javascript super-cooool 😎

## opérateur `?:`

Expression conditionnelle courte.

```javascript
const note = 5.2;
const resultat = note >= 4 ? 'réussi' : 'échec'; // 'réussi'
```

## opérateur `??`

Renvoie la valeur de droite seulement si celle de gauche vaut `null` ou `undefined`.

```javascript
const branche = null ?? 'Inconnue'; // 'Inconnue'
const note = 0 ?? 4;                // 0 (0 est accepté)
```

⚠️ Contrairement à `||`, les valeurs `0`, `''` ou `false` ne sont pas remplacées.

```javascript
const a = 0 || 4;  // 4  (dangereux si 0 est valide)
const b = 0 ?? 4;  // 0  (correct ici)
```

## opérateur `??=`

Assigne une valeur uniquement si la variable est `null` ou `undefined`.

```javascript
const config = { seuilReussite: null };

config.seuilReussite ??= 4; // devient 4
config.mode ??= 'standard'; // devient 'standard'
```

## opérateur de décomposition 'spread' `...`

Copier, fusionner et étendre facilement des tableaux / objets.

```javascript
const base = ['Français', 'Maths'];
const options = ['Physique', 'Chimie'];

const toutesBranches = [...base, ...options];

const eleve = { nom: "TERNET", prenom: "Alain" };
const details = { classe: "EMF", annee: "2024-2025" };

const eleveComplet = { ...eleve, ...details };
```

## Déstructuration

Extraire rapidement certaines valeurs et récupérer le reste.

```javascript
const [premiereEval, ...autres] = jsonData.evaluations;
const { etablissement, annee_scolaire } = jsonData;
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Date et Heure

Lien : <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Date>

## Obtenir la date et/ou heure actuelle

```javascript
const maintenant = new Date();

console.log(maintenant.toLocaleDateString());
console.log(maintenant.toLocaleTimeString());

const jour = maintenant.getDate();
const mois = maintenant.getMonth() + 1; // janvier = 0
const annee = maintenant.getFullYear();

console.log(`${jour}.${mois}.${annee}`);
console.log(maintenant.toISOString());
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Math

Lien : <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Math>

## `Math.PI` - la constante π

Accès direct à π, utile pour les calculs géométriques.

```javascript
console.log(Math.PI); // 3.141592653589793
```

## `Math.abs()` - la |valeur absolue| d'un nombre

Retire le signe, utile pour les écarts.

```javascript
const ecart = Math.abs(4 - 5.7); // 1.7
```

## `Math.pow()` - élever à une puissance

`Math.pow(base, exposant)` ou `base ** exposant`.

```javascript
console.log(Math.pow(2, 3)); // 8
console.log(2 ** 3);         // 8
```

## `Math.min()` - plus petite valeur

```javascript
console.log(Math.min(3, 7, -2, 0)); // -2
```

## `Math.max()` - plus grande valeur

```javascript
console.log(Math.max(3, 7, -2, 0)); // 7
```

## `Math.ceil()` - arrondir à l'entier supérieur

```javascript
console.log(Math.ceil(4.2)); // 5
```

## `Math.floor()` - arrondir à l'entier inférieur

```javascript
console.log(Math.floor(4.9)); // 4
```

## `Math.round()` - arrondir à l'entier le plus proche

```javascript
console.log(Math.round(4.4)); // 4
console.log(Math.round(4.6)); // 5
```

## `Math.trunc()` - partie entière

```javascript
console.log(Math.trunc(4.9));  // 4
console.log(Math.trunc(-4.9)); // -4
```

## `Math.sqrt()` - racine carrée

```javascript
console.log(Math.sqrt(9)); // 3
```

## `Math.random()` - nombre aléatoire `[0, 1)`

```javascript
const tirage = Math.random(); // ex: 0.37
const noteRandom = Math.round(1 + Math.random() * 5); // 1 à 6
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# JSON

Lien : <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/JSON>

## `JSON.stringify()` - transformer un objet Javascript en JSON

Pour stocker ou transmettre des données.

```javascript
const extrait = {
  etablissement: jsonData.etablissement,
  nbEvaluations: jsonData.evaluations.length
};

console.log(JSON.stringify(extrait));
```

## `JSON.parse()` - transformer du JSON en objet Javascript

Pour relire une chaîne JSON.

```javascript
const texteJson = '{"nom":"TARISTE","note":5.9}';
const evalObj = JSON.parse(texteJson);

console.log(evalObj.nom);
console.log(evalObj.note);
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Chaînes de caractères

Lien : <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/String>

## `split()` - découper en tableau

```javascript
const date = "19.08.2024";
const [jour, mois, annee] = date.split('.');
```

## `trim()`, `trimStart()` et `trimEnd()`

```javascript
const nomSale = "  VOYANTE  ";
nomSale.trim();      // "VOYANTE"
nomSale.trimStart(); // "VOYANTE  "
nomSale.trimEnd();   // "  VOYANTE"
```

## `padStart()` et `padEnd()`

```javascript
const num = '5';
num.padStart(3, '0'); // '005'
num.padEnd(4, '-');   // '5---'
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Console

Lien : <https://developer.mozilla.org/fr/docs/Web/API/console>

## `console.log()`

```javascript
console.log('Début de l’analyse des évaluations');
```

## `console.info()`, `warn()`, `error()`

```javascript
console.info('Chargement terminé.');
console.warn('Aucune note pour cette branche.');
console.error('Erreur critique.');
```

## `console.table()`

```javascript
console.table(jsonData.evaluations.slice(0, 5));
```

## `console.time()`, `timeLog()`, `timeEnd()`

```javascript
console.time('calcul');

const moyenne = jsonData.evaluations
  .filter(e => e.branche === 'Maths')
  .reduce((sum, e) => sum + e.note, 0) / jsonData.evaluations.filter(e => e.branche === 'Maths').length;

console.timeLog('calcul');
console.timeEnd('calcul');
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Tableaux

Lien : <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array>

```javascript
const evaluations = jsonData.evaluations;
```

## `forEach`

```javascript
evaluations.slice(0, 3).forEach(e => {
  console.log(`${e.nom} ${e.prenom} : ${e.note}`);
});
```

## `entries()`

```javascript
for (const [index, evalObj] of evaluations.slice(0, 3).entries()) {
  console.log(index, evalObj.nom, evalObj.note);
}
```

## `in`

Parcourt les index d'un tableau.

```javascript
for (const i in evaluations.slice(0, 3)) {
  console.log(i); // 0, 1, 2
}
```

## `of`

Parcourt directement les valeurs.

```javascript
for (const evalObj of evaluations.slice(0, 3)) {
  console.log(evalObj.nom, evalObj.note);
}
```

## `find()`

```javascript
const premiereSous4 = evaluations.find(e => e.note < 4);
```

## `findIndex()`

```javascript
const indexVoyante = evaluations.findIndex(e => e.nom === 'VOYANTE');
```

## `indexOf()` / `lastIndexOf()`

Sur un tableau de valeurs simples.

```javascript
const branches = ['Maths', 'Physique', 'Maths', 'Français'];
branches.indexOf('Maths');     // 0
branches.lastIndexOf('Maths'); // 2
```

## `push()`, `pop()`, `shift()`, `unshift()`

```javascript
const notes = [4, 5];
notes.push(5.5);
notes.pop();
notes.unshift(3);
notes.shift();
```

## `slice()`

```javascript
const top5 = evaluations.slice(0, 5);
```

## `splice()`

```javascript
const copie = [...evaluations];
copie.splice(0, 1); // supprime le premier
```

## `concat()`

```javascript
const a = evaluations.slice(0, 2);
const b = evaluations.slice(2, 4);
const fusion = a.concat(b);
```

## `join()`

```javascript
const noms = ['VOYANTE', 'TERNET', 'TARISTE'];
noms.join(', '); // "VOYANTE, TERNET, TARISTE"
```

## `keys()` et `values()` (objets)

```javascript
const exemple = evaluations[0];
Object.keys(exemple);   // ['date','nom','prenom','branche','note']
Object.values(exemple); // [...]
```

## `includes()`

```javascript
const toutesBranches = evaluations.map(e => e.branche);
toutesBranches.includes('Maths');
```

## `every()` et `some()`

```javascript
evaluations.every(e => e.note >= 4); // tous réussis ?
evaluations.some(e => e.note < 3);   // au moins un gros échec ?
```

## `fill()`

```javascript
new Array(3).fill('en attente');
```

## `flat()`

```javascript
[ [1, 2], [3, 4] ].flat(); // [1, 2, 3, 4]
```

## `sort()`

```javascript
const notesTriees = [...evaluations]
  .map(e => e.note)
  .sort((a, b) => a - b);
```

## `map()`

```javascript
const nomsComplets = evaluations.map(e => `${e.nom} ${e.prenom}`);
```

## `filter()`

```javascript
const maths = evaluations.filter(e => e.branche === 'Maths');
const echec = evaluations.filter(e => e.note < 4);
```

## `groupBy()`

```javascript
const parBranche = Object.groupBy(evaluations, e => e.branche);
// parBranche['Maths'] → toutes les évaluations de Maths
```

## `flatMap()`

```javascript
const elevesUniques = [...new Set(
  evaluations.flatMap(e => `${e.nom} ${e.prenom}`)
)];
```

## `reduce()` / `reduceRight()`

```javascript
const sommeNotes = evaluations.reduce((sum, e) => sum + e.note, 0);
const moyenneGlobale = sommeNotes / evaluations.length;
```

## `reverse()`

```javascript
const ordreInverse = [...evaluations].reverse();
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Techniques

## `` (backticks) - templates

```javascript
const e = evaluations[0];
`Le ${e.date}, ${e.prenom} ${e.nom} a obtenu ${e.note} en ${e.branche}.`;
```

## `new Set()` - supprimer les doublons

```javascript
const eleves = [...new Set(evaluations.map(e => `${e.nom} ${e.prenom}`))].sort();
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Fonctions

## Déclaration de fonction

```javascript
function moyenneBranche(branche) {
  const notes = evaluations.filter(e => e.branche === branche);
  return notes.reduce((sum, e) => sum + e.note, 0) / notes.length;
}

const moyenneEleve = function (nom, prenom) {
  const notes = evaluations.filter(e => e.nom === nom && e.prenom === prenom);
  return notes.reduce((sum, e) => sum + e.note, 0) / notes.length;
};

const estReussi = (note) => note >= 4;

const labelNote = (note) => note >= 4 ? 'OK' : 'NOK';
```

## Fonctions immédiatement invoquées (IIFE)

```javascript
(() => {
  const nbEvaluations = evaluations.length;
  console.log(`Nombre total d'évaluations : ${nbEvaluations}`);
})();
```

<svg height="12" width="100%" style="padding-top:2em;padding-bottom:1em">
  <rect y="5" width="100%" height="5" fill="#7191B8"/>
</svg>

# Conclusion

Ce module m’a obligé à structurer ma manière de coder.  
Travailler sur un gros jeu de données comme `jsonData` m’a montré l’intérêt concret de la programmation fonctionnelle :

- parcourir, filtrer et transformer des tableaux sans multiplier les boucles imbriquées ;
- écrire des fonctions courtes, réutilisables et prévisibles ;
- utiliser des méthodes comme `map`, `filter`, `reduce`, `groupBy`, combinées aux opérateurs modernes (`...`, `??`, backticks, `new Set`) pour obtenir rapidement les infos utiles.

Ces outils rendent le code plus clair, plus compact et plus simple à maintenir, surtout lorsqu’il s’agit de manipuler des données réelles comme des résultats scolaires.
