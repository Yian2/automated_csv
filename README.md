# CV Sender https://yian2.github.io/automated_csv/

App web estàtica per enviar candidatures des del teu Gmail: puges un CSV d'empreses i els CVs en PDF, l'app ho revisa (correus vàlids, repetits, bústies genèriques, CV que falta) i envia cada correu amb el CV adjunt, amb un registre en directe. Després, al panell **Seguiment**, et diu qui t'ha respost, quantes respostes hi ha i quins correus han rebotat. Està en català, castellà i anglès. No cal instal·lar res: funciona al navegador, al mòbil i a l'ordinador.

## Format del CSV

| columna | obligatòria | què és |
|---|---|---|
| `empresa` | sí | Nom de l'empresa |
| `email` | sí | Correu de destí (millor el de RRHH) |
| `cv` | no | Nom del fitxer PDF que vols adjuntar. Si està buit, s'adjunta el primer CV pujat |
| `assumpte` | no | Assumpte del correu. Si està buit, s'usa el per defecte |
| `missatge` | no | Text del correu. Admet `{empresa}` i qualsevol altra columna entre claus |

Mira `exemple.csv`.


## Configurar Google (una sola vegada)

El **mode prova** funciona sense això. Per enviar correus reals cal un Client ID de Google:

1. Entra a <https://console.cloud.google.com/> i crea un projecte nou.
2. **APIs i serveis → Biblioteca** → cerca **Gmail API** → *Habilita*.
3. **APIs i serveis → Pantalla de consentiment OAuth** → tipus *Extern* → omple el nom de l'app i el teu correu. A *Usuaris de prova* afegeix els comptes de Gmail que la faran servir.
   A *Accés a dades* (Data access) → *Afegeix o elimina àmbits* afegeix `.../auth/gmail.send` i `.../auth/gmail.metadata` (el segon serveix per al panell de seguiment).
4. **APIs i serveis → Credencials → Crea credencials → ID de client OAuth** → tipus *Aplicació web*. A *Orígens de JavaScript autoritzats* posa `https://<el-teu-usuari>.github.io` (sense barra final).
5. Copia el Client ID (`…apps.googleusercontent.com`) i enganxa'l a l'app, a *Configuració de Google*. Si vols que hi sigui sempre, posa'l a la constant `DEFAULT_CLIENT_ID` de `index.html`.

### Notes

- Mentre l'app estigui en mode *Testing*, només els usuaris de prova poden iniciar la sessió, i Google mostrarà l'avís «Google no ha verificat aquesta aplicació» (toca *Continua*). Perquè qualsevol persona la pugui fer servir sense aquest límit, cal demanar la verificació de Google per a l'abast `gmail.send`.
- L'app demana permís per **enviar** correus i per veure'n les **metadades** (remitent i data), per saber qui t'ha respost. No llegeix el contingut dels correus.
- Gmail no avisa quan algú obre un correu, així que no es pot saber si l'han llegit. El seguiment es basa en respostes i rebots.
- L'historial de seguiment es guarda al navegador. Per passar-lo a un altre dispositiu, fes *Exporta* i després *Importa CSV*.
- Les dades i els CVs no surten del navegador: van directament de l'ordinador a Gmail.
- Hi ha una pausa entre correus (30 s per defecte) perquè Gmail no els marqui com a spam. Els comptes gratuïts tenen un límit d'uns 500 correus al dia.
