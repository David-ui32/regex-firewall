## Task 1
Command: grep -Ec '^[0-9]' firewall.log
Result: 100000
Explanation: El ancla ^ obliga a que la coincidencia empiece al inicio de la línea, y la clase [0-9] exige que el primer carácter sea un dígito (el año de la fecha). Las 4 líneas de cabecera empiezan con # y no cumplen esto, así que quedan excluidas; grep -c cuenta las líneas que sí coinciden.

## Task 2
Command: grep -Ec '^[0-9-]+ [0-9:]+ (DROP|REJECT) ' firewall.log
Result: 60156
Explanation: Para asegurarme de matchear el campo action y no esas letras en otro lugar de la línea, primero "consumo" los dos campos anteriores: ^[0-9-]+ matchea la fecha, un espacio, [0-9:]+ matchea la hora, y otro espacio, dejándome justo al inicio del campo action. Ahí el grupo (DROP|REJECT) usa el operador de alternación | para matchear cualquiera de las dos palabras, y el espacio final asegura que sea la palabra completa del campo.

## Task 3
Command: grep -Ec '^[0-9-]+ [0-9:]+ [A-Z]+ (TCP|UDP) 11\.' firewall.log
Result: 33217
Explanation: Consumo fecha, hora, action ([A-Z]+ cubre ACCEPT, DROP, REJECT, FORWARD) y protocol (TCP|UDP) para llegar exactamente al inicio del campo src-ip. Ahí 11\. matchea el literal "11." — el carácter \ escapa el punto para que se trate como punto literal y no como el metacaracter "cualquier carácter".

## Task 4
Command: grep -Ec ' [0-9]{7}$' firewall.log
Result: 2343
Explanation: El ancla $ marca el final de línea, donde está el campo size. El cuantificador {7} exige exactamente 7 dígitos consecutivos justo antes de ese final. El espacio antes del [0-9]{7} asegura que el campo completo tenga exactamente 7 dígitos y no se trate de los últimos 7 dígitos de un número más largo.

## Task 5
Command: grep -Ev '^#' firewall.log | sed -E 's/^([0-9-]+) ([0-9:]+) ([A-Z]+) (TCP|UDP) .*/\1 \3 \4/' | head -5
Result:
2018-05-25 FORWARD TCP
2018-02-22 FORWARD UDP
2018-03-20 REJECT UDP
2018-11-08 REJECT TCP
2018-07-24 REJECT TCP
Explanation: Primero grep -Ev '^#' elimina las 4 líneas de cabecera (que no siguen el formato de los eventos). Luego sed -E usa 4 grupos de captura (): el grupo 1 captura la fecha, el grupo 2 la hora, el grupo 3 el action y el grupo 4 el protocol; el resto de la línea (.*) se descarta. La sustitución reconstruye la línea usando solo \1 \3 \4 (backreferences a los grupos capturados), dejando únicamente date, action y protocol.

## Task 6
Command: grep -Ec '^[0-9-]+ [0-9:]+ ACCEPT TCP .* 80 [0-9]+$' firewall.log
Result: 93
Explanation: Ancla fecha y hora con clases de caracteres, luego exige los literales ACCEPT y TCP en sus posiciones exactas. La parte ".* 80 " busca el literal "80" como campo completo, y como el campo siguiente y último es size ([0-9]+$, anclado al final de línea), esto garantiza que el "80" matcheado es justo el campo dst-port (penúltimo campo) y no, por ejemplo, parte de un puerto distinto como 180 u 8090.

