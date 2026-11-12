# EU Digital Solutions: plataforma de páginas de pedidos

Una sola web con todo adentro:

| Dirección | Qué es |
|---|---|
| `/` | Tu portada de EU Digital, con las 5 plantillas para mostrárselas a tus clientes |
| `/?n=usuario` | La página de un cliente (por ejemplo `/?n=pizzeria-luigi`) |
| `/panel/` | El panel de tus clientes: precios, fotos, tasa, agotados, colores, sus pedidos, su código QR y el pago de su suscripción |
| `/admin/` | Tu panel: creas clientes, apruebas pagos, suspendes, eliminas y cambias tus ajustes |
| `/t/pizzeria/`, `/t/arepera/`, `/t/burgers/`, `/t/sushi/`, `/t/boutique/` | Las demos de cada plantilla (los pedidos de prueba llegan a tu WhatsApp) |

Los datos viven en Supabase (proyecto **eudigital**).

## Dónde está publicada

En Vercel: **https://eu-digital-solutions.vercel.app**. El código está en GitHub, en el repositorio `EU-digital-solution`. Cada cambio que se sube ahí se publica solo en uno o dos minutos.

## Entrar a tu panel

- Dirección: `/admin/`
- Usuario: `eudigital`
- Contraseña: la tuya. Si se te olvida, pídeme que te ponga una nueva.

La sesión del panel de administración se cierra sola al cerrar la pestaña.

## Crear un cliente

1. En tu panel, **Nuevo cliente**: nombre del negocio, usuario (sale solo), plantilla, WhatsApp donde recibe pedidos, precio y fecha de corte.
2. Al crear, te sale la ficha con el enlace de su página, el enlace del panel, el usuario y la contraseña. Mándasela por WhatsApp con el botón.
3. El cliente entra a `/panel/`, cambia su menú, sus fotos y su tasa, y toca **Guardar**.

Cada suscripción es de una plantilla. Si un cliente quiere otra, la cambias desde su ficha (**Cambiar plantilla**): se conservan su nombre, contacto, horario, formas de pago y tasa.

Los usuarios solo se crean desde tu panel. El registro libre de Supabase está cerrado, así que tampoco funciona **Add user** en el panel de Supabase.

## Pedidos y estadísticas

Cuando un cliente final toca **Enviar por WhatsApp** en una página, el pedido queda guardado: productos, total, tipo de entrega y forma de pago. No se guardan nombres, teléfonos ni direcciones.

- El dueño ve en su panel, en **Mis pedidos**, los pedidos de hoy, de la semana y del mes, un gráfico de los últimos 14 días, lo que más le piden y sus últimos pedidos. En **Hoy** le sale el resumen.
- Tú lo ves en la ficha de cada cliente. Sirve para mostrarle lo que le trae la página cuando le toca renovar.
- Si un cliente toca enviar dos veces con el mismo pedido, cuenta una. Las demos no cuentan.
- Los pedidos de más de 13 meses se borran solos.

## Compartir y código QR

En el panel del cliente, **Compartir** tiene:

- Su enlace, con botones para copiarlo y compartirlo.
- Cómo se ve el enlace en WhatsApp: una tarjeta con su nombre, su logo, una foto de su página y sus colores. Se crea sola cuando abre el panel y se actualiza cada vez que guarda.
- Su código QR: un cartel listo para imprimir (con su nombre y su logo) o solo el código.

La vista previa también funciona en Facebook, Instagram, Telegram y X. La portada y las demos tienen su propia tarjeta (están en `img/compartir.jpg` y `t/<plantilla>/compartir.jpg`; se hacen con `tools/tarjetas.html`).

## Pagos y suspensiones

- Cuando un cliente reporta un pago en su panel, te aparece en **Pagos** con la referencia y la captura. **Aprobar** corre su fecha de corte los meses que pagó y, si estaba suspendida, la reactiva.
- Si te pagan por fuera (efectivo, transferencia), ábrelo en **Clientes** y usa **Registrar un pago**: queda registrado y aprobado de una vez.
- Un cliente puede tener hasta 5 pagos esperando revisión y reportar hasta 8 en un día; así no se te llena la lista con reportes repetidos.
- **Suspender página** la oculta al instante; **Reactivar** la vuelve a mostrar.
- En **Ajustes** puedes encender **Suspender solas las páginas vencidas**: cada noche a las 12:30 am se suspenden las que pasaron los días de gracia sin pagar.
- **Eliminar página** borra la página, sus fotos, sus pagos, sus pedidos y su usuario. Te pide escribir el usuario para confirmar.

En **Ajustes** pon también tus datos de cobro (Pago Móvil, Zelle, Binance): tus clientes los ven en su panel con botón para copiar.

## Cuando compres el dominio

1. En Vercel, abre el proyecto **eu-digital-solutions** y ve a **Settings > Domains**. Agrega `tudominio.com`.
2. Para que cada cliente tenga su subdominio (`pizzeria-luigi.tudominio.com`), agrega también `*.tudominio.com`. Vercel te pide usar sus servidores de nombres (nameservers) para eso; te dice cuáles poner donde compraste el dominio.
3. En `core/config.js` cambia `dominioBase: ""` por `dominioBase: "tudominio.com"` y súbelo.

Si un cliente compra su propio dominio, agrégalo también en **Settings > Domains** y escríbelo en su ficha (**Datos de la cuenta > Dominio propio**).

## Supabase

El plan gratis pausa el proyecto si pasa una semana sin visitas y no guarda copias de seguridad. Con clientes usando sus páginas no se pausa, pero si un día ves todo caído, entra a Supabase y toca **Restore**. Cuando tengas varios clientes pagando, conviene el plan Pro.

## Qué hay en cada carpeta

- `core/`: lo que comparten todas las páginas (conexión con Supabase, temas, animaciones, el editor de los paneles, el QR y la tarjeta para compartir).
- `t/<plantilla>/`: cada plantilla con sus fotos, y su `datos.js`, que dice qué puede cambiar el cliente.
- `panel/`, `admin/`: los dos paneles.
- `middleware.js`: arma la vista previa cuando se comparte un enlace.
- `img/`: el ícono de EU Digital y la tarjeta de la portada.
- `sql/`: los cambios de la base de datos (ya están aplicados).
- `tools/`: herramientas internas; no se publican.
