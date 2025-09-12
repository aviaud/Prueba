# 📦 InventarioWeb (Grupo 52)

Aplicación web sencilla para la **gestión de inventarios** conectada con **Google Sheets** mediante **Google Apps Script**.  
Incluye catálogo de productos (codigos precargados), inventario en tiempo real, exportación/importación en CSV y un acceso con contraseña básica.

---

## 🛠️ Tecnologías utilizadas
- **Frontend:** HTML, CSS, JavaScript, Bootstrap
- **Backend:** Google Apps Script
- **Base de datos:** Google Sheets
- **Hosting sugerido:** GitHub Pages

---

## 🚀 Funcionalidades
- 🔑 **Login de acceso** con contraseña predeterminada (`123456`)
- 📋 **Gestión de catálogo**: agregar, editar, eliminar productos
- 📦 **Gestión de inventario**: registro de entradas y salidas
- 🔎 **Búsqueda dinámica** en catálogo e inventario
- ⬇️ **Exportación a CSV**
- ⬆️ **Importación desde CSV**
- 🌐 Integración con **Google Sheets API** vía Apps Script

---

## 📂 Estructura del Proyecto
InventarioWeb/
│── index.html # Interfaz principal con Bootstrap
│── script.js # Lógica frontend (login, fetch API, gestión de inventario)
│── styles.css # Estilos personalizados
│── apps_script.gs # Backend en Google Apps Script (API REST)
│── README.md # Documentación del proyecto

---

## ⚙️ Instalación y Configuración
1. 📑 Crear un **Google Sheet** con dos hojas:  
   - `Catalogo`  
   - `Inventario`  

2. 🔧 En **Google Apps Script**:
   - Crear un nuevo proyecto.
   - Pegar el contenido de:
     function doGet(e) {
  var action = e.parameter.action;

  if (action === "getCatalogo") return getCatalogo();
  if (action === "getInventario") return getInventario();

  return ContentService.createTextOutput("Acción no válida");
}

function getCatalogo() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Catalogo");
  var data = sheet.getDataRange().getValues();
  data.shift(); // elimina encabezado
  return ContentService.createTextOutput(JSON.stringify(data))
    .setMimeType(ContentService.MimeType.JSON);
}

function getInventario() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Inventario");
  var data = sheet.getDataRange().getValues();
  data.shift(); // elimina encabezado
  return ContentService.createTextOutput(JSON.stringify(data))
    .setMimeType(ContentService.MimeType.JSON);
}

function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Inventario");
  var sheetCatalogo = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Catalogo");
  var body = JSON.parse(e.postData.contents);
  var action = body.action;

  if (action === "guardar") {
    var data = sheet.getDataRange().getValues();
    data.shift(); // elimina encabezado
    var index = data.findIndex(row => row[0].toString().trim() === body.codigo.toString().trim());

    if (index !== -1) {
      // Actualiza fila existente
      sheet.getRange(index + 2, 1, 1, 3).setValues([[body.codigo, body.descripcion, body.cantidad]]);
    } else {
      // Agrega nueva fila
      sheet.appendRow([body.codigo, body.descripcion, body.cantidad]);
    }
  }

  if (action === "eliminar") {
    var data = sheet.getDataRange().getValues();
    for (var i = 1; i < data.length; i++) {
      if (data[i][0].toString().trim() === body.codigo.toString().trim()) {
        sheet.deleteRow(i + 1);
        break;
      }
    }
  }

  if (action === "eliminar1") {
    var data = sheetCatalogo.getDataRange().getValues();
    for (var i = 1; i < data.length; i++) {
      if (data[i][0].toString().trim() === body.codigo.toString().trim()) {
        sheetCatalogo.deleteRow(i + 1);
        break;
      }
    }
  }

  return ContentService.createTextOutput(JSON.stringify({ status: "OK" }))
    .setMimeType(ContentService.MimeType.JSON);
}

   - Publicar como **Web App** (acceso: Cualquiera con el enlace).
   - Copiar la URL generada.

3. 🌐 En `script.js`, reemplazar la constante:
   ```javascript
   const API_URL = "https://script.google.com/macros/s/AKfycbxmkE3ripjsF5xwG-rBESZNy7UYnaJTDUOqx3V9FATruNGbXApGajEr7j2Got1-sPc/exec";
4. 📤 Subir el proyecto a GitHub Pages o abrir index.html en tu navegador.

   👨‍💻 Manual de Usuario

Acceder con contraseña predeterminada 123456.

Navegar entre Catálogo e Inventario.

Agregar productos con código, descripción y stock.

Editar o eliminar registros fácilmente.

Exportar o importar CSV desde la interfaz.

🔒 Seguridad

Actualmente usa una contraseña estática.
Mejoras recomendadas:

Autenticación con Google OAuth.

Control de usuarios y roles.

📌 Posibles mejoras

📊 Reportes gráficos de entradas y salidas.

📱 Diseño optimizado para móviles.

📝 Historial de movimientos por producto.

🔐 Login con usuarios múltiples.

👥 Autores

Proyecto desarrollado por Grupo 52  (Antonio Ulises Viaud Fajardo, José Elías Hernández Aguilar, Oscar Mauricio Renderos Reyes, Sail Ivan Perez Marroquin, Melvin Adonay Gabriel Palacios)
Materia: Software Libre y Código Abierto 02-2025
TSU en Desarrollo de Software en Código Ab
