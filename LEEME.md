# Vilux — Perfil y Librería renovados

Parche para el proyecto ANDROID actual. No subir a vilux-web ni copiar en Vercel.
Archivos completos, sin copiar funciones sueltas. Haz copia/commit de tu app antes de aplicar.
Combina app/ de este ZIP con vilux/app/ y reemplaza los archivos incluidos, sin borrar carpetas.
No debe quedar app/app. Si tienes cambios locales adicionales, compara antes de reemplazar.

## Cambios
- Perfil sin contador visible. Conserva internamente el cooldown de 12 horas y sus preferencias actuales.
- Botón de consulta oculto durante la espera; aparece cuando se puede consultar.
- Cuando hay actualización, Actualizar/Instalar no quedan bloqueados por ese cooldown.
- No presenta como actualizado un estado sin consulta válida ni un error de conexión.
- Soporte con chat de WhatsApp y texto preparado: el usuario decide si lo envía.
- Librería con tarjetas individuales, portada si existe, Favoritos con corazón y menú de opciones.
- Fondo con degradado discreto, tarjetas grises en modo oscuro y claras en modo claro.
- Conserva LibraryFragment: no cambia la lógica de crear, borrar, renombrar ni ordenar listas.
- No cambia base de datos, migraciones, reproductor, descarga de APK ni manifest.
- No añade ecualizador/metadatos: siguen siendo las herramientas pendientes de la versión anterior.

## Soporte: un campo en Firestore
En app_updates/vilux añade:
Campo: supportWhatsapp
Tipo: string
Valor: TU número completo con código de país, preferiblemente solo dígitos.
NO se incluye un número inventado ni personal del asistente.
No uses un número con ceros de llamada internacional (00); usa directamente país+número.
También se normalizan +, espacios, guiones y paréntesis. No pegues una URL: este campo espera el número.
La aplicación construye https://wa.me/NUMERO y un mensaje con versión de Vilux.
El número será público para quienes lean este documento: usa uno destinado a soporte.
No se envía un mensaje por abrir Perfil ni al abrir el enlace. El usuario pulsa Enviar en WhatsApp.
Si el campo está vacío o no es válido, el botón de soporte permanece desactivado.
No necesitas claves ni permisos nuevos.

## La espera existente se conserva
El contacto se recibe con la siguiente consulta permitida de Firestore, no con una lectura extra.
Si acabas de agregar supportWhatsapp y faltan horas de cooldown, espera a la siguiente consulta.
No borres los datos de Vilux para forzarlo. Se conserva el archivo de preferencias anterior.
El temporizador local de Perfil solo permite que reaparezca el botón al terminar la espera;
no hace consultas remotas cada minuto y no muestra un contador.

## Probar
1. Compila en Android Studio e instala sobre la app actual, sin borrar datos.
2. Perfil: contador ausente, estado de actualización y botones correctos.
3. Comprueba que Actualizar siga disponible si hay una publicación más nueva.
4. Al llegar la siguiente consulta, prueba WhatsApp y revisa que el destinatario sea el tuyo.
5. Librería: abrir lista, opciones, crear, renombrar, eliminar y mantener pulsado para ordenar.
6. Favoritos y portadas; cambia entre modo oscuro/claro.
7. Comprueba reproducción y minirreproductor.

Se validó el XML y referencias de ViewBinding y se comprobaron reglas de visibilidad con estados simulados.
NO se compiló con Gradle ni se probó en un teléfono o con tu Firestore. Comparte el primer error si aparece.
Este parche requiere el proyecto previo ya configurado con Firebase, Material, Glide y ViewBinding.
