# **Aplicació web de registre de bany amb XML**
Aquest document explica el funcionament del codi **HTML + JavaScript** que permet registrar quan un alumne va al bany en un institut.

L'aplicació:

- demana el **NIA de l'alumne**
- valida que el **NIA tinga 8 dígits**
- genera **data i hora automàticament**
- crea un **registre XML**
- guarda el XML en **localStorage**
- permet **descarregar el fitxer** bany.xml

# **1. Estructura general del document HTML**
El document està format per tres parts principals:

1. **HTML** → estructura de la pàgina
1. **CSS** → estil visual
1. **JavaScript** → lògica de l'aplicació

Estructura bàsica:

<!DOCTYPE html>

<html>

<head>

</head>

<body>

</body>

</html>

# **2. Interfície d'usuari (HTML)**
La pàgina mostra:

- un camp per introduir el **NIA**
- un botó per **registrar**
- un botó per **descarregar el XML**
- una zona per visualitzar el **XML generat**

<label>NIA (8 dígits)</label>

<input type="text" id="nia" maxlength="8">

<button onclick="registrar()">Registrar</button>

<button onclick="descarregar()">Descarregar XML</button>

<pre id="xmlOutput"></pre>
### **Elements importants**
**Input NIA**

<input type="text" id="nia" maxlength="8">

- id="nia" permet accedir-hi des de JavaScript
- maxlength="8" limita a 8 caràcters

# **3. Validació del NIA**
El codi comprova que el NIA té **exactament 8 números**.

function validarNIA(nia){

return /^[0-9]{8}$/.test(nia);

}

Explicació de l'expressió regular:

|**Expressió**|**Significat**|
| :-: | :-: |
|^|inici del text|
|[0-9]|qualsevol número|
|{8}|exactament 8|
|$|final del text|

Exemple vàlid:

12345678

Exemple invàlid:

12345

abc12345

123456789

# **4. Inicialització del XML**
Quan s'obri la pàgina, el sistema intenta carregar el XML guardat en **localStorage**.

let xml = localStorage.getItem("xmlBany");

Si no existeix, es crea un XML buit:

xml = `<?xml version="1.0" encoding="UTF-8"?>

<registres>

</registres>`;

Aquest serà l'arrel del document.

# **5. Registrar un alumne**
Quan es prem **Registrar**, s'executa:

function registrar()

El procés és:

1️⃣ llegir el NIA\
2️⃣ validar-lo\
3️⃣ generar data i hora\
4️⃣ crear el registre XML\
5️⃣ afegir-lo al document\
6️⃣ guardar-lo en localStorage

## **5.1 Obtindre el NIA**
let nia = document.getElementById("nia").value;

## **5.2 Generar data i hora**
```
let ara = new Date();
let data = ara.toISOString().split("T")[0];
let hora = ara.toTimeString().split(" ")[0];

Exemple:
data = 2026-03-10
hora = 11:42:15
```
## **5.3 Crear el registre XML**
```
let registre = `
  <registre>
    <nia>${nia}</nia>
    <data>${data}</data>
    <hora>${hora}</hora>
  </registre>
`;

Exemple generat:
<registre>
  <nia>12345678</nia>
  <data>2026-03-10</data>
  <hora>11:42:15</hora>
</registre>
```
# **6. Afegir el registre al XML**
Per inserir el registre dins del document es reemplaça el tancament de l'arrel.

xml = xml.replace("</registres>", registre + "\n</registres>");

Resultat:

<registres>

`  `<registre>...</registre>

</registres>

# **7. Guardar en localStorage**
El XML es guarda al navegador:

localStorage.setItem("xmlBany", xml);

Això permet que:

- si es refresca la pàgina
- o es tanca el navegador

els registres **no es perden**.

# **8. Mostrar el XML en pantalla**
document.getElementById("xmlOutput").textContent = xml;

S'utilitza <pre> perquè es mantinga el format del XML.

# **9. Descarregar el fitxer XML**
El botó **Descarregar XML** crea un fitxer:

let blob = new Blob([xml], {type:"text/xml"});

Després es crea un enllaç temporal:

a.download = "bany.xml";

Això provoca la descàrrega del fitxer:

bany.xml

# **10. Exemple de XML generat**
<?xml version="1.0" encoding="UTF-8"?>

<registres>

`  `<registre>

`    `<nia>12345678</nia>

`    `<data>2026-03-10</data>

`    `<hora>11:42:15</hora>

`  `</registre>

`  `<registre>

`    `<nia>87654321</nia>

`    `<data>2026-03-10</data>

`    `<hora>12:05:44</hora>

`  `</registre>

</registres>

# **11. Avantatges d'aquest sistema**
✔ funciona sense servidor\
✔ utilitza **XML real**\
✔ guarda dades en **localStorage**\
✔ permet descarregar el document XML\
✔ validació de NIA

# **12. Possibles millores**
Es podria ampliar amb:

- historial en **taula HTML**
- botó **esborrar registres**
- evitar **NIA duplicats**
- registrar també **hora de tornada**
- enviar el XML a un **servidor Java o PHP**

# **Conclusió**
Aquesta aplicació és un exemple senzill de **generació i manipulació de XML des d'una aplicació web amb JavaScript**, utilitzant el navegador com a sistema d'emmagatzematge temporal mitjançant **localStorage**.



