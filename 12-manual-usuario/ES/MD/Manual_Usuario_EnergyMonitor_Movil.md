**MANUAL DE USUARIO**

**EnergyMonitor**

_Sistema de monitoreo del consumo eléctrico del hogar_

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
<th>EnergyMonitor – Monitor de consumo eléctrico</th>
</tr>
<tr class="header">
<th><strong>Tipo de documento</strong></th>
<th>Manual de usuario</th>
</tr>
<tr class="odd">
<th><strong>Integrantes del equipo</strong></th>
<th><p>Juan Esteban Cortez Parra (Proyecto Leader / DB Manager)</p>
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
<th><strong>N.º de ficha</strong></th>
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
<th>3.0</th>
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

Registro

Verificación de cuenta

Inicio de sesión

Recuperación de contraseña

Verificación de recuperación

Panel principal

Consumo (detalle del proyecto)

Perfil

Configuración

Favoritos

Notificaciones

Cerrar sesión

Mensajes del sistema

Preguntas frecuentes

Glosario

Anexos

# **Introducción**

EnergyMonitor nació de una idea sencilla: que cualquier persona pueda saber cuánto está gastando en electricidad sin tener que esperar a que su proveedor de servicios entregue la cifra final de consumo. Detrás de la aplicación hay sensores de corriente PZEM-004T V3, instalados junto a las tomas de corriente del hogar y controlados por un microcontrolador ESP32, que envían la información que el sistema utiliza para crear paneles y gráficos de consumo casi en tiempo real.

Este documento explica cómo utilizar la plataforma tal como está disponible actualmente: crear una cuenta, verificarla, iniciar sesión, recuperar la contraseña en caso de olvidarla y navegar por el panel principal, el perfil y la configuración. El proyecto todavía está en construcción, por lo que algunas funciones, como el detalle del consumo por electrodoméstico, aún no aparecen aquí; se añadirán a este manual a medida que estén disponibles en el frontend.

# **Objetivo**

Que cualquier usuario, sin ayuda del equipo de desarrollo, pueda registrarse, iniciar sesión en el sistema y navegar con confianza por las pantallas que EnergyMonitor tiene disponibles actualmente.

# **Alcance**

Este documento cubre lo que un usuario final necesita para dar sus primeros pasos en la plataforma: registro de cuenta y verificación por correo electrónico, inicio de sesión, recuperación de contraseña, panel principal, creación y gestión de hogares, roles dentro de un hogar y sus flujos, gestión del perfil personal, configuración y cierre de sesión.

Aquí no encontrarás la arquitectura interna del sistema, el código fuente, el diseño de la base de datos ni los módulos que todavía están en construcción, como el detalle del consumo por proyecto, la vinculación de dispositivos, los umbrales de alerta o la administración de usuarios dentro de un hogar. Esas pantallas se añadirán a este manual una vez implementadas y disponibles para los usuarios finales.

# **Requisitos para usar el sistema**

Para acceder a EnergyMonitor solo necesitas un computador o un teléfono móvil con un navegador actualizado (Chrome, Firefox, Edge o Safari), una conexión estable a internet y una dirección de correo electrónico válida para crear la cuenta. El sistema no requiere instalar nada adicional: todo funciona directamente desde el navegador.

El hardware de medición (los sensores PZEM-004T V3 y el ESP32) se instala y configura por separado y no forma parte de este manual; aquí solo se explica el uso de la plataforma web.

## **Registro**

**Descripción: Formulario en el que una persona crea su cuenta en EnergyMonitor con nombre, correo electrónico y contraseña, o directamente con su cuenta de Google.**

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
<th><strong>Rol con acceso</strong></th>
<th><p>General public (without an</p>
<p>account created).</p></th>
</tr>
<tr class="header">
<th><strong>Ruta de acceso</strong></th>
<th>Página de inicio &gt; botón Registrarse.</th>
</tr>
<tr class="odd">
<th><strong>Componentes de la pantalla</strong></th>
<th>Campos Nombre, Correo electrónico y Contraseña; casilla para aceptar los términos de uso y la política de privacidad; botón Registrarse; opción Registrarse con Google; enlace ¿Ya tienes una cuenta? Inicia sesión.</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

**Pasos para utilizarla:**

> 1\. Desde la página de inicio, haz clic en Registrarse.
>
> 2\. Completa los campos Nombre, Correo electrónico y Contraseña, o elige la opción de Google.
>
> 3\. Acepta los términos de uso y la política de privacidad.
>
> 4\. Confirma con el botón Registrarse.

**Resultado esperado: El sistema crea la cuenta y te redirige inmediatamente a la pantalla de verificación del correo electrónico.**

**Notas: Si el correo electrónico ya está registrado, el sistema lo indica y sugiere iniciar sesión.**

![alt text](../../assets/Mobile/image.png)

_Registro_

## **Verificación de cuenta**

**Descripción: Pantalla que confirma que el correo electrónico utilizado durante el registro es válido. El sistema envía un código de 6 dígitos que debe introducirse aquí para activar la cuenta.**

| **Campo**                      | **Detalle**                                                                                                      |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Usuario recién registrado, cuya cuenta aún no ha sido verificada.                                                |
| **Ruta de acceso**             | Se abre automáticamente al completar el registro.                                                                |
| **Componentes de la pantalla** | Campo para el código de 6 dígitos; enlace ¿No recibiste el código?; enlace ¿Ya tienes una cuenta? Inicia sesión. |

**Pasos para utilizarla:**

> 1\. Revisa la bandeja de entrada del correo electrónico utilizado para registrar la cuenta.
>
> 2\. Copia el código de 6 dígitos recibido.
>
> 3\. Introdúcelo en el campo de verificación.
>
> 4\. Confirma para activar la cuenta.

**Resultado esperado: La cuenta queda verificada y el usuario ya puede iniciar sesión normalmente.**

**Notas: Si el código no llega, puedes solicitar que se reenvíe desde el enlace ¿No recibiste el código?**

![alt text](../../assets/Mobile/image-1.png)

_Verificación de cuenta_

## **Inicio de sesión**

**Descripción: Formulario para iniciar sesión con una cuenta que ya ha sido creada y verificada.**

| **Campo**                      | **Detalle**                                                                                                                                                                                     |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Público general con una cuenta creada y verificada.                                                                                                                                             |
| **Ruta de acceso**             | Página de inicio \> botón Iniciar sesión.                                                                                                                                                       |
| **Componentes de la pantalla** | Campos Correo electrónico y Contraseña; casilla Recuérdame; enlace ¿Olvidaste tu contraseña?; botón Iniciar sesión; opción Iniciar sesión con Google; enlace ¿No tienes una cuenta? Regístrate. |

**Pasos para utilizarla:**

> 1\. Abre la URL de EnergyMonitor.
>
> 2\. Haz clic en Iniciar sesión.
>
> 3\. Introduce el correo electrónico y la contraseña registrados.
>
> 4\. Confirma con el botón Iniciar sesión.

**Resultado esperado: El sistema valida las credenciales y te lleva directamente al panel principal.**

**Notas: Si el correo electrónico o la contraseña no coinciden, aparece un mensaje de error y no puedes continuar.**

![alt text](../../assets/Mobile/image-3.png)

_Inicio de sesión_

## **Recuperación de contraseña**

**Descripción: Punto de entrada para recuperar el acceso cuando se olvida la contraseña. En esta pantalla solo se solicita el correo electrónico utilizado para crear la cuenta.**

| **Campo**                      | **Detalle**                                                                                                  |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| **Rol con acceso**             | Público general (con una cuenta ya creada).                                                                  |
| **Ruta de acceso**             | Inicio de sesión \> ¿Olvidaste tu contraseña? link.                                                          |
| **Componentes de la pantalla** | Campo de correo electrónico; botón para enviar el código de recuperación; enlace Volver al inicio de sesión. |

**Pasos para utilizarla:**

> 1\. En la pantalla de inicio de sesión, haz clic en ¿Olvidaste tu contraseña?
>
> 2\. Introduce el correo electrónico utilizado para crear la cuenta.
>
> 3\. Confirma el envío.

**Resultado esperado: El sistema envía un código de verificación de 6 dígitos al correo electrónico para continuar con la recuperación.**

**Notas: Si el correo electrónico introducido no está registrado en el sistema, comprueba que esté escrito correctamente antes de intentarlo de nuevo.**

![alt text](../../assets/Mobile/image-4.png)

_Recuperación de contraseña_

## **Verificación de recuperación**

**Descripción: Paso en el que se introduce el código enviado por correo electrónico para confirmar que el titular real de la cuenta está realizando la recuperación y se define la nueva contraseña.**

| **Campo**                      | **Detalle**                                                                                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Usuario en proceso de recuperación de contraseña.                                                     |
| **Ruta de acceso**             | Opens automatically after requesting contraseña recovery.                                             |
| **Componentes de la pantalla** | Campo para el código de verificación de 6 dígitos; campos para la nueva contraseña y su confirmación. |

**Pasos para utilizarla:**

> 1\. Revisa el correo electrónico y copia el código recibido.
>
> 2\. Introdúcelo en la pantalla de verificación.
>
> 3\. Establece la nueva contraseña y confírmala.
>
> 4\. Guarda los cambios.

**Resultado esperado: La contraseña se actualiza y el sistema vuelve a la pantalla de inicio de sesión.**

**Notas: Por seguridad, el código tiene un tiempo límite de uso; si caduca, debes solicitar uno nuevo desde Recuperación de contraseña.**

![alt text](../../assets/Mobile/image-5.png)

_Verificar correo electrónico_

## **Panel principal**

**Descripción: Es el panel principal, lo primero que aparece después de iniciar sesión. Muestra los proyectos (hogares u oficinas) que el usuario ha registrado o una invitación para crear el primero si aún no existe ninguno.**

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Rol con acceso**             | Usuario autenticado.                                                                                                                                                                                                                                                                                                                                                                                               |
| **Ruta de acceso**             | Se abre automáticamente después de iniciar sesión; también está disponible desde el icono Inicio del menú lateral.                                                                                                                                                                                                                                                                                                 |
| **Componentes de la pantalla** | Menú lateral con el icono Inicio y acceso a Configuración; icono de perfil en la parte inferior del menú; en el área central, tarjetas de los proyectos registrados o, si aún no hay ninguno, un mensaje guía con el botón Crear proyecto. El menú lateral incluye otras opciones (como Favoritos y Notificaciones) que corresponden a módulos todavía en construcción y se documentarán cuando estén disponibles. |

**Pasos para utilizarla:**

> 1\. Inicia sesión en el sistema.
>
> 2\. El panel principal aparece automáticamente, sin necesidad de buscarlo en el menú.
>
> 3\. Si es la primera vez, utiliza el botón Crear proyecto para registrar el primer hogar u oficina.
>
> 4\. Si ya existen proyectos, haz clic en una de sus tarjetas para acceder al detalle (pestañas Consumo, Historial, Usuarios, Dispositivos, Umbrales y Proyecto).

**Resultado esperado: El usuario puede ver de un vistazo el estado de sus proyectos y crear uno nuevo cuando lo necesite.**

**Notas: El detalle de cada proyecto se documenta más adelante en la sección Consumo (detalle del proyecto) de este manual.**

![alt text](../../assets/Mobile/image-6.png)

_Panel principal sin proyectos registrados_

![alt text](../../assets/Mobile/image-7.png)

_Panel principal con proyectos registrados_

### **Formulario para registrar hogares**

**Descripción: Este formulario permite que cualquier usuario autenticado cree un hogar para administrarlo.**

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                          |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Usuario autenticado.                                                                                                                                                                                                                                                 |
| **Ruta de acceso**             | En el panel principal hay dos formas de acceder: la primera aparece en el centro al iniciar sesión y llegar al panel; la segunda está en la parte superior derecha, en el botón con el signo +.                                                                      |
| **Componentes de la pantalla** | En el formulario para crear un hogar hay cuatro campos para introducir el nombre, el tipo de hogar, la dirección y una descripción opcional. También hay tres botones: uno para cancelar el proceso, otro para registrar el hogar y una X para cerrar el formulario. |

**Pasos para utilizarla:**

> 1\. El panel principal aparece automáticamente, sin necesidad de buscarlo en el menú
>
> 2\. Si es la primera vez, utiliza el botón Crear proyecto para registrar el primer hogar.
>
> 3\. Si ya existen proyectos, haz clic en el botón con el signo + situado en la parte superior derecha.
>
> 4\. Completa el formulario para registrar un hogar.

**Resultado esperado: El usuario verá una tarjeta con el hogar registrado.**

**Notas: Un hogar solo se puede administrar desde su interior.**

![alt text](../../assets/Mobile/image-8.png)

_Formulario para registrar hogares_

### **Formulario para unirse a un hogarW**

**Descripción: Este formulario permite que cualquier persona registrada que tenga el código de acceso de un hogar pueda acceder a él.**

| **Field**             | **Detail**                                                                                                                                                                                                                        |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                                                                                                                                               |
| **Access path**       | On the dashboard we can find 2 access paths: the first can be found when we authenticate within the page and arrive at the dashboard, in the center, and the second can be found at the top right, on the button with the + sign. |
| **Screen components** | Within the form to join a home you can find a field where you can enter the access code, in addition to 3 buttons: one to cancel the process, register home, and an X to exit the form.                                           |

**Pasos para utilizarla:**

> 1\. El panel principal aparece automáticamente, sin necesidad de buscarlo en el menú
>
> 2\. Si es la primera vez, utiliza el botón para unirte a un hogar.
>
> 3\. Si ya existen hogares, haz clic en el botón con el signo + situado en la parte superior derecha.
>
> 4\. Completa el formulario para acceder a un hogar.

**Resultado esperado: El usuario verá una tarjeta con el hogar al que se ha unido.**

**Notas: Dentro del hogar, el miembro solo puede consultar la información del hogar.**

![alt text](../../assets/Mobile/image-9.png)

_Formulario para registrar hogares_

### **Consumo**

**Descripción: Es la pestaña que se abre por defecto al entrar en un proyecto. Muestra el estado del consumo eléctrico casi en tiempo real: potencia actual, consumo del día, dispositivos conectados, curva de consumo de las últimas 24 horas, distribución por dispositivo y progreso respecto a los límites diarios y mensuales.**

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Rol con acceso**             | Usuario autenticado, cualquier miembro del proyecto.                                                                                                                                                                                                                                                               |
| **Ruta de acceso**             | Panel principal \> proyecto card \> Consumo tab.                                                                                                                                                                                                                                                                   |
| **Componentes de la pantalla** | Tarjetas de indicadores para la potencia actual, el consumo del día y los dispositivos activos; gráfico de área del consumo de las últimas 24 horas; gráfico circular con la distribución actual del consumo; gráfico de barras del consumo por dispositivo; barras de progreso para los límites diario y mensual. |

**Pasos para utilizarla:**

> 1\. Ve al panel principal y haz clic en la tarjeta del proyecto que quieres consultar.
>
> 2\. El sistema abre el detalle del proyecto en la pestaña Consumo.
>
> 3\. Revisa las tarjetas de indicadores para ver el estado general.
>
> 4\. Desplázate hacia abajo para consultar los gráficos de consumo por hora y por dispositivo, así como el progreso de los límites configurados.

**Resultado esperado: El usuario comprende cómo va el consumo eléctrico del proyecto en el momento de la consulta.**

**Notas: Los datos mostrados dependen de la información enviada por los dispositivos ESP32 vinculados; si un dispositivo está desconectado, su aporte no se refleja en tiempo real.**

![alt text](../../assets/Mobile/image-10.png)

_Consumo_

### **Historial**

**Descripción: Almacena el consumo del proyecto a lo largo del tiempo, no solo lo ocurrido hoy. Se puede filtrar por día, semana, mes o año y, en la vista mensual, consultar semana por semana.**

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                                         |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Usuario autenticado, cualquier miembro del proyecto.                                                                                                                                                                                                                                                                |
| **Ruta de acceso**             | Proyecto detail \> Historial tab.                                                                                                                                                                                                                                                                                   |
| **Componentes de la pantalla** | Selector de período (Día, Semana, Mes, Año); selector de semana dentro del mes; gráfico de barras con la distribución del consumo por dispositivo durante el período; tabla de clasificación del consumo por dispositivo; tarjetas con el consumo total del período, el promedio y el dispositivo de mayor consumo. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Historial.
>
> 2\. Elige el período que quieres consultar (día, semana, mes o año).
>
> 3\. Si elegiste Mes, selecciona la semana específica o la opción Mostrar todo.
>
> 4\. Revisa el gráfico y la tabla de clasificación para identificar qué dispositivo consumió más.

**Resultado esperado: El usuario puede comparar el consumo entre distintos períodos e identificar qué electrodoméstico tiene mayor impacto en la factura.**

![alt text](../../assets/Mobile/image-11.png)

_Historial_

### **Usuarios (Propietario)**

**Descripción: Muestra quién tiene acceso al proyecto y permite gestionar los miembros del hogar.**

| **Campo**                      | **Detalle**                                                        |
| ------------------------------ | ------------------------------------------------------------------ |
| **Rol con acceso**             | Propietario del hogar (solo el propietario puede invitar).         |
| **Ruta de acceso**             | Proyecto detail \> Usuarios tab.                                   |
| **Componentes de la pantalla** | Lista de miembros del proyecto con su rol (Propietario o Miembro). |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Usuarios.
>
> 2\. Revisa la lista de miembros actuales.
>
> 3\. Si tienes el rol de propietario, puedes compartir el código de acceso para permitir que se unan otros miembros.

**Resultado esperado: Consultar la lista de miembros actuales.**

**Notas: Solo el propietario del proyecto puede eliminar miembros.**

![alt text](../../assets/Mobile/image-12.png)

_Usuarios_

### **Usuarios (Miembro)**

**Descripción: Muestra quién tiene acceso al proyecto.**

| **Campo**                      | **Detalle**                                                        |
| ------------------------------ | ------------------------------------------------------------------ |
| **Rol con acceso**             | Miembro (solo el propietario puede invitar).                       |
| **Ruta de acceso**             | Proyecto detail \> Usuarios tab.                                   |
| **Componentes de la pantalla** | Lista de miembros del proyecto con su rol (Propietario o Miembro). |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Usuarios.
>
> 2\. Revisa la lista de miembros actuales.

**Resultado esperado: Consultar la lista de miembros actuales.**

**Notas: Solo el propietario del proyecto puede eliminar miembros.**

![alt text](../../assets/Mobile/image-13.png)

_Usuarios_

###

###

### **Dispositivos (Propietario)**

**Descripción: Muestra los dispositivos ESP32 vinculados al proyecto, su estado de conexión y el consumo que reportan; además, permite vincular más dispositivos al hogar.**

| **Campo**                      | **Detalle**                                                                                                                                                      |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Propietario del hogar.                                                                                                                                           |
| **Ruta de acceso**             | Proyecto detail \> Dispositivos tab.                                                                                                                             |
| **Componentes de la pantalla** | Botón Vincular dispositivo; lista de dispositivos con nombre, ubicación, estado (En línea / Desconectado) y consumo actual; opción para eliminar un dispositivo. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Dispositivos.
>
> 2\. Revisa el estado de cada dispositivo vinculado.
>
> 3\. Para añadir uno nuevo, utiliza el botón Vincular dispositivo.
>
> 4\. Para eliminar uno, utiliza el icono de eliminación situado junto al dispositivo.

**Resultado esperado: El usuario sabrá cuántos dispositivos están activos y podrá vincularlos o eliminarlos según sea necesario.**

**Notas: Un dispositivo aparece como Desconectado cuando deja de enviar datos durante un período prolongado.**

![alt text](../../assets/Mobile/image-14.png)

_Dispositivos_

### **Dispositivos (Miembro)**

**Descripción: Muestra los dispositivos ESP32 vinculados al proyecto, su estado de conexión y el consumo que reportan.**

| **Campo**                      | **Detalle**                                                                                                                 |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Miembro del hogar.                                                                                                          |
| **Ruta de acceso**             | Proyecto detail \> Dispositivos tab.                                                                                        |
| **Componentes de la pantalla** | Botón Vincular dispositivo; lista de dispositivos con nombre, ubicación, estado (En línea / Desconectado) y consumo actual. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Dispositivos.
>
> 2\. Revisa el estado de cada dispositivo vinculado.

**Resultado esperado: El usuario sabrá cuántos dispositivos están activos.**

**Notas: Un dispositivo aparece como Desconectado cuando deja de enviar datos durante un período prolongado.**

![alt text](../../assets/Mobile/image-15.png)

_Dispositivos_

### **Umbrales (Propietario)**

**Descripción: Define los límites diarios y mensuales de consumo del proyecto, que se utilizan para generar alertas y recomendaciones.**

| **Field**             | **Detail**                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Home owner                                                                                                |
| **Access path**       | Project detail \> Thresholds tab.                                                                         |
| **Screen components** | Switch to use the system's default thresholds; Daily limit and Monthly limit fields; Save changes button. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Umbrales.
>
> 2\. Decide si quieres utilizar los umbrales predeterminados o personalizarlos.
>
> 3\. Si deseas personalizarlos, desactiva el interruptor e introduce los nuevos límites diarios y mensuales.
>
> 4\. Confirm with Guardar cambios.

**Resultado esperado: Los nuevos límites se guardan y el sistema los utiliza para generar alertas cuando el consumo se aproxima a ellos o los supera.**

**Notas: Los umbrales se guardan por proyecto; cambiarlos en uno no afecta a los demás proyectos del usuario.**

![alt text](../../assets/Mobile/image-16.png)

_Umbrales_

### **Umbrales (Miembro)**

**Descripción: Permite consultar los límites diarios y mensuales de consumo registrados para el hogar, utilizados para generar alertas y recomendaciones.**

| **Campo**                      | **Detalle**                            |
| ------------------------------ | -------------------------------------- |
| **Rol con acceso**             | Miembro del hogar                      |
| **Ruta de acceso**             | Proyecto detail \> Umbrales tab.       |
| **Componentes de la pantalla** | Campos Límite diario y Límite mensual. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Umbrales.
>
> 2\. Decide si quieres utilizar los umbrales predeterminados o personalizarlos.

**Resultado esperado: Consultar los límites diarios y mensuales de consumo registrados para el hogar.**

**Notas: Los umbrales se definen por hogar.**

![alt text](../../assets/Mobile/image-17.png)

_Umbrales_

### **Hogar information (Propietario)**

**Descripción: Reúne la información general del hogar (nombre, tipo, dirección, descripción y fecha de creación), los datos de su propietario y el código de acceso para compartir el hogar con otras personas.**

| **Campo**                      | **Detalle**                                                                                                                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Propietario del hogar (solo el responsable puede eliminar el proyecto).                                                                                       |
| **Ruta de acceso**             | Proyecto detail \> Proyecto tab.                                                                                                                              |
| **Componentes de la pantalla** | Bloque con la información del proyecto; bloque del propietario con su correo electrónico; código de acceso para compartir e invitar; botón Eliminar proyecto. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Proyecto.
>
> 2\. Revisa los datos generales y los del propietario.
>
> 3\. Si necesitas invitar a otra persona, copia y comparte el código de acceso.
>
> 4\. Si eres el propietario y quieres eliminar el proyecto, utiliza el botón Eliminar proyecto.

**Resultado esperado: El usuario tendrá a mano la información administrativa del hogar y podrá compartir el acceso o eliminarlo, si corresponde.**

**Notas: Eliminar un proyecto es una acción permanente.**

![alt text](../../assets/Mobile/image-18.png)

_Proyecto_

### **Hogar information (Miembro)**

**Descripción: Reúne la información general del hogar (nombre, tipo, dirección, descripción y fecha de creación) y los datos de su propietario.**

| **Campo**                      | **Detalle**                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Miembro del hogar (solo el responsable puede eliminar el proyecto).                                                       |
| **Ruta de acceso**             | Proyecto detail \> Proyecto tab.                                                                                          |
| **Componentes de la pantalla** | Bloque con la información del proyecto, bloque del propietario con su correo electrónico y botón para abandonar el hogar. |

**Pasos para utilizarla:**

> 1\. Desde el detalle del proyecto, haz clic en la pestaña Proyecto.
>
> 2\. Revisa los datos generales y los del propietario.

**Resultado esperado: El usuario tendrá a mano la información administrativa del hogar.**

**Notas: Si abandonas el hogar, no podrás volver a entrar a menos que el propietario vuelva a compartir contigo el código de acceso.**

![alt text](../../assets/Mobile/image-19.png)

_Proyecto_

## **Perfil**

**Descripción: Reúne los datos personales de la cuenta: nombre, correo electrónico y opción para actualizar la contraseña.**

| **Campo**                      | **Detalle**                                                                          |
| ------------------------------ | ------------------------------------------------------------------------------------ |
| **Rol con acceso**             | Usuario autenticado.                                                                 |
| **Ruta de acceso**             | Panel principal \> icono del perfil, en la parte inferior del menú lateral.          |
| **Componentes de la pantalla** | Datos de la cuenta (nombre y correo electrónico) y opción para editar la contraseña. |

**Pasos para utilizarla:**

> 1\. Desde el panel principal, haz clic en el icono del perfil.
>
> 2\. Revisa los datos registrados.
>
> 3\. Edita los datos que necesiten actualizarse.
>
> 4\. Guarda los cambios.

**Resultado esperado: Los datos de la cuenta se actualizan.**

**Notas: Esta pantalla puede tener ajustes de diseño en próximas versiones; el flujo general de edición seguirá siendo el mismo.**

![alt text](../../assets/Mobile/image-20.png)

_Perfil_

## **Configuración**

**Descripción: Reúne las preferencias personales de la cuenta: el idioma de la interfaz y el tema visual del sistema.**

| **Campo**                      | **Detalle**                                                                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------ |
| **Rol con acceso**             | Usuario autenticado.                                                                                   |
| **Ruta de acceso**             | Menú lateral \> Configuración.                                                                         |
| **Componentes de la pantalla** | Selector de idioma (español, inglés, portugués o francés) y selector del tema visual (claro u oscuro). |

**Pasos para utilizarla:**

> 1\. Entra en Configuración desde el menú lateral.
>
> 2\. Elige el idioma que prefieras entre las opciones disponibles.
>
> 3\. Elige el tema visual del sistema.
>
> 4\. Confirma; el cambio se aplica inmediatamente, sin recargar la página.

**Resultado esperado: Las preferencias se guardan y se aplican en toda la plataforma, no solo en esa pantalla.**

![alt text](../../assets/Mobile/image-21.png)

_Configuración_

## **Cerrar sesión**

**Descripción: Acción para salir del sistema de forma segura.**

| **Campo**                      | **Detalle**                                      |
| ------------------------------ | ------------------------------------------------ |
| **Rol con acceso**             | Usuario autenticado.                             |
| **Ruta de acceso**             | Perfil icon \> Cerrar sesión.                    |
| **Componentes de la pantalla** | Opción Cerrar sesión dentro del menú del perfil. |

**Pasos para utilizarla:**

> 1\. Haz clic en el icono del perfil, en la parte inferior del menú lateral.
>
> 2\. Selecciona la opción Cerrar sesión.

**Resultado esperado: El sistema cierra la sesión activa y vuelve a la página de inicio o a la pantalla de inicio de sesión.**

![alt text](../../assets/Mobile/image-22.png)

_Cerrar sesión_

## **Favoritos**

**Descripción:** El módulo Favoritos permite al usuario acceder rápidamente a los proyectos que consulta con mayor frecuencia. En lugar de buscarlos en el panel principal cada vez que inicia sesión, puede marcarlos como favoritos para encontrarlos en un solo lugar.

| **Campo**                      | **Detalle**                                                                                       |
| ------------------------------ | ------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Usuario autenticado.                                                                              |
| **Ruta de acceso**             | Menú lateral \> Favoritos                                                                         |
| **Componentes de la pantalla** | Vista vacía si no hay proyectos favoritos, o acceso rápido a los proyectos mediante sus tarjetas. |

**Pasos para utilizarla:**

1.  Marca los proyectos como favoritos mediante el botón con forma de corazón situado en la parte superior derecha de sus tarjetas.

2.  Entra en Favoritos desde el menú lateral.

**Resultado esperado: Poder encontrar un proyecto específico con mayor facilidad si hay muchos proyectos vinculados o creados.**

![alt text](../../assets/Mobile/image-23.png)

_Favoritos_

## **Notificaciones**

**Descripción:** Reúne las alertas y recomendaciones que el sistema genera a partir del consumo de los proyectos del usuario. Las notificaciones se dividen en dos tipos: alertas (avisos sobre situaciones que requieren atención, como un límite superado o un dispositivo desconectado) y recomendaciones (sugerencias para optimizar el consumo). Se pueden filtrar o marcar como leídas desde la misma pantalla.

| **Campo**                      | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol con acceso**             | Usuario autenticado.                                                                                                                                                                                                                                                                                                                                                                                    |
| **Ruta de acceso**             | Menú lateral \> Notificaciones                                                                                                                                                                                                                                                                                                                                                                          |
| **Componentes de la pantalla** | Pestañas Todas, Alertas y Recomendaciones, cada una con un contador de pendientes; enlace Marcar todas como leídas; lista de tarjetas de notificación, cada una con un icono según su tipo, título, breve descripción, proyecto y dispositivo relacionados, fecha y hora, etiqueta de estado (Nueva, Activa o Resuelta) y un botón de acción (Resolver o Marcar como leída) o un icono para eliminarla. |

**Pasos para utilizarla:**

> 1\. Desde el menú lateral, haz clic en Notificaciones. Revisa la lista completa o filtra por Alertas o Recomendaciones según lo que quieras consultar.
>
> 2\. Abre una tarjeta para ver los detalles de la notificación.
>
> 3\. Márcala como leída o resuélvela, según corresponda; también puedes marcar todas como leídas a la vez.

**Resultado esperado: El usuario recibe información oportuna sobre situaciones importantes en sus proyectos (como un límite superado o un dispositivo desconectado) y puede hacerles seguimiento o descartarlas una vez atendidas.**

![alt text](../../assets/Mobile/image-24.png)

_Notificaciones_

#

#

#

#

#

#

#

# **Mensajes del sistema**

| **System message**                        | **Significado**                                                            | **Acción del usuario**                                                     |
| ----------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Campo obligatorio                         | Falta un dato obligatorio en el formulario.                                | Completa el campo marcado antes de continuar.                              |
| Invalid username or contraseña            | Las credenciales introducidas no coinciden con ninguna cuenta.             | Comprueba el correo electrónico y la contraseña, o recupera la contraseña. |
| Correo electrónico ya registrado          | El correo electrónico introducido ya está asociado a una cuenta existente. | Inicia sesión o recupera la contraseña de esa cuenta.                      |
| Invalid or expired código de verificación | El código introducido no coincide o ha caducado.                           | Solicita que se reenvíe el código e inténtalo de nuevo.                    |
| Successful registration or verification   | La cuenta se creó o verificó correctamente.                                | Continúa con el inicio de sesión.                                          |
| Password updated                          | La nueva contraseña se guardó correctamente.                               | Inicia sesión con la nueva contraseña.                                     |

# **Preguntas frecuentes**

**¿Qué hago si olvidé mi contraseña?**

Ve a la pantalla de inicio de sesión y haz clic en ¿Olvidaste tu contraseña? El sistema envía un código por correo electrónico para establecer una nueva contraseña.

**¿Qué ocurre si no llega el código de verificación?**

Revisa la carpeta de correo no deseado o spam. Si aún no aparece, puedes solicitar que se reenvíe desde la misma pantalla.

**¿Puedo cambiar el idioma o el tema cuando quiera?**

Sí. Ambos se pueden ajustar desde Configuración y cambiar tantas veces como sea necesario.

**¿Puedo registrarme con mi cuenta de Google?**

Sí. Tanto en Registro como en Inicio de sesión hay una opción para acceder directamente con Google.

**¿EnergyMonitor funciona sin internet?**

No. La plataforma necesita conexión para sincronizar los datos y mostrar la información en tiempo real.

**¿Cuándo estarán disponibles pantallas como Favoritos o Notificaciones?**

Esas dos pantallas siguen en desarrollo y estarán disponibles en próximas versiones del sistema; este manual se actualizará cuando estén listas.

# **Glosario**

| **Término**         | **Descripción**                                                                                                                                                           |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ESP32               | Microcontrolador utilizado para capturar y transmitir las mediciones de consumo eléctrico de cada electrodoméstico.                                                       |
| PZEM-004T V3 sensor | Sensor de corriente instalado junto a la toma de corriente que mide el consumo eléctrico en tiempo real.                                                                  |
| kWh                 | Kilowatt-hour, unit used to measure electrical energy consumo.                                                                                                            |
| Verification code   | Código de 6 dígitos que el sistema envía por correo electrónico para confirmar la identidad del usuario, tanto durante el registro como en la recuperación de contraseña. |
| Proyecto            | Hogar, apartment, or office that a user registers in EnergyMonitor to monitor its electricity consumo.                                                                    |
| Panel principal     | Panel principal que aparece después de iniciar sesión y muestra los proyectos registrados por el usuario.                                                                 |
| Session             | Período durante el cual un usuario permanece autenticado en el sistema, desde que inicia sesión hasta que la cierra.                                                      |

# **Anexos**

Antes de entregar la versión final, esta sección debe completarse con evidencias reales del sistema para las pantallas Favoritos y Notificaciones, que aún están en desarrollo.

Este manual se elaboró con base en la Especificación de Requisitos de Software (SRS) y el Informe Técnico del proyecto EnergyMonitor, y se revisó comparando cada pantalla documentada con el estado actual del frontend. Los módulos Favoritos y Notificaciones no se incluyeron por completo en esta versión porque aún están en desarrollo; cuando el equipo de desarrollo los publique, deberán documentarse siguiendo la misma estructura utilizada en las demás pantallas (descripción, rol, ruta, componentes, pasos, resultado esperado y notas), de acuerdo con la guía del programa ADSO para la elaboración de manuales de software.
