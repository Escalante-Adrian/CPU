# Versiones
CPU 1.0: Exclusivamente ALU muy primitiva
CPU 2.0: ALU implementada en un prototipo incompleto de la CPU
CPU 3.0: Prototipo completado, CPU de 32 bits y programación secuencia de Fibonacci errónea
CPU 4.0: CPU32Bits completamente funcional y capaz de ejecutar la secuencia de Fibonacci
CPU 5.0: Adaptación de la CPU a 16 bits, con programación funcional de la secuencia de Fibonacci
CPU 6.0: CPU16Bits completamente funcional, optimizada y con enormes cambios estéticos, además de que habilita el uso de la memoria. Capaz de ejecutar la secuencia de Fibonacci y una prueba para verificar el uso de la memoria

# Instrucciones de ALU (Completa)
1. add - 0000 - Sumar
2. sub - 0001 - Restar
3. OR - 0010 - Compuerta OR
4. AND - 0011 - Compuerta AND
5. XOR - 0100 - Compuerta XOR
6. NAND - 0101 - Compuerta NAND
7. NOR - 0110 - Compuerta NOR
8. XNOR - 0111- Compuerta XNOR
**_ESTAS INSTRUCCIONES NO SIRVEN PARA LA ALU DEL CPU YA QUE USA UNA SIMPLIFICADA

# Estructura 16 BITS

**_TODOS USAN 3 OPCODE

### Tipo R

| op  | rs  | rt  | rd  | nada  | funct |
| --- | --- | --- | --- | ----- | ----- |
| 3   | 2   | 2   | 2   | 5     | 2     |
| 000 | 00  | 00  | 00  | 00000 | 00    |

0. opcode
1. opcode
2. opcode
3. reg rs
4. reg rs
5. reg rt
6. reg rt
7. reg rd
8. reg rd
9. nada
10. nada
11. nada
12. nada
13. nada
14. funct
15. funct

### Tipo I

| op  | rs  | rt  | nada | imm      |
| --- | --- | --- | ---- | -------- |
| 3   | 2   | 2   | 1    | 8        |
| 000 | 00  | 00  | 0    | 00000000 |

0. opcode
1. opcode
2. opcode
3. reg rs
4. reg rs
5. reg rt
6. reg rt
7. nada
8. imm
9. imm
10. imm
11. imm
12. imm
13. imm
14. imm
15. imm


### Tipo J

| op  | nada  | addr     |
| --- | ----- | -------- |
| 3   | 5     | 8        |
| 000 | 00000 | 00000000 |

0. opcode
1. opcode
2. opcode
3. nada
4. nada
5. nada
6. nada
7. nada
8. address
9. address
10. address
11. address
12. address
13. address
14. address
15. address


## Opcodes
000 - Tipo R
001 - BEQ
010 - Jump
011 - Addi
100 - LW
101 - SW
110 - NOP (No Operation)
111 - Restart
## Funct
AND - 00
OR - 01
SUMA - 10
RESTA - 11

Si ALUOP es 01: Toma Branch (Activa solo resta)
Si ALUOP es 10: Toma Tipo R
Si ALUOP es 11: Toma addi, lw y sw (Activa solo suma)
## Registros
0. A - 00 - Primer operando
1. B - 01 - Segundo operando
2. C - 10 - Tercer operando auxiliar
3. D - 11 - Vale 233, sirve para el beq
4. PC - N/A - Contador de programa
5. 
# Secuencia de Fibonacci
## Pseudo código MIPS
main:
a = 1
b = 1
d = 233

loop:
c = a + b
a = b
b = c
if (c == d) ----> restart
else PC = PC + 1
jump to loop

## Código C++

```cpp
#include <stdio.h>

int main(void) {
	int a = 0;
	int b = 1;
	int c = 0;
	
	printf("%d", a);
	printf("%d", b);
	
	for(int i = 2 ; i < 14 ; i++) { //Corta en 233
		c = a + b;
		printf("%d", c);
		a = b;
		b = c;
	}
	printf("/n");
}
```

## Código MIPS

```asm
main:
0   addi a, a, 0        # Se asigna el valor 0 a A
1   addi b, b, 1        # Se asigna el valor 1 a B
2   addi c, c, 0        # Se asigna el valor 0 a C
3   addi d, d, 233      # Se asigna el valor 233 a D

loop:
4   beq c, d, reiniciar # Si C es igual a D (233), salta a reiniciar
5   add c, a, b         # c = a + b
6   addi a, b, 0        # a = b // a = b + 0
7   addi b, c, 0        # b = c // b = c + 0
8   j loop              # Salta a la etiqueta loop

reiniciar:
9   restart             # Reinicia
```


restart: Llama un JUMP que usa los bits no utilizados, y en vez de saltar en el PC, activa el restart del PC y los registros con el 1110 0000 0000 0000

## Código a Lenguaje Maquina

main: 

| 00               | addi a, a, 0         |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 00 00 0 00000000 |
| Binario          | 0110000000000000     |
| Hex              | 6000                 |

| 01               | addi b, b, 1         |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 01 01 0 00000001 |
| Binario          | 0110101000000001     |
| Hex              | 6A01                 |

| 02               | addi c, c, 0         |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 10 10 0 00000000 |
| Binario          | 0111010000000000     |
| Hex              | 7400                 |

| 03               | addi d, d, 233       |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 11 11 0 11101001 |
| Binario          | 0111111011101001     |
| Hex              | 7EE9                 |

loop:

| 04               | beq c, d, reiniciar  |
| ---------------- | -------------------- |
| Lenguaje Maquina | 001 10 11 0 00000100 |
| Binario          | 0011011000000100     |
| Hex              | 3604                 |

| 05               | add c, a, b           |
| ---------------- | --------------------- |
| Lenguaje Maquina | 000 00 01 10 00000 10 |
| Binario          | 000 00 01 10 00000 10 |
| Hex              | 0302                  |


| 06               | addi a, b, 0         |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 00 01 0 00000000 |
| Binario          | 011 00 01 0 00000000 |
| Hex              | 6800                 |

| 07               | addi b, c, 0         |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 01 10 0 00000000 |
| Binario          | 011 01 10 0 00000000 |
| Hex              | 7200                 |

| 08               | j loop             |
| ---------------- | ------------------ |
| Lenguaje Maquina | 010 00000 00000100 |
| Binario          | 0100000000000100   |
| Hex              | 4004               |

reiniciar:

| 09               | restart           |
| ---------------- | ----------------- |
| Lenguaje Maquina | 111 0000000000000 |
| Binario          | 1110000000000000  |
| Hex              | E000              |

## Programa


| PC  | Instrucción         | Lenguaje Maquina |
| --- | ------------------- | ---------------- |
| 00  | addi a, a, 0        | 6000             |
| 01  | addi b, b, 1        | 6A01             |
| 02  | addi c, c, 0        | 7400             |
| 03  | addi d, d, 233      | 7EE9             |
| 04  | beq c, d, reiniciar | 3604             |
| 05  | add c, a, b         | 0302             |
| 06  | addi a, b, 0        | 6800             |
| 07  | addi b, c, 0        | 7200             |
| 08  | j loop              | 4004             |
| 09  | restart             | E000             |
Para copiar y pegar:
```a
6000 6A01 7400 7EE9 3604 0302 6800 7200
4004 E000
```


# Test de la Memoria
## Pseudo código MIPS
main:
Espacio 0: 32
Espacio 4: 68
a = Espacio 0 (32)
b = Espacio 4 (68)
d = a + b
Espacio 8: d
c = Espacio 8 (d)
restart

## Código C++

```cpp
#include <stdio.h>

int main(void) {
	int a = 32;
	int b = 68;
	int c = 0;
	int d = 0;
	
	printf("%d", a);
	printf("%d", b);
	
	d = a + b;
	c = d
	
	printf("%d", c);
}
```

# Código MIPS

## Código original

``` asm
.data
numbers: .word 32, 68          # reservo dos words y le doy valores

.text
.globl main
main:
  la        $t0, numbers       # guardo en $t0 la direccion de numbers
  lw        $t1, 0($t0)        # guardo en $t1 el 32
  lw        $t2, 4($t0)        # guardo en $t2 el 68
  add       $t1, $t1, $t2      # sumo $t1 = $t1 + $t2
  li        $v0, 1             # syscall print_int code
  move      $a0, $t1           # muevo el resultado ($t1) a $a0 para la syscall
  syscall                      # print_int syscall
  li        $v0, 10            # syscall exit code
  syscall                      # exit syscall
```

## Código adaptado y simplificado

``` asm
main:
0	addi a, a, 32        # A = 32
1	addi b, b, 68        # B = 68
2	add d, a, b          # D = A + B
3	sw d, 0(c)           # Guarda en el espacio 0 (c) el valor de D
4	sub d, d, d          # D = D - D (Establece en 0)
5	lw c, 0(d)           # Carga en c el valor del espacio 0 (d)
6   NOP                  # Delay para mostrar mas tiempo en pantalla
7   sw d, 0(d)           # Carga 0 en el valor del espacio 0
8	restart              # Reiniciar
```

## Código a Lenguaje Maquina

main:

| 00               | addi a, a, 32        |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 00 00 0 00100000 |
| Binario          | 0110000000100000     |
| Hex              | 6020                 |

| 01               | addi b, b, 68        |
| ---------------- | -------------------- |
| Lenguaje Maquina | 011 01 01 0 01000100 |
| Binario          | 0110101001000100     |
| Hex              | 6A44                 |

| 02               | add d, a, b           |
| ---------------- | --------------------- |
| Lenguaje Maquina | 000 00 01 11 00000 10 |
| Binario          | 0000001110000010      |
| Hex              | 0382                  |

| 03               | sw d, 0(c)           |
| ---------------- | -------------------- |
| Lenguaje Maquina | 101 10 11 0 00000000 |
| Binario          | 1011011000000000     |
| Hex              | B600                 |

| 04               | sub d, d, d           |
| ---------------- | --------------------- |
| Lenguaje Maquina | 000 11 11 11 00000 11 |
| Binario          | 0001111110000011      |
| Hex              | 1F83                  |

| 05               | lw c, 0(d)           |
| ---------------- | -------------------- |
| Lenguaje Maquina | 100 11 10 0 00000000 |
| Binario          | 1001110000000000     |
| Hex              | 9C00                 |

| 06               | NOP               |
| ---------------- | ----------------- |
| Lenguaje Maquina | 110 0000000000000 |
| Binario          | 1100000000000000  |
| Hex              | C000              |

| 07               | sw d, 0(d)           |
| ---------------- | -------------------- |
| Lenguaje Maquina | 101 11 11 0 00000000 |
| Binario          | 1011111000000000     |
| Hex              | BE00                 |

| 08               | restart           |
| ---------------- | ----------------- |
| Lenguaje Maquina | 111 0000000000000 |
| Binario          | 1110000000000000  |
| Hex              | E000              |
## Programa

| PC  | Instrucción   | Lenguaje Maquina |
| --- | ------------- | ---------------- |
| 00  | addi a, a, 32 | 6020             |
| 01  | addi b, b, 68 | 6A44             |
| 02  | add d, a, b   | 0382             |
| 03  | sw d, 0(c)    | B600             |
| 04  | sub d, d, d   | 1F83             |
| 05  | lw c, 0(d)    | 9C00             |
| 06  | NOP           | C000             |
| 07  | sw d, 0(d)    | BE00             |
| 08  | restart       | E000             |
Para copiar y pegar:
``` asm
6020 6A44 0382 B600 1F83 9C00 C000 BE00
E000
```
