| Operación                     | Tipo de Formato | Op-code (binario) | Func-code (binario) | Descripción Detallada                                  |
| :---------------------------- | :-------------- | :---------------- | :------------------ | :----------------------------------------------------- |
| **Instrucciones Aritméticas** |                 |                   |                     |                                                        |
| add                           | R               | 000000            | 100000              | Suma los contenidos de Rs y Rt, almacena en Rd.        |
| sub                           | R               | 000000            | 100010              | Resta el contenido de Rt de Rs, almacena en Rd.        |
| mult                          | R               | 000000            | 011000              | Multiplica Rs y Rt, almacena resultado en Hi y Lo.     |
| mulu                          | R               | 000000            | 011001              | Multiplicación sin signo de Rs y Rt, en Hi y Lo.       |
| div                           | R               | 000000            | 011010              | Divide Rs por Rt, cociente en Lo, residuo en Hi.       |
| divu                          | R               | 000000            | 011011              | División sin signo de Rs por Rt, cociente en Lo, residuo en Hi. |
| addi                          | I               | 001000            | N/A                 | Suma inmediato a Rs, almacena en Rt.                   |
| **Instrucciones Lógicas**     |                 |                   |                     |                                                        |
| and                           | R               | 000000            | 100100              | AND lógico entre Rs y Rt, almacena en Rd.              |
| or                            | R               | 000000            | 100101              | OR lógico entre Rs y Rt, almacena en Rd.               |
| nor                           | R               | 000000            | 100111              | NOR lógico entre Rs y Rt, almacena en Rd.              |
| xor                           | R               | 000000            | 101000              | XOR lógico entre Rs y Rt, almacena en Rd.              |
| andi                          | I               | 001100            | N/A                 | AND lógico entre Rs y un inmediato, almacena en Rt.    |
| ori                           | I               | 001101            | N/A                 | OR lógico entre Rs y un inmediato, almacena en Rt.     |
| xori                          | I               | 001110            | N/A                 | XOR lógico entre Rs y un inmediato, almacena en Rt.    |
| **Instrucciones de Comparación** |                 |                   |                     |                                                        |
| slt                           | R               | 000000            | 101010              | Set if less than: si Rs < Rt, Rd = 1, sino Rd = 0.     |
| sltu                          | R               | 000000            | 101011              | Set if less than unsigned: si Rs < Rt, Rd = 1, sino Rd = 0. |
| slti                          | I               | 001010            | N/A                 | Set if less than immediate: si Rs < Inm, Rt = 1, sino Rt = 0. |
| sltiu                         | I               | 001011            | N/A                 | Set if less than immediate unsigned: si Rs < Inm, Rt = 1, sino Rt = 0. |
| **Instrucciones de Copia**    |                 |                   |                     |                                                        |
| mfhi                          | R               | 000000            | 010000              | Mueve contenido de Hi a Rd.                            |
| mflo                          | R               | 000000            | 010010              | Mueve contenido de Lo a Rd.                            |
| **Instrucciones de Salto**    |                 |                   |                     |                                                        |
| nop                           | R               | 000000            | 000000              | No operation (no hace nada).                           |
| beq                           | I               | 000100            | N/A                 | Branch if equal: salta si Rs == Rt.                    |
| bne                           | I               | 000101            | N/A                 | Branch if not equal: salta si Rs != Rt.                |
| blez                          | I               | 000110            | N/A                 | Branch if less than or equal zero: salta si Rs <= 0.   |
| bgtz                          | I               | 000111            | N/A                 | Branch if greater than zero: salta si Rs > 0.          |
| bltz                          | I               | 000001            | N/A                 | Branch if less than zero: salta si Rs < 0.             |
| j                             | J               | 000010            | N/A                 | Jump: salta a la dirección especificada.               |
| jr                            | R               | 000000            | 001000              | Jump register: salta a la dirección en Rs.             |
| **Instrucciones de Memoria**  |                 |                   |                     |                                                        |
| lw                            | I               | 100011            | N/A                 | Load word: carga una palabra de memoria a Rt.          |
| sw                            | I               | 101011            | N/A                 | Store word: almacena una palabra de Rt a memoria.      |
| pop                           | R               | 111000            | 000000              | Pop from stack: carga palabra del tope de pila a Rd.   |
| push                          | R               | 111000            | 000001              | Push to stack: almacena palabra de Rs en el tope de pila. |
| **Instrucciones Especiales**  |                 |                   |                     |                                                        |
| halt                          | R               | 111111            | 111111              | Detiene la simulación del programa.                    |
| tty                           | R               | 111111            | 000001              | Write to terminal: envía un caracter (de Rs) a la TTY. |
| rnd                           | R               | 111111            | 000010              | Set random: almacena un número aleatorio en Rd.        |
| kbd                           | R               | 111111            | 000100              | Set key: lee un caracter del teclado y lo almacena en Rd. |





| ALUOp (3 bits) | Func-code (6 bits, I5-I0) | ALUControl (4 bits) | Operación ALU                  | Contexto de Instrucción/Ciclo (Ejemplos)           |
| :------------- | :------------------------ | :------------------ | :----------------------------- | :------------------------------------------------- |
| **000**        | XXXXXX                    | **0000**            | ADD                            | Cálculo PC + 4 (fetch), Dir. memoria (base + offset para lw/sw), SP + 4 (pop), SP - 4 (push) |
| **001**        | XXXXXX                    | **0001**            | SUB                            | Comparación para ramas condicionales (beq, bne, blez, bgtz, bltz) |
| **010**        |                           |                     | *Tipo R (depende de Func-code)* |                                                    |
|                | 100000                    | **0000**            | ADD                            | `add`                                              |
|                | 100010                    | **0001**            | SUB                            | `sub`                                              |
|                | 100100                    | **0010**            | AND                            | `and`                                              |
|                | 100101                    | **0011**            | OR                             | `or`                                               |
|                | 100111                    | **0011**            | OR (o NOR si la ALU lo soporta) | `nor`                                              |
|                | 101000                    | **0100**            | XOR                            | `xor`                                              |
|                | 101010                    | **0101**            | SLT                            | `slt`                                              |
|                | 101011                    | **0101**            | SLT                            | `sltu`                                             |
|                | 011000                    | **XXXX**            | *Activa Mult. Unit*            | `mult`                                             |
|                | 011001                    | **XXXX**            | *Activa Mult. Unit*            | `mulu`                                             |
|                | 011010                    | **XXXX**            | *Activa Div. Unit*             | `div`                                              |
|                | 011011                    | **XXXX**            | *Activa Div. Unit*             | `divu`                                             |
|                | 010000                    | **0110**            | Passthrough A                  | `mfhi` (mover Hi a registro)                       |
|                | 010010                    | **0110**            | Passthrough A                  | `mflo` (mover Lo a registro)                       |
|                | 000000                    | **0000**            | ADD (o no-op real)             | `nop` (no operation)                               |
|                | 001000                    | **XXXX**            | *No usa ALU principal*         | `jr` (jump register)                               |
| **011**        | XXXXXX                    |                     | *Tipo I con inmediato*          |                                                    |
| *Op-code: 001000* | XXXXXX                 | **0000**            | ADD                            | `addi`                                             |
| *Op-code: 001100* | XXXXXX                 | **0010**            | AND                            | `andi`                                             |
| *Op-code: 001101* | XXXXXX                 | **0011**            | OR                             | `ori`                                              |
| *Op-code: 001110* | XXXXXX                 | **0100**            | XOR                            | `xori`                                             |
| *Op-code: 001010* | XXXXXX                 | **0101**            | SLT                            | `slti`                                             |
| *Op-code: 001011* | XXXXXX                 | **0101**            | SLT                            | `sltiu`                                            |
| **100**        | XXXXXX                    | *Podría ser 0001 (SUB)* | *Aritmética SP para pop/push* | Si `ALUOp=100` y `Func-code` indica `push` (000001) -> SUB; `pop` (000000) -> ADD. Esto requiere que `ALUOp` sea más específico o que el ALU Decoder vea el `Op-code` de `pop`/`push`. |
| **101**        | XXXXXX                    | **0000**            | ADD                            | `nop` (si se usa ALU para "nada")                  |
| **110**        | XXXXXX                    | **XXXX**            | *No usa ALU principal*         | `j` (jump), `jr` (si no se manejó en 010 Func-code) |
| **111**        | XXXXXX                    | **XXXX**            | *No usa ALU principal*         | `halt`, `tty`, `rnd`, `kbd`, `mult/div` (si no se manejaron antes) |

*Nota:* `XXXX` en `ALUControl` significa "no importa" o que la ALU principal no realiza la operación, sino otra unidad especializada que se activa con otras señales de control de la Unidad de Control principal. Para `nor`, si tu ALU no tiene operación NOR directa, podrías usar `OR` y luego un inversor de la señal, o bien usar un `ALUOp` más granular para "NOR".

---

### Consejos para hacer el ALU Decoder en Logisim

1.  **Modularidad:**
    *   **Subcircuito para el ALU Decoder:** Crea un subcircuito dedicado para tu ALU Decoder. Tendrá como entradas `ALUOp` (3 bits) y `Func-code` (6 bits), y como salida `ALUControl` (4 bits).
    *   **Unidad de Control Principal:** La lógica que genera `ALUOp` y otras señales de control (`MemRead`, `RegWrite`, `MemtoReg`, etc.) debe estar en la Unidad de Control principal.

2.  **Entradas y Salidas en Logisim:**
    *   Usa **pines de entrada** para `ALUOp` (un pin de 3 bits) y `Func-code` (un pin de 6 bits).
    *   Usa un **pin de salida** para `ALUControl` (un pin de 4 bits).

3.  **Implementación de la Lógica:**

    *   **Combinacional:** El ALU Decoder es un circuito puramente combinacional. No necesita flip-flops ni elementos de estado.
    *   **Tablas de Verdad (Logisim):** Para un número relativamente pequeño de entradas (como `Func-code` cuando `ALUOp` es fijo), puedes usar el componente "Combinational Analysis" de Logisim para generar la lógica automáticamente. Sin embargo, para 9 entradas (`ALUOp` + `Func-code`), esto puede ser pesado.

    *   **Decodificadores y Multiplexores (Método recomendado):** Esta es la forma más "limpia" y comprensible para el ALU Decoder en un multiciclo.
        *   **Decodificador para `ALUOp`:** Usa un decodificador (ej. 3 a 8) para las señales `ALUOp`. Cada salida del decodificador se activará para un valor específico de `ALUOp`.
        *   **Múltiples ALU Decoders "pequeños":**
            *   Cuando `ALUOp = 000` (ADD para direcciones/PC): La salida `ALUControl` es fija `0000`. Conecta la salida del decodificador `ALUOp=000` a una serie de compuertas AND con las constantes `0000` y luego a un OR final.
            *   Cuando `ALUOp = 001` (SUB para ramas): La salida `ALUControl` es fija `0001`.
            *   **Cuando `ALUOp = 010` (Tipo R):** Aquí es donde necesitas el `Func-code`.
                *   Crea un subcircuito pequeño que tome solo el `Func-code` (6 bits) como entrada y produzca las 4 bits de `ALUControl` para las operaciones Tipo R (`add`, `sub`, `and`, etc.). Este subcircuito lo puedes hacer con "Combinational Analysis" o compuertas lógicas manuales si quieres simplificar.
                *   Conecta la salida `ALUOp=010` del decodificador a la habilitación (enable) de este subcircuito "Tipo R". La salida de este subcircuito se conecta a un multiplexor que selecciona la `ALUControl` final.
            *   **Para otras `ALUOp`:** Aplica lógica similar, ya sea valores fijos o decodificación de más bits si el `ALUOp` es muy granular y necesitas más bits del Op-code.
        *   **Multiplexor final:** Usa un multiplexor (o una red de compuertas OR) para combinar las salidas de los diferentes decodificadores/lógicas "pequeñas" en la salida final de `ALUControl`. El `ALUOp` puede ser directamente la señal de selección del multiplexor si lo diseñas así.

4.  **Consideraciones para `mult`, `div`, `halt`, `tty`, `rnd`, `kbd`, `j`, `jr`:**
    *   Estas instrucciones a menudo no utilizan la ALU principal para su función principal.
    *   Para `mult`/`div`, el `ALUControl` podría ser `XXXX` (no relevante), y la Unidad de Control principal enviaría señales a una unidad de multiplicación/división separada para habilitarlas.
    *   Para `halt`, `tty`, `rnd`, `kbd`, `j`, `jr`, la ALU principal podría estar inactiva (`XXXX`) o realizar una `ADD` genérica (como `nop`) mientras otras partes del procesador se encargan de la función. **Lo importante es que el ALU Decoder NO emita una señal de `ALUControl` que haga algo incorrecto.**

5.  **Pruebas exhaustivas:**
    *   Después de construir el subcircuito, pruébalo con cada combinación de `ALUOp` y `Func-code` relevante. Asegúrate de que para `ALUOp = 010`, las salidas cambian correctamente con el `Func-code`.
    *   Asegúrate de que para `ALUOp` que no dependen de `Func-code`, la salida es constante independientemente de los valores de `Func-code`.

6.  **Unidad de Control Multiciclo:**
    *   El **núcleo del diseño multiciclo es la Unidad de Control principal**. Utilizará un "contador de estado" (por ejemplo, un registro de 3-4 bits) para llevar la cuenta del ciclo actual (Fetch, Decode, Execute, Memory, Writeback).
    *   La Unidad de Control tomará el `Op-code` de la instrucción y el **estado actual** para generar **todas las señales de control** necesarias para ese ciclo, incluyendo el `ALUOp` para el ALU Decoder. Esto significa que la lógica de la Unidad de Control principal será una máquina de estados finitos.

**Ejemplo de cómo la Unidad de Control principal podría generar `ALUOp` en diferentes estados:**

| Estado (ciclo) | Op-code       | Señal de control `ALUOp` (para el ALU Decoder) |
| :------------- | :------------ | :--------------------------------------------- |
| State 0 (Fetch) | XXXXXX        | `000` (para PC = PC + 4)                       |
| State 1 (Decode) | XXXXXX       | `XXXX` (ALU no se usa en este estado)         |
| State 2 (Execute) | `000000` (Tipo R) | `010` (Tipo R, Func-code relevante)          |
| State 2 (Execute) | `001000` (addi) | `011` (Tipo I inmediato)                     |
| State 2 (Execute) | `000100` (beq) | `001` (para comparación SUB)                 |
| State 3 (Memory) | `100011` (lw) | `000` (para cálculo de dirección)              |
| State 3 (Memory) | `101011` (sw) | `000` (para cálculo de dirección)              |
| State 4 (Writeback)| XXXXXX     | `XXXX` (ALU no se usa típicamente)            |
| ...             | ...           | ...                                            |

Este enfoque hará que tu ALU Decoder sea limpio y la Unidad de Control principal maneje la complejidad secuencial. ¡Ánimo con el proyecto en Logisim!

+------------------+
 Opcode --|                  |
(6 bits)  |                  |--> RegDest
          |                  |--> MemRead
   Zero --|   MAIN           |--> MemWrite
  (1 bit) |   CONTROL        |--> ALUOp (3 bits)
          |   UNIT           |--> ... (Resto de salidas)
   Sign --|   (FSM)          |
  (1 bit) |                  |
          |                  |
MemReady--|                  |
  (1 bit) |                  |
          +------------------+
             ^            ^
             |            |
            CLK         Reset