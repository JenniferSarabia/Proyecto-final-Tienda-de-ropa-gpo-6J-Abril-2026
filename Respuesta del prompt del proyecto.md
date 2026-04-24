¡Excelente iniciativa! Vamos a estructurar el agente global **.agents** para la tienda **Urba & Flow**. Este agente actuará como tu orquestador para gestionar las habilidades de diseño, código y scraping, asegurando que el entorno de Flutter y Firebase esté perfectamente sincronizado.

---

## 1. Estructura del Agente Global (`.agents`)

Primero, definimos la jerarquía de archivos para que tu agente tenga "memoria" y herramientas organizadas.

### Archivo: `SKILL.md`
Este archivo define la lógica de operación del agente.
```markdown
# Skill: Urba & Flow Global Agent
**Versión:** 1.0
**Objetivo:** Gestionar el ciclo de vida de la app móvil (Diseño, Código, Scraping).

## Carpetas de Trabajo
- `/scripts`: Automatización de tareas (scraping de productos).
- `/ejemplos`: Plantillas de UI y lógica de Firebase.
- `/resources`: Assets, logos de Urba & Flow y configuración JSON.

## Flujo de Trabajo
1. Validación de entorno (Flutter/Firebase).
2. Generación de módulos CRUD.
3. Sincronización con Firestore.
```

---

## 2. Configuración del Entorno (Prerrequisitos)

Antes de codificar, debemos asegurar que tus herramientas estén listas en la terminal de **VS Code** o **Antigravity**.

### Verificación e Instalación
Ejecuta estos comandos para validar tu sistema:

1.  **Flutter:** `flutter --version` (Si no está, descarga el SDK desde flutter.dev).
2.  **Firebase CLI:** Si no tienes `firebase-tools`, instálalo con npm:
    `npm install -g firebase-tools`
3.  **FlutterFire CLI:** Crucial para conectar Flutter con Firebase:
    `dart pub global activate flutterfire_cli`

### Conexión con Firebase Console
```bash
# 1. Login
firebase login

# 2. Configurar el proyecto en la carpeta raíz
flutterfire configure
```
*Esto generará automáticamente el archivo `firebase_options.dart` en tu carpeta `lib`.*

---

## 3. Estructura del Proyecto: `proyectoropa`

Creamos la estructura base siguiendo una arquitectura limpia (Clean Architecture simplificada).

### Configuración de `pubspec.yaml`
Asegúrate de incluir las dependencias necesarias para Firestore y navegación:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.x.x
  cloud_firestore: ^4.x.x
  cupertino_icons: ^1.0.2
```

---

## 4. Desarrollo del CRUD de Productos

A continuación, el código base para la funcionalidad principal de **Urba & Flow**.

### A. Modelo de Datos (`lib/models/product_model.dart`)
```dart
class Product {
  String id;
  String nombre;
  double precio;
  String imagen;

  Product({required this.id, required this.nombre, required this.precio, required this.imagen});

  Map<String, dynamic> toMap() => {
    "nombre": nombre,
    "precio": precio,
    "imagen": imagen,
  };
}
```

### B. Servicio Firestore (`lib/services/firebase_service.dart`)

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

FirebaseFirestore db = FirebaseFirestore.instance;

// CREATE
Future<void> addProduct(String nombre, double precio) async {
  await db.collection('productos').add({"nombre": nombre, "precio": precio});
}

// READ
Future<List> getProducts() async {
  List productos = [];
  QuerySnapshot querySnapshot = await db.collection('productos').get();
  for (var doc in querySnapshot.docs) {
    productos.add(doc.data());
  }
  return productos;
}
```

### C. Interfaz de Usuario: Pantalla de Inicio (`lib/pages/home_page.dart`)
```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Urba & Flow - Tienda')),
      body: FutureBuilder(
        future: getProducts(),
        builder: (context, snapshot) {
          if (snapshot.hasData) {
            return ListView.builder(
              itemCount: snapshot.data?.length,
              itemBuilder: (context, index) {
                return ListTile(
                  title: Text(snapshot.data?[index]['nombre']),
                  subtitle: Text("\$${snapshot.data?[index]['precio']}"),
                  trailing: Icon(Icons.edit),
                );
              },
            );
          } else {
            return Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () async {
          // Aquí llamarías a la pantalla de creación
          await addProduct("Camiseta Oversize", 25.0);
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

---

## 5. Pruebas de Verificación

Para garantizar que todo funcione dentro de **.agents**, sigue esta secuencia:

1.  **Terminal:** Ejecuta `flutter doctor` para verificar que no haya errores de SDK.
2.  **Firebase:** Revisa en la consola web que la base de datos **Firestore** esté en "Modo de Prueba" (para evitar bloqueos de reglas de seguridad iniciales).
3.  **Ejecución:** Usa `flutter run` en un emulador o dispositivo físico.

> **Nota de Agente:** He configurado las carpetas `scripts/`, `ejemplos/` y `resources/` en tu directorio raíz para que puedas empezar a guardar los JSON de scraping de otras tiendas de ropa y alimentar tu base de datos automáticamente.

¿Deseas que profundice en el código del **Skill de Scraping** para extraer precios de competencia o prefieres terminar de pulir la **UI de Diseño**?
