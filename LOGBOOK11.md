# Trabalho Realizado nas Semanas #12 e #13

## Task 1  

- Que parte do certificado indica que é um certificado CA? Como podemos ver, a extensão CA está definida como TRUE.

![img](images/w12/1_a.png)

- Que parte do certificado indica que é um certificado "self-signed"? Na secção onde é possível encontrar o subject e o issuer, reparamos que estes coincidem, logo o certificado é "self-signed".

![img](images/w12/1_b.png)

- No algoritmo RSA, temos um expoente público e, um expoente privado d, um módulo n, e dois números secretos p e q, tal que n = pq. Identifica os valores para estes elementos no teu certificado e ficheiros chave.

**módulo n:**  
![img](images/w12/1_c.png)

**expoente público e:**  
![img](images/w12/1_d.png)

**expoente privado d:**  
![img](images/w12/1_e.png)

**números secretos p e q:**  
![img](images/w12/1_f.png)

## Task 2  

- Aqui se segue o comando criado para gerar um CSR para o server especificado, juntamente com a opção adicional, adicionando-se dois nomes alternativos ao CSR.

![img](images/w12/2.png)

## Task 3  

- Após assinar o certificado e executar o comando proposto que dá print ao conteúdo decodificado do certificado, podemos verificar que os nomes alternativos estão, de facto, incluídos.

![img](images/w12/3_a.png)

![img](images/w12/3_b.png)

## Task 4  

- 

![img](images/w12/4c.png)

![img](images/w12/4d.png)

![img](images/w12/4e.png)

![img](images/w12/4f.png)

![img](images/w12/4g.png)

![img](images/w12/4h.png)

![img](images/w12/4a.png)

![img](images/w12/4b.png)

## Task 5  

- 

![img](images/w12/5a.png)

![img](images/w12/5b.png)

![img](images/w12/5c.png)

![img](images/w12/5d.png)

## Task 6  

- 

![img](images/w12/6a.png)

![img](images/w12/6b.png)

![img](images/w12/6c.png)

![img](images/w12/6d.png)

# CTF

## Semanas #12 e #13 - Desafio 1
Utilizando o site https://www.numberempire.com/primenumbers.php, encontramos p e q (primeiro primo maior que 2^512 e primeiro primo maior que 2^513, respetivamente).
- p =13407807929942597099574024998205846127479365820592393377723561443721764030073546976801874298166903427690031858186486050853753882811946569946433649006084171
- q =26815615859885194199148049996411692254958731641184786755447122887443528060147093953603748596333806855380063716372972101707507765623893139892867298012168351 <br>
 De seguida, sabendo que (d * e) mod ((p-1) * (q-1)) = 1 e que o valor de e é padrão - 0x10001, calculamos d utilizando a seguinte expressão:
- pow(e,-1,(p-1)*(q-1))
Utilizando este valor, basta utilizar a função dec fornecida para descodificar o criptograma.

![img](images/w12/desafio1_a.png)

## Semanas #12 e #13 - Desafio 2
- Após alguma pesquisa, descobrimos (em https://infosecwriteups.com/rsa-attacks-common-modulus-7bdb34f331a5) que quando a mesma mensagem é cifrada duas vezes com os mesmos números primos p e q mas dois e's diferentes, é possível recuperar a mensagem original se tivermos acesso aos dois criptogramas resultantes, ao n e aos dois e's (todos fornecidos neste desafio), utilizando o teorema de Bezout.
- Para isto, basta descobrir dois números a e b, utilizando o teorema de Bezout, tal que a * e1 + y * e2 = 1, em que e1 e e2 são os dois e's utilizados na encriptação.
- Após isto, é possível provar que, módulo n:
- c1^a * c2^b = M, em que c1 corresponde ao criptograma 1, c2 corresponde ao criptograma 2 e M corresponde à mensagem original.
- Como tal, basta calcular c1^a*c2^b (módulo n) para obter a mensagem original (ou seja, a flag).
![img](images/w12/desafio2_a.png)
