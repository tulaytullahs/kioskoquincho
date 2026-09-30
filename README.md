# Kiosco del Club — MVP

Sistema web local de gestión de kiosco: menú y pedidos públicos, administración con productos, categorías, imágenes, inventario, ventas, compras, gastos, caja, pedidos y tablero de indicadores. Los importes se almacenan como centavos (`INTEGER`) para evitar errores de coma flotante.

## Arquitectura

- `client/`: React + Vite, interfaz pública responsive y panel administrativo.
- `server/`: Express REST API, JWT, validación de imágenes con Multer y SQLite.
- `server/data/kiosco.db`: se crea automáticamente; `server/uploads/` guarda imágenes locales.

Tablas principales: `users`, `categories`, `products`, `stock_movements`, `sales`, `sale_items`, `payments`, `purchases`, `purchase_items`, `expenses`, `orders`, `order_items`, `cash_registers`. También se incluyen `ingredients`, `recipes` y `recipe_items` para extender productos preparados.

## Requisitos e instalación

Se necesita Node.js 20+.

```powershell
npm.cmd install
npm.cmd --prefix server install
npm.cmd --prefix client install
Copy-Item server/.env.example server/.env
npm.cmd --prefix server run seed
npm.cmd install
npm.cmd run dev
```

Abrí `http://localhost:5173`. La API corre en `http://localhost:3001`.

Usuario inicial: `admin@kiosco.local` / `admin123`. Cambialo antes de usarlo fuera de desarrollo.

## Uso

- El menú público permite confirmar pedidos para retirar.
- En `/admin` se inicia sesión y se administran productos, existencias, ventas y pedidos.
- Al entregar un pedido se genera la venta y se descuenta el stock. Las ventas manuales hacen lo mismo; las compras incrementan stock y los gastos se reflejan en el dashboard.
- La API también expone compras, gastos y caja para completar la operación desde clientes posteriores.

## Pagos

Transferencias conservan estado pendiente. La integración de Mercado Pago está preparada para recibir `MERCADOPAGO_ACCESS_TOKEN`; antes de producción falta implementar el endpoint de preferencia y el webhook firmado que confirme pagos. No hay credenciales hardcodeadas.

## Rutas API principales

`/api/auth/login`, `/api/products`, `/api/categories`, `/api/stock`, `/api/sales`, `/api/purchases`, `/api/expenses`, `/api/orders`, `/api/cash`, `/api/dashboard`.
