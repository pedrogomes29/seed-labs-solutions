# Trabalho realizado na Semana #5

## Task 1

- Com esta task aprendemos que é possível pôr o código binário de um programa (malicioso ou não) num buffer na stack e executá-lo. Neste caso nós temos uma shell code que executa uma shell, tal como é verificável na imagem abaixo. 

![img](/images/LOGBOOK5-1.png)  

## Task 2  

- Este programa lê 517 bytes de um ficheiro e copia-os para um buffer de apenas 100 bytes sem qualquer verificação. Como tal, os 417 bytes restantes podem ser escritos por cima do resto da stack (fora do espaço alocado para o buffer), mudando possivelmente outras variáveis locais, o endereço de retorno, os argumentos, o frame pointer, etc.
- Há que ter em conta que, aquando da compilação, são desativadas configurações relativas a proteção de stack e é definida a ownership do programa como root bem como a passagem deste para um SET-UID. 

## Task 3

- 
