**MANUAL DE USUARIO**

**EnergyMonitor**

_Sistema de monitoreo de consumo eléctrico para el hogar_

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Campo</strong></th>
<th><strong>Detalle</strong></th>
</tr>
<tr class="odd">
<th><strong>Proyecto</strong></th>
<th>EnergyMonitor – Monitor de Consumo Eléctrico</th>
</tr>
<tr class="header">
<th><strong>Tipo de documento</strong></th>
<th>Manual de Usuario</th>
</tr>
<tr class="odd">
<th><strong>Integrantes</strong></th>
<th><p>Juan Esteban Cortez Parra (Líder de proyecto / Gestor de BD)</p>
<p>Miguel Ángel García Artunduaga (Analista / UX-UI)</p></th>
</tr>
<tr class="header">
<th><strong>Programa de formación</strong></th>
<th>Tecnólogo en Análisis y Desarrollo de Software (ADSO)</th>
</tr>
<tr class="odd">
<th><strong>Instructor</strong></th>
<th>Karol Dayana Correa</th>
</tr>
<tr class="header">
<th><strong>Ficha</strong></th>
<th>3145556</th>
</tr>
<tr class="odd">
<th><strong>Centro de formación</strong></th>
<th>Sena Industria-Neiva</th>
</tr>
<tr class="header">
<th><strong>Fecha</strong></th>
<th>23 de julio de 2026</th>
</tr>
<tr class="odd">
<th><strong>Versión del documento</strong></th>
<th>2.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **Control de versiones**

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 14%" />
<col style="width: 53%" />
<col style="width: 19%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Versión</strong></th>
<th><strong>Fecha</strong></th>
<th><strong>Descripción del cambio</strong></th>
<th><strong>Responsable</strong></th>
</tr>
<tr class="odd">
<th>1.0</th>
<th>09/07/2026</th>
<th>Creación inicial del manual de usuario de EnergyMonitor.</th>
<th>Juan Esteban Cortes Parra<br />
Miguel Angel Garcia Artunduaga</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **Tabla de contenido**

Control de versiones

Introducción

Objetivo

Alcance

Requisitos para usar el sistema

Landing Page

Registro

Verificación de cuenta

Inicio de sesión

Recuperación de contraseña

Verificación para recuperación

Dashboard

Consumo (detalle del proyecto)

Perfil

Configuración

Favoritos

Notificaciones

Cierre de sesión

Mensajes del sistema

Preguntas frecuentes

Glosario

Anexos

# **Introducción**

EnergyMonitor nació de una idea sencilla: que cualquier persona pueda saber cuánto está gastando en electricidad sin tener que esperar a que su proveedor del servicio haga entrega del consumo final. Detrás de la aplicación hay sensores de corriente PZEM-004T V3, instalados junto a los tomacorrientes del hogar y controlados por un microcontrolador ESP32, que envían la información con la que el sistema arma paneles y gráficos de consumo casi en tiempo real.

Este documento explica cómo usar la plataforma tal como está disponible hoy: crear una cuenta, verificarla, iniciar sesión, recuperar la contraseña cuando se olvida, y moverse por el panel principal, el perfil y la configuración. El proyecto sigue en construcción, así que funciones como el detalle de consumo por electrodoméstico todavía no aparecen aquí; se irán agregando a este manual a medida que queden disponibles en el frontend.

# **Objetivo**

Que cualquier usuario, sin ayuda del equipo de desarrollo, pueda registrarse, entrar al sistema y moverse con confianza por las pantallas que EnergyMonitor tiene disponibles en la actualidad.

# **Alcance**

Este documento cubre lo que un usuario final necesita para dar sus primeros pasos en la plataforma: el registro de una cuenta y su verificación por correo, el inicio de sesión, la recuperación de contraseña, la navegación por la landing page pública, el panel principal (Dashboard),creación y gestión de hogares,roles que podemos encontrar dentro del hogar y flujo de estos, la gestión del perfil personal, los ajustes de configuración y el cierre de sesión.

Lo que no va a encontrar aquí es la arquitectura interna del sistema, el código fuente, el diseño de la base de datos, ni los módulos que todavía están en construcción, como el detalle de consumo por proyecto, la vinculación de dispositivos, los umbrales de alerta o la administración de usuarios dentro de un hogar. Esas pantallas se agregarán a este manual cuando estén implementadas y disponibles para los usuarios finales.

# **Requisitos para usar el sistema**

Para entrar a EnergyMonitor solo hacen falta un computador o un celular con un navegador actualizado (Chrome, Firefox, Edge o Safari), conexión estable a internet y un correo electrónico válido para crear la cuenta. El sistema no pide instalar nada adicional: todo funciona directamente desde el navegador.

El hardware de medición (los sensores PZEM-004T V3 y el ESP32) se instala y configura aparte, y no hace parte de este manual; aquí solo se explica el uso de la plataforma web.

## **Landing Page**

**Descripción:** Es la puerta de entrada al sistema: la pantalla pública que cualquier persona puede ver sin necesidad de tener una cuenta. Presenta de qué se trata EnergyMonitor y da acceso directo a Iniciar sesión o Registrarse.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                    |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Público general (no requiere cuenta).                                                                                                                                                                                                                                                          |
| **Ruta de acceso**             | URL principal del sistema (página de inicio).                                                                                                                                                                                                                                                  |
| **Componentes de la pantalla** | Barra de navegación superior con los enlaces Inicio, Beneficios, Cómo funciona, Sobre nosotros y Soporte; selector de idioma; botón para cambiar el tema visual; botones Iniciar sesión y Registrarse; sección principal con el mensaje de bienvenida y los beneficios destacados del sistema. |

**Pasos para utilizarla:**

> 1\. Abrir la URL de EnergyMonitor en el navegador.
>
> 2\. Recorrer la página para conocer qué hace el sistema, revisando las secciones Beneficios y Cómo funciona.
>
> 3\. Si se quiere más contexto del proyecto, entrar al enlace Sobre nosotros.
>
> 4\. Hacer clic en Registrarse si es la primera vez, o en Iniciar sesión si ya se tiene una cuenta.

**Resultado esperado:** La persona entiende para qué sirve la plataforma y puede continuar hacia el registro o el inicio de sesión.

**Observaciones:** El idioma y el tema visual que se elijan aquí se mantienen luego dentro de la aplicación.

![alt text](../../assets/Web/image.png)

_Landing Page_

El menú de inicio nos lleva a Cómo funciona: Este apartado nos presenta cómo funciona el flujo dentro de la aplicación de manera general desde registrarse o iniciar sesión hasta recibir alertas inteligentes.

![alt text](../../assets/Web/image-1.png)

_Como funciona_

Dentro del menú de inicio también podremos encontrar la sección de Beneficios, una sección con tarjetas cortas que resumen por qué conviene usar el sistema: ahorrar en la factura al identificar consumos innecesarios, recibir alertas en tiempo real ante picos o actividad inusual, reducir la huella ambiental con un uso más eficiente de la energía, y contar con una configuración simple que no exige conocimientos técnicos para conectar los dispositivos.

![alt text](../../assets/Web/image-2.png)

_Beneficios_

Desde ese mismo menú se llega a Sobre nosotros, una página que cuenta con más detalle el problema que busca resolver EnergyMonitor, la misión del proyecto y una breve explicación del hardware que hace posible el monitoreo.

![alt text](../../assets/Web/image-3.png)

_Sobre nosotros_

##

El espacio pensado para cuando el usuario necesita ayuda: encontrar respuestas a dudas frecuentes o tener un canal de contacto directo con el equipo de EnergyMonitor si algo no funciona como debería.

![alt text](../../assets/Web/image-4.png)

_Soporte_

##

## **Registro**

**Descripción:** Formulario donde una persona nueva crea su cuenta en EnergyMonitor, con nombre, correo y contraseña, o directamente con su cuenta de Google.

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Campo</strong></th>
<th><strong>Detalle</strong></th>
</tr>
<tr class="odd">
<th><strong>Rol que puede acceder</strong></th>
<th><p>Público general (sin cu</p>
<p>enta creada).</p></th>
</tr>
<tr class="header">
<th><strong>Ruta de acceso</strong></th>
<th>Landing Page &gt; botón Registrarse.</th>
</tr>
<tr class="odd">
<th><strong>Componentes de la pantalla</strong></th>
<th>Campos Nombre, Correo electrónico y Contraseña; casilla de aceptación de términos de uso y política de privacidad; botón Registrarse; opción Registrarse usando Google; enlace ¿Ya tienes una cuenta? Inicia sesión.</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

**Pasos para utilizarla:**

> 1\. Desde la Landing Page, hacer clic en Registrarse.
>
> 2\. Completar los campos Nombre, Correo electrónico y Contraseña, o elegir la opción de Google.
>
> 3\. Aceptar los términos de uso y la política de privacidad.
>
> 4\. Confirmar con el botón Registrarse.

**Resultado esperado:** El sistema crea la cuenta y redirige de inmediato a la pantalla de verificación de correo.

**Observaciones:** Si el correo ya está registrado, el sistema lo indica y sugiere iniciar sesión en su lugar.

![alt text](../../assets/Web/image-5.png)

_Registro_

## **Verificación de cuenta**

**Descripción:** Pantalla que confirma que el correo usado en el registro es real. El sistema envía un código de 6 dígitos que hay que escribir aquí para activar la cuenta.

| **Campo**                      | **Detalle**                                                                                                     |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario recién registrado, con la cuenta todavía sin verificar.                                                 |
| **Ruta de acceso**             | Se abre automáticamente al terminar el Registro.                                                                |
| **Componentes de la pantalla** | Campo para el código de 6 dígitos; enlace ¿No te llegó el código?; enlace ¿Ya tienes una cuenta? Inicia sesión. |

**Pasos para utilizarla:**

> 1\. Revisar la bandeja de entrada del correo con el que se registró la cuenta.
>
> 2\. Copiar el código de 6 dígitos recibido.
>
> 3\. Escribirlo en el campo de verificación.
>
> 4\. Confirmar para activar la cuenta.

**Resultado esperado:** La cuenta queda verificada y el usuario ya puede iniciar sesión con normalidad.

**Observaciones:** Si el código no llega, se puede solicitar que se reenvíe desde el enlace ¿No te llegó el código?

![alt text](../../assets/Web/image-6.png)

_Verificación de cuenta_

## **Inicio de sesión**

**Descripción:** Formulario para entrar con una cuenta que ya fue creada y verificada.

| **Campo**                      | **Detalle**                                                                                                                                                                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Rol que puede acceder**      | Público general con cuenta creada y verificada.                                                                                                                                                        |
| **Ruta de acceso**             | Landing Page \> botón Iniciar sesión.                                                                                                                                                                  |
| **Componentes de la pantalla** | Campos Correo electrónico y Contraseña; casilla Recordar datos; enlace ¿Olvidaste tu contraseña?; botón Iniciar sesión; opción Iniciar sesión usando Google; enlace ¿No tienes una cuenta? Regístrate. |

**Pasos para utilizarla:**

> 1\. Abrir la URL de EnergyMonitor.
>
> 2\. Hacer clic en Iniciar sesión.
>
> 3\. Escribir el correo y la contraseña registrados.
>
> 4\. Confirmar con el botón Iniciar sesión.

**Resultado esperado:** El sistema valida las credenciales y lleva directamente al Dashboard.

**Observaciones:** Si el correo o la contraseña no coinciden, aparece un mensaje de error y no se permite continuar.

![alt text](../../assets/Web/image-7.png)

_Inicio de sesión_

## **Recuperación de contraseña**

**Descripción:** Punto de entrada para recuperar el acceso cuando la contraseña se olvida. En esta pantalla solo se pide el correo con el que se creó la cuenta.

| **Campo**                      | **Detalle**                                                                                              |
| ------------------------------ | -------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Público general (con una cuenta ya creada).                                                              |
| **Ruta de acceso**             | Inicio de sesión \> enlace ¿Olvidaste tu contraseña?                                                     |
| **Componentes de la pantalla** | Campo Correo electrónico; botón para enviar el código de recuperación; enlace Volver a Inicio de sesión. |

**Pasos para utilizarla:**

> 1\. En la pantalla de Inicio de sesión, hacer clic en ¿Olvidaste tu contraseña?
>
> 2\. Escribir el correo con el que se creó la cuenta.
>
> 3\. Confirmar el envío.

**Resultado esperado:** El sistema envía un código de verificación de 6 dígitos al correo para continuar con la recuperación.

**Observaciones:** Si el correo escrito no está registrado en el sistema, conviene revisar que esté bien digitado antes de insistir.

![alt text](../../assets/Web/image-8.png)

_Recuperación de contraseña_

## **Verificación para recuperación**

**Descripción:** Paso donde se digita el código enviado por correo para confirmar que la recuperación la está haciendo el dueño real de la cuenta, y desde donde se define la nueva contraseña.

| **Campo**                      | **Detalle**                                                                                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario en proceso de recuperación de contraseña.                                                     |
| **Ruta de acceso**             | Se abre automáticamente después de solicitar la recuperación de contraseña.                           |
| **Componentes de la pantalla** | Campo para el código de verificación de 6 dígitos; campos para la nueva contraseña y su confirmación. |

**Pasos para utilizarla:**

> 1\. Revisar el correo y copiar el código recibido.
>
> 2\. Escribirlo en la pantalla de verificación.
>
> 3\. Definir la nueva contraseña y confirmarla.
>
> 4\. Guardar los cambios.

**Resultado esperado:** La contraseña queda actualizada y el sistema regresa a la pantalla de inicio de sesión.

**Observaciones:** Por seguridad, el código tiene un tiempo límite de uso; si expira, hay que solicitar uno nuevo desde Recuperación de contraseña

![alt text](../../assets/Web/image-9.png)

_Verificar correo_

## **Dashboard**

**Descripción:** Es el panel principal, lo primero que aparece después de iniciar sesión. Muestra los proyectos (hogares u oficinas) que el usuario tiene registrados, o una invitación a crear el primero si todavía no existe ninguno.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Rol que puede acceder**      | Usuario autenticado.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **Ruta de acceso**             | Se abre automáticamente después del inicio de sesión; también está disponible desde el ícono Inicio del menú lateral.                                                                                                                                                                                                                                                                                                                |
| **Componentes de la pantalla** | Menú lateral con el ícono de Inicio y el acceso a Ajustes; ícono de perfil en la parte inferior del menú; en la zona central, las tarjetas de los proyectos registrados o, si todavía no hay ninguno, un mensaje guía con el botón Crear Proyecto. El menú lateral incluye otros accesos (como Favoritos y Notificaciones) que corresponden a módulos que aún están en construcción y que se documentarán cuando queden disponibles. |

**Pasos para utilizarla:**

> 1\. Iniciar sesión en el sistema.
>
> 2\. El Dashboard aparece automáticamente, sin necesidad de buscarlo en el menú.
>
> 3\. Si es la primera vez, usar el botón Crear Proyecto para registrar el primer hogar u oficina.
>
> 4\. Si ya existen proyectos, hacer clic sobre la tarjeta de alguno para entrar a su detalle (pestañas Consumo, Historial, Usuarios, Dispositivos, Umbrales y Proyecto).

**Resultado esperado:** El usuario ve de un vistazo el estado de sus proyectos y puede crear uno nuevo cuando lo necesite.

**Observaciones:** El detalle de cada proyecto se documenta en la sección Consumo (detalle del proyecto), más adelante en este manual.

![alt text](../../assets/Web/image-10.png)

_Dashboard sin proyectos registrados_

![alt text](../../assets/Web/image-11.png)

_Dashboard con proyectos registrados_

## **Formulario para registrar hogares:**

**Descripción:** Este formulario le permite a cualquier usuario autenticado crear un hogar para poderlo administrar.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                         |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado.                                                                                                                                                                                                                                                |
| **Ruta de acceso**             | En el dashboard podemos encontrar 2 rutas de acceso la primera se puede encontrar cuando nos autenticamos dentro de la página y llegamos al dashboard en el centro y la segunda la podemos encontrar en la parte superior derecha en el botón con signo de +.       |
| **Componentes de la pantalla** | Dentro del formulario para crear hogar podemos encontrar 4 campos donde podemos diligenciar nombre, tipo de hogar, dirección y descripción como campo opcional además de 3 botones uno para cancelar el proceso, registrar hogar y una X para salir del formulario. |

**Pasos para utilizarla:**

> 1\. El Dashboard aparece automáticamente, sin necesidad de buscarlo en el menú
>
> 2\. Si es la primera vez, usar el botón Crear Proyecto para registrar el primer hogar.
>
> 3\. Si ya existen proyectos, hacer clic sobre el botón en la parte superior derecha con signo de +.
>
> 4\. Diligenciar formulario para registrar hogar.

**Resultado esperado:** El usuario ve una tarjeta con el hogar registrado.

**Observaciones:** Solamente se puede administrar el hogar dentro del mismo.

![alt text](../../assets/Web/image-12.png)

_Formulario para registrar hogares_

## **Formulario para unirse a hogar:**

**Descripción:** Este formulario le permite a cualquier que se encuentre registrador y cuente con el código de acceso de un hogar pueda acceder al mismo.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado.                                                                                                                                                                                                                                          |
| **Ruta de acceso**             | En el dashboard podemos encontrar 2 rutas de acceso la primera se puede encontrar cuando nos autenticamos dentro de la página y llegamos al dashboard en el centro y la segunda la podemos encontrar en la parte superior derecha en el botón con signo de +. |
| **Componentes de la pantalla** | Dentro del formulario para unirse a hogar puedes encontrar un campo donde podrás digitar el código de acceso, además de 3 botones uno para cancelar el proceso, registrar hogar y una X para salir del formulario.                                            |

**Pasos para utilizarla:**

> 1\. El Dashboard aparece automáticamente, sin necesidad de buscarlo en el menú
>
> 2\. Si es la primera vez, usar el botón para unirte a un hogar.
>
> 3\. Si ya existen hogares, hacer clic sobre el botón en la parte superior derecha con signo de +.
>
> 4\. Diligenciar el formulario para acceder a un hogar.

**Resultado esperado:** El usuario ve una tarjeta con el hogar al que se ha unido.

**Observaciones:** Dentro del hogar el miembro solo le es permitido consultar información del hogar.

![alt text](../../assets/Web/image-13.png)

_Formulario para unirse a hogares_

### **Consumo**

**Descripción:** Es la pestaña que se abre por defecto al entrar a un proyecto. Muestra el estado del consumo eléctrico casi en tiempo real: la potencia actual, el consumo del día, los dispositivos conectados, la curva de consumo de las últimas 24 horas, la distribución por dispositivo y el avance frente a los límites diario y mensual.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado,cualquier miembro del proyecto.                                                                                                                                                                                                                                                   |
| **Ruta de acceso**             | Dashboard \> tarjeta del proyecto \> pestaña Consumo.                                                                                                                                                                                                                                                 |
| **Componentes de la pantalla** | Tarjetas KPI de potencia actual, consumo de hoy y dispositivos activos; gráfico de área con el consumo de las últimas 24 horas; gráfico circular con la distribución actual del consumo; gráfico de barras con el consumo por dispositivo; barras de progreso del límite diario y del límite mensual. |

**Pasos para utilizarla:**

> 1\. Entrar al Dashboard y hacer clic sobre la tarjeta del proyecto que se quiere consultar.
>
> 2\. El sistema abre el detalle del proyecto en la pestaña Consumo.
>
> 3\. Revisar las tarjetas KPI para ver el estado general.
>
> 4\. Bajar por la pantalla para consultar los gráficos de consumo por horas, por dispositivo y el avance de los límites configurados.

**Resultado esperado:** El usuario entiende cómo va el consumo eléctrico del proyecto en el momento de la consulta.

**Observaciones:** Los datos mostrados dependen de la información que reportan los dispositivos ESP32 vinculados; si un dispositivo está desconectado, su aporte no se refleja en tiempo real.

![alt text](../../assets/Web/image-14.png)

_Consumo_

### **Historial**

**Descripción:** Guarda el consumo del proyecto a lo largo del tiempo, no solo lo que pasó hoy. Se puede filtrar por día, semana, mes o año, y dentro de una vista mensual, revisar semana por semana.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                               |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado, cualquier miembro del proyecto.                                                                                                                                                                                                                                      |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Historial.                                                                                                                                                                                                                                                |
| **Componentes de la pantalla** | Selector de periodo (Día, Semana, Mes, Año); selector de semana dentro del mes; gráfico de barras con la distribución de consumo por dispositivo a lo largo del periodo; tabla de ranking de consumo por dispositivo; tarjetas de consumo total del periodo, promedio y mayor consumidor. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Historial.
>
> 2\. Elegir el periodo que se quiere consultar (día, semana, mes o año).
>
> 3\. Si se eligió Mes, seleccionar la semana específica o la opción Mostrar todo.
>
> 4\. Revisar el gráfico y la tabla de ranking para identificar qué dispositivo consumió más.

**Resultado esperado:** El usuario puede comparar el consumo entre distintos periodos y detectar qué electrodoméstico pesa más en la factura.

![alt text](../../assets/Web/image-15.png)

_Historial_

### **Usuarios (Propietario)**

**Descripción:** Muestra quién tiene acceso al proyecto y administra los miembros pertenecientes al hogar.

| **Campo**                      | **Detalle**                                                          |
| ------------------------------ | -------------------------------------------------------------------- |
| **Rol que puede acceder**      | Propietario del hogar (invitar solo disponible para el propietario). |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Usuarios.                            |
| **Componentes de la pantalla** | Lista de miembros del proyecto con su rol (Propietario o Miembro).   |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Usuarios.
>
> 2\. Revisar la lista de miembros actuales.
>
> 3\. Si se tiene el rol de propietario, puedes compartir el código de acceso para permitir que miembros se unan.

**Resultado esperado:** Visualizar lista de miembros actuales.

**Observaciones:** Solo el propietario del proyecto puede eliminar miembros.

![alt text](../../assets/Web/image-16.png)

_Usuarios_

### **Usuarios (Miembro)**

**Descripción:** Muestra quién tiene acceso al proyecto.

| **Campo**                      | **Detalle**                                                        |
| ------------------------------ | ------------------------------------------------------------------ |
| **Rol que puede acceder**      | Miembro (invitar solo disponible para el propietario).             |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Usuarios.                          |
| **Componentes de la pantalla** | Lista de miembros del proyecto con su rol (Propietario o Miembro). |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Usuarios.
>
> 2\. Revisar la lista de miembros actuales.

**Resultado esperado:** Visualizar lista de miembros actuales.

**Observaciones:** Solo el propietario del proyecto puede eliminar miembros.

![alt text](../../assets/Web/image-17.png)

_Usuarios_

###

###

### **Dispositivos (Propietario)**

**Descripción:** Lista los dispositivos ESP32 vinculados al proyecto, con su estado de conexión y el consumo que están reportando, además de poder vincular más dispositivos al hogar.

| **Campo**                      | **Detalle**                                                                                                                                                      |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Propietario del hogar.                                                                                                                                           |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Dispositivos.                                                                                                                    |
| **Componentes de la pantalla** | Botón Vincular dispositivo; lista de dispositivos con nombre, ubicación, estado (En línea / Desconectado) y consumo actual; opción para eliminar un dispositivo. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Dispositivos.
>
> 2\. Revisar el estado de cada dispositivo vinculado.
>
> 3\. Para agregar uno nuevo, usar el botón Vincular dispositivo.
>
> 4\. Para retirar uno, usar el ícono de eliminar junto al dispositivo.

**Resultado esperado:** El usuario sabe cuántos dispositivos están activos y puede vincular o retirar dispositivos según lo necesite.

**Observaciones:** Un dispositivo aparece como Desconectado cuando deja de reportar datos por un tiempo prolongado.

![alt text](../../assets/Web/image-18.png)

_Dispositivos_

### **Dispositivos (Miembro)**

**Descripción:** Lista los dispositivos ESP32 vinculados al proyecto, con su estado de conexión y el consumo que están reportando.

| **Campo**                      | **Detalle**                                                                                                                 |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Miembro del hogar.                                                                                                          |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Dispositivos.                                                                               |
| **Componentes de la pantalla** | Botón Vincular dispositivo; lista de dispositivos con nombre, ubicación, estado (En línea / Desconectado) y consumo actual. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Dispositivos.
>
> 2\. Revisar el estado de cada dispositivo vinculado.

**Resultado esperado:** El usuario sabe cuántos dispositivos están activos.

**Observaciones:** Un dispositivo aparece como Desconectado cuando deja de reportar datos por un tiempo prolongado.

![alt text](../../assets/Web/image-19.png)

_Dispositivos_

### **Umbrales (Propietario)**

**Descripción:** Define los límites de consumo diario y mensual del proyecto, usados para generar alertas y recomendaciones.

| **Campo**                      | **Detalle**                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Propietario del hogar                                                                                                     |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Umbrales.                                                                                 |
| **Componentes de la pantalla** | Interruptor para usar los umbrales por defecto del sistema; campos Límite diario y Límite mensual; botón Guardar cambios. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Umbrales.
>
> 2\. Decidir si se usan los umbrales por defecto o si se van a personalizar.
>
> 3\. Si se personalizan, desactivar el interruptor y escribir los nuevos límites diario y mensual.
>
> 4\. Confirmar con Guardar cambios.

**Resultado esperado:** Los nuevos límites quedan guardados y el sistema los usa para generar alertas cuando el consumo se acerque o los supere.

**Observaciones:** Los umbrales se guardan por proyecto; cambiarlos en uno no afecta a los demás proyectos del usuario.

![alt text](../../assets/Web/image-20.png)

_Umbrales_

### **Umbrales (Miembro)**

**Descripción:** Visualizar los límites de consumo diario y mensual del hogar que se encuentra registrados, usados para generar alertas y recomendaciones.

| **Campo**                      | **Detalle**                               |
| ------------------------------ | ----------------------------------------- |
| **Rol que puede acceder**      | Miembro del hogar                         |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Umbrales. |
| **Componentes de la pantalla** | campos Límite diario y Límite mensual.    |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Umbrales.
>
> 2\. Decidir si se usan los umbrales por defecto o si se van a personalizar.

**Resultado esperado:** Visualizar los límites de consumo diario y mensual del hogar que se encuentra registrados.

**Observaciones:** Los umbrales se definen por hogar.

![alt text](../../assets/Web/image-21.png)

_Umbrales_

### **Información de hogar (Propietario)**

**Descripción:** Reúne la información general del hogar (nombre, tipo, dirección, descripción, fecha de creación), los datos del propietario del hogar y el código de acceso para compartir el hogar con otras personas.

| **Campo**                      | **Detalle**                                                                                                                            |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Propietario del hogar (eliminar el proyecto solo disponible para el responsable).                                                      |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Proyecto.                                                                                              |
| **Componentes de la pantalla** | Bloque de información del proyecto; bloque propietario con correo; código de acceso para compartir e invitar; botón Eliminar proyecto. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Proyecto.
>
> 2\. Revisar los datos generales y del propietario.
>
> 3\. Si se necesita invitar a alguien más, copiar el código de acceso y compartirlo.
>
> 4\. Si se es propietario y se quiere borrar el proyecto, usar el botón Eliminar proyecto.

**Resultado esperado:** El usuario tiene a la mano la información administrativa del hogar y puede compartir el acceso o eliminarlo si le corresponde.

**Observaciones:** Eliminar un proyecto es una acción permanente.

![alt text](../../assets/Web/image-22.png)

_Proyecto_

### **Información de hogar (Miembro)**

**Descripción:** Reúne la información general del hogar (nombre, tipo, dirección, descripción, fecha de creación) y los datos del propietario del hogar.

| **Campo**                      | **Detalle**                                                                                     |
| ------------------------------ | ----------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Miembro del hogar (eliminar el proyecto solo disponible para el responsable).                   |
| **Ruta de acceso**             | Detalle del proyecto \> pestaña Proyecto.                                                       |
| **Componentes de la pantalla** | Bloque de información del proyecto, bloque propietario con correo y botón para salir del hogar. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, hacer clic en la pestaña Proyecto.
>
> 2\. Revisar los datos generales y del propietario.

**Resultado esperado:** El usuario tiene a la mano la información administrativa del hogar.

**Observaciones:** Si sales del hogar no podrás ingresar a menos que el propietario te comparta nuevamente el código de acceso.

![alt text](../../assets/Web/image-23.png)

_Proyecto_

## **Perfil**

**Descripción:** Reúne los datos personales de la cuenta: nombre, correo y la opción de actualizar la contraseña.

| **Campo**                      | **Detalle**                                                              |
| ------------------------------ | ------------------------------------------------------------------------ |
| **Rol que puede acceder**      | Usuario autenticado.                                                     |
| **Ruta de acceso**             | Dashboard \> ícono de perfil, en la parte inferior del menú lateral.     |
| **Componentes de la pantalla** | Datos de la cuenta (nombre y correo) y opción para editar la contraseña. |

**Pasos para utilizarla:**

> 1\. Desde el Dashboard, hacer clic en el ícono de perfil.
>
> 2\. Revisar los datos registrados.
>
> 3\. Editar el dato que se necesite actualizar.
>
> 4\. Guardar los cambios.

**Resultado esperado:** Los datos de la cuenta quedan actualizados.

**Observaciones:** Esta pantalla puede tener ajustes de diseño en próximas versiones; el flujo general de edición se mantendrá igual.

![alt text](../../assets/Web/image-24.png)

_Perfil_

## **Configuración**

**Descripción:** Reúne las preferencias personales de la cuenta: el idioma de la interfaz y el tema visual del sistema.

| **Campo**                      | **Detalle**                                                                                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado.                                                                                  |
| **Ruta de acceso**             | Menú lateral \> Ajustes.                                                                              |
| **Componentes de la pantalla** | Selector de idioma (español, inglés, portugués o francés) y selector de tema visual (claro u oscuro). |

**Pasos para utilizarla:**

> 1\. Entrar a Ajustes desde el menú lateral.
>
> 2\. Elegir el idioma de preferencia entre las opciones disponibles.
>
> 3\. Elegir el tema visual del sistema.
>
> 4\. Confirmar; el cambio se aplica de inmediato, sin recargar la página.

**Resultado esperado:** Las preferencias quedan guardadas y se reflejan en toda la plataforma, no solo en esa pantalla.

![alt text](../../assets/Web/image-25.png)

_Configuración_

## **Cierre de sesión**

**Descripción:** Acción para salir del sistema de forma segura.

| **Campo**                      | **Detalle**                                     |
| ------------------------------ | ----------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado.                            |
| **Ruta de acceso**             | Ícono de perfil \> Cerrar sesión.               |
| **Componentes de la pantalla** | Opción Cerrar sesión dentro del menú de perfil. |

**Pasos para utilizarla:**

> 1\. Hacer clic en el ícono de perfil, en la parte inferior del menú lateral.
>
> 2\. Seleccionar la opción Cerrar sesión.

**Resultado esperado:** El sistema cierra la sesión activa y regresa a la Landing Page o a la pantalla de inicio de sesión.

![alt text](../../assets/Web/image-26.png)

_Cerrar sesión_

## **Favoritos**

**Descripción:** El módulo Favoritos permite al usuario acceder rápidamente a los proyectos que consulta con mayor frecuencia. En lugar de buscarlos dentro del Dashboard cada vez que inicia sesión, puede marcarlos como favoritos para encontrarlos desde un único lugar.

| **Campo**                      | **Detalle**                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado.                                                                                                      |
| **Ruta de acceso**             | Menú lateral \> Favoritos                                                                                                 |
| **Componentes de la pantalla** | Visualización vacía en caso de que no halla proyectos favoritos o el acceso rápido de los proyectos a través de las cards |

**Pasos para utilizarla:**

1.  Marcar los proyectos como favoritos a través del botón de corazón ubicado en la parte superior derecha de las card de proyecto.

2.  Entrar a Favoritos desde el menú lateral.

**Resultado esperado:** Poder encontrar mucho más fácil un proyecto en especifico por si tenemos demasiados proyectos vinculados o creados

![alt text](../../assets/Web/image-27.png)

> _Favoritos_

## **Notificaciones**

**Descripción:** Reúne las alertas y recomendaciones que el sistema genera a partir del consumo de los proyectos del usuario. Las notificaciones se dividen en dos tipos: alertas (avisos sobre algo que requiere atención, como un límite superado o un dispositivo desconectado) y recomendaciones (sugerencias para optimizar el consumo), y se pueden filtrar o marcar como leídas desde la misma pantalla.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol que puede acceder**      | Usuario autenticado.                                                                                                                                                                                                                                                                                                                                                                                                  |
| **Ruta de acceso**             | Menú lateral \> Notificaciones                                                                                                                                                                                                                                                                                                                                                                                        |
| **Componentes de la pantalla** | Pestañas Todas, Alertas y Recomendaciones, cada una con un contador de pendientes; enlace Marcar todas como leídas; lista de tarjetas de notificación, cada una con un ícono según su tipo, un título, una descripción breve, el proyecto y el dispositivo relacionado, la fecha y hora, una etiqueta de estado (Nueva, Activa o Resuelta) y un botón de acción (Resolver o Marcar leída) o un ícono para eliminarla. |

**Pasos para utilizarla:**

> 1\. Desde el menú lateral, hacer clic en Notificaciones. Revisar la lista completa o filtrar por Alertas o Recomendaciones según lo que se quiera consultar.
>
> 2\. Abrir una tarjeta para ver el detalle de la notificación.
>
> 3\. Marcarla como leída o resolverla, según corresponda; también se pueden marcar todas como leídas de una vez.

**Resultado esperado:** El usuario se entera a tiempo de situaciones importantes en sus proyectos (como un límite superado o un dispositivo desconectado) y puede darles seguimiento o descartarlas una vez atendidas.

![alt text](../../assets/Web/image-28.png)

_Notificaciones_

# **Mensajes del sistema**

| **Mensaje del sistema**                    | **Significado**                                              | **Acción del usuario**                                          |
| ------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------------------- |
| Campo obligatorio                          | Falta completar un dato requerido en el formulario.          | Diligenciar el campo marcado antes de continuar.                |
| Usuario o contraseña inválidos             | Las credenciales ingresadas no coinciden con ninguna cuenta. | Verificar el correo y la contraseña, o recuperar la contraseña. |
| Correo ya registrado                       | El correo ingresado ya está asociado a una cuenta existente. | Iniciar sesión o recuperar la contraseña de esa cuenta.         |
| Código de verificación inválido o expirado | El código digitado no corresponde o ya venció.               | Solicitar el reenvío del código e intentarlo de nuevo.          |
| Registro o verificación exitosa            | La cuenta fue creada o verificada correctamente.             | Continuar con el inicio de sesión.                              |
| Contraseña actualizada                     | La nueva contraseña quedó guardada correctamente.            | Iniciar sesión con la nueva contraseña.                         |

# **Preguntas frecuentes**

**¿Qué hago si olvidé mi contraseña?**

Ir a la pantalla de Inicio de sesión y hacer clic en ¿Olvidaste tu contraseña? El sistema envía por correo un código para definir una nueva.

**¿Qué pasa si no me llega el código de verificación?**

Conviene revisar la carpeta de spam o correo no deseado. Si sigue sin aparecer, se puede pedir que se reenvíe desde la misma pantalla.

**¿Puedo cambiar el idioma o el tema cuando quiera?**

Sí. Ambas cosas se ajustan desde Configuración, y se pueden cambiar las veces que haga falta.

**¿Puedo registrarme con mi cuenta de Google?**

Sí, tanto en Registro como en Inicio de sesión hay una opción para entrar directamente con Google.

**¿EnergyMonitor funciona sin internet?**

No. La plataforma necesita conexión para sincronizar los datos y mostrar la información en tiempo real.

**¿Cuándo van a estar disponibles pantallas como Favoritos o Notificaciones?**

Esas dos pantallas todavía están en desarrollo y se irán liberando en próximas versiones del sistema; este manual se actualizará cuando estén listas.

# **Glosario**

| **Término**            | **Descripción**                                                                                                                                          |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ESP32                  | Microcontrolador utilizado para capturar y transmitir las mediciones de consumo eléctrico de cada electrodoméstico.                                      |
| Sensor PZEM-004T V3    | Sensor de corriente que se instala junto al tomacorriente y mide el consumo eléctrico en tiempo real.                                                    |
| kWh                    | Kilovatio-hora, unidad utilizada para medir el consumo de energía eléctrica.                                                                             |
| Código de verificación | Código de 6 dígitos que el sistema envía por correo para confirmar la identidad del usuario, tanto en el registro como en la recuperación de contraseña. |
| Proyecto               | Hogar, apartamento u oficina que un usuario registra dentro de EnergyMonitor para monitorear su consumo eléctrico.                                       |
| Dashboard              | Panel principal que aparece después de iniciar sesión, con los proyectos registrados por el usuario.                                                     |
| Sesión                 | Periodo de tiempo en el que un usuario permanece autenticado dentro del sistema, desde que inicia sesión hasta que la cierra.                            |

# **Anexos**

Antes de entregar la versión final falta completar este apartado con evidencia real del sistema para las pantallas de Favoritos y Notificaciones, que siguen en desarrollo.

Este manual se redactó tomando como base el SRS y el Informe Técnico del proyecto EnergyMonitor, y se revisó comparando cada pantalla documentada contra el estado actual del frontend. Los módulos de Favoritos y Notificaciones no se incluyeron por completo en esta versión porque todavía están en desarrollo; cuando el equipo desarrollador los libere, se deberán documentar siguiendo la misma estructura usada en las demás pantallas (descripción, rol, ruta, componentes, pasos, resultado esperado y observaciones), conforme a la guía para elaborar manuales de software del programa ADSO.
