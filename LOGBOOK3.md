# Trabalho realizado na Semana #3

## Identificação

- webClassifieds (uma aplicação web) é vulnerável a SQL Injection
- Foi possível executar comandos SQL para o script index.php nos parâmetros do username e da password na sign_in action
- O sistema operativo em questão é irrelevante no que diz respeito a esta vulnerabilidade
- Relatório original: https://packetstormsecurity.com/files/73461/webClassifieds-2005-Login-Bypass.html

## Catalogação

- A organização em questão foi a Web Scribble Solutions - webClassifieds, 2008
- O autor foi o user AnGeL25dZ, sendo divulgada a 28/12/2008 e com update a 02/01/2009
- Uso de comandos/termos SQL nos campos de username e password em sign in
- Não encontramos registo de qualquer bug bounty, CVSS Score 6.8 (gravidade moderada)

## Exploit

- Exploit no seguinte URL: http://[PATH]//classifieds/index.php?page=sign_in, por via de SQL Injection
- Recorreu aos seguintes username e password, user :           ' or '1=1   /   password :     ' or '1=1 
- Por alternativa, user :           admin   /   password :     ' or '1=1 
- Temos o sqlmap como meio de automação existente

## Ataques

- Não foram encontrados registos de ataques
- Roubar credenciais — atacantes conseguem obter credenciais via SQL, fingir que são outros utilizadores (incluindo admin) e usar as suas permissões
- Alterar dados — atacantes conseguem alterar, apagar ou adicionar dados à base de dados comprometida
- Produtos afetados pelo CVE: webClassifieds (versão 2005); número de versões vulneráveis: 1 
