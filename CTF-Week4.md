# Web
* Após procurar informação relevante para o ataque pretendido ao website, encontramos a versão de wordpress e dos plugins instalados
![](/images/CTF-Week4-plugins-and-versions.png).
<br> e ainda dois utilizadores: Admin e Orvald Sanford
* Depois, procuramos vulnerabilidades nos plugins e no wordpress nas versões usadas. Após isto, encontramos uma vulnerabilidade que permitia autenthication bypass na versão 5.4.3 do Booster for WooCommerce (CVE-2021-34646)
* Após isto, procuramos um exploit para esta vulnerabilidade, que encontramos no exploit.db, [aqui](https://www.exploit-db.com/exploits/50299).
* Seguimos os passos explicados neste site e conseguimos dar log in com a conta de admin identificada previamente. Após isto, navegamos para o link fornecido que tinha a flag.
![](/images/admin-bypass-exploit.png)
