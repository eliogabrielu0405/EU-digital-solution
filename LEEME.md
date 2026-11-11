# EU Digital Solutions: plataforma de páginas de pedidos

Una sola web con todo adentro:

| Dirección | Qué es |
|---|---|
| `/` | Tu portada de EU Digital, con las 5 plantillas para mostrárselas a tus clientes |
| `/?n=usuario` | La página de un cliente (por ejemplo `/?n=pizzeria-luigi`) |
| `/panel/` | El panel de tus clientes: cambian precios, fotos, tasa, agotados, colores y pagan su suscripción |
| `/admin/` | Tu panel: creas clientes, apruebas pagos, suspendes, eliminas y cambias tus ajustes |
| `/t/pizzeria/`, `/t/arepera/`, `/t/burgers/`, `/t/sushi/`, `/t/boutique/` | Las demos de cada plantilla (los pedidos de prueba llegan a tu WhatsApp) |

Los datos viven en Supabase (proyecto **eudigital**). La web no necesita servidor propio: se sube tal cual a Cloudflare.

## Subirla a Cloudflare

1. Entra a Cloudflare, ve a **Workers y Pages** y toca **Crear**.
2. Elige **Subir recursos estáticos** (o "Upload assets") y ponle de nombre `eudigital`.
3. Sube la carpeta **`web`** de este zip (adentro está el `index.html`; ese archivo tiene que quedar en la raíz de lo que subes).
4. Toca **Implementar**. Te queda una dirección como `eudigital.tu-usuario.workers.dev`.

Para actualizarla más adelante: abre el Worker, ve a **Implementaciones** y sube la carpeta `web` nueva.

## Entrar a tu panel

- Dirección: `/admin/`
- Usuario: `eudigital`
- Contraseña: la que te di cuando creamos la base de datos. Si no la tienes, pídeme que te ponga una nueva.

La sesión del panel de administración se cierra sola al cerrar la pestaña.

## Crear un cliente

1. En tu panel, **Nuevo cliente**: nombre del negocio, usuario (sale solo), plantilla, WhatsApp donde recibe pedidos, precio y fecha de corte.
2. Al crear, te sale la ficha con el enlace de su página, el enlace del panel, el usuario y la contraseña. Mándasela por WhatsApp con el botón.
3. El cliente entra a `/panel/`, cambia su menú, sus fotos y su tasa, y toca **Guardar**.

Cada suscripción es de una plantilla. Si un cliente quiere otra, la cambias desde su ficha (**Cambiar plantilla**): se conservan su nombre, contacto, horario, formas de pago y tasa.

## Pagos y suspensiones

- Cuando un cliente reporta un pago en su panel, te aparece en **Pagos** con la referencia y la captura. **Aprobar** corre su fecha de corte los meses que pagó y, si estaba suspendida, la reactiva.
- Si te pagan por fuera (efectivo, transferencia), ábrelo en **Clientes** y usa **Registrar un pago**.
- **Suspender página** la oculta al instante; **Reactivar** la vuelve a mostrar.
- En **Ajustes** puedes encender **Suspender solas las páginas vencidas**: cada noche a las 12:30 am se suspenden las que pasaron los días de gracia sin pagar.
- **Eliminar página** borra la página, sus fotos, sus pagos y su usuario. Te pide escribir el usuario para confirmar.

En **Ajustes** pon también tus datos de cobro (Pago Móvil, Zelle, Binance): tus clientes los ven en su panel con botón para copiar.

## Cuando compres el dominio

1. Agrega el dominio a Cloudflare.
2. En el Worker, en **Configuración > Dominios y rutas**, agrega `tudominio.com` y la ruta `*.tudominio.com/*`.
3. En `core/config.js` cambia `dominioBase: ""` por `dominioBase: "tudominio.com"` y vuelve a subir la web.

Desde ahí cada cliente tiene su dirección propia: `pizzeria-luigi.tudominio.com`. Si un cliente compra su propio dominio, apúntalo al mismo Worker y escríbelo en su ficha (**Datos de la cuenta > Dominio propio**).

## Dos ajustes recomendados en Supabase

- **Authentication > Sign In / Providers**: apaga **Allow new users to sign up**. Los usuarios los crea tu panel; nadie más debería poder registrarse.
- El plan gratis de Supabase pausa el proyecto si pasa una semana sin visitas. Con clientes usando sus páginas no pasa, pero si un día ves todo caído, entra a Supabase y toca **Restore**.

## Qué hay en cada carpeta

- `core/`: lo que comparten todas las páginas (conexión con Supabase, temas, animaciones, el editor de los paneles).
- `t/<plantilla>/`: cada plantilla con sus fotos, y su `datos.js`, que dice qué puede cambiar el cliente.
- `panel/`, `admin/`: los dos paneles.
- `img/`: el ícono de EU Digital.
