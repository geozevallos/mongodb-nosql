## Contexto del caso

---

La cadena de restaurantes emergente **Sabor & Fusión** ha crecido rápidamente. Su gestión actual basada en comandas de papel y una hoja de Excel para el menú se ha vuelto inmanejable.

Te han contratado para migrar su operación a MongoDB Atlas. Necesitan gestionar dos aspectos críticos:

1. El Menú (Platillos): Que cambia frecuentemente y tiene detalles variados (ingredientes, si es apto para veganos, etc.).
2. Las Comandas (Pedidos): Que entran en tiempo real y tienen estados cambiantes.

## Estructura de datos

---

Vas a trabajar con una base de datos llamada **dbGastroManager** y dos colecciones: menu y pedidos.

### Colección menu

A diferencia de los equipos de cómputo, aquí ejercitaremos los tipos de datos booleanos y arrays para búsquedas más interesantes.

```json
// Estructura de colección
{
  "nombre": "String",
  "categoria": "String (Entrada, Fondo, Bebida, Postre)",
  "precio": "Number",
  "ingredientes": ["Array de Strings"],
  "esVegano": "Boolean",
  "disponible": "Boolean"
}
```

### Colección pedidos

Esta colección permitirá registrar las operaciones diarias:

```json
// Estructura de la colección
{
  "mesa": "Number",
  "mesero": "String",
  "items": ["Array de Strings (Nombres de platillos)"],
  "total": "Number",
  "estado": "String (Pendiente, En Cocina, Servido, Pagado)"
}
```

## Actividades

---

### Inserción en colección menu

La cocina ha diseñado nuevos platillos. Inserta documentos utilizando **insertOne** en la colección `menu` los siguientes documentos uno por uno:

1. **Lomo Saltado**: Fondo, 45.00, Ingredientes: ["Lomo fino", "Cebolla", "Tomate", "Papas fritas"], No es vegano, Disponible.
2. **Ensalada César**: Entrada, 22.00, Ingredientes: ["Lechuga", "Crutones", "Parmesano", "Pollo"], No es vegano, Disponible.
3. **Limonada Frozen**: Bebida, 12.00, Ingredientes: ["Limón", "Hielo", "Jarabe"], Es vegano, Disponible.
4. Agrega 4 opciones de menú.

### Inserción en colección pedidos

El restaurante abre sus puertas. Inserta en la colección `pedidos` las siguientes comandas usando `insertMany`:

1. Mesa 5, Mesero Juan, Items: ["Lomo Saltado", "Limonada Frozen"], Total: 57.00, Estado: "Servido".
2. Mesa 2, Mesera Ana, Items: ["Ensalada César"], Total: 22.00, Estado: "Pendiente".
3. Mesa 8, Mesero Juan, Items: ["Lomo Saltado", "Lomo Saltado", "Inca Kola"], Total: 95.00, Estado: "En Cocina".
4. Mesa 1, Mesera Ana, Items: ["Café", "Torta de Chocolate"], Total: 18.00, Estado: "Pagado".
5. Agrega 11 pedidos a la colección pedidos

### Consultas estratégicas

El gerente necesita reportes rápidos. Escribe las consultas para:

1. Listar todos los platillos que sean de la categoría "Fondo".
2. Buscar todos los platillos que **NO** contengan "Pollo" en sus ingredientes (Uso de operador `$ne` o lógica inversa).
3. Listar solo el `nombre` y `precio` de los platillos que sean Veganos (`esVegano: true`).
4. Encontrar todos los pedidos que esté atendiendo el mesero "Juan" y que todavía estén en estado "En Cocina".

### Mantenimiento del Menú

La inflación y la operación diaria obligan a realizar cambios.

1. **Ajuste de Precios:** El limón ha subido de precio. Actualiza el precio de la "Limonada Frozen" a 15.00.
2. **Mejora de Receta (Reto Array):** El Chef ha decidido agregar "Sillao" a los ingredientes del "Lomo Saltado". (Uso sugerido de `$push`).
3. **Cambio de Estado:** La Mesa 2 ya recibió su comida. Actualiza el estado del pedido de la Mesa 2 de "Pendiente" a "Servido".
4. Agrega 3 propuestas más de cambios de datos en las colecciones.

### Depuración

1. **Eliminación Específica:** Se ha retirado la "Ensalada César" de la carta. Elimínala de la colección `menu`.
2. **Limpieza Operativa:** El día ha terminado, elimina todos los pedidos que tengan el estado "Pagado" para limpiar el sistema para mañana.