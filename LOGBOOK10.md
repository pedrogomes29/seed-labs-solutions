# Trabalho Realizado nas Semanas #10 e #11

## Task 1  

- Tal como foi pedido, preenchemos o campo brief description no perfil de um user com código JavaScript, de modo a dar display de um simples alert, aquando de uma visita ao perfil.

![img](images/w9/1a.png)

![img](images/w9/1b.png)

## Task 2  

- Tal como na tarefa 1, inserimos um script no mesmo campo, mas desta vez de modo a dar display dos cookies relativos à sessão do user, aquando de uma visita ao perfil.

![img](images/w9/2a.png)

![img](images/w9/2b.png)

## Task 3  

- Nesta tarefa, queremos ter acesso às cookies de um user sem precisar de visitar o seu perfil. Para isso, recorre-se a um script (no campo brief description, por exemplo), que um HTTP request para nós.
- Através da adição de um elemento img, quando o browser tentar carregar os dados nela contidos, o request é enviado.
- Por fim, usamos o comando nc (netcat) que revela informação enviada no port especificado. 

![img](images/w9/3a.png)

![img](images/w9/3b.png)

![img](images/w9/3c.png)

## Task 4  

- Nesta tarefa, pretendemos que quem visite o perfil do user Samy adicione-o como amigo automaticamente.
- Para isso, podemos colocar um script na secção About me do perfil do Samy (em Text mode).

![img](images/w9/4a.png)

![img](images/w9/4b.png)

- Conseguimos assim criar e enviar um request Ajax, aproveitando-nos da informação no campo de action no form relativo à adição como amigo de um user.
- **Resposta à questão 1:** De modo a prevenir ataques de request forgery, qualquer action que se pretenda fazer na plataforma em questão requere a passagem de um CSRF token. Neste caso, a linha 1 contém informação de uma timestamp atual e, por sua vez, a linha 2 contém o token gerado a partir desse timestamp. Assim, para que a action seja permitida, temos de recolher os dados do token (linhas 1 e 2) (por exemplo, usando JavaScript), sendo que essa proteção não irá impedir o nosso script de executar e fazer o pretendido.
- **Resposta à questão 2:** Sim, seria possível executar um ataque bem sucedido; bastaria alterar o código HTML, adaptando-o de modo a permitir aquilo que pretendemos.

![img](images/w9/4c.png)
