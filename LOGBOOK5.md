# Trabalho realizado na Semana #5

## Task 1

- Com esta task aprendemos que é possível pôr o código binário de um programa (malicioso ou não) num buffer na stack e executá-lo. Neste caso nós temos uma shell code que executa uma shell, tal como é verificável na imagem abaixo. 

![img](/images/LOGBOOK5-1.png)  

## Task 2  

- Este programa lê 517 bytes de um ficheiro e copia-os para um buffer de apenas 100 bytes sem qualquer verificação. Como tal, os 417 bytes restantes podem ser escritos por cima do resto da stack (fora do espaço alocado para o buffer), mudando possivelmente outras variáveis locais, o endereço de retorno, os argumentos, o frame pointer, etc.
- Há que ter em conta que, aquando da compilação, são desativadas configurações relativas a proteção de stack e é definida a ownership do programa como root bem como a passagem deste para um SET-UID. 

## Task 3

- Nesta tarefa, o importante seria descobrir o offset entre a posição inicial do buffer e o local onde é guardado o endereço de retorno.
- Com o gdb, ao realizarmos debugging obtivemos os endereços do ebp e do buffer, cuja distância é de 108 bytes. O endereço de retorno encontra-se imediatamente a seguir ao ebp, pelo que será preciso considerar uns extra 4 bytes; ao todo, um offset de 112.
- Ao correr o programa com o gdb, há variáveis adicionais na stack e, portanto, o endereço que obtivemos do frame pointer terá um desfasamento ligeiro em relação a quando se corre o executável por si no terminal (o executável tem um valor superior).
- Com isto, a shell code é escrita o mais longe possível da stack e o endereço de retorno é, do mesmo modo, bastante maior, para serem maiores as chances de esta calhar na zona dos NOPs.
- Ao explorar esta vulnerabilidade foi possível correr uma shell de owner root.

![img](images/5_3_a.png)
![img](images/5_3_c.png)
![img](images/5_3_d.png)
