# Advertencia: ¡Este repositorio contiene contenido relacionado a un intento de SCAM!

---

Este repositorio fue creado como parte de un intento de SCAM del que fui víctima. **No debe ejecutarse bajo ninguna circunstancia.**

Por favor, revise cuidadosamente todo el código si está explorando con fines educativos y, bajo ninguna circunstancia, utilice este código en un entorno de producción o para propósitos maliciosos.

## Propósito

Esto se mantiene aquí unicamente como una muestra para identificar y comprender técnicas que pueden ser utilizadas en fraudes similares. Toda responsabilidad recae estrictamente en el uso final que haga cualquier usuario o desarrollador.

---

# Análisis Detallado del Código Malicioso en el Repositorio

## Archivos Analizados

### Archivo: `backend/app.js`
- **Uso de Criptografía Potencialmente Maliciosa:**
  - En la ruta `/test`, el código recibe una clave secreta (`pvkey`) y la encripta utilizando `CryptoJS`. Este flujo podría ser explotado para obtener claves privadas de los usuarios sin que se den cuenta.
  - Genera datos encriptados usando algoritmos clave, pero las claves privadas nunca deberían ser procesadas o almacenadas de esta forma.

```javascript
var salt = CryptoJS.lib.WordArray.random(128 / 8);
var key = CryptoJS.PBKDF2(pass, salt, { keySize: keySize / 32, iterations: iterations });
var encrypted = CryptoJS.AES.encrypt(private_key, key, { iv: iv, padding: CryptoJS.pad.Pkcs7, mode: CryptoJS.mode.CBC });
res.json({ key: encrypted.toString() });
```
- **Problema:** Este script no verifica la procedencia de las claves, abriendo la puerta a la captura de información confidencial de los usuarios.
- **Riesgo:** La lógica puede ser manipulada para registrar claves privadas de los usuarios.

---

### Archivo: `backend/config.js`
- **Configuraciones Sensibles Incluidas:**
  - Contiene información sensible, como:
    - Credenciales para acceder al servidor MySQL:
      - `mysqlHost`: `13.233.12.75`
      - `user`: `esp`
      - `password`: `Espsoft123#`
    - Clave secreta para JSON Web Tokens (JWT):
      - `JWT_SECRET_KEY`: `ly27lg35kci85tvgvl0zgbod4`
    - Direcciones relacionadas con redes blockchain:
      - `clientDepositAddress`: `0xEfcd2e9ca6483147A25a106C654a6E557eb8f916`

```javascript
module.exports = { 
  mysqlHost: "13.233.12.75",
  user: "esp",
  password: 'Espsoft123#',
  JWT_SECRET_KEY: 'ly27lg35kci85tvgvl0zgbod4',
};
```
- **Problema:** Las credenciales expuestas directamente en el código hacen que el sistema sea extremadamente vulnerable a ataques.
- **Riesgo:** Cualquier atacante con acceso al repositorio podría aprovechar estas credenciales para obtener acceso al servidor y otras funcionalidades críticas.

---

### Directorio: `backend/controllers`
- **Controladores Sospechosos:**
  - Archivos como `TokenMainnetBSC_ethers.js` y `TokenTestBSC_ethers.js` gestionan operaciones relacionadas con redes blockchain (por ejemplo, Binance Smart Chain) y podrían ser manipulados para explotar transacciones.
  - Otros controladores, como `auth.controller.js` y `transaction.controller.js`, manejan procesos críticos como la autenticación y transferencia de fondos que podrían ser vulnerables.

---

### Directorio: `backend/middleware`
- **Middleware `auth.middleware.js`:**
  - Este archivo gestiona sesiones y tokens JWT, pero no posee validaciones robustas para garantizar que los usuarios estén autorizados.
  
```javascript
const jwt = require("jsonwebtoken");
module.exports = async (req, res, next) => {
    const token = req.header("Authorization");
    if (!token) throw new Error("No token proporcionado");
};
```
- **Problema:** No valida correctamente el token ni gestiona errores de manera segura, dejando al sistema vulnerable a usuarios no autorizados.

---

## Resumen

El repositorio contiene múltiples configuraciones y scripts relacionados con criptografía, blockchain y almacenamiento de datos sensibles. Muchas de estas técnicas pueden ser utilizadas para actividades fraudulentas, incluyendo:
- Captura y encriptación de claves privadas del usuario.
- Exposición de credenciales y claves sensibles.
- Manipulación de redes blockchain a través de integraciones específicas.

---

## Recomendaciones

1. **Eliminar Credenciales Expuestas:**
   - Actualizar todos los valores sensibles en `config.js` y reemplazarlos con variables de entorno protegidas.
2. **Endpoints Maliciosos:**
   - Monitorear y eliminar puntos de entrada no autorizados, como el endpoint `/test` en `backend/app.js`.
3. **Auditoría de Dependencias:**
   - Revisar librerías como `crypto-js` y `jsonwebtoken` para garantizar prácticas seguras.

---

### Propósito del Análisis
Este análisis es una muestra educativa de cómo identificar y mitigar intentos de scam. **No se debe utilizar este repositorio con fines maliciosos ni para producción.**

--

## Instrucciones Previas (No Usar)

### Install Dependencies (Evitar)

```bash
1. Front-end    `npm install --force`
2. Backe-end    `cd backend & npm install --force`
```

### Run (Evitar)

```bash
cd ..
npm start
```
