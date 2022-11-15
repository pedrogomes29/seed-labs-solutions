# Trabalho Realizado na Semana #8

## Task 1

- Utilizamos o mysql para interagir com a base de dados como root. De seguida utilizamos o show tables para ver as tabelas da base de dados.
- E corremos o comando SQL mostrado em baixo para ver os dados da alice.

![ver dados da alice](images/w8/task1_1.png)

## Task 2  

### 2.1 

- Analisando o código PHP em unsafe_home.php, bastaria inserir um pedaço de comando SQL no campo correspondente ao $input_uname, de modo a aceder à informação pretendida.

![img](images/w8/task2_1_a.png)

- No nosso caso, adicionamos um #, ficando o código que se segue (que verifica a password) comentado.
- WHERE name= ’admin';#

![img](images/w8/task2_1_b.png)

### 2.2

- Recorremos à mesma estratégia usada na alínea anterior.
- O comando executado foi: curl 'www.seed-server.com/unsafe_home.php?username=admin%27%3B%23&Password='.
- Verifica-se, pela seguinte imagem, que foi obtida a informação dos funcionários.

![img](images/w8/task2_2.png)

### 2.3 

- Existe, de facto, uma medida que impede a execução de 2 comandos SQL em um ataque. Para se executar vários comandos SQL (agrupados), o comando multi_querie() precisa de estar presente.
- As funções de querie, por norma, não ativam uma flag de conexão necessária para ativar multi queries no servidor. 

## Task 3

### 3.1

- 

![img]()
