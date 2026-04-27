Perfecto, esto ya está pensado para **MongoDB**, pero te lo dejo más claro, ordenado y listo como si lo fueras a entregar o usar directamente 👇

---

# 🧢 Base de datos: **Urba & Flow (MongoDB)**

## 🗂️ Crear base de datos

```js
use urba_flow
```

---

# 📁 Colección: `clientes`

```js
db.clientes.insertMany([
{
  nombre: "Juan Pérez",
  correo: "juan@gmail.com",
  telefono: "6561234567",
  direccion: {
    calle: "Av. Central",
    numero: 123,
    ciudad: "Ciudad Juárez",
    cp: "32000"
  },
  fecha_registro: new Date(),
  activo: true
},
{
  nombre: "María López",
  correo: "maria@gmail.com",
  telefono: "6569876543",
  direccion: {
    calle: "Calle Norte",
    numero: 456,
    ciudad: "Ciudad Juárez",
    cp: "32010"
  },
  fecha_registro: new Date(),
  activo: true
}
])
```

🔹 **Tipos de datos:**

* String → nombre, correo, teléfono
* Object → dirección
* Date → fecha_registro
* Boolean → activo

---

# 👕 Colección: `productos`

```js
db.productos.insertMany([
{
  nombre: "Playera Oversize",
  marca: "Urba & Flow",
  precio: 299.99,
  stock: 100,
  categorias: ["playeras", "streetwear"],
  tallas: ["S", "M", "L"],
  colores: ["negro", "blanco"],
  fecha_creacion: new Date(),
  activo: true
},
{
  nombre: "Pantalón Cargo",
  marca: "Urba & Flow",
  precio: 699.99,
  stock: 50,
  categorias: ["pantalones"],
  tallas: ["M", "L", "XL"],
  colores: ["verde", "beige"],
  fecha_creacion: new Date(),
  activo: true
}
])
```

🔹 **Tipos:**

* Number → precio, stock
* Array → categorías, tallas, colores

---

# 🛒 Colección: `pedidos`

```js
db.pedidos.insertOne({
  cliente_id: ObjectId("ID_CLIENTE"),
  productos: [
    {
      producto_id: ObjectId("ID_PRODUCTO"),
      nombre: "Playera Oversize",
      cantidad: 2,
      precio_unitario: 299.99
    }
  ],
  total: 599.98,
  estado: "pendiente",
  direccion_envio: {
    ciudad: "Ciudad Juárez",
    calle: "Av. Central"
  },
  fecha: new Date()
})
```

🔹 **Tipos:**

* ObjectId → referencias
* Array → productos
* String → estado
* Number → total

---

# 👨‍💼 Colección: `empleados`

```js
db.empleados.insertOne({
  nombre: "Ana Torres",
  puesto: "Vendedora",
  salario: 8000,
  sucursal_id: ObjectId("ID_SUCURSAL"),
  fecha_ingreso: new Date(),
  activo: true
})
```

---

# 🏪 Colección: `sucursales`

```js
db.sucursales.insertOne({
  nombre: "Sucursal Centro",
  direccion: {
    calle: "Av. Juárez",
    numero: 100
  },
  ciudad: "Ciudad Juárez",
  telefono: "6561112233"
})
```

---

# 💳 Colección: `pagos`

```js
db.pagos.insertOne({
  pedido_id: ObjectId("ID_PEDIDO"),
  metodo: "tarjeta",
  monto: 599.98,
  fecha_pago: new Date(),
  estado: "pagado"
})
```

---

# 🔗 Relaciones (forma NoSQL)

* `cliente_id` conecta pedidos con clientes
* `producto_id` conecta pedidos con productos
* `sucursal_id` conecta empleados con sucursales

---

# 💡 Extra para que te luzcas en tu proyecto

Puedes mencionar que en MongoDB:

* No necesitas tablas, usas **colecciones**
* Los datos son **flexibles (JSON/BSON)**
* Puedes **anidar objetos** (como dirección o productos)

---

Si quieres, en el siguiente paso te puedo hacer:
✅ Consultas tipo examen (find, aggregate)
✅ Diagrama sencillo
✅ O cómo conectarlo con XAMPP y tu página

Solo dime 👍
