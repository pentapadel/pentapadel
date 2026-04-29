# INSTRUCCIONES PARA CLAUDE CODE

Reescribí el index.html COMPLETO desde cero con un diseño nuevo y moderno.
Tenés libertad total de diseño — hacelo lo más copado posible, pero que sea funcional.

## STACK
- HTML + CSS + JS vanilla en un solo archivo index.html
- Firebase Realtime Database config: apiKey "AIzaSyACkd5Tv1lcr5WEWXGr0vCzk_rr81K7s9c", databaseURL "https://penta-padel-default-rtdb.firebaseio.com" usando scripts compat NO módulos ES6
- Sin frameworks

## DISEÑO — LIBERTAD TOTAL
- Podés elegir el layout, tipografías, animaciones, efectos que quieras
- Tiene que verse profesional, moderno y copado
- Tiene que funcionar perfecto en celular (mobile-first) Y en desktop
- Colores base del club: navy #0a1628 y lima #b5e52a — podés complementar con lo que quieras
- Fuente sugerida: Barlow Condensed o cualquier otra que quede bien
- PWA: apple-touch-icon + manifest + theme-color para que se pueda instalar en el celu
- Contraseña de acceso: penta2025 en sessionStorage
- Login screen con logo del club

## NAVEGACIÓN MOBILE
- Nav inferior fijo con íconos grandes para mobile
- En desktop podés usar sidebar o nav superior
- Transiciones suaves entre secciones

## PANTALLA DE INICIO — EN LUGAR DEL DASHBOARD
- Reemplazar el dashboard aburrido de stats por algo más copado
- Ideas: pantalla de bienvenida con el logo grande, último torneo jugado, próximo torneo, accesos rápidos a las funciones más usadas, animación de entrada, frase motivacional de pádel, etc.
- Que tenga personalidad y se vea como una app real

## SECCIONES
1. Inicio (reemplaza al dashboard, hacelo copado)
2. Jugadores
3. Torneos
4. Ranking

## JUGADORES
- Campos: nombre, celular, catNum (3 a 9), catVar (+/base/-), genero (C/D)
- Anti-duplicado por nombre Y por celular al crear y editar
- Botón WhatsApp verde que abre wa.me/54{numero}
- Celular solo visible en sección Jugadores
- Listado agrupado por categoría, colapsable

## TORNEOS CREACIÓN
- Campos: nombre, tipo (por categoría o por suma), fecha
- Selección de parejas con buscador
- Al crear: partidos vacío [], presentes vacío []
- Solapas activos / finalizados

## CHECK-IN Y FIXTURE DINÁMICO — REGLA CRÍTICA
- Array "presentes" guarda el orden de llegada
- Cuando llegan 2 parejas (posición par): crear partido entre ellas inmediatamente
  - presentes[0] y presentes[1] llegan → partido entre ellos
  - presentes[2] y presentes[3] llegan → partido entre ellos
- Cuando llegan TODOS: completar fixture para que cada pareja juegue exactamente 2 partidos
  - Respetar partidos ya creados, completar faltantes evitando repetir rivales
- Mostrar número de llegada #1, #2, etc. en cada botón

## ESTADÍSTICAS — REGLA MUY IMPORTANTE
- NUNCA acumular pts/gf/gc/wins/played en el objeto pareja
- Calcular SIEMPRE on-the-fly filtrando partidos
- Tabla de posiciones: solo partidos donde fase === 'Clasificación'
- Ranking global: puntos por fase eliminatoria

## BRACKET ELIMINATORIO — LEY UNIVERSAL 7 A 16 PAREJAS
- Siempre terminar con exactamente 8 equipos en Cuartos de final
- Partidos de Octavos = N - 8
- Equipos con bye directo a Cuartos = 16 - N
- N=8: directo a Cuartos
- N=9: 1 octavo, 7 byes
- N=10: 2 octavos, 6 byes
- N=12: 4 octavos, 4 byes
- N=16: 8 octavos, 0 byes

## CRUCES OCTAVOS — ANTI-REPETICIÓN OBLIGATORIA
- byeTeams = sorted[0] a sorted[nByes-1]
- playInTeams = sorted[nByes] en adelante, dividido en topHalf y bottomHalf
- Generar todas las permutaciones de bottomHalf
- Elegir la que tenga CERO cruces repetidos de clasificación
- Si no existe con cero, elegir la de menos repeticiones
- yaJugaron(a,b) verifica SOLO en partidos con fase === 'Clasificación'

## PROPAGACIÓN DE GANADORES
- Cada partido tiene nextPid y nextSlot
- Al guardar resultado: propagateWinner actualiza el slot del siguiente partido automáticamente

## PUNTOS RANKING
- Campeón: 100 puntos
- Finalista: 50 puntos
- Semifinalista: 25 puntos
- Cuartos perdedor: 15 puntos
- Octavos perdedor: 5 puntos
- +1 punto por torneo jugado

## VISTA TV — FULLSCREEN OVERLAY
- Activar con botón "EN VIVO" desde cualquier torneo activo
- Diseño espectacular para pantalla grande de TV
- Mostrar todo en una sola pantalla sin scroll si es posible
- Tabs: Clasificación y Eliminatorias
- En Clasificación:
  - Tabla de posiciones con check-in clickeable (botón verde/gris)
  - Lista de partidos por turno con indicadores de presencia
  - Botón X para cancelar resultado en partidos jugados
- En Eliminatorias: bracket visual completo con inputs para cargar resultados
- Auto-escala con transform scale

## FUNCIONES ADICIONALES
- Backup JSON descargable y Restore desde archivo
- Corregir resultado: botón en partidos jugados, revierte y aplica nuevas stats
- Cancelar resultado desde TV
- Finalizar torneo otorga puntos de ranking
- Eliminar torneo con confirmación modal
- Resetear ranking con confirmación

## AL TERMINAR
Ejecutar: git add . && git commit -m "Rewrite completo - nuevo diseño" && git push
