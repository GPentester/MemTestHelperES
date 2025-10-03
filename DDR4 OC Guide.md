# Tabla de Contenido
- [Tabla de Contenido](#tabla-de-contenido)
- [Setup](#setup)
  - [Software para Testear Memoria RAM](#software-para-testear-memoria-ram)
    - [Evitar](#evitar)
    - [Recomendaciones](#recomendaciones)
    - [Alternativas](#alternativas)
    - [Comparaciones](#comparaciones)
  - [Software para Visualizar Timings](#software-para-visualizar-timings)
  - [Benchmarks](#benchmarks)
- [Información General Acerca de la RAM](#información-general-acerca-de-la-ram)
  - [Relación entre Frecuencia y Timings](#relación-entre-frecuencia-y-timings)
  - [Timings Primarios, Secundarios y Terciarios](#timings-primarios-secundarios-y-terciarios)
- [Expectativas/Limitaciones](#expectativaslimitaciones)
  - [Placa Madre/Motherboard](#placamadremotherboard)
  - [Circuitos Integrados (ICs)](#circuitos-integrados-ics)
    - [Anotación Simple](#anotacion-simple)
    - [Etiqueta en las RAMs](#etiqueta-en-las-rams)
      - [Corsair Version Number](#corsair-version-number)
      - [G.Skill 042 Code](#gskill-042-code)
      - [Kingston Code](#kingston-code)
    - [Nota en los Rangos Lógicos y la Densidad](#nota-en-los-rangos-logicos-y-la-densidad)
    - [Escala de Voltaje](#escala-de-voltaje)
    - [Frecuencia Máxima Esperada](#frecuencia-maxima-esperada)
    - [Bineado](#bineado)
    - [Voltaje Maxímo Diario Recomendado](#voltaje-maximo-diario-recomendado)
    - [Ranking](#ranking)
    - [Temperaturas y sus Efectos en la Estabilidad](#temperaturas-y-sus-efectos-en-la-estabilidad)
  - [Integrated Memory Controller (IMC)](#integrated-memory-controller-imc)
    - [Intel IMC](#intel-imc)
    - [AMD IMC](#amd-imc)
- [Overclocking](#overclocking)
  - [Miscelanea de Tips](#miscelanea-de-tips)
    - [Intel](#intel)
    - [AMD](#amd)
  - [Encontrar una línea base](#encontrar-una-linea-base)
  - [Ajustando Timings](#ajustando-timings)
- [Useful Link](#useful-links)
  - [Benchmarks](#benchmarks-1)
  - [Información](#informacion)

# Setup
## Software para Testear Memoria RAM
Usted siempre debería testear con varios test de estrés para asegurarse de que su overclock es estable.
### Evitar
* Yo no recomendaría el test de estrés de AIDA64 ni [Memtest64](https://forums.anandtech.com/threads/techpowerups-memtest-64-is-it-better-than-hci-memtest-for-determining-stability.2532209/) ya que ambos no son muy buenos al encontrar errores en la memoria.
### Recomendaciones
* [TM5](https://mega.nz/file/vLhxBahB#WwJIpN3mQOaq_XsJUboSIcaMg3RlVBWvFnVspgJpcLY) con cualquiera de las configs listadas:
  * [Extreme by anta777](TM5-Configs/extreme@anta777.cfg) (recomendada). Asegúrese de cargar la config. El programa debería decir 'Customize: Extreme1 @anta777' si fue cargado correctamente.  
  Credits: [u/nucl3arlion](https://www.reddit.com/r/overclocking/comments/dlghvs/micron_reve_high_training_voltage_requirement/f4zcs04/).
  * [Absolut](TM5-Configs/absolutnew.cfg)
  * [PCBdestroyer](TM5-Configs/PCBdestroyer.cfg)
  * [LMHz Universal 2](TM5-Configs/Universal-2@LMhz.cfg)
  * Si experimenta problemas con todos los hilos que se bloquean en el inicio con la configuración extrema, podría ayudar a editar la fila "Testing Window Size (Mb)=1408". Reemplace el tamaño de la ventana con su RAM total (menos algún margen para Windows) dividido por los hilos disponibles del procesador (por ejemplo, 12800/16 = 800 MB por hilo).
* [OCCT](https://www.ocbase.com/) con la prueba de memoria dedicada utilizando las instrucciones SSE o AVX.
  * Tenga en cuenta que AVX y SSE pueden variar en la velocidad de detección de errores. En los sistemas basados en Intel, SSE aparece mejor para probar voltajes de IMC, mientras que AVX aparece mejor para voltaje de DRAM.
  * La prueba de CPU AVX2 grande es una gran prueba de estabilidad para su CPU y RAM al mismo tiempo. Cuanto más ajuste su ram, más difícil será ser estable en esta prueba. Asegúrese de ejecutar el Modo Normal ya que Extreme no utilizará tanta RAM.
  * La prueba de VRAM a máxima utilización en conjunción con Prime95 Large FFTs pondrá tensión sobre el FCLK y se recomienda al probar la estabilidad del FCLK.
### Alternativas
* [GSAT](https://github.com/stressapptest/stressapptest).
  1. [Instalar WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) y [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab).
  2. Abra una consola de Ubuntu y escriba `sudo apt update`.
  3. Escriba `sudo apt-get install stressapptest`.
  4. Para iniciar el test escriba `stressapptest -M 13000 -s 3600 -W --pause_delay 3600`.
     * `-M` es la cantidad de memoria para testear (MB).
     * `-s` es el tiempo de prueba (segundos).
     * `--pause_delay` es el retraso entre los picos de potencia. Deberia ser el mismo que el argumento `-s`, para saltarse la prueba de los picos de potencia.
* [Karhu RAM Test](https://www.karhusoftware.com/ramtest/) (paga).
* [y-cruncher](http://www.numberworld.org/y-cruncher/) con [esta config](https://pastebin.com/dJQgFtDH).
  * Pegue esto en un nuevo archivo llamado `memtest.cfg` en el mismo folder que `y-cruncher.exe`.
  * Ajuste los siguientes campos si es necesario:
    * `LogicalCores`: Hilos CPU, e.g. `[0 1 2 3 4 5]` en 6C**6**T o `[0 1 2 3 4 5 6 7]` en 4C**8**T
    * `TotalMemory`: Memoria (bytes) usada por y-cruncher
  * Cree un shortcut para `y-cruncher.exe` y agregue `pause:1 config memtest.cfg` al campo de destino.
    Your target field should look something like this: `"path\to\y-cruncher\y-cruncher.exe" pause:1 config memtest.cfg`
  * Créditos: [u/Nerdsinc](https://www.reddit.com/r/overclocking/comments/iyp1n7/ycruncher_is_a_really_effective_tool_for_testing/)
* [Prime95](https://www.mersenne.org/download/) large FFTs también es decente en la búsqueda de errores de memoria.
  * He estado usando un rango FFT personalizado de 800K - 800K, aunque creo que cualquier valor FFT dentro del rango large FFTs debería funcionar.
    * Asegúrese de que 'Run FFTs in place' no está marcada.
    * En `prime.txt`, añada `TortureAlternateInPlace=0` bajo `TortureWeak` para evitar que P95 se ponga a prueba en su lugar. La prueba en el lugar solo utilizará un poco de RAM, que no queremos.
  * Puede crear un acceso directo a `prime95.exe` y añadir `-t` al campo 'Properties > Target' para comenzar inmediatamente las pruebas utilizando la configuración en `prime.txt`.  
    El campo de destino debe tener un aspecto similar al siguiente: `"path\to\prime95\prime95.exe" -t`.
  * También puede cambiar el directorio de trabajo de los archivos de configuración de Prime95 para tener una configuración para probar su CPU y otra configuración para probar su RAM.
    1. En el folder con `prime95.exe`, cree otro folder. Este ejemplo se llamará 'RAM' (sin las comillas).
    2. Copie `prime.txt` y `local.txt` al folder que acaba de crear.
    3. Ajuste las configuraciones en `prime.txt` según sea necesario.
    4. Cree otro shortcut para `prime95.exe`, y en el campo de desitno, agregue `-t -W<folder_name>`.  
       El campo de destino debería verse así: `"path\to\prime95\prime95.exe" -t -WRAM`.
    5. Ahora puede usar el shortcut para iniciar Prime95 con las configuraciones proporcionadas.
* [randomx-stress](https://github.com/00-matt/randomx-stress/releases) - Puede ser usado para testear la estabilidad de FCLK.
### Comparaciones
[Comparación](https://imgur.com/a/jhrFGhg) entre Karhu RAMTest, TM5 con la configuración extrema, y GSAT.
  * TM5 es el más rápido y más estresante por un margen considerable, aunque he tenido casos en los que pasaría 30 minutos de TM5 pero fallaría a 10 minutos de Karhu. Otro usuario tuvo una experiencia similar. YMMV.
    
## Software para Visualizar Timings
* Para visualizar timings en Windows: 
  * Intel: 
    * X99: [Asrock Timing Configurator v3.0.6](https://www.asrock.com/mb/Intel/X99%20OC%20Formula3.1/#Download).
    * Z370(?)/Z390: [Asrock Timing Configurator v4.0.4](https://www.asrock.com/MB/Intel/X299%20OC%20Formula/index.asp#Download) (no es necesario tener una Motherboard AsRock).
    * Motherboards EVGA y Z170/Z270(?)/Z490: [Asrock Timing Configurator v4.0.3](https://www.asrock.com/mb/Intel/Z170%20OC%20Formula/#Download).
    * Para Rocket Lake: [ASRock Timing Configurator v4.0.10](https://web.archive.org/web/20211010085116/http://picx.xfastest.com/nickshih/asrock/AsrTCSetup(v4.0.10).rar).
    * Para Alder Lake: [ASRock Timing Configuator v4.0.14](https://picx.xfastest.com/nickshih/asrock/AsrTCSetup(v4.0.14).rar) o [MSI Dragon Ball](https://drive.google.com/file/d/1XmKv13D0MgC9fPaA91535wCe9ztoeaHV/view?usp=sharing).
  * AMD: 
    * [ZenTimings](https://zentimings.protonrom.com/).
    
## Benchmarks

* [AIDA64](https://www.aida64.com/downloads) - prueba gratuita de 30 días. Vamos a utilizar el benchmark de caché y memoria de referencia (se encuentra en las herramientas) para ver cómo funciona nuestra memoria. Puede hacer clic con el botón derecho en el botón de inicio de la prueba de referencia y ejecutar pruebas de memoria solo para omitir las pruebas de caché.
* [Intel Memory Latency Checker](https://software.intel.com/content/www/us/en/develop/articles/intelr-memory-latency-checker.html) - contiene muchas pruebas útiles para medir el rendimiento de la memoria. Datos más extensos que AIDA64 y los números de ancho de banda difieren entre las pruebas. Tenga en cuenta que debe ser ejecutado como administrador para desactivar el prefetching. En sistemas AMD, puede tener que desactivarlo en la BIOS.
* [Intel MLC GUI](https://github.com/FarisR99/IMLCGui) - GUI para Intel Memory Latency Checker hecho por Faris.
* [xmrig](https://github.com/xmrig/xmrig) es muy sensible a la memoria, por lo que es útil probar los efectos de tiempos específicos. Primero, ejecute como administrador con el argumento de la línea de comandos `--bench=1M` para iniciar el punto de referencia. Luego, use el tiempo del punto de referencia para comparar.
* [MaxxMEM2](https://www.softpedia.com/get/System/Benchmarks/MaxxMEM2.shtml) - alternativa gratuita para AIDA64, pero las pruebas de ancho de banda suelen ser muy lentas, por lo que no es directamente comparable a AIDA64.
* [Super Pi Mod v1.5 XS](https://www.techpowerup.com/download/super-pi/) - otro benchmark de memoria sensible, pero no lo he usado mucho como AIDA64. digitos entre 1M-8M deberían ser suficientes para un benchmark rápido. Solo necesitas visualizar el último tiempo (total), mientras más bajo, mejor.
* [HWBOT x265 Benchmark](https://hwbot.org/benchmark/hwbot_x265_benchmark_-_1080p/) - He escuchado que este benchmark también es sensible a la memoria, pero no le testeado realmente por mí cuenta.
* [PYPrime 2.x](https://github.com/monabuntur/PYPrime-2.x) - Este benchmark es rápido y escala muy bien con "CPU core clock", "caché/FCLK," frecuencia de memoria, y timings

# Información General Acerca de la RAM
## Relación entre Frecuencia y Timings
* La frecuencia de la memoria RAM está medida en megahertz (MHz) o millones de ciclos por segundo. Mayor frecuencia significa más ciclos por segundo, que se traduce a un mejor rendimiento.
* Nota esotérica: La gente a menudo se refiere a DDR4-3200 como siendo 3200 **MHz** sin embargo, en realidad, la frecuencia real de la memoria RAM es solo 1600 MHz. A medida que los datos se transfieren tanto en el borde del reloj ascendente como en el borde del reloj descendente en DDR (Double Data Rate), la frecuencia real de la RAM es la mitad del número de transferencias que hace por segundo. DDR4-3200 transfiere 3200 millones de bits por segundo, y así, 3200 **MT/s** (MegaTransfers por segundo) opera a una frecuencia de 1600 **MHz***.
* Los timings de RAM están medidos en ciclos de reloj o ticks. Menores timings significan menos ciclos para realizar una operación, lo que se traduce en mejor rendimiento.
  * La excepción a esto es tREFI, que es el intervalo de refresco. Como su nombre lo dice, tREFI es el tiempo entre refrescos. Mientras que la RAM se refresca, no puede hacer nada, por lo que vas a querer que se refresque lo más infrecuentemente como sea posible. Para hacer eso, necesitas que el tiempo entre refrescos sea tan largo como sea posible. Esto significa que necesitas que tREFI tenga el mayor valor posible.
* Aunque los timings más bajos pueden ser mejores, esto también depende de la frecuencia de la memoria RAM. Por ejemplo, DDR4-3000 CL15 y DDR4-3200 CL16 tienen la misma latencia, a pesar de que DDR4-3000 se ejecuta con un CL absoluto más bajo. Esto se debe a que la mayor frecuencia compensa el aumento de CL.
* Para calcular el tiempo actual en nanosegundos (ns) acerca de un timing determinado: `2000 * timing / ddr_freq`.
  * Por ejemplo, CL15 en DDR4-3000 es `2000 * 15 / 3000 = 10 ns`.
  * Similarmente, CL16 en DDR4-3200 es `2000 * 16 / 3200 = 10 ns`.

## Timings Primarios, Secundarios y Terciarios
* Intel

  ![](Images/intel-primary-secondary-tertiary.png)

* AMD

  ![](Images/amd-primary-secondary-tertiary.png)

* Los timings de la memoria RAM están divididos en 3 categorias: primarios, secundarios, y terciarios. Estos están indicados por 'P', 'S', and 'T', respectivamente.
  * Los timings primarios y secundarios afectan la latencia y el ancho de banda.
  * Los timings terciarios afectan el ancho de banda.
    * La excepción a esto es tREFI/tREF, quienes affectan latencia y ancho de banda, aunque no es modificable en AMD (es modificable hasta AMD 7000).

# Expectativas/Limitaciones
* Esta sección abarca sobre 3 componentes que pueden influir en tu experiencia de overclocking: ICs, motherboard, e IMC.

## Placa Madre/Motherboard
* Motherboards con 2 módulos DIMM podrán alcanzar las frecuencias más altas.
* Para las motherboards con 4 módulos DIMM, el número de RAMs instaladas afectará tu frecuencia máxima de memoria.
  * En las motherboards que usan daisy chain [memory trace layout](https://www.youtube.com/watch?v=3vQwGGbW1AE), es preferible usar 2 sticks. Usar 4 sticks puede impactar de manera significativa su frecuencia máxima de memoria.
  * Por otra mano, las motherboards que usan T-topology se podrán overclockear mejor con 4 sticks. Usar 2 sticks no afectará su máxima frecuencia de memoria tanto como el uso de 4 palos en una placa madre daisy chain (?).
  * Existen algunas motherboards con T-Topology que se overclockean igual o mejor con solo un DIMM por canal.
  * La mayoría de los proveedores no anuncian su diseño de traza de memoria, pero usted puede hacer una suposición basada en el QVL. Por ejemplo, el Z390 Aorus Master utiliza T-Topology ya que su frecuencia validada más alta es con 4 DIMMs. Sin embargo, si la frecuencia validada más alta se hizo con 2 DIMMs, **probablemente** utiliza daisy chain.
  * Según Buildzoid, Daisy Chain vs. T-Topology solo importa por encima de DDR4-4000. Siguiendo la lógica de Buildzoid, si estás en Ryzen 3000 o 5000, esto no importa ya que DDR4-3800 es la frecuencia máxima típica de memoria cuando se ejecuta MCLK: FCLK 1:1.
* Las motherboards de gama baja podrían no overclockearse tan bien, posiblemente debido a la baja calidad del PCB y al número de capas (?).
  
## Circuitos Integrados (ICs)
* Saber qué ICs (a veces llamados "dies") están en su RAM le dará una idea de lo que puede esperar. Incluso si no los conoces, todavía puedes hacer overclock a tu RAM.

### Anotación Simple

Para facilitar la lectura rápida de los ICs, se utilizará una notación abreviada.

XYZ donde:
* X es la primera letra de la manofactura (S para Samsung, H para Hynix, M para Micron, N para Nanya, etc.).
* Y es la densidad (8 para 8 Gb, 16 para 16 Gb).
* Z es la revisión del die (C Die, B Die, E Die, etc.).

Por ejemplo, la lectura rápida para Samsung 8 Gb B-die es S8B.

### Etiqueta en las RAMs

Usar la etiqueta en los sticks es la manera más adecuada para identificar el IC. Sin embargo, actualmente solo las etiquetas de Corsair, G.Skill, y Kingston han sido decodeadas.

Vea [HardwareLuxx](https://www.hardwareluxx.de/community/threads/ryzen-ram-oc-m%C3%B6gliche-limitierungen.1216557/) para una infografía que resume la siguiente información.

* [SpecTek](https://www.micron.com/support/spectek-support) Circuitos integrados Micron de menor categoria.
* Nota esotérica: Muchas personas han comenzado a llamar a este Micron E-die o E-die. El primero está bien, pero el último puede causar confusión como letra-die se utiliza típicamente para ICs de Samsung, es decir, Samsung 4 Gb E-die. Samsung está implícito cuando dices E-die, pero como la gente está llamando a Micron Rev. E E-die, probablemente sería una buena idea prefijar el fabricante.

#### Corsair Version Number
* Corsair tiene un número de versión de 3 dígitos en la etiqueta de los sticks, indicando que IC se encuentra en el stick.
* El primer dígito es la manofactura.
  * 3 = Micron
  * 4 = Samsung
  * 5 = Hynix
  * 8 = Nanya
* El segundo dígito es la densidad.
  * 1 = 2 Gb
  * 2 = 4 Gb
  * 3 = 8 Gb
  * 4 = 16 Gb
* El último dígito es la revisión.
* Lea [r/overclocking wiki](https://www.reddit.com/r/overclocking/wiki/ram/ddr4#wiki_corsair) para una lista completa.
#### G.Skill 042 Code
* Similar a Corsair, G.Skill usa un 042 code para indicar el IC.
* Ejemplo: 04213X**8**8**1**0B
  * El primer carácter en negrita es la densidad. 4 para 4 Gb, 8 para 8 Gb y S para 16 Gb.
  * El segundo número en negrita es el fabricante. 1 para Samsung, 2 para Hynix, 3 para Micron, 4 para PSC (powerchip), 5 para Nanya y 9 para JHICC.
  * El último caracter es la revisión.
  * Este es el código para Samsung 8 Gb B-die.
* Lea [r/overclocking wiki](https://www.reddit.com/r/overclocking/wiki/ram/ddr4#wiki_g.skill_sn_table) para una lista completa.
#### Kingston Code
* Ejemplo: DPM**M**16A1823
  * La letra en negritas indica el fabricante. H para Hynix, M para Micron, y S para Samsung.
  * Los siguientes 2 dígitos indican ranks. 08 = single rank y 16 = dual rank.
  * La siguiente letra indica el mes de producción. 1-9, A, B, C.
  * Los siguientes 2 dígitos indican el año de producción.
  * Este es el código para dual-rank Micron producido en Octubre de 2018.
* [Fuente](http://www.xtremesystems.org/forums/showthread.php?285750-Interesting-memory-deals-thread&p=5230258&viewfull=1#post5230258)

### Nota en los Rangos Lógicos y la Densidad
* Los sticks single-rank suelen escalar más alto que los dual-rank, pero dependiendo del benchmark, la ganancia de rendimiento por *rank interleaving*<sup>1</sup> puede ser lo bastante grande como para superar a sticks single-rank más rápidos. [Esto se ve claro en synthetics y juegos](https://kingfaris.co.uk/ram).
   * En plataformas recientes (Comet Lake y Zen3), el soporte en BIOS y memory controller para dual-rank ha mejorado mucho. En muchas Z490, los Samsung B-die de 8 Gb en configuración 2x16 GB (dual-rank) pueden clockear igual de alto que un single-rank B-die, lo que significa que tienes todos los gains del *rank interleaving* casi sin downsides.
   * <sup>1</sup>El *rank interleaving* le permite al memory controller paralelizar requests, por ejemplo escribir en un rank mientras el otro refreshea. Esto impacta directo en AIDA64 copy bandwidth. Para el controlador da igual si el segundo rank está en el mismo DIMM o en otro dentro del mismo channel, aunque para overclock sí importa por el layout de las memory traces y el soporte en BIOS.
   * Tener un segundo rank del mismo IC significa el doble de bank groups disponibles. Eso habilita timings cortos como RRD_S en vez de RRD_L con más frecuencia, ya que hay más probabilidades de que haya un bank group “fresco”. Con 7 grupos en vez de 3, hay mucho más margen para evitar latencias largas.
   * También significa el doble de banks, por lo que más rows pueden estar abiertas al mismo tiempo. Así es más probable que la row que necesitas ya esté abierta, evitando ciclos extra de open/close (RAS/RC/RCD y RP).
   * Configs x16 tienen la mitad de banks y bank groups comparadas con x8, lo que reduce performance. Mira [el video de buildzoid](https://www.youtube.com/watch?v=k6SIdxq2yxE) para más info.
* La densidad de los IC también importa en el OC. Ejemplo: 4 Gb AFR y 8 Gb AFR no clockean igual aunque tengan el mismo nombre. Lo mismo pasa con Micron Rev. B, que existe en 8 Gb y 16 Gb. Los ICs de 16 Gb escalan mejor y se venden tanto en sticks de 16 GB como de 8 GB (aunque usen 8 chips). Los de 8 GB suelen traer SPD modificado y aparecen en kits Crucial high-end (BLM2K8G51C19U4B).
* A mayor cantidad de ranks en el sistema, más carga sobre el memory controller. Eso casi siempre implica necesidad de más voltaje, sobre todo VCCSA en Intel y SOC voltage en AMD.

### Escala de Voltaje
* Voltage scaling básicamente significa cómo responde un IC al voltaje.  
* En muchos ICs, **tCL escala con voltaje**, lo que significa que al darle más voltaje puedes bajar tCL. Por el contrario, **tRCD y/o tRP usualmente no escalan con voltaje** en la mayoría de ICs, es decir, no importa cuánto voltaje les metas, no se van a mover.  
Hasta donde sé, tCL, tRCD, tRP y posiblemente tRFC pueden (o no) mostrar voltage scaling.  
* De forma similar, si un timing escala con voltaje, puedes aumentar el voltaje para mantener el mismo timing a una frecuencia más alta.  

![CL11 Voltage Scaling](Images/cl-voltage-scaling.png)  
  * Se puede ver que **tCL escala casi linealmente hasta DDR4-2533 con voltaje en H8C**.  
  * **tCL en S8B tiene scaling lineal perfecto con voltaje.**  
  * **tCL en M8E también escala de forma lineal perfecta.**  
  * Adapté estos datos en una [calculadora](https://www.desmos.com/calculator/psisrpx3oh). Ajusta los sliders *f* y *v* a la frecuencia y voltaje que quieras, y te mostrará qué combinaciones de frecuencia y voltaje son alcanzables para un CL dado (asumiendo que CL escala linealmente hasta 1.50 V).  
    - Ejemplo: DDR4-3200 CL14 a 1.35 V debería llegar a ~DDR4-3333 CL14 con 1.40 V, ~DDR4-3533 CL14 con 1.45 V, y DDR4-3733 CL14 con 1.50 V.  

* **B-die tRFC Voltage Scaling**  
![B-die tRFC Voltage Scaling](Images/b-die-trfc-voltage-scaling.png)  
  * Aquí se puede ver que **tRFC escala bastante bien en B-die**.  

* Algunos ICs Micron más antiguos (anteriores a M8E) son conocidos por **escalar de manera negativa con voltaje**. Es decir, se vuelven inestables en la misma frecuencia y timings solo por aumentar el voltaje (normalmente por encima de 1.35 V).  

* Aquí una tabla de ICs probados y si el timing escala o no con voltaje:  

  | IC  | tCL | tRCD | tRP | tRFC |
  | :-: | :-: | :--: | :-: | :--: |
  | S8B | Y | Y | Y | Y |
  | H8C, H8D | Y | N | N | Y |
  | H8A | Y | N | N | ? |
  | M8B, M8E, M16B, N8B, S4E, S8D | Y | N | N | N |

  * Los timings que **no escalan con voltaje** normalmente deben aumentarse a medida que subes la frecuencia.

  
### Frecuencia Máxima Esperada
* Abajo están las frecuencias máximas esperadas para algunos de los ICs más comunes:

  | IC  | Velocidad Efectiva Máx. Esperada (MT/s) |
  | :-: | :-: |
  | H8D, M8E, M16B, S8B, S8D | 5000+ |
  | N8B, S4E | 4000+ |
  | H8C | 4133<sup>1</sup> |
  | H8A, M8B | 3600 |
  * <sup>1</sup>H8C es algo inconsistente en mis tests. Probé 3 sticks RipJaws V 3600 CL19 de 8 GB:  
    - uno se quedó clavado en DDR4-3600,  
    - otro en DDR4-3800,  
    - y el último llegó a DDR4-4000, todos en CL16 con 1.45 V.  
  * No esperes que los ICs con bin más bajo hagan OC igual de bien que los de bin más alto. Esto es especialmente cierto con [B-die](https://www.youtube.com/watch?v=rmrap-Jrfww).  
  * Estos valores reflejan las capacidades promedio del IC; sin embargo, otros factores como la motherboard y el CPU impactan fuertemente en si dichos valores son alcanzables o no.  

  
### Bineado
* **Binning o Bineado** básicamente es clasificar componentes en función de sus características de rendimiento.  
  Los fabricantes separan los ICs en diferentes contenedores/bins dependiendo de la frecuencia. De ahí viene el término *binning*.  
* **G.Skill** es un fabricante conocido por su binning y categorización extensiva. Varios SKUs de memorias G.Skill a menudo provienen del mismo bin de fábrica (ejemplo: DDR4-3600 16-16-16-36 1.35 V B-Die siendo el mismo bin que DDR4-3200 14-14-14-34 1.35 V B-Die).  
* Un B-Die binned para 2400 15-15-15 es significativamente peor que un buen B-Die binned para DDR4-3200 14-14-14 o incluso DDR4-3000 14-14-14. No esperes que tenga el mismo voltage scaling que un buen B-Die.  
* Para identificar qué frecuencia y timings corresponden a un bin más ajustado (*tighter bin*) dentro del mismo IC al mismo voltaje, hay que encontrar qué timing no escala con voltaje.  
  Simplemente divide la frecuencia entre ese timing: el valor más alto corresponde al bin más apretado.  
  * Ejemplo: Crucial Ballistix DDR4-3000 15-16-16 y DDR4-3200 16-18-18 usan ICs Micron Rev. E.  
    Si dividimos la frecuencia por tCL, da el mismo valor (200), lo que parecería indicar que son el mismo bin.  
    Pero no.  
    **tRCD no escala con voltaje**, lo que significa que debe aumentar cuando sube la frecuencia.  
    `3000 / 16 = 187.5` mientras que `3200 / 18 = 177.78`.  
    Como se ve, DDR4-3000 15-16-16 es un bin más tight que DDR4-3200 16-18-18.  
    Esto significa que un kit calificado para DDR4-3000 15-16-16 probablemente pueda correr DDR4-3200 16-18-18, pero un kit calificado para DDR4-3200 16-18-18 quizá no pueda correr DDR4-3000 15-16-16.  
    Sin embargo, la diferencia entre frecuencia y timings es pequeña, por lo que probablemente ambos hagan OC de manera similar.  
  
### Voltaje Maxímo Diario Recomendado
* [JEDEC JESD79-4B (p.174)](http://www.softnology.biz/pdf/JESD79-4B.pdf) especifica que el máximo absoluto es **1.50 V**.  
  > Tensiones mayores a las listadas bajo “Absolute Maximum Ratings” pueden causar daño permanente al dispositivo. Esta es solo una especificación de *stress rating*, y no implica operación funcional en estas u otras condiciones por encima de las secciones operativas del estándar. La exposición prolongada a estas condiciones puede afectar la fiabilidad.  

* Este valor es el máximo oficial del estándar DDR4 para el cual todos los DDR4 están calificados. Sin embargo, muchos ICs no son seguros a voltajes tan altos de manera sostenida. [S8C](https://www.hardwareluxx.de/community/f13/samsung-8gbit-ddr4-c-die-k4a8g045wc-overclocking-ergebnisse-im-startbeitrag-1198323.html) puede degradarse con voltajes tan bajos como **1.35 V** bajo ciertas condiciones de temperatura y entrega de potencia. Por otro lado, ICs como H8D o S8B han sido usados a diario con voltajes de más de **1.55 V**. Haz tu propia investigación sobre qué voltajes son seguros para tu IC, o quédate en torno a **1.35 V** si no lo sabes. Debido a la aleatoriedad y la variación de silicio, *YMMV* (Your Mileage May Vary), así que ten cuidado.  

* Un limitante común para el máximo voltaje seguro es la arquitectura de tu CPU. Según [JEDEC](https://www.jedec.org/standards-documents/dictionary/terms/output-stage-drain-power-voltage-vddq), **VDDQ** (voltaje de salida de datos) está ligado a **VDD**, conocido como **VDIMM o DRAM Voltage**. Este voltaje interactúa con el PHY (Physical Layer) del CPU y puede causar degradación a largo plazo del **IMC** si es demasiado alto.  
  Por esto, **no se recomienda** usar voltajes diarios de más de **1.60 V en Ryzen 3000/5000** y **1.65 V en Intel Consumer Lake-series**, ya que la degradación del PHY es difícil de detectar hasta que el problema es serio.  

* Puede ser seguro usar **1.60 V daily**, ya que existen kits en la [QVL de B550 Unify-X](https://www.msi.com/Motherboard/support/MEG-B550-UNIFY-X#support-mem-20) calificados para ello. ICs como H8D, M8E, M16B y S8B *deberían* estar bien corriendo 1.60 V daily, aunque se recomienda tener **airflow activo**. Voltajes más altos generan más temperatura, y las temperaturas elevadas reducen el umbral de lo que se considera seguro.  

* Lista de ICs y voltajes comúnmente usados:  

  | IC  | Daily Voltage (V) | Extreme Voltage (V) |
  | :-: | :---------------: | :-----------------: |
  | H8D, H16A, M8E, M16B, S4D, S4E, S8B | Up to 1.55 | Above 1.55 |
  | H4A, H8A, H8C<sup>1</sup>, H16C, N8B | Up to 1.45 | Above 1.45 |
  | S8C | Up to 1.35 | N/A<sup>2</sup> |

* Los voltajes marcados como *Daily Voltage* son seguros para el IC correspondiente **siempre que se mantengan las temperaturas bajo control**.  
* Los voltajes marcados como *Extreme Voltage* probablemente no degraden el IC, pero deben usarse con precaución. Se recomienda enfriamiento activo (RAM fan).  
* <sup>1</sup> Por encima de **1.45 V** se ha reportado degradación en H8C. Úsalo con precaución.  
* <sup>2</sup> S8C es conocido por escalar de manera negativa con voltaje. Se recomienda quedarse en o por debajo del máximo *daily voltage*.  
  
### Ranking
* A continuación se muestra cómo se clasifican los ICs más comunes en términos de frecuencia y timings.

  | Nivel | ICs | Descripción |
  | :-:  | :-: | :--:        |
  | S | S8B | Mejor IC DDR4 para rendimiento general |
  | A | H8D, M8E<sup>1</sup>, M16B | ICs de máximo rendimiento. Conocidos por no toparse con el "clock wall" y por escalar bien con voltaje. |
  | B | H8C, N8B, S4E | ICs de gama alta capaces de correr altas frecuencias con buenos timings. |
  | C | H8J, H16M, H16C, M16E, S8D, M8F | ICs decentes con buen rendimiento y escalado de frecuencia aceptable. |
  | D | H8A, M8B, S8C, S4D | ICs de gama baja comunes en kits baratos. La mayoría están EOL y ya no son relevantes. |
  | F | H8M, M4A, S4S, N8C | ICs pésimos, incapaces de alcanzar de forma fiable ni siquiera el nivel base de la especificación JEDEC. |

* Basado parcialmente en el ranking antiguo de [Buildzoid](https://www.reddit.com/r/overclocking/comments/8cjla5/the_best_manufacturerdie_of_ddr_ram_in_order/dxfgd4x/). Algunos ICs no aparecen por la antigüedad del post.

* <sup>1</sup> Las revisiones de M8E difieren principalmente en el tRCD mínimo alcanzable y en hasta qué frecuencia pueden clockear sin modificar VTT manteniendo estabilidad. En general, revisiones más nuevas de M8E (C9BKV, C9BLL, etc.) consiguen tRCD más tight y clockean más alto sin tocar VTT.
 
### Temperaturas y sus Efectos en la Estabilidad
* Generalmente, mientras más caliente esté tu RAM, menor será su estabilidad a frecuencias más altas y/o con timings más tight.  
* Los timings tRFC dependen mucho de la temperatura, ya que están relacionados con la fuga de los capacitores, la cual se ve afectada por el calor. Por lo tanto, a mayor temperatura, se requieren valores más altos de tRFC. Los timings tRFC2 y tRFC4 se activan cuando la DRAM alcanza los 85 °C. Por debajo de esa temperatura, estos timings no hacen nada.  
* En términos generales, la RAM es sensible a la temperatura y su rango ideal suele estar entre ~30-40 °C. Sin embargo, algunos ICs pueden tolerar temperaturas más altas, por lo que YMMV.  
* M8E, por otro lado, no parece ser tan sensible a la temperatura, como lo demostró [Buildzoid](https://www.youtube.com/watch?v=OeHEtULQg3Q).  
* Puede que encuentres estabilidad al pasar un test de memoria, pero que tu sistema crashee mientras juegas. Esto ocurre porque tu CPU y/o GPU generan calor dentro del gabinete, elevando la temperatura de la RAM en el proceso. Por esta razón, es recomendable estresar la GPU mientras ejecutas un test de memoria, para simular la estabilidad real en gaming.  
 
## Integrated Memory Controller (IMC)
### Intel IMC
* El IMC de Intel Skylake es bastante sólido, por lo que no debería ser el cuello de botella al hacer overclock.  
  ¿Qué esperarías de 14+++++?  
* El IMC de Rocket Lake, aparte de las limitaciones relacionadas con el soporte de Gear 1 y Gear 2, es el **memory controller más fuerte de todos los CPUs Intel de consumo** por un margen considerable.  
* Se prefiere **Gear 1** porque el clock del memory controller está sincronizado con el clock de la DRAM. La desincronización incurre en penalización de latencia.  
* Los CPUs Non-K de Alder Lake tienen **VCCSA bloqueado** y pueden no funcionar a frecuencias más altas en Gear 1. Se pueden esperar 3200 - 3466 MT/s en Gear 1.  
* Hay **2 voltajes que debes ajustar al hacer OC de RAM**: system agent (VCCSA) y IO (VCCIO).  
  **NO** los dejes en Auto, ya que pueden enviar niveles de voltaje peligrosos al IMC, degradándolo o incluso matándolo. La mayoría de veces puedes mantener VCCSA y VCCIO iguales, pero a veces demasiado voltaje puede afectar la estabilidad (créditos: Silent_Scone).  
  
  ![](Images/vccsa-vccio-sweet-spot.png)

  * Abajo están mis sugerencias de **VCCSA y VCCIO** para 2 DIMMs single-rank:

  | Velocidad Efectiva (MT/s) | VCCSA/VCCIO (V) |
  | :-------------: | :-------------: |
  | 3000 - 3600 | 1.15 - 1.20 |
  | 3600 - 4000 | 1.20 - 1.25 |
  | 4000 - 4200 | 1.25 - 1.30 |
  | 4200 - 4400 | 1.30 - 1.35 |

  * VCCIO generalmente debe estar **50 mV por debajo de VCCSA**, y correr 1.4 V VCCSA + 1.35 V VCCIO es aceptable como límite superior.  
  * Los voltajes seguros en Alder Lake no se conocen bien, ya que es relativamente nuevo. 1.25-1.35 V en VCCSA y VDDQ no ha mostrado degradación considerable.  
    * Para más info ver [Information](#informacion).  
  * Con más DIMMs y/o DIMMs dual-rank, puede que necesites **mayor VCCSA y VCCIO** que los sugeridos.  

* En CPUs de **Skylake a Rocket Lake** (inclusive), **tRCD y tRP están ligados**, lo que significa que si configuras tRCD 16 y tRP 17, ambos funcionarán al valor más alto (17).  
  * Esta limitación es por la que muchos ICs no rinden igual en Intel y por qué **B-Die es un buen match para Intel**.  
  * En UEFIs Asrock y EVGA están combinados como tRCDtRP. En ASUS, tRP está oculto. En MSI y Gigabyte, tRCD y tRP son visibles, pero al poner distintos valores ambos toman el mayor.  

* En CPUs **Alder Lake**, tRCD y tRP **ya no están ligados**.  

* Rango esperado de latencia de memoria: **40 ns - 50 ns**.  
  * Rango esperado para **B-Die**: 35 ns - 45 ns.  
  * En general, la latencia varía entre generaciones por el tamaño del die (ring bus). Por ejemplo, un 9900K tendrá latencia ligeramente menor que un 10700K con los mismos settings, ya que el 10700K comparte el die con el 10900K.  
  * La latencia también se ve afectada por **RTLs e IOLs**. En general, motherboards de mejor calidad y orientadas a overclocking tienen routing más directo de las memory traces y probablemente menores RTLs e IOLs. En algunas motherboards, cambiar RTLs e IOLs no tiene efecto.  
  
### AMD IMC
Algunas terminologías:  
* **MCLK**: Clock real de la memoria (la mitad de la velocidad efectiva de la RAM). Por ejemplo, DDR4-3200 → MCLK = 1600 MHz.  
* **FCLK**: Clock del Infinity Fabric.  
* **UCLK**: Clock del memory controller unificado. Mitad de MCLK cuando MCLK y FCLK no están sincronizados (modo 2:1).  
* En Zen y Zen+, **MCLK == FCLK == UCLK**. En Zen2 y Zen3 puedes especificar FCLK. Si MCLK = 1600 MHz (DDR4-3200) y FCLK = 1600 MHz, entonces UCLK = 1600 MHz salvo que configures ratio MCLK:UCLK a 2:1 (modo UCLK DIV, etc.). Si FCLK ≥ 1800 MHz, UCLK funcionará a 800 MHz (desincronizado).  

* El IMC de Ryzen 1000 y 2000 puede ser delicado al hacer OC y no alcanza frecuencias tan altas como Intel. Ryzen 3000 y 5000 tienen IMCs mucho mejores, prácticamente a la par de CPUs Intel Skylake recientes (9ª y 10ª gen).  
* **SOC voltage** es el voltaje del IMC y, al igual que en Intel, no se recomienda dejarlo en Auto. Rango típico: 1.00 V – 1.125 V. Valores más altos pueden ser necesarios para estabilidad en memorias de alta capacidad y para FCLK.  
* Por el contrario, un SOC voltage demasiado alto puede causar inestabilidad. Esto suele ocurrir entre **1.15 V y 1.25 V** en la mayoría de Ryzen.  
  > Hay diferencias claras en cómo el memory controller responde según el ejemplar de CPU. La mayoría alcanza 3466 MHz o más a 1.050 V SOC, pero algunos ejemplares escalan con el aumento de SOC, otros no escalan o incluso muestran **negative scaling** (más errores o fallos al entrenar) si se pasa de 1.150 V SOC. El máximo se logra generalmente ≤ 1.100 V SOC.  
  [~ The Stilt](https://forums.anandtech.com/threads/ryzen-strictly-technical.2500572/page-72#post-39391302)  

* En Ryzen 3000 también está **CLDO_VDDG** (abreviado VDDG, no confundir con CLDO_VDDP), que alimenta el Infinity Fabric. SOC voltage debe ser ≥ 40 mV sobre VDDG.  
  > La mayoría de los cLDOs se regulan desde los rails principales de CPU (VDDCR_SoC).  
  > Ejemplo: si pones VDDG a 1.10 V y el SOC real bajo carga es 1.05 V, VDDG llegará aprox. a 1.01 V.  
  > Ajustar solo SOC voltage, a diferencia de gen previos, no hace mucho. Default = 1.100 V. AMD recomienda mantenerlo así. Aumentar VDDG ayuda con OC del fabric en ciertos casos.  
  > FCLK 1800 MHz debería ser posible con default 0.950 V y para pushing limits se puede aumentar a ≤ 1.05 V (1.100 - 1.125 V SOC según load-line).  
  [~ The Stilt](https://www.overclock.net/threads/strictly-technical-matisse-not-really.1728758/page-2#post-28031966)  

* En AGESA ≥ 1.0.0.4, VDDG se separa en **VDDG IOD** y **VDDG CCD** para I/O die y chiplets.  

* Rangos de frecuencia esperados para **2 DIMMs single-rank**:

  | Ryzen | Velocidad Efectiva (MT/s) |
  | :---: | :----------------------: |
  | 1000 | 3000 - 3600 |
  | 2000 | 3400 - 3800<sup>1</sup> |
  | 3000 | 3600 - 3800 (1:1 MCLK:FCLK) <br/> 3800+ (2:1 MCLK:FCLK) |

  * Con más DIMMs o dual-rank DIMMs, la frecuencia esperada puede ser menor.  
  * <sup>1</sup>3600+ suele lograrse en 1 DIMM por canal y buena placa/IMC.  
    * Ver [aquí](https://docs.google.com/spreadsheets/d/1dsu9K1Nt_7apHBdiy0MWVPcYjf6nOlr9CtkkfN78tSo/edit#gid=1814864213).  
  * DDR4-3400 - DDR4-3533 es alcanzable por la mayoría de IMCs Ryzen 2000.  
    > Distribución de frecuencia máxima: DDR4-3400 = 12.5 %, DDR4-3466 = 25 %, DDR4-3533 = 62.5 %.  
    [~ The Stilt](https://forums.anandtech.com/threads/ryzen-strictly-technical.2500572/page-72#post-39391302)  

  * CPUs 2 CCD Ryzen 3000 (3900X, 3950X) parecen preferir 4 single-rank sticks sobre 2 dual-rank.  
    [~ The Stilt](https://www.overclock.net/forum/10-amd-cpus/1728758-strictly-technical-matisse-not-really-26.html#post28052342)  

* tRCD se divide en **tRCDRD (read)** y **tRCDWR (write)**. Normalmente tRCDWR puede ser menor, pero no se observan mejoras de rendimiento; mejor mantenerlos iguales.  

* **Geardown Mode (GDM)** se activa automáticamente > DDR4-2666, forzando incluso tCL, tCWL, tRTP, tWR y CR 1T. Para tCL impar, desactivar GDM.  
  * Si hay inestabilidad, usar CR 2T, aunque puede anular la ganancia de rendimiento de tCL reducido.  
  * Ejemplo: DDR4-3000 CL15 con GDM → CL redondea a 16.  
  * Rendimiento: GDM deshabilitado CR1T > GDM habilitado CR1T > GDM deshabilitado CR2T.  

* CPUs Ryzen 3000 single CCD < 3900X → ancho de banda de escritura se reduce a la mitad.  
  [~ TweakTown](https://www.tweaktown.com/reviews/9051/amd-ryzen-3900x-3700x-zen2-review/index3.html)  

* Rango de latencia esperado:

  | Ryzen | Latencia (ns) |
  | :---: | :----------: |
  | 1000 | 65 - 75 |
  | 2000 | 60 - 70 |
  | 3000 | 65 - 75 (1:1 MCLK:FCLK) <br/> 75+ (2:1 MCLK:FCLK) |
  | 4000/5000G | 55 - 65 |
  | 5000 | 60 - 70 (1:1 MCLK:FCLK) <br/> 70+ (FCLK desincronizado) |

* En Ryzen 3000/5000, un FCLK suficientemente alto puede compensar la penalización de MCLK y FCLK desincronizados si UCLK se puede bloquear a MCLK.  

  ![Chart](Images/optimal-fclk-vs-mclk.png)  
  * Créditos: [Buildzoid](https://www.youtube.com/watch?v=10pYf9wqFFY)  
  
# Overclocking
* **Disclaimer**: La “silicon lottery” afectará tu potencial de overclock, por lo que puede haber desviaciones respecto a mis sugerencias.  
* **Advertencia**: La corrupción de datos es posible al hacer OC de RAM. Se recomienda ejecutar `sfc /scannow` de vez en cuando para asegurar que cualquier archivo de sistema corrupto sea reparado.  
* El proceso de overclock es bastante simple y se resume en 3 pasos:  
  * Configura timings muy sueltos (altos).  
  * Incrementa la frecuencia de DRAM hasta que se vuelva inestable.  
  * Aprieta (reduce) los timings.

## Miscelanea de Tips
* Generalmente, un aumento de **200 MHz en la frecuencia efectiva de DRAM** compensa la penalización de latencia de aflojar tCL, tRCD y tRP en 1, pero ofrece mayor ancho de banda.  
  Por ejemplo, DDR4-3000 15-17-17 tiene la misma latencia que DDR4-3200 16-18-18, pero DDR4-3200 16-18-18 tiene mayor ancho de banda. Esto aplica típicamente después de la sintonización inicial, no en XMP.  
* En general, se debe **priorizar la frecuencia sobre timings más ajustados**, siempre que el rendimiento no se vea afectado por sincronización FCLK, Command Rate o Memory Gear mode.  
* Los timings secundarios y terciarios (excepto tRFC) no cambian mucho a lo largo del rango de frecuencia. Si tienes timings secundarios y terciarios estables a DDR4-3200, probablemente puedas correrlos a DDR4-3600 o incluso DDR4-4000, siempre que tus ICs, IMC y placa madre lo permitan.  

### Intel
* Aflojar **tCCDL a 8** puede ayudar con la estabilidad, especialmente por encima de DDR4-3600. Esto no genera una penalización significativa de latencia, pero puede afectar considerablemente el ancho de banda de lectura y escritura de la memoria.  

* Mayor frecuencia de **cache** (aka uncore, ring) puede aumentar el ancho de banda y reducir latencia.  

* Para placas **Asus Maximus**:  
  * Experimenta con los **Maximus Tweak Modes**; a veces una configuración POSTea donde otra no.  
  * Puedes habilitar **Round Trip Latency** bajo Memory Training Algorithms para que la placa intente entrenar valores RTL e IOL.  
  * Si no puedes bootear, prueba ajustando los valores de **skew control**.  

* **tXP** (y subsecuentemente PPD) tiene un gran impacto en la latencia de memoria medida con AIDA64.  
* **RTT Wr, Park y Nom** pueden afectar enormemente el overclocking. Los valores ideales dependen de tu placa, IC de memoria y densidad. Los valores “óptimos” permiten alcanzar frecuencias más altas con menor voltaje al memory controller. Algunas placas muestran los valores auto (MSI), otras no (Asus). Encontrar la combinación perfecta lleva tiempo pero es muy útil para tuning avanzado.  

* En algunas placas, habilitar **XMP** puede permitir un mejor overclocking.  
  * Gracias a Bored y Muren por encontrar y verificar esto en sus placas Asrock.

### AMD
* Prueba jugar con **ProcODT** si no puedes bootear. Esta configuración determina la impedancia de terminación interna del procesador. Según [Micron](https://www.micron.com/support/~/media/D546161C2C6140BCB0BAEE954AA53433.pdf), valores más altos de ProcODT pueden mejorar la estabilidad de la RAM, pero el trade-off es que podrías necesitar voltajes más altos.  

  * En **Ryzen 1000 y 2000**, prueba valores entre 40Ω y 68.6Ω debido al memory controller más débil.  
  * En **Ryzen 3000 y 5000**, [1usmus](https://www.overclock.net/threads/new-dram-calculator-for-ryzen%E2%84%A2-1-7-3-overclocking-dram-on-am4-membench-0-8-dram-bench.1640919/page-240#post-28049664) sugiere 28Ω - 40Ω. Valores más bajos pueden ser más difíciles de correr, pero ayudan con los requisitos de voltaje. Valores más altos pueden mejorar la estabilidad según [Micron](https://media-www.micron.com/-/media/client/global/documents/products/technical-note/dram/tn4040_ddr4_point_to_point_design_guide.pdf?la=en&rev=d58bc222192d411aae066b2577a12677); valores de ODT sobre 60Ω solo son adecuados para memory controllers extremadamente débiles y soluciones de bajo consumo.  

  Esto coincide con los ajustes de [The Stilt](https://www.overclock.net/forum/10-amd-cpus/1728758-strictly-technical-matisse-not-really-26.html).  
  > Phy en valores por defecto de AGESA, excepto ProcODT en 40.0Ω, una regla automática de ASUS para Optimem III.  

* Reducir **SOC voltage** y/o **VDDG IOD** puede ayudar con la estabilidad.  

* En **Ryzen 3000 y 5000**, valores más altos de **CLDO_VDDP** pueden ayudar con la estabilidad por encima de DDR4-3600.  
  > Aumentar cLDO_VDDP parece beneficioso > 3600MHz MEMCLKs, ya que mejora los márgenes y ayuda con posibles problemas de training.  
  Fuente: [The Stilt](https://www.overclock.net/forum/10-amd-cpus/1728758-strictly-technical-matisse-not-really-26.html)  

  > Pequeños cambios en VDDP pueden tener gran efecto, y VDDP no puede fijarse a un valor mayor que **VDIMM-0.1V** (**no superar 1.05V**).  
  Fuente: [AMD](https://web.archive.org/web/20210520115124/https://community.amd.com/t5/blogs/community-update-4-let-s-talk-dram/ba-p/415902)  

* Al empujar **FCLK** cerca de 1800 MHz, errores intermitentes de entrenamiento de RAM pueden aliviarse o eliminarse aumentando **VDDG CCD**.

## Encontrando una Línea Base
1. * Asegúrate de que tus módulos estén en los slots DIMM recomendados (generalmente 2 y 4).

   * Asegúrate de que el overclock del CPU esté desactivado al tunear la RAM, ya que un CPU inestable puede causar errores de memoria. De igual manera, al empujar frecuencias altas con timings ajustados, tu CPU puede volverse inestable y requerir reinicio.

   * Asegúrate de que tu UEFI/BIOS esté actualizado.
  
2. Desactiva el DRAM PowerDown Mode en el UEFI. Esto también elimina la necesidad de tunear timings relacionados como tCKE y tXP.

3. En Intel, ajusta el Command Rate (CR) a 2T si no lo está ya y configura tCCDL a 8.

   En AMD, habilita Gear Down Mode si no está activado.

4. En Intel, comienza con 1.2 V VCCSA y 1.15 V VCCIO. Para ADL, VCCIO no existe. Nota que si tienes un SKU no-K de Alder Lake, VCCSA estará bloqueado y tu potencial de overclock será limitado.

   En AMD, comienza con 1.10 V SOC, 0.95 V VDDP, 0.95 V VDDG CCD y 1.05 V VDDG IOD.
   * Si no puedes bootear o encuentras errores al aumentar frecuencia o ajustar timings, entonces estos voltajes pueden necesitar incrementarse. Consulta [Integrated Memory Controller (IMC)](#integrated-memory-controller-imc) para voltajes máximos recomendados y más información. Ten cuidado de no aumentarlos demasiado, ya que puede ocurrir “negative scaling”. VCCSA/VCCIO o SOC son los que probablemente necesiten aumento, en pasos de 25-50 mV.
   * El voltaje SOC puede llamarse diferente según el fabricante.
     * Asrock: CPU VDDCR_SOC Voltage. Si no lo encuentras, puedes usar SOC Overclock VID oculto en el menú AMD CBS.
       * [Valores VID](https://www.reddit.com/r/Amd/comments/842ehb/asrock_ab350_pro4_guide_bios_overclocking_raven/).
     * Asus: VDDCR SOC.
     * Gigabyte: (Dynamic<sup>1</sup>) Vcore SOC.
       * <sup>1</sup>Dynamic Vcore SOC se encuentra en ciertas motherboards Gigabyte y es un voltaje de offset. Por lo tanto, el voltaje base puede cambiar automáticamente al aumentar la frecuencia de DRAM. Por ejemplo, +0.100 V a DDR4-3000 podría resultar en 1.10 V real, pero +0.100 V a DDR4-3400 podría resultar en 1.20 V real.
     * MSI: CPU NB/SOC.
5. Para determinar qué voltaje usar para tu IC, consulta la [sección de voltaje diario máximo recomendado](#voltaje-maximo-diario-recomendado).
   * “Roll over” significa que el IC se vuelve más inestable al aumentar el voltaje, a veces hasta no POSTear.
   * ICs conocidos por “roll over” por encima de 1.35 V incluyen, pero no se limitan a: Samsung C-die de 8 Gb y Micron/SpecTek más antiguos (antes de M8E).

6. Configura timings primarios sueltos. Revisa la tabla a continuación.

   |Frecuencia|tCL|tRCD|tRP|tRAS|
   |---|---|---|---|---|
   |<=3200|16|20|20|40|
   |3201-3600|18|22|22|44|
   |3601-4000|20|24|24|48|
   |4001-4400|22|26|26|52|
   |4400+|24|28|28|56|

   Fuente: Eden de [Overclocking Discord](discord.gg/overclock)

   * Algunos ICs pueden no bootear con timings primarios muy sueltos al inicio. Se recomienda aflojarlos mientras se aumenta la frecuencia según la tabla anterior.
   * Algunas motherboards tienen reglas automáticas que pueden causar problemas, como tCWL = tCL - 1, lo que puede llevar a que tCWL sea impar. Si es así, prueba configurar tCWL un valor más bajo.
     * tCWL mayor a 18 o 20 puede no funcionar, aunque no es necesario ponerlo tan alto.
   * Consulta [este post](https://redd.it/ahs5a2) para más información sobre estos timings.
  
7. Incrementa la frecuencia de DRAM hasta que Windows no bootee más. Ten en cuenta las expectativas detalladas arriba, incluyendo los timings para cada rango de frecuencia.
   * Ryzen 3000/5000:
     * Desincronizar MCLK y FCLK puede generar un enorme penal de latencia, así que es mejor ajustar timings para mantener MCLK:FCLK 1:1. Revisa [AMD - AM4](#amd-imc) para más información.
   * En Intel, una forma rápida de saber si eres inestable es examinar los RTLs e IOLs. Cada grupo de RTLs y IOLs corresponde a un canal. Dentro de cada grupo, 2 valores corresponden a cada DIMM.

   RTLs e IOLs en Asrock Timing Configurator:
   
   ![](Images/intel-rtl-iol-difference-stable.png)

   Con mis módulos instalados en canal A slot 2 y canal B slot 2, debo revisar D1 dentro de cada grupo de RTLs e IOLs.  
   Los RTLs no deben diferir más de 2, y los IOLs no más de 1.  
   En mi caso, los RTLs son 53 y 55 (exactamente 2 de diferencia) y los IOLs ambos 7.
   Nota: tener RTLs e IOLs dentro de estos rangos no garantiza estabilidad.
   * En Ryzen 3000 o 5000, asegúrate que la frecuencia de Infinity Fabric (FCLK) esté a la mitad de tu frecuencia efectiva de DRAM. Confírmalo en ZenTimings asegurando que FCLK coincida con UCLK y MCLK.
8. Ejecuta un tester de memoria de tu elección.
   * Windows usará ~2000 MB, asegúrate de tenerlo en cuenta al ingresar la cantidad de RAM a testear si el test requiere input manual. Por ejemplo, tengo 16 GB de RAM y usualmente testeo 14000 MB.
   * Cobertura/tiempo mínimo recomendado:
     * **Para AMD, corre Prime95 Large FFTs y OCCT VRAM con uso máximo simultáneo para estresar FCLK y garantizar estabilidad. Esto debe hacerse después de cualquier cambio de frecuencia/FCLK.**
     * MemTestHelper (HCI MemTest): 20 % por hilo.
     * Karhu RAMTest: 5000 %.
       * En la pestaña avanzada, asegúrate que CPU cache esté habilitada. Esto acelera el test ~20 %.
       * Testear 6400 % de cobertura durante 1 hora tiene tasa de error de 99,41 % y 98,43 % respectivamente ([Fuente - sección FAQ](https://www.karhusoftware.com/ramtest/)).
     * TM5 anta777 Extreme: 3 ciclos.
       * El tiempo varía según densidad. Para 16 GB RAM, usualmente toma 1.5-2 horas. Para 32 GB RAM, puedes poner la fila 12 de la config (Time (%)) a la mitad y obtendrás un tiempo similar a 16 GB.
     * OCCT Memory: 30 minutos cada uno para SSE y AVX.
     * **Puedes ejecutar más tests como otras configs de TM5 para asegurar estabilidad. Se recomienda correr varias pruebas para máxima cobertura de errores.**
9. Si crash/freeze/BSOD o encuentras un error, baja la frecuencia de DRAM un notch y vuelve a testear.
10. Guarda tu perfil de overclock en tu UEFI.
11. Desde aquí puedes: intentar aumentar frecuencia o trabajar en ajustar los timings.
   * Ten en cuenta las expectativas anteriores. Si estás al límite de tus ICs y/o IMC, es mejor afinar los timings.
   
## Ajustando Timings
* Asegúrate de correr un test de memoria y benchmark después de cada cambio para verificar que el rendimiento esté mejorando.
  * Recomiendo correr el benchmark de 3 a 5 veces y promediar los resultados, ya que los benchmarks de memoria pueden variar ligeramente.
  * Ancho de banda máximo teórico (MB/s) = `Transfers per clock * Clock real * Número de canales * Bus Width * Conversión bits a bytes`.
       * Transfers per clock: número de transferencias de datos en un ciclo completo de memoria. Ocurre dos veces por ciclo en DDR, en flancos ascendente y descendente.
       * Clock real: frecuencia real de la memoria, medida en MHz. Programas como CPU-Z muestran esta frecuencia.
       * Número de canales: cantidad de canales activos en tu CPU.
       * Bus Width: ancho de cada canal de memoria, en bits. Desde DDR1, esto es 64 bits.
       * Conversión bits a bytes: constante 1/8 (0.125).

    | Velocidad Efectiva (MT/s) | Máx Ancho de Banda Dual Channel (MB/s) |
    | :-----------------: | :-------------------: |
    | 3000 | 48000 |
    | 3200 | 51200 |
    | 3400 | 54440 |
    | 3466 | 55456 |
    | 3600 | 57600 |
    | 3733 | 59728 |
    | 3800 | 60800 |
    | 4000 | 64000 |
    
    * Tu ancho de banda de lectura y escritura debería ser 90 % - 98 % del ancho de banda teórico máximo.
      * En Ryzen 3000-5000 de CCD único, el ancho de banda de escritura debería ser 90 % - 98 % de la mitad del máximo teórico.  
        Es posible alcanzar la mitad del ancho de banda de escritura teórico máximo. Ver [aquí](https://redd.it/cgc9bh).
      * El porcentaje del ancho de banda máximo teórico es inversamente proporcional a la mayoría de los timings. En general, a medida que se ajustan los timings de RAM, este valor aumenta.

1. Recomiendo empezar ajustando algunos de los **timings secundarios**, ya que aceleran las pruebas de memoria.  
   Mis sugerencias:

   | Timing | Safe | Tight | Extreme |
   | ------ | ---- | ----- | ------- |
   | tRRDS tRRDL tFAW | 6 6 24 | 4 6 16 | 4 4 16 |
   | tWR tRTP<sup>1</sup> | 20 10 | 16 8 | 12 6 |

   * En AMD, si GDM está habilitado, tWR y tRTP se redondean, baja 2 o mantenlos pares.   
   * El valor mínimo donde bajar tFAW afectará el rendimiento es `tRRDS * 4` o `tRRDL * 4`, el que sea menor.
   * No es necesario aplicar todos los presets a todos los timings al mismo tiempo. Por ejemplo, puedes correr tRRDS tRRDL tFAW en preset tight, pero tWR en extreme.
   * En algunas motherboards Intel, tWR en UEFI no tiene efecto y debe controlarse con tWRPRE (a veces tWRPDEN). Reducir tWRPRE en 1 bajará tWR en 1, siguiendo tWR = tWRPRE - tCWL - 4.
   * <sup>1</sup>tWR = 2*tRTP según datasheet DDR4 Micron y JEDEC DDR4.  
     ![Relación tWR-tRTP](Images/tWR-tRTP-relationship.png)  
     Gracias a [junkmann](https://github.com/integralfx/MemTestHelper/issues/55).

2. Ahora tRFC. Por defecto para ICs de 8 Gb es 350 **ns**.
   * Nota: ajustar demasiado tRFC puede causar freezes o lock-ups.
   * tRFC es el número de ciclos que los condensadores de DRAM necesitan para “recargarse” o refrescarse. Como la pérdida de carga es proporcional a la temperatura, la RAM a mayor temperatura necesita tRFC más alto.
   * Conversión a ns: `2000 * timing / ddr_freq`.  
     Ejemplo: tRFC 250 a DDR4-3200 = `2000 * 250 / 3200 = 156.25 ns`.
   * Conversión de ns a UEFI: `ns * ddr_freq / 2000`.  
     Ejemplo: 180 ns a DDR4-3600 = `180 * 3600 / 2000 = 324`, ingresa 324 en UEFI.
   * tRFC típico en ns según ICs:

     | IC | tRFC (ns) |
     | :-: | :-------: |
     | S8B | 120 - 180 |
     | N8B | 150 - 170 |
     | H8D | 240 - 260 |
     | H8A, H8C | 260 - 280 |
     | M8E, M16B | 280 - 310 |
     | S8C | 300 - 340 |
     
   * Para otros ICs, recomiendo **binary search** para encontrar tRFC estable mínimo.

3. Sugerencias para el resto de los secundarios:

   | Timing | Safe | Tight | Extreme |
   | :----: | :--: | :---: | :-----: |
   | tWTRS tWTRL | 4 12 | 4 10 | 4 8 |
   | tCWL<sup>1</sup> | tCL | tCL - 1 | tCL - 2 |

   * En AMD, si GDM está activo, tCWL se redondea; baja 2 o mantenlo par.
   * En Intel, tWTRS/L en auto y controlado con tWRRD_dg/sg. Ajusta manualmente al mínimo estable.
   * Cambiar tCWL afectará tWRRD_dg/sg y por tanto tWTRS/L. Baja ambos proporcionalmente.
   * <sup>1</sup>Algunas motherboards no soportan tCWL impar; ajustar manualmente según paridad de tCL puede resolverlo.
   * Preset extremo no es mínimo absoluto; tRTP puede ir hasta 5 (6 con GDM on), tWTRS/L hasta 1/6. Esto aumenta carga en IMC.

4. Tertiarías:
    * AMD: [post referencial](https://redd.it/ahs5a2). Sugerencia:

       | Timing | Safe | Tight | Extreme |
       | ------ | ---- | ----- | ------- |
       | tRDRDSCL tWRWRSCL | 4 4 | 3 3 | 2 2 |
     
        * Valores bajos (como 2) solo estables en ICs como Samsung 8 Gb B-Die. Mezclar presets posible; tRDRDSCL suele necesitar +1 o +2 respecto a tWRWRSCL.

    * Intel: ajusta un grupo a la vez.

      | Timing | Safe | Tight | Extreme |
      | ------ | ---- | ----- | ------- |
      | tRDRD_sg/dg/dr/dd | 8/4/8/8 | 7/4/7/7 | 6/4/6/6 |
      | tWRWR_sg/dg/dr/dd | 8/4/8/8 | 7/4/7/7 | 6/4/6/6 |
      * tWRRD_sg/dg: ajusta según paso 3.
      * tWRRD_dr/dd: baja hasta inestabilidad o degradación.
      * tRDWR_sg/dg/dr/dd: baja hasta inestabilidad o degradación; usualmente se igualan.

      * dr solo afecta dual rank, dd solo si 2 DIMMs/slot.

      * Dual rank: tRDRD_dr/dd puede bajarse a 5 para boost de lectura, tWRWR_sg 6 puede degradar escritura pese a estabilidad.

5. Baja tCL de 1 en 1 hasta inestabilidad.
   * AMD con GDM: redondear tCL, bajar 2 o mantener par.

6. Intel: baja tRCD y tRP de 1 en 1 hasta inestabilidad.  
   AMD: baja tRCD y luego tRP de 1 en 1.
   * Nota: puede requerirse más voltaje IMC.

7. Configura `tRAS = tRCD(RD) + tRTP`. Incrementa si inestable.
   * tRAS mínimo absoluto.  
   ![tRAS](Images/tras-datasheet-diagram.png)
     * ACT a READ = tRCD
     * READ a PRE = tRTP
     * tRAS = tRCD + tRTP

8. Configura `tRC = tRP + tRAS`. Incrementa si inestable.
   * Solo disponible en AMD y algunos Intel UEFI.

9. Incrementa tREFI hasta inestabilidad. Binary search aplicable.  
   Sugerencias:

   | Timing | Safe | Tight | Extreme |
   | ------ | ---- | ----- | ------- |
   | tREFI | 32768 | 40000 | Max (65535 o 65534) |

   * No conviene subir demasiado tREFI por cambios de temperatura ambiente.
   * tREFI máximo puede corromper archivos; usar con cuidado.

10. **Command Rate**

    AMD:
    * Deshabilitar GDM y lograr CR 1 estable es difícil, pero vale la pena.
    * Si GDM off y CR 1 funcionan sin cambios, salta esta sección.
    * CR 1 se vuelve más difícil con mayor frecuencia; CR 2 puede ayudar.
    * GDM overridea CR, por lo que deshabilitar GDM para CR 2 puede mejorar estabilidad.
    
    1. Posibilidad: Drive strengths 60-20-20-24 y Setup times 63-63-63.
       * Drive strengths: ClkDrvStr, AddrCmdDrvStr, CsOdtDrvStr, CkeDrvStr.
       * Setup times: AddrCmdSetup, CsOdtSetup, CkeSetup.
    2. Si no bootea, ajusta setup times juntos hasta lograr POST.
    3. Corre test de memoria.
    4. Ajusta setup times y luego drive strengths si inestable.
    * Mis settings estables GDM off CR 1:

      ![](Images/gdm-off-cr-1t-stable.png)

    1. Drive strength > 24 Ω puede dañar estabilidad. Setup times >0 raramente necesarios, pero ayudan a CR 1.

    Intel:
    * DDR4 <4400: intenta CR 1T, si falla, usa CR 2T.
    * Asus Maximus: habilitar Trace Center ayuda a CR 1T a frecuencias altas.

11. Intel: aumenta offsets IOL para reducir IOLs, luego test de memoria. Más info [aquí](https://hwbot.org/newsflash/3058_advanced_skylake_overclocking_tune_ddr4_memory_rtlio_on_maximus_viii_with_alexaros_guide).
  * RTL e IOL impactan rendimiento; bajarlos aumenta bandwidth y reduce latencia.
  
    ![](Images/rtl-iol-aida-impact.png)

  * Valores bajos pueden ayudar estabilidad y reducir voltaje IMC. Algunos boards entrenan solos; otros permiten tuning manual.

12. Puedes aumentar voltaje DRAM para bajar timings aún más. Ten en cuenta el [escalado de voltaje de tus ICs](#escala-de-voltaje) y [voltaje diario máximo recomendado](#voltaje-maximo-diario-recomendado).

# Useful Links
## Benchmarks
* [Impact of RAM on Intel's Skylake desktop architecture by KingFaris](https://kingfaris.co.uk/blog/intel-ram-oc-impact)
* [RAM timings and their influence on games and applications (AMD) by Reous](https://www.hardwareluxx.de/community/threads/ram-timings-und-deren-einfluss-auf-spiele-und-anwendungen-amd-update-23-05-2020.1269156/)
## Información
* [r/overclocking Wiki - DDR4](https://www.reddit.com/r/overclocking/wiki/ram/ddr4)
* [Demystifying Memory Overclocking on Ryzen: OC Guidelines and Explaining Subtimings, Resistances, Voltages, and More! by varexos717](https://redd.it/ahs5a2)
* [Maximus Z690 and Alder Lake: Modern CPU’s require Modern Overclocking Solutions](https://rog.asus.com/forum/showthread.php?126369-Maximus-Z690-and-Alder-Lake-Modern-CPU%92s-require-Modern-Overclocking-Solutions)
* [12th Gen Intel Memory Overclocking Voltages - buildzoid](http://buildzoid.blogspot.com/2022/03/12th-gen-intel-memory-overclocking.html)
* [HardwareLUXX Ryzen RAM OC Thread](https://www.hardwareluxx.de/community/f13/ryzen-ram-oc-thread-moegliche-limitierungen-1216557.html)
* [Ryzen 3000 Memory / Fabric (X370/X470/X570) by elmor](https://www.overclock.net/forum/13-amd-general/1728878-ryzen-3000-memory-fabric-x370-x470-x570.html)
* [Intel Memory Overclocking Quick Reference by sdch](https://www.overclock.net/threads/official-intel-ddr4-24-7-memory-stability-thread.1569364/page-392#post-27784556)
* [Advanced Skylake Overclocking: Tune DDR4 Memory RTL/IO on Maximus VIII with Alex@ro's Guide](https://hwbot.org/newsflash/3058_advanced_skylake_overclocking_tune_ddr4_memory_rtlio_on_maximus_viii_with_alexaros_guide)
* [BSOD codes when OC'ing and possible actions](https://www.reddit.com/r/overclocking/comments/atwtt5/psa_bsod_codes_when_ocing_and_possible_actions/)
