## Task 1
Command: grep -Ec '^[0-9]' firewall.log
Result: 100000
Explanation: El ancla ^ obliga a que la coincidencia empiece al inicio de la línea, y la clase [0-9] exige que el primer carácter sea un dígito (el año de la fecha). Las 4 líneas de cabecera empiezan con # y no cumplen esto, así que quedan excluidas; grep -c cuenta las líneas que sí coinciden.

## Task 2
Command: grep -Ec '^[0-9-]+ [0-9:]+ (DROP|REJECT) ' firewall.log
Result: 60156
Explanation: Para asegurarme de matchear el campo action y no esas letras en otro lugar de la línea, primero "consumo" los dos campos anteriores: ^[0-9-]+ matchea la fecha, un espacio, [0-9:]+ matchea la hora, y otro espacio, dejándome justo al inicio del campo action. Ahí el grupo (DROP|REJECT) usa el operador de alternación | para matchear cualquiera de las dos palabras, y el espacio final asegura que sea la palabra completa del campo.

