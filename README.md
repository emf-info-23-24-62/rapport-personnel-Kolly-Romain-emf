<h1>Module 323 — Programmer de manière fonctionnelle</h1>

# RP / Cheat-sheet (SIMPLE)

---

# 1) Reduce : la seule “mécanique” à savoir

## Reduce = une boucle qui construit une sortie
```js
const res = tableau.reduce((acc, item) => {
  // 1) tu modifies acc
  // 2) tu return acc
  return acc;
}, INIT);
```

## INIT = la forme de ta sortie
- **NOMBRE** (somme, compteur) → `0`
- **TABLEAU** (liste) → `[]`
- **OBJET** (groupBy, byId) → `{}`
- **MIN/MAX** → `{ min: null, max: null }`

⚠️ **Bug classique**
Si tu oublies `INIT`, `acc` = **premier élément** → résultat incohérent.

---

# 2) Helper Pack (copie-colle tel quel)

## 2.1 Dates → nombre (ms)
### Date FR `"JJ.MM.AAAA"` -> ms
```js
function frDateToTime(str) {
  const parts = str.split('.');
  const day = parseInt(parts[0], 10);
  const month = parseInt(parts[1], 10) - 1;
  const year = parseInt(parts[2], 10);
  return new Date(year, month, day).getTime();
}
```

### Date ISO `"YYYY-MM-DD"` -> ms
```js
function isoDateToTime(str) {
  const parts = str.split('-');
  const year = parseInt(parts[0], 10);
  const month = parseInt(parts[1], 10) - 1;
  const day = parseInt(parts[2], 10);
  return new Date(year, month, day).getTime();
}
```

---

## 2.2 String helpers (format)
### Nom en MAJUSCULES
```js
function upper(str) {
  return String(str).toUpperCase();
}
```

### Prénom “Capitalized”
```js
function capitalize(str) {
  const s = String(str).toLowerCase();
  if (s.length === 0) return s;
  return s[0].toUpperCase() + s.slice(1);
}
```

### Full name propre (NOM Prenom)
```js
function fullName(last, first) {
  return upper(last) + " " + capitalize(first);
}
```

---

## 2.3 Comparateurs “prêts”
### Alpha FR
```js
function cmpAlphaFr(a, b) {
  return a.localeCompare(b, "fr");
}
```

### Num ASC / DESC
```js
function cmpNumAsc(a, b) {
  return a - b;
}
function cmpNumDesc(a, b) {
  return b - a;
}
```

---

## 2.4 Helpers de tri “tu donnes un tableau → ça rend un nouveau tableau trié”

### Trier des strings (A → Z)
```js
function sortStringsFrAsc(arr) {
  const copy = arr.slice();
  copy.sort(cmpAlphaFr);
  return copy;
}
```

### Trier objets par texte (A → Z)
```js
function sortObjectsByTextKeyAsc(arr, key) {
  const copy = arr.slice();
  copy.sort((a, b) => {
    return String(a[key]).localeCompare(String(b[key]), "fr");
  });
  return copy;
}
```

### Trier objets par nombre (ASC)
```js
function sortObjectsByNumberKeyAsc(arr, key) {
  const copy = arr.slice();
  copy.sort((a, b) => {
    return Number(a[key]) - Number(b[key]);
  });
  return copy;
}
```

### Trier objets par nombre (DESC)
```js
function sortObjectsByNumberKeyDesc(arr, key) {
  const copy = arr.slice();
  copy.sort((a, b) => {
    return Number(b[key]) - Number(a[key]);
  });
  return copy;
}
```

### Trier objets par date FR (ASC)
```js
function sortObjectsByFrDateKeyAsc(arr, key) {
  const copy = arr.slice();
  copy.sort((a, b) => {
    return frDateToTime(a[key]) - frDateToTime(b[key]);
  });
  return copy;
}
```

### Trier objets par date ISO (ASC)
```js
function sortObjectsByIsoDateKeyAsc(arr, key) {
  const copy = arr.slice();
  copy.sort((a, b) => {
    return isoDateToTime(a[key]) - isoDateToTime(b[key]);
  });
  return copy;
}
```

### Trier sur 2 critères (date FR puis texte)
```js
function sortByFrDateThenText(arr, dateKey, textKey) {
  const copy = arr.slice();

  copy.sort((a, b) => {
    const da = frDateToTime(a[dateKey]);
    const db = frDateToTime(b[dateKey]);

    if (da < db) return -1;
    if (da > db) return 1;

    return String(a[textKey]).localeCompare(String(b[textKey]), "fr");
  });

  return copy;
}
```

---

## 2.5 Object.values (rappel)
Si tu as un objet “indexé par id” :
```js
const obj = {
  "IDC-001": { nom: "A", ca: 10 },
  "IDC-002": { nom: "B", ca: 5 }
};
```

`Object.values(obj)` donne :
```js
[
  { nom: "A", ca: 10 },
  { nom: "B", ca: 5 }
]
```

✅ C’est **le pont** pour passer de `byId {}` à un tableau triable (`sort`, `slice`).

---

# 3) Patterns EXAM (copier / adapter)

## P01 — Somme (NOMBRE)
**Ex : total km / total CA / total jours**
```js
const total = data.reduce((acc, x) => {
  acc = acc + x;
  return acc;
}, 0);
```

---

## P02 — Compter avec condition (NOMBRE)
```js
const count = data.reduce((acc, x) => {
  if (x >= 10) {
    acc = acc + 1;
  }
  return acc;
}, 0);
```

---

## P03 — Somme avec condition (NOMBRE)
**Ex : somme km seulement si type === "Moyenne"**
```js
const totalKm = locations.reduce((acc, loc) => {
  if (loc.vehicule.vehicule_type === "Moyenne") {
    acc = acc + loc.location.location_km;
  }
  return acc;
}, 0);
```

---

## P04 — Construire un TABLEAU (équivalent map)
```js
const names = people.reduce((acc, p) => {
  acc.push(p.last + " " + p.first);
  return acc;
}, []);
```

---

## P05 — Uniques simple (TABLEAU sans doublons)
```js
const types = data.reduce((acc, item) => {
  if (!acc.includes(item.type)) {
    acc.push(item.type);
  }
  return acc;
}, []);

const typesSorted = sortStringsFrAsc(types);
```

---

## P06 — Dataset MIXED (sale + location) : filtrer dans reduce
### Version “champ discriminant” (`kind`)
```js
const salesOnly = records.reduce((acc, item) => {
  if (item.kind !== "sale") return acc;
  acc.push(item);
  return acc;
}, []);
```

### Version “champ absent” (`date` existe que sur sales)
```js
const salesOnly2 = records.reduce((acc, item) => {
  if (!item.date) return acc;
  acc.push(item);
  return acc;
}, []);
```

---

## P07 — Min/Max sur date FR (JJ.MM.AAAA) (OBJET)
**Sortie attendue : strings**
```js
const mm = sales.reduce((acc, v) => {
  const t = frDateToTime(v.date);

  if (acc.min === null || t < acc.minTime) {
    acc.min = v.date;
    acc.minTime = t;
  }
  if (acc.max === null || t > acc.maxTime) {
    acc.max = v.date;
    acc.maxTime = t;
  }

  return acc;
}, { min: null, max: null, minTime: null, maxTime: null });

const result = { dateMin: mm.min, dateMax: mm.max };
```

---

## P08 — GroupBy (OBJET -> tableaux) + pas de doublons + tri
**Objectif :**
```js
// {
//   "Moyenne": ["Mazda 3 [IDV-A1] ...", ...],
//   "Grande":  [...]
// }
```

### 1) Construire le groupBy
```js
const grouped = locations.reduce((acc, loc) => {
  const type = loc.vehicule.vehicule_type;

  if (!acc[type]) {
    acc[type] = [];
  }

  const label =
    loc.vehicule.vehicule_nom +
    " [" + loc.vehicule.vehicule_id + "], à " +
    loc.vehicule.vehicule_prix_par_jour + " Frs/jour et " +
    loc.vehicule.vehicule_prix_par_km + " Frs/km";

  if (!acc[type].includes(label)) {
    acc[type].push(label);
  }

  return acc;
}, {});
```

### 2) Trier chaque tableau (SANS forEach)
```js
const groupedSorted = Object.keys(grouped).reduce((acc, key) => {
  const copy = grouped[key].slice();
  copy.sort(cmpAlphaFr);

  acc[key] = copy;
  return acc;
}, {});
```

---

## P09 — Le hack le plus rentable : indexer par ID + Object.values
### A) Client uniques (depuis locations)
```js
const byId = locations.reduce((acc, loc) => {
  const id = loc.client.client_id;

  if (!acc[id]) {
    acc[id] = {
      nom_prenom: fullName(loc.client.client_nom, loc.client.client_prenom),
      date_naissance: loc.client.client_date_naissance,
    };
  }

  return acc;
}, {});

const arr = Object.values(byId);
const sorted = sortObjectsByFrDateKeyAsc(arr, "date_naissance");
```

### B) Stats véhicules (CA, km, jours, locations)
```js
const byCar = locations.reduce((acc, loc) => {
  const v = loc.vehicule;
  const l = loc.location;
  const id = v.vehicule_id;

  if (!acc[id]) {
    acc[id] = {
      vehicule_id: id,
      vehicule_type: v.vehicule_type,
      vehicule_nom: v.vehicule_nom,
      vehicule_prix_par_jour: v.vehicule_prix_par_jour,
      vehicule_prix_par_km: v.vehicule_prix_par_km,
      ca: 0,
      locations: 0,
      jours: 0,
      km: 0
    };
  }

  // CA = jours*prix_jour + km*prix_km
  acc[id].ca = acc[id].ca + (l.location_jours * v.vehicule_prix_par_jour) + (l.location_km * v.vehicule_prix_par_km);
  acc[id].locations = acc[id].locations + 1;
  acc[id].jours = acc[id].jours + l.location_jours;
  acc[id].km = acc[id].km + l.location_km;

  return acc;
}, {});

const carsArr = Object.values(byCar);
const carsSorted = sortObjectsByNumberKeyDesc(carsArr, "ca");
```

⚠️ Bug classique : si tu recrées l’objet à chaque tour sans `if (!acc[id])` → tu remets `ca:0` → tu perds l’historique.

---

## P10 — Incidents (TABLEAU) triés par date ISO
```js
const incidents = locations.reduce((acc, loc) => {
  if (loc.location.has_incident === true) {
    acc.push({
      date: loc.location.location_date,
      vehicule: loc.vehicule.vehicule_nom + " [" + loc.vehicule.vehicule_id + "]",
      client: fullName(loc.client.client_nom, loc.client.client_prenom),
      details: loc.location.incident_details
    });
  }
  return acc;
}, []);

const sortedIncidents = sortObjectsByIsoDateKeyAsc(incidents, "date");
```

---

## P11 — CA d’une vente = somme prix produits (sous-tableau)
```js
function caVente(vente) {
  const total = vente.produits.reduce((sum, p) => {
    sum = sum + p.prix;
    return sum;
  }, 0);

  return total;
}
```

---

## P12 — CA par DATE (OBJET)
```js
const caByDate = sales.reduce((acc, vente) => {
  const date = vente.date;
  const ca = caVente(vente);

  if (!acc[date]) {
    acc[date] = 0;
  }

  acc[date] = acc[date] + ca;

  return acc;
}, {});
```

---

## P13 — CA par CLIENT (OBJET byId -> TABLEAU trié)
```js
const byClient = sales.reduce((acc, vente) => {
  const id = vente.client.id;

  if (!acc[id]) {
    acc[id] = {
      id: id,
      nom: upper(vente.client.nom),
      prenom: capitalize(vente.client.prenom),
      ca: 0
    };
  }

  acc[id].ca = acc[id].ca + caVente(vente);

  return acc;
}, {});

const arr = Object.values(byClient);
const sorted = sortObjectsByNumberKeyDesc(arr, "ca");
```

---

## P14 — TOP N (toujours le même plan)
Plan :
1) **byId** (objet)  
2) `Object.values()`  
3) `sort`  
4) `slice(0, N)`

### Exemple : TOP 3 produits par CA
```js
// 1) rassembler tous les produits (sans forEach)
const allProducts = sales.reduce((acc, vente) => {
  const produits = vente.produits;

  produits.reduce((acc2, p) => {
    acc2.push(p);
    return acc2;
  }, acc);

  return acc;
}, []);

// 2) byId + ca
const byProd = allProducts.reduce((acc, p) => {
  const id = p.id;

  if (!acc[id]) {
    acc[id] = { id: id, nom: p.nom, ca: 0 };
  }

  acc[id].ca = acc[id].ca + p.prix;

  return acc;
}, {});

// 3) values + tri + top
const arr = Object.values(byProd);
const sorted = sortObjectsByNumberKeyDesc(arr, "ca");
const top3 = sorted.slice(0, 3);
```

---

# 4) Mini-check “je lis l’énoncé” (ultra rapide)

1) **Format demandé ?**
- nombre → `0`
- tableau → `[]`
- objet → `{}`

2) **Filtre ?** (ex: kind, type, incident)
- si pas bon → `return acc;`

3) **Doublons ?**
- facile → `includes`
- mieux → index par id `{}` + `Object.values()`

4) **Tri ?**
- alpha → `localeCompare("fr")`
- nombre → `a - b` / `b - a`
- date FR → `frDateToTime(...)`
- date ISO → `isoDateToTime(...)`

5) **Top N ?**
- `sort` puis `slice(0, N)`

---

# 5) Erreurs qui te tuent (à éviter)

## 5.1 Oublier INIT
```js
// ❌ mauvais : acc devient le premier élément
numbers.reduce((acc, x) => { ... });

// ✅ bon
numbers.reduce((acc, x) => { ... }, 0);
```

## 5.2 Réinitialiser l’objet à chaque tour
```js
// ❌ mauvais : tu écrases tout le temps
acc[id] = { ca: 0 };

// ✅ bon : seulement si pas encore créé
if (!acc[id]) acc[id] = { ca: 0 };
```

## 5.3 Mélanger [] et {}
- `acc[id] = ...` → INIT doit être `{}` (objet)
- `acc.push(...)` → INIT doit être `[]` (tableau)

---

<img src="res/EMF_logo_RVB_Info_long.png" width="25%" style="margin-left:-20px;">
