
DNSChess
Gracias a <b>@Chm0d </b>y <b>@Perzival</b> por su ayuda en este reto:

Empezamos ejecutando el programa para ver que hacia el reto se compone de tres archivos un binario, una librería “Shared Object” y un pcap, una vez dándole permisos al ejecutable se procede a correrlo.
![Alt text](https://github.com/xxmeshx/flareon6/blob/master/DNSChess/deepresigned.png)

Vemos un tablero de ajedrez por lo que deducimos que se tiene que jugar una partida. Pero al mover cualquier pieza el programa nos regresa el siguiente mensaje.


<p>El juego se queda “Pensando” un momento y después sale el mensaje de <b>“DeepFlare has resigned”</b>.</p>

Desensamblamos el binario, pero no encontramos mucho por lo que decidimos intentar todos los movimientos de ajedrez posibles, peones e inclusive caballos.

![Alt text](https://github.com/xxmeshx/flareon6/blob/master/DNSChess/getnextmove.png)

Acto seguido desensamblamos en IDA el .so y encontramos una función interesante dentro del pseudocodigo llamada <b>“gethostbyname”</b> vemos que hace una validación que si <b>*v10!=127</b> y otras que aparecen dentro del programa retorna 0  por lo que leyendo un poco acorde a la petición realizada hace un request a una IP pudimos corroborarlo poniendo el wireshark y viendo que solicitudes hacia.

Efectivamente vemos que Wireshark trata de hacer una resolución de la siguiente manera:

        Nombre de la pieza + cuadro inicial + cuadro final + .game-of-thrones.flare-on.com

Es decir que si movemos un peon de D2 a D4 hace la siguiente petición:

        pawn-d2-d4.game-of-thrones.flare-on.com
Y realiza la solicitud esto coincide con lo que tenemos en el pcap y viendo detenidamente, observamos que tiene unas direcciones ip iniciando con 127 por lo que procedimos a limpiarlos y agregarlos a nuestro archivos hosts en <b>/etc/hosts</b>.


Con las IP y la resolución DNS procedemos a realizar todos los movimientos que previamente agregamos. 

![Alt text](https://github.com/xxmeshx/flareon6/blob/master/DNSChess/hosts.png)

Cuando movemos uno de los peones <b>VOILA!</b> Tenemos respuesta del juego por lo que nuevamente intentamos todos los movimientos posibles hasta que no quedara ni uno solo. Quedando los movimientos de la siguiente manera:

    D2-D4
    C2-C4
    B1-C3
    E2-E4
    G1-F3
    C1-F4
    F1-E2
    E2-F3
    F4-G3
    E4-E5
    F3-C6
    C6-A8
    E5-E6
    D1-H5



![Alt text](https://github.com/xxmeshx/flareon6/blob/master/DNSChess/dnschessflag.png)


Una vez ganandole a la IA con los movimientos que contiene el PCAP podemos ver la flag:

                                  LooksLikeYouLockedUpTheLookupZ@flare-on.com





