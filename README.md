# ARM 7

**SPANISH/ESPAÑOL**

## Índice
* [Get Started](#get-started)
* [Instrucciones](#instrucciones)
  * [Mover](#mover)
    * [mov](#mov)
  * [Sumar/Restar](#sumarrestar)
    * [add](#add)
    * [sub](#sub)
  * [Multiplicar/Dividir](#multiplicardividir)
    * [mul](#mul)
  * [Comparar](#comparar)
    * [cmp](#cmp)
  * [Saltos](#saltos)
    * [bl](#bl)
  * [Desplazar](#desplazar)
    * [lsr](#lsr)
    * [lsl](#lsl)
    * [asr](#asr)
  * [Load/Store](#loadstore)
    * [ldr](#ldr)
    * [ldrsb](#ldrsb)
    * [str](#str)
    * [push](#push)
    * [pop](#pop)
* [Prácticas](#prácticas)
  * [1. Básico](#1-básico)
  * [2. Suma](#2-suma)
    * [2.1. Usando registros](#21-usando-registros)
    * [2.2. Usando memoria](#22-usando-memoria)
    * [2.3. Usando memoria y registros](#23-usando-memoria-y-registros)
  * [3. Condicionales](#3-condicionales)
    * [Saltos condicionales](#saltos-condicionales-`b`)
    * [3.1. Ej 1](#31-ej-1)
    * [3.2. Ej 2](#32-ej-2)
  * [4. Desplazamientos](#4-desplazamientos)
    * [4.1. Invertir/intercambiar 2 números en memoria](#41-invertirintercambiar-2-números-en-memoria)
    * [4.2. Invertir bytes de un número](#42-invertir-bytes-de-un-numero)
  * [5. División](#5-división)
  * [6. Funciones](#6-funciones)
    * [6.1. Comprobación primo](#61-comprobación-primo)
  * [7. Direccionamiento](#7-direccionamiento)
    * [Modos de direccionamiento](#modos-de-direccionamiento)
  * [8. Llamada a funciones de C desde ASM](#8-llamada-a-funciones-de-c-desde-asm)
    * [8.1. Ej 1](#81-ej-1)
    * [8.2. Ej 2](#82-ej-2)
  * [9. Arrays](#9-arrays)
    * [9.1. Ej 1](#91-ej-1)
    * [9.2. Sumar elementos vector](#92-sumar-elementos-vector)

## Get started

### Comprobación de red

```bash
ping 192.168.29.100
```

### Comprobación de Raspberry

```bash
ping 192.168.29.214 (14)
```

### Abrir Putty:

```text
192.168.29.214
Port: 22
OPEN
```

```bash
login as: root
password: root
```

### Cambiar fecha Raspberry

(en Putty)

```bash
date
man date
date [MMDDhhmm[[CC]YY][.ss]]
date 0916182622    (09/16 18:26 22)
```

### Crear proyecto NetBeans

```text
New project > C/C++ Aplication
Sin main.c
```





## Instrucciones

```asm
r3 << 3 == r3 * 2^3
r3 >> 3 == r3 / 2^3

00000111 << 2 : 00011100 (7*4  = 28)
00000111 >> 2 : 00000001 (7//4 = 1)
```


### Mover

#### mov

| Ejemplo | Descripción |
|--------|-----------|
| `mov r0, #1` | r0 = 1 |
| `mov r0, r1` | r0 = r1 |
| `mov r0, r1, LSL #3` | r0 = r1 * 2^3 |
| `mov r0, r1, LSR #4` | r0 = r1 / 2^5 |
| `mov r0, r1, ASR #5` | r0 = r1 / 2^5 |



### Sumar/Restar

#### add

| Ejemplo | Descripción |
|-------|-----------|
| `add r1, #2` | r1 += 2 |
| `add r1, r2` | r1 += r2 |
| `add r1, r2, r3` | r1 += r2 + r3 |
| `add r1, r2, #3` | r1 += r2 + 3 |
| `add r1, r2, r3, lsl #3` | r1 += r2 + (r3 << 3) |
| `add r1, r2, r3, lsr r4` | r1 += r2 + (r3 >> r4) |

#### sub
| Ejemplo | Descripción |
|-------|-----------|
| `sub r1, #2` | r1 -= 2 |
| `sub r1, r2` | r1 -= r2 |
| `sub r1, r2, r3` | r1 -= r2 - r3 |
| `sub r1, r2, #3` | r1 -= r2 - 3 |
| `sub r1, r2, r3, lsl #3` | r1 -= r2 - (r3 << 3) |
| `sub r1, r2, r3, lsr r4` | r1 -= r2 - (r3 >> r4) |



### Multiplicar/Dividir
#### mul

| Ejemplo | Descripción |
|-------|-----------|
| `mul r1, r2` | r1 *= r2 |
| `mul r1, r2, r3` | r1 = r2 * r3 |



### Comparar

#### cmp

| Ejemplo | Descripción |
|-------|-----------|
| `cmp r1, #2` | r1 cmp 2 |
| `cmp r1, r2` | r1 cmp r2 |
| `cmo r1, r2, lsl #3` | r1 cmp r2 << 3 |



### Saltos

#### bl

* Para llamar funciones
* Si al final de la función no hay un `bx lr`, se vuelve a la dirección de la instrucción siguiente a la llamada

| Ejemplo | Descripción |
|-------|-----------|
| `bl func` | bl func |



### Desplazar

#### lsr

| Ejemplo | Descripción |
|-------|-----------|
| `lsr r1, #3` | r1 >> 3  == r1 / 2^3 |
| `lsr r1, r2` | r1 >> r2 |

#### lsl

| Ejemplo | Descripción |
|-------|-----------|
| `lsl r1, #3` | r1 << 3 == r1 * 2^3 |
| `lsl r1, r2` | r1 << r2 |

#### asr

Takes into account the sign of the number

| Ejemplo | Descripción |
|-------|-----------|
| `asr r1, #3` | r1 >> 3 |
| `asr r1, r2` | r1 >> r2 |



### Load/Store

```asm
.data
    var1: .word 8
```

#### ldr

Guardar el valor de `var1` en `r1`
```asm
ldr r0, =var1    // r0 <- &var1
ldr r1, [r0]    // r1 <- var1
```
ldr r2, [r1], +r3, LSL #2 // r2 <- *(r1+r3*4)
| Ejemplo | Descripción |
|-------|-----------|
| `ldr r1, =#2` | r1 = 2 |
| `ldr r1, =var1` | r1 = &var1 (Se guarda en r1 la dirección de var1) |
| `ldr r1, [r1]` | r1 = *r1 (Se guarda en r1 el valor de la dirección que hay en r1) |
| `ldr r1, [r1, #20]` | r1 = *(r1 + 4) (Cargar valor de 5 direcciones después de r1)|
| `ldr r1, [r1, r2]` | r1 = *(r1 + r2) |
| `ldr r1, [r1, r2, lsl #2]` | r1 = *(r1 + (r2 << 2)) |
| `ldr r1, [r1, #4]!` | r1 += 4 ; Después, r1 = *(r1) |
| `ldr r1, [r1, r2]!` | r1 += r2 ; Después, r1 = *(r1) |
| `ldr r1, [r1, r2, lsr #2]!` | r1 += (r2 >> 2) ; Después, r1 = *(r1) |
| `ldr r1, [r1], #4` | r1 = *(r1) ; Después, r1 += 4 |
| `ldr r1, [r1], r2` | r1 = *(r1) ; Después, r1 += r2 |
| `ldr r1, [r1], r2, lsr #2` | r1 = *(r1) ; Después, r1 += (r2 >> 2) |

#### ldrsb

* Guardar un byte con signo en un registro de 32 bits (el resto igual a ldr)

#### str

Guardar el valor de `r1` en `var1`

```asm
ldr r0, =var1    // r0 <- &var1
str r5, [r0]    // var1 <- r5
```

| Ejemplo | Descripción |
|-------|-----------|
| `str r1, [r2]` | *r2 = r1 (Se guarda en la dirección que hay en r2 el valor de r1) |
| `str r1, [r2, #20]` | *(r2 + 4) = r1  (Cargar valor de 5 direcciones después de r1)|
| `str r1, [r2, r3]` | *(r2 + r3) = r1 |
| `str r1, [r2, r3, ls3 #2]` | *(r2 + (r3 >> 2)) = r1 |
| `str r1, [r2, #4]!` | r2 += 4 ; Después, *(r2) = r1 |
| `str r1, [r2, r3]!` | r2 += r3 ; Después, *(r2) = r1 |
| `str r1, [r2, r3, lsl #2]!` | r2 += (r3 << 2) ; Después, *(r2) = r1 |
| `str r1, [r2], #4` | *(r2) = r1 ; Después, r2 += 4 |
| `str r1, [r2], r3` | *(r2) = r1 ; Después, r2 += r3 |
| `str r1, [r2], r3, lsl #2` | *(r2) = r1 ; Después, r2 += (r3 << 2) |

#### push

| Ejemplo | Descripción |
|-------|-----------|
| `push {r1, r2, r3}` | Se guarda en la pila los valores de r1, r2 y r3 |

#### pop

| Ejemplo | Descripción |
|-------|-----------|
| `pop {r1, r2, r3}` | Se sacan de la pila los valores de r1, r2 y r3 |



## Prácticas

### 1. Básico

```r
r0 <- 2
```

```asm
.global main /* 'main' es nuestro punto de entrada y debe ser global */
main:
    mov r0, #2 // r0 <- 2
    bx lr      // Retorno de main
```



### 2. Suma

#### 2.1. Usando registros
```r
r0 <- r1 + r2
```

Instrucciones: `add`

```asm
.global main
main:
    // r1 <- 3, r2 <- 4
    mov r1, #3
    mov r2, #4

    // Los sumamos y lo guardamos en r0
    add r0, r1, r2

    bx lr
```



#### 2.2. Usando memoria

**Este es muy raro, creo que no hay necesidad de hacerlo así**

```r
var3 <- var1 + var2
```

```asm
.data
    var1 : .word 3
    var2 : .word 4
    var3 : .word 0x1234

.text
.global main
main:
    // r1 <- var1, r2 <- var2
    ldr r1, puntero_var1    // r1 <- &var1
    ldr r1, [r1]            // r1 <- *r1
    ldr r2, puntero_var2    // r2 <- &var2
    ldr r2, [r2]            // r2 <- *r2

    // r0 <- r1, r2
    add r0, r1, r2          // r0 <- r1 + r2

    // var3 <- r0
    ldr r3, puntero_var3    // r3 <- &var3
    str r0, [r3]            // *r3 <- r0

    bx lr

puntero_var1 : .word var1
puntero_var2 : .word var2
puntero_var3 : .word var3
```


#### 2.3. Usando memoria y registros

```r
r0 <- var1 + var2
```

```asm
.data
    var1 : .byte 0b00110010
    var2 : .byte 0b11000000

.text
.global main
main:
push {lr}

    // r1 <- var1, r2 <- var2
    ldr r1, =var1   // r1 <- &var1
    ldrsb r1, [r1]  // r1 <- *r1
    ldr r2, =var2   // r2 <- &var2
    ldrsb r2, [r2]  // r2 <- *r2

    // r0 <- r1, r2
    add r0, r1, r2  // r0 <- r1 + r2

pop {lr}
bx lr
```





### 3. Condicionales

#### Saltos condicionales (`b..`)

| Instrucción | Descripción |
| --- | --- |
| b | |
| beq | == |
| bne | != |
| bge | >= |
| bgt/bhi | > |
| ble | <= |
| blt | < |
| bpl | >= 0 |
| bmi | < 0 |

#### 3.1. Ej 1

```c
int a, b;

if (a == b) // ...entonces...
else // ...sino...
```

```asm
.data
    a : .word 3
    b : .word 4

.text
.global main
main:
    // r1 <- a, r2 <- b
    ldr r1, =a
    ldr r1, [r1]
    ldr r2, =b
    ldr r2, [r2]

    cmp r1, r2      // r1 - r2
    bne entonces        // if (r1 == r2) goto entonces
    // ...sino...

entonces:
    // ...entonces...
```
 

#### 3.2. Ej 2

```c
int val_inicial, val_final, incr, i;

for (i = val_inicial; i <= val_final, i+=inc)
    // ...bucle...

/* Que es igual que */
i = val_inicial;
while ( i <= val_final )
    // ...bucle...
    i += inc;
```

```asm
.data
    val_inicial : .word 11
    val_final : .word 7
    inc : .word 2

.text
.global main
main:
    // r1 <- val_inicial, r2 <- val_final, r3 <- inc
    ldr r1, =val_inicial
    ldr r1, [r1]
    ldr r2, =val_final
    ldr r2, [r2]
    ldr r3, =inc
    ldr r3, [r3]

bucle: cmp r1, r2
bhi fin_bucle
    add r1, r1, r3
b bucle

fin_bucle:
```





### 4. Desplazamientos

#### 4.1. Invertir/intercambiar 2 números en memoria

```text
Invertir los contenidos de los 2 números enteros en memoria
```

**(HECHO POR MI)**

```asm
.data
    n1 : .word 7
    n2 : .word 4

.text
.global main
main:
push {lr}
    // r0 <- n1, r1 <- n2
    ldr r0, =n1
    ldr r0, [r0]
    ldr r1, =n2
    ldr r1, [r1]

    // r2 <-> r0
    mov r2, r0
    mov r0, r1
    mov r1, r2
	
    // n1 <- r0, n2 <- r1
    ldr r2, =n1
    str r0, [r2]
    ldr r2, =n2
    str r1, [r2]

pop {lr} 		    // Se devuelve el valor original de lr
bx lr			    // Fin
```


#### 4.2. Invertir bytes de un número

```text
Invertir los bytes del primer número en memoria y almacenarlo en otra posición de memoria
```

**(HECHO POR MI)**

```asm
.data
    num : .word 0x12345678
    invertido: .word -1

.text
.global main
main:               // main es la función principal
push {lr} 		    // Se guarda lr en pila
    ldr r0, =num 	// Cargar la dirección de num de memoria
    ldr r0, [r0]	// Cargar el contenido de r0 (num)

    // Byte 4 -> Pos 1 (0x12345678 -> 0x78000000)
	lsl r1, r0, #24
	
    // Byte 3 -> Pos 2
	lsl r2, r0, #16
	lsr r2, r2, #24
	lsl r2, r2, #16
	
    // Byte 2 -> Pos 3
	lsl r3, r0, #8
	lsr r3, r3, #24
	lsl r3, r3, #8
	
    // Byte 1 -> Pos 4
	lsr r4, r0, #24
	
    // Sumar los 4 bytes en r5
	add r5, r4, r3
	add r5, r5, r2
	add r5, r5, r1
	
    // Guarda el resultado en invertido
	ldr r0, =invertido  // r0 <- &invertido
    str r5, [r0]        // invertido <- r5

pop {lr} 		// Se devuelve el valor original de lr
bx lr			// Fin
```




### 5. División

* Algoritmo que calcule el **cociente** y el **resto** da división de 2 posiciones de memoria **DIVIDENDO** y **DIVISOR**. 
* El divisor nunca deberá será 0.
* Comparar la velocidad de ejecución de este código con una versión optimizada.

**(HECHO POR MI)**

```text
14 / 3
c = 0 
r = 14
```

```asm
/*
    r1 = dividendo
    r2 = divisor
    r3 = cociente
    r4 = resto
*/
.text
.global division
division:
    mov r3, #0          // cociente <- 0
    mov r4, r1          // resto <- dividendo

bucle: 
    cmp r4, r2          // if (resto < divisor) goto fin_bucle
        blt fin_bucle
    sub r4, r2          // resto -= divisor
    add r3, #1          // cociente++
    b loop              // goto loop

fin_bucle:
    bx lr
```





### 6. Funciones

#### 6.1. Comprobación primo

```text
Verificar si un número N es primo, o sea, que el resto de la división por
todos los antecesores sea 0.
```

```asm
/* 
Check primo 
    r1 = n (dividendo)
    r2 = 2 (divisor, índice)
    r5 = resultado (0 - 1)
*/
.data
n : .word 11

.text
.global main
main:
    push {lr}
    ldr r1, =n
    ldr r1, [r1]

    mov r2, #2
    cmp r1,r2       // n cmp 2
    blt fin_false    // n < 2
    beq fin_true   // n == 2

    bl division
    cmp r4, #0      // resto cmp 0
    beq fin_false   // n % 2 == 0

    mov r0, r1      // r0 = n
    bl raiz         // sqrt(n)
    add r0, #1      // n++ (n = sqrt(n))
    mov r2, #3      // i = 3

bucle1:
    cmp r2, r0      // i cmp sqrt(n)
    bge fin_true    // i >= sqrt(n)
    bl division
    cmp r4, #0      // resto cmp 0
    beq fin_false   // n % 2 == 0
    add r2, #2      // i += 2
    b bucle1

fin_true:
    mov r5, #1
    b fin
fin_false:
    mov r5, #0
fin:
    bx lr



/* DIVISION */
division:
    push {lr}
    mov r3, #0
    mov r4, r1

bucle_div: cmp r4, r2
    blt fin_bucle_div
    sub r4, r2      // resto -= divisor
    add r3, #1      // cociente++
    b bucle_div

fin_bucle_div:
    pop {lr}
    bx lr
/* FIN DIVISION */



/* RAIZ */
raiz:
    push {r10,r11,r12,lr}
    mov r10,#1    // i=1

bucle_raiz:
    mul r11, r10, r10    /* i*i */
    cmp r11,r0
    beq fin_bucle_raiz

    subgt r10,#1
    bgt fin_bucle_raiz

    add r10,#1   // i--
    b bucle_raiz

fin_bucle_raiz: 
    mov r0,r10
    pop {r10,r11,r12,lr}
    bx lr
/* FIN RAIZ */
```





### 7. Direccionamiento

#### Modos de direccionamiento

##### Direccionamiento inmediato
* El operando es una constante
```asm
mov r0, #1
add r2, r3, #4
```

##### Direccionamiento inmediato con rotación
* Igual, pero con operaciones intermedias
```asm
mov r1, r2, LSL #1  // r1 <- (r2 * 2)
mov r1, r2, LSR #2  // r1 <- (r2 / 4)
mov r1, r3, ASR #3  // r1 <- (r3 / 8)
```

##### Direccionamiento a memoria
* Sin actualizar registro puntero
* Útil cuando la distancia en bytes es fija
* Distancia máxima: 12 bits (0..4096)
* **1 palabra = 4 bytes**

**a)** [Rx, #+inmediato] / [Rx, #-inmediato]
```asm
// a[3] = 1
ldr r1, =a
mov r2, #1          // r2 <- 1
str r2, [r1, #+12]  // *(r1+12) <- r2
```

**b)** [Rx, +Ry] / [Rx, -Ry]
```asm
// a[3] = 1
ldr r1, =a
mov r2, #1          // r2 <- 1
mov r3, #12         // r3 <- 12
str r2, [r1, +r3]   // *(r1+r3) <- r2
```

**c)** [Rx, +Ry, operac_despl #inmediato] / [Rx, -Ry, operac_despl #inmediato]
* Para multiplicar un valor fijo a la dirección. 4 bytes = 1 word = 32 bits
```asm
// a[3] = 1
ldr r1, =a
mov r2, #1                  // r2 <- 1
mov r3, #3                  // r3 <- 3
str r2, [r1, +r3, LSL #2]   // *(r1+r3*4) <- r2
```

##### Direccionamiento a memoria, actualizando registro puntero

###### Postindexados: Actualizan registro después de ejecutar la instrucción

**a)** [Rx], #+inmediato / [Rx], #-inmediato
* Poner a 0 los 3 primeros elementos a[0], a[1], a[2] del array
```asm
mov r2, #0          // r2 <- 0
str r2, [r1], #+4   // a[0] <- r2
str r2, [r1], #+4   // a[1] <- r2
str r2, [r1], #+4   // a[2] <- r2
```

**b)** [Rx], +Ry / [Rx], -Ry
* Igual, pero con registro en lugar de inmediato
```asm
mov r2, #0          // r2 <- 0
mov r3, #4          // r3 <- 4
str r2, [r1], +r3   // a[0] <- r2
```

**c)** [Rx], +Ry, operac_despl #inmediato / [Rx], -Ry, operac_despl #inmediato
```asm
ldr r2, [r1], +r3, LSL #2 // r2 <- *(r1+r3*4)
```
Otra forma
```asm
ldr r2, [r1]
add r1, r1, r3, LSL #2
```

###### Preindexados: Actualizan registro antes de ejecutar la instrucción

* Para diferenciarlos de los casso que no actualiza el registro le añadimos un ! al final

**a)** [Rx, #+inmediato]! / [Rx, #-inmediato]!
* Útil si queremos reusar en una futura instrucción la direccion que hemos calculado.
* En este ejemplo, duplicamos el valor que se encuentra en a[3]
```asm
// a[3] = a[3] + a[3]
ldr r2, [r1, #+12]!     // r2 <- r1 + 12 then r2 <- *r1
add r2, r2, r2          // r2 <- r2 + r2
str r2, [r1]            // *r1 <- r2
```

**b)** [Rx, +Ry]! / [Rx, -Ry]!
```asm
ldr r2, [r1, r3]!       // r2 <- r1 + r3 then r2 <- *r1
```

**c)** [Rx, +Ry, operac_despl #inmediato]! / [Rx, -Ry, operac_despl #inmediato]!
```asm
ldr r2, [r1, +r3, LSL #2]!  // r1 <- r1 + r3*4 then r2 <- *r1
```





### 8. Llamada a funciones de C desde ASM

#### 8.1. Ej 1

Función en C
```c
# include <stdio.h>
void main(void) {
    int i;
    for (i = 0; i < 5; i++) {
        printf(" %d\n", i);
    }
}
```

```asm
.data
    var1 : .asciz " %d\n"

.text
.global printf
.global main
main: push {r4, lr}
    mov r4, #0

.L2:
    mov r1, r4
    ldr r0, =var1
    bl printf       // formato (r0), parámetros (r1)

    add r4, r4, #1
    cmp r4, #5
    bne .L2

pop {r4, pc}
```


#### 8.2. Ej 2

```c
# include <stdio.h>
void main(void) {
    int num;
    printf("Hola, teclea un número");
    scanf("%d", &num);
    printf("El número leído es: %d\n", num);
    return num;
}
```

```asm
.data
    message1 : .asciz "Hola, teclea un número: "
    message2 : .asciz "El número leído es: %d\n"

    // Patrón de formato para scanf (se usa para leer un entero)
    scan pattern : .asciz "%d"

    // Donde scanf guardará el número leído
    number_read : .word 0

    retorno : .word 0

.text

// Externos
.global printf
.global scanf

.global main
main:
    // r1 <- lr
    ldr r1, =retorno        // r1 <- &retorno
    str lr, [r1]            // *r1 <- lr

    ldr r0, =message1       // r0 <- &message1
    bl printf               // llamada a printf (formato r0)

    ldr r0, =scan_pattern   // r0 <- &scan_pattern
    ldr r1, =number_read    // r1 <- &number_read
    bl scanf                // llamada a scanf (formato r0, parámetros r1)

    ldr r0, =message2       // r0 <- &message2
    ldr r1, =number_read    // r1 <- &number_read
    ldr r1, [r1]            // r1 <- *r1
    bl printf               // llamada a scanf (formato r0, parámetros r1)

    ldr r0, =number_read    // r0 <- &number_read
    ldr r0, [r0]            // r0 <- *r0

    ldr lr, =retorno        // lr <- &retorno
    ldr lr, [lr]            // lr <- *lr
    bx lr                   // return
```





### 9. Arrays

#### 9.1. Ej 1

```c
for (i = 0; i < 100; i++)
    a[i] = i;
```

```asm
.data
    .balign 4       // Avanzar el contador hasta que es un múltiplo de 4
    a : .skip 400   // Reserva 400 bytes

.text
.global main
main:
    ldr r1, =a      // r1 <- &a
    mov r2, #0      // r2 <- 0

loop:
    cmp r2, #100            // Se ha llegado a 100?
    beq end                 // Entonces, sal del loop
                            // Si no, sigue
    add r3, r1, r2, LSL #2  // r3 <- r1 + (r2 * 4)
    str r2, [r3]            // *r3 <- r2
    add r2, r2, #1          // r2 <- r2 + 1
    b loop                  // Repetir loop

end:
    bx lr
```


#### 9.2. Sumar elementos vector

```c
# include <stdio.h>

void main(void) {
    int i, suma;
    int vector[5] = {128, 32, 100, -30, 124};
    for (suma = i = 0; i < 5; i++) { // inicializar a 0 suma e i
        suma += vector[i]
    }
    printf("La suma es %d", suma)
}
```

```asm
.data
    var1 : .asciz "La suma es %d\n"
    var2 : .word 128, 32, 100, -30, 124

.text
.global main
main: push {r4, lr}
    // Inicializamos variables
    mov r0, #5
    mov r1, #0

    ldr r2, =var2       // r2 <- &var2

bucle: // Bucle que hace la suma
    ldr r3, [r2], #4    // r3 <- *r2  then r2 <- r2 + 4
    add r1, r1, r3      // r1 <- r1 + r3
    subs r0, r0, #1     // r0 <- r0 - 1
    bne bucle           // si r0 != 0 goto bucle

// Imprimir resultado
ldr r0, =var1
b1 printf               // r1 contiene el valor a imprimir

// Recuperamos registros y salimos
pop {r4, lr}
bx lr
```
