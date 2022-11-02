# Trabalho Realizado na Semana #6

## Task 1

- O objetivo nesta task é fazer o programa crashar.

![input](images/w6/1_a.png)

- Nós enviamos o seguinte input "%s" para o server e conseguimos crashar o programa, como se vê no print a seguir.

![crash](images/w6/1_b.png)

- Isso acontece uma vez que o printf assume que tem como parâmetro um endereço de memória para o endereço base de uma string. No entanto, nenhum parâmetro é passado à função printf e, como tal, é lido um endereço fora do espaço virtual do processo, o que causa que o programa crashe.

## Task 2  

### 2.A

- 

![img]()

### 2.B 

- 

![img]()

## Task 3

- 

![img]()
