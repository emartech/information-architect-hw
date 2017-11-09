# API / Integrations Information Architect HW

## Feladat

Készítse el az alább található végpont angol nyelvű dokumentációját a rendelkezésre álló vázlat alapján.
A feladat teljesítéséhez illetve a megoldás prezentálásához bármilyen arra alkalmas eszközt illetve szolgáltatást használhat.

Megoldásként magát a dokumentációt illetve hosztolt szolgáltatás igénybevétele esetén annak elérhetőségét, az arra mutató linket küldje el.

## Végpont vázlat

### Feliratkozók tömeges módosítása

Egy hívással tudja módosítani tetszőleges feliratkozók bármely adatát. Feliratkozónként egy-egy objektumban kell átadni a módosítandó adatokat. Feliratkozónként akár más-más adatokat is lehet módosítani. Ha a módosítandó adatok között szerepel az `id` kulcs, akkor az adott azonosítójú feliratkozó kerül módosításra, de lehetőség van bármely mező értéke alapján is módosítani. Ez utóbbi esetben az `external_key_field` mezőben kell megadni, hogy mely kulcs alapján történjen az azonosítás.

FONTOS: `external_key_field` alapján történő azonosítás esetén minden objektumnak tartalmaznia kell az `external_key_field` által hivatkozott mezőt.

FONTOS: ha nem `id` alapján történik a frissítés, akkor előfordulhat, hogy egynél több rekord is módosításra kerül.

FIGYELEM: a kötegelt módosítás nagy mennyiségű feliratkozó gyors módosítására szolgál, az adatmódosítás során nem történik mezőszinkron, nem hajtódnak végre műveletek, sem időzítések. Tehát csak abban az esetben használja a tömeges módosítást, ha az előbbiekre nincs szüksége.

### kérés:

**POST** https://emarsys.net/api/contacts/updates

```json
{
    "external_key_field": "some_id_field",
    "contacts": [
        {
            "first_name": "Alma",
            "last_name": "Fa",
            "some_id_field": "qwe123"
        },
               {
            "email": "korte.fa@emarsys.com",
            "some_id_field": "asd456"
        }
    ]
}
```

### válasz:

**HTTP 200**

```json
{
    "results": [
        {
            "id": 9,
            "some_field_id": "qwe123",
            "_links": {
                "href": "https://emarsys.net/api/contacts/9"
            }
        },
        {
            "id": 28,
            "some_field_id": "asd456",
            "_links": {
                "href": "https://emarsys.net/api/contacts/28"
            }
        },
        {
            "id": 41,
            "some_field_id": "asd456",
            "_links": {
                "href": "https://emarsys.net/api/contacts/41"
            }
        }
    ]
}
```
