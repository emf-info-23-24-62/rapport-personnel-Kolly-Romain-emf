<h1>Module 323 - Programmer de manière fonctionnelle</h1>

# 👍 RP / `Cheat-sheet` perso 🍒

# CheatList Module 323 — Version SIMPLE (copier / adapter)
Objectif : tu regardes l’exercice → tu prends le **pattern** → tu changes juste les chemins (data.xxx).

---

## 0) Le seul truc à retenir sur `reduce()`

### Reduce = une boucle
```js
tableau.reduce((acc, item) => {
  // 1) tu modifies acc
  // 2) tu return acc
  return acc;
}, INIT);
```

✅ **INIT = la forme de ta sortie**
- Sortie **nombre** → `INIT = 0`
- Sortie **tableau** → `INIT = []`
- Sortie **objet** → `INIT = {}`
- Sortie **min/max** → `INIT = { min: ..., max: ... }`

⚠️ Si tu oublies `INIT`, `acc` devient le **premier élément** du tableau → résultats absurdes (ex: tu comptes “à partir de 4”).

---

## 1) Helpers indispensables (copie-colle tel quel)
### 1.1 Dates FR `JJ.MM.AAAA`
```js
function parseFrDate(str) {
  // "05.01.2026" -> Date(2026, 0, 5)
  const parts = str.split('.');
  const day = parseInt(parts[0], 10);
  const month = parseInt(parts[1], 10) - 1;
  const year = parseInt(parts[2], 10);
  return new Date(year, month, day);
}

function formatFrDate(dateObj) {
  const d = String(dateObj.getDate()).padStart(2, '0');
  const m = String(dateObj.getMonth() + 1).padStart(2, '0');
  const y = dateObj.getFullYear();
  return d + '.' + m + '.' + y;
}
```

### 1.2 Dates ISO `YYYY-MM-DD`
⚠️ **Ne fais pas** `new Date("2026-01-06")` (problèmes timezone parfois).
```js
function parseIsoDate(str) {
  // "2026-01-06" -> Date(2026-01-06 à minuit local)
  return new Date(str + 'T00:00:00');
}
```

### 1.3 Comparateurs de tri (sort)
```js
function sortNumberAsc(a, b) {
  return a - b;
}
function sortNumberDesc(a, b) {
  return b - a;
}
function sortAlphaFr(a, b) {
  return a.localeCompare(b, "fr");
}

function sortDateFrAsc(a, b) {
  return parseFrDate(a) - parseFrDate(b);
}
function sortDateIsoAsc(a, b) {
  return parseIsoDate(a) - parseIsoDate(b);
}
```

---

## 2) Patterns EXAM — copier / adapter

## P01 — Somme (NOMBRE)
**Sortie :** un nombre (ex: total km, total CA, total jours)
```js
const total = data.reduce((acc, x) => {
  acc = acc + x;
  return acc;
}, 0);
```

---

## P02 — Compter avec condition (NOMBRE)
**Sortie :** un nombre (ex: nb >= 10, nb incidents, nb locations)
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
**Ex :** somme km uniquement si type === "Moyenne"
```js
const totalKm = data.reduce((acc, item) => {
  if (item.vehicule.vehicule_type === "Moyenne") {
    acc = acc + item.location.location_km;
  }
  return acc;
}, 0);
```

---

## P04 — Min/Max sur NOMBRE
```js
const res = data.reduce((acc, x) => {
  if (x < acc.min) acc.min = x;
  if (x > acc.max) acc.max = x;
  return acc;
}, { min: data[0], max: data[0] });
```

---

## P05 — Min/Max sur DATE FR (JJ.MM.AAAA)
**Sortie finale attendue :** strings
```js
const first = parseFrDate(sales[0].date);

const mm = sales.reduce((acc, vente) => {
  const d = parseFrDate(vente.date);

  if (d < acc.min) acc.min = d;
  if (d > acc.max) acc.max = d;

  return acc;
}, { min: first, max: first });

// sortie string
const result = {
  dateMin: formatFrDate(mm.min),
  dateMax: formatFrDate(mm.max),
};
```

---

## P06 — Filtrer DANS le reduce (dataset MIXED)
Quand il y a plusieurs types (sale / location), tu fais un `if` au début.
```js
const res = records.reduce((acc, item) => {
  if (item.kind !== "sale") {
    return acc; // ignore
  }

  // ici c’est sûr que c’est une vente
  acc = acc + 1;
  return acc;
}, 0);
```

Autre version “champ pas présent” :
```js
const res = records.reduce((acc, item) => {
  if (!item.date) return acc; // ignore les locations
  // ...
  return acc;
}, { Min: null, Max: null });
```

---

## P07 — Construire un TABLEAU (equivalent map) avec reduce
```js
const labels = data.reduce((acc, item) => {
  acc.push(item.nom);
  return acc;
}, []);
```

---

## P08 — Uniques (liste sans doublons) en TABLEAU
```js
const types = data.reduce((acc, item) => {
  if (!acc.includes(item.type)) {
    acc.push(item.type);
  }
  return acc;
}, []);

// tri alpha
types.sort(sortAlphaFr);
```

---

## P09 — GroupBy simple (OBJET contenant des tableaux)
**Objectif :** `{ "Moyenne": ["Mazda ...", ...], "Grande": [...] }`
```js
const grouped = locations.reduce((acc, loc) => {
  const type = loc.vehicule.vehicule_type;

  // 1) si le tableau n'existe pas encore, le créer
  if (!acc[type]) {
    acc[type] = [];
  }

  // 2) construire le texte
  const label =
    loc.vehicule.vehicule_nom +
    " [" + loc.vehicule.vehicule_id + "], à " +
    loc.vehicule.vehicule_prix_par_jour + " Frs/jour et " +
    loc.vehicule.vehicule_prix_par_km + " Frs/km";

  // 3) éviter doublons
  if (!acc[type].includes(label)) {
    acc[type].push(label);
  }

  return acc;
}, {});
```

### Trier chaque tableau du groupBy (SANS forEach)
```js
const groupedSorted = Object.keys(grouped).reduce((acc, key) => {
  // copie puis tri (évite d’abîmer l’original)
  const copy = grouped[key].slice();
  copy.sort(sortAlphaFr);

  acc[key] = copy;
  return acc;
}, {});
```

---

## P10 — Indexer par ID (OBJET), puis récupérer en TABLEAU
✅ C’est ton “hack” qui simplifie plein d’exos.

```js
const byId = locations.reduce((acc, loc) => {
  const id = loc.client.client_id;

  if (!acc[id]) {
    acc[id] = {
      nom_prenom: loc.client.client_nom + " " + loc.client.client_prenom,
      date_naissance: loc.client.client_date_naissance,
    };
  }

  return acc;
}, {});

// objet -> tableau
const arr = Object.values(byId);
```
```js
function actionA6() {
    const utilisationDesVehiculesTrie = jsonData.locations.reduce((accStatByCar, currentLocation) => {
        const id = currentLocation.vehicule.vehicule_id 
        const vehicule = currentLocation.vehicule
        const location = currentLocation.location
        if(!accStatByCar[id]){

            accStatByCar[id] = {
                vehicule_id: id,
                vehicule_type: vehicule.vehicule_type,
                vehicule_nom: vehicule.vehicule_nom,
                vehicule_prix_par_jour: vehicule.vehicule_prix_par_jour,
                vehicule_prix_par_km: vehicule.vehicule_prix_par_km,
                ca:0,
                locations: 0,
                jours:0,
                km: 0 
            }
        }

        accStatByCar[id].ca += location.location_jours * vehicule.vehicule_prix_par_jour + location.location_km * vehicule.vehicule_prix_par_km
        accStatByCar[id].locations +=1
        accStatByCar[id].jours += location.location_jours
        accStatByCar[id].km += location.location_km

        return accStatByCar

    }, {});

    const tab = Object.values(utilisationDesVehiculesTrie)
    const res= tab.sort((a,b) => {
        return b.ca - a.ca
    })

    afficherObjet(res);
}
```
```js
function actionA9() {
    const TOP = 5;

    const clientsTries = jsonData.locations.reduce((accClient, currentLocation) => {
        const vehicule = currentLocation.vehicule
        const location = currentLocation.location
        const client = currentLocation.client

        const id = client.client_id

        if (!accClient[id]){
            accClient[id] = {
                nom_prenom: client.client_nom + client.client_prenom,
                date_naissance: client.client_date_naissance,
                age_str:  client.client_age_str,
                ca:0,
                locations: 0,
                jours:0,
                km: 0 
            }

        }

        accClient[id].locations +=1
        accClient[id].jours += location.location_jours
        accClient[id].km += location.location_km
        accClient[id].ca += location.location_jours * vehicule.vehicule_prix_par_jour + location.location_km * vehicule.vehicule_prix_par_km

        return accClient

    },[]);
    
    const tab  = Object.values(clientsTries)
    const res = tab.sort((a,b) => {
        return b.ca - a.ca
    })
    afficherObjet(res);
}
```


### Trier par date naissance (FR)
```js
arr.sort((a, b) => {
  return parseFrDate(a.date_naissance) - parseFrDate(b.date_naissance);
});
```

### Trier par nom_prenom (alpha)
```js
arr.sort((a, b) => {
  return a.nom_prenom.localeCompare(b.nom_prenom, "fr");
});
```

### Trier par 2 critères (nom puis prénom)
```js
arr.sort((a, b) => {
  const cmpNom = a.nom.localeCompare(b.nom, "fr");
  if (cmpNom !== 0) {
    return cmpNom;
  }
  return a.prenom.localeCompare(b.prenom, "fr");
});
```

---

## P11 — CA (somme de sous-tableau) : ventes -> produits
**CA d’une vente = somme(prix des produits)**
```js
function caVente(vente) {
  return vente.produits.reduce((sum, p) => {
    sum = sum + p.prix;
    return sum;
  }, 0);
}
```

---

## P12 — CA par DATE (OBJET)
```js
const caByDate = sales.reduce((acc, vente) => {
  const ca = caVente(vente);
  const date = vente.date;

  if (!acc[date]) {
    acc[date] = 0;
  }
  acc[date] = acc[date] + ca;

  return acc;
}, {});
```

---

## P13 — CA par CLIENT (TABLEAU trié)
Objectif :
`[ {id, nom, prenom, ca}, ... ]` tri DESC ca

```js
const byClient = sales.reduce((acc, vente) => {
  const id = vente.client.id;

  if (!acc[id]) {
    acc[id] = {
      id: id,
      nom: vente.client.nom,
      prenom: vente.client.prenom,
      ca: 0
    };
  }

  acc[id].ca = acc[id].ca + caVente(vente);

  return acc;
}, {});

const arr = Object.values(byClient);

arr.sort((a, b) => {
  return b.ca - a.ca;
});
```

---

## P14 — Incidents (TABLEAU) triés par date ISO
Objectif format :
```js
[
  { date:"2026-01-07", vehicule:"...", client:"...", details:"..." }
]
```

```js
const incidents = locations.reduce((acc, loc) => {
  if (loc.location.has_incident === true) {
    acc.push({
      date: loc.location.location_date,
      vehicule: loc.vehicule.vehicule_nom + " [" + loc.vehicule.vehicule_id + "]",
      client: loc.client.client_nom + " " + loc.client.client_prenom,
      details: loc.location.incident_details,
    });
  }
  return acc;
}, []);

incidents.sort((a, b) => {
  return parseIsoDate(a.date) - parseIsoDate(b.date);
});
```

---

## P15 — TOP N (ex: top 3 produits par CA)
### Étapes (toujours les mêmes)
1) indexer par id dans un objet  
2) `Object.values()`  
3) `sort`  
4) `slice(0, N)`

```js
// 1) récupérer tous les produits de toutes les ventes
const allProducts = sales.reduce((acc, vente) => {
  // on ajoute tous les produits de cette vente
  vente.produits.forEach(p => acc.push(p)); // ⚠️ pas autorisé si forEach interdit
  return acc;
}, []);
```

⚠️ Si forEach interdit → fais-le en `reduce` aussi :
```js
const allProducts = sales.reduce((acc, vente) => {
  const produits = vente.produits;

  produits.reduce((acc2, p) => {
    acc2.push(p);
    return acc2;
  }, acc);

  return acc;
}, []);
```

Puis index + top :
```js
const byProd = allProducts.reduce((acc, p) => {
  const id = p.id;

  if (!acc[id]) {
    acc[id] = { id: p.id, nom: p.nom, ca: 0 };
  }
  acc[id].ca = acc[id].ca + p.prix;

  return acc;
}, {});

const arr = Object.values(byProd);

arr.sort((a, b) => {
  return b.ca - a.ca;
});

const top3 = arr.slice(0, 3);
```

---

## 3) Object.values — ce que ça fait (simple)
Si tu as ça :
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

✅ Donc tu passes de **OBJET** à **TABLEAU** pour pouvoir faire `.sort()` / `.slice()`.

---

## 4) Checklist “je lis l’énoncé” (anti-panique)
Quand tu lis une question, fais juste ça :

### A) Ils veulent quoi comme FORMAT ?
- **NOMBRE** → init `0`
- **TABLEAU** → init `[]`
- **OBJET** → init `{}`

### B) Il y a un filtre ?
Ex : seulement kind='sale' ou type='Moyenne'  
→ `if (...) return acc;`

### C) Ils veulent un tri ?
- alpha → `localeCompare`
- num → `a - b`
- date FR → `parseFrDate`
- date ISO → `parseIsoDate`

### D) Dedup ?
- tableau : `includes`
- objet : index par id → plus simple

Fin.

---

<img src="res/EMF_logo_RVB_Info_long.png" width="25%" style="margin-left:-20px;">
