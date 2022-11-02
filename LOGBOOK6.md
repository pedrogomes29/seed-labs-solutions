# Trabalho Realizado na Semana #6

## Task 1

- O objetivo nesta task é fazer o programa crashar.

![input](images/w6/1_a.png)

- Nós enviamos o seguinte input "%s" para o server e conseguimos crashar o programa, como se vê no print a seguir.

![crash](images/w6/1_b.png)

- Isso acontece uma vez que o printf assume que tem como parâmetro um endereço de memória para o endereço base de uma string. No entanto, nenhum parâmetro é passado à função printf e, como tal, é lido um endereço fora do espaço virtual do processo, o que causa que o programa crashe.

## Task 2  

### 2.A

- Por tentativa erro, fomos adicionando %x até ao output conter no seu final "41414141" que corresponde a 4 'A's representados em ASCII, em hexadecimal. Concluimos que são necessário 64 %x.
![input](images/w6/2_a_1.png)
![output](images/w6/2_a_2.png)

### 2.B 

- Sabendo que é necessário um offset de 63 '%x' para chegar ao início do nosso input, se no início colocarmos um endereço de memória e depois dos 63 '%x' colocarmos um '%s', a string vai ser lida do endereço que está no início do nosso input e, assim, o output vai ser a string nesse endereço. Assim, basta colocar o endereço fornecido pelo servidor no início do input, seguido de 63 '%x' e de um '%s' (como foi feito no script python)

![input](images/w6/2_b_1.png)
![output](images/w6/2_b_2.png)

## Task 3
### 3.A

- Desta vez, queremos que o endereço lido para o '%n' seja o endereço no início do nosso input, correspondente ao endereço da variável target fornecido pelo servidor. Para isso, necessitamos de colocar esse mesmo endereço, seguido de 63 '%x' e, por fim, um '%n', como foi feito no script de python.
![input](images/w6/3_a_1.png)
- Já que o '%n' corresponde a guardar na variável dada como parâmetro (neste caso, o target) o número de caratérs escritos até esse instante, esse mesmo número (0xEE) é guardado no target. Isto é verificável pelo output do servidor.
![output](images/w6/3_a_2.png)

### 3.B 

- Sabendo que na alínea a deste mesmo exercício foram lidos 238 caratérs (0xEE), então é necessário escrever mais 20242(0x5000-0xEE) caratérs antes do '%n'. Para tal, após o endereço do target, em vez de adicionarmos os 63 '%x' adicionamos 62 '%x' sem quaisquer alterações e um '20244%x' (20242 + os dois caratérs que o último '%x' escrevia), seguido do '%n', como foi feito no script python.
![input](images/w6/3_b_1.png)
- O resultado é verificável pelo output do servidor
![output](images/w6/3_b_2.png)
