Lista de blocos georregionais de IPs com ufw no Linux
Digamos, por exemplo, que você deseja bloquear intervalos de IP por região, como bloquear a China. Isso é fácil de fazer com um site bacana e ufw no Ubuntu ou outras distros Linux. Eu vou te mostrar como!

1. Primeiro, obtenha uma lista de endereços IP de uma região que deseja bloquear. Um site que fornece isso é:
http://www.ip2location.com/free/visitor-blocker

Selecione iptables, China (ou qualquer outro país), formato CIDR e Download.

A lista será semelhante à seguinte, com intervalos no formato CIDR, um em uma linha. Salve como, digamos, block.txt. 
Eu também recomendaria testar esta lista primeiro em um ambiente que não seja de produção! Essas listas são geralmente precisas, mas tenha muito cuidado e use-as com cautela.

2. Em seguida, execute cuidadosamente o seguinte comando para bloquear todos os intervalos nessa lista:

$ cat block.txt | awk '/^[^#]/ { print $1 }' | sudo xargs -I {} ufw deny from {} to any

Para uma lista grande (digamos, a lista de porcelanas), pode levar vários minutos para ser executada.

3. Quando concluído, você pode executar o seguinte para verificar se as regras estão em vigor:

$ sudo ufw status

Para remover ou reverter essas regras, mantenha essa lista de IPs! Em seguida, execute um comando como este para remover as regras:

$ cat block.txt | awk '/^[^#]/ { print $1 }' | sudo xargs -I {} ufw delete deny from {}
