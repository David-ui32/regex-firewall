## Task 1
Command: grep -Ec '^[0-9]' firewall.log
Result: 100000
Explanation: El ancla ^ obliga a que la coincidencia empiece al inicio de la línea, y la clase [0-9] exige que el primer carácter sea un dígito (el año de la fecha). Las 4 líneas de cabecera empiezan con # y no cumplen esto, así que quedan excluidas; grep -c cuenta las líneas que sí coinciden.
