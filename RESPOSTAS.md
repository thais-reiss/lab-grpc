# Respostas

## Parte A

### 4.1

**1. O endereço do servidor (localhost, IP, grupo multicast) está escrito diretamente no código do cliente? Isso favorece ou prejudica a transparência de localização?**

Sim, nas implementações em TCP, UDP e WebSocket, o cliente possui, em seu código, o endereço do servidor, neste caso localhost, e a porta da aplicação. Isso prejudica a transparência de localização, pois, se o servidor mudar de máquina, o cliente não conseguirá mais se comunicar com o servidor. Nesse caso, será necessário alterar diretamente no código do cliente o endereço do servidor e, se necessário, a porta utilizada pela aplicação. Já no multicast, o cliente possui o endereço IP do grupo ao qual deve se inscrever. Ele não apresenta o endereço do servidor escrito no código, o que favorece a transparência de localização.

**2. Para “perguntar uma coisa” ao servidor, o cliente precisa montar uma string de texto manualmente (e o servidor precisa interpretá-la/fazer parsing)? Isso é meio-termo, presença ou ausência de transparência de acesso?**

Sim, nos 4 protocolos implementados o cliente precisa montar manualmente uma string de texto para se comunicar, e o servidor precisa interpretá-la, e isso caracteriza ausência de transparência de acesso. No caso do TCP e do UDP, o cliente monta o texto digitado e o converte para bytes, usando o método .encode no Python e .getBytes() no Java. Do lado do servidor, os bytes recebidos são convertidos de volta para texto com .decode no Python e new String(...) no Java, para então serem interpretados. Já no no Multicast, isso muda um pouco, pois quem monta e converte o texto manualmente é o servidor, que gera avisos periódicos e os converte para bytes antes de enviar ao grupo. Os clientes apenas recebem esses bytes e os convertem de volta para texto para exibi-los. Por fim, no WebSocket, mesmo usando bibliotecas prontas, a websockets em Python e a Java-WebSocket em Java, para lidar com a parte técnica do protocolo, o conteúdo trocado continua sendo texto simples, montado e interpretado à mão. O cliente manda direto o que o usuário digitou, e o servidor só coloca esse texto dentro de uma frase pronta ("Aviso da turma: " + mensagem) antes de mandar para os outros clientes.

**3. O que aconteceria com o cliente se o servidor mudasse de máquina amanhã? Alguma dessas quatro soluções sobreviveria a essa mudança sem alterar o código-fonte do cliente?**

O cliente não iria conseguir se comunicar com o servidor, exceto na implementação do multicast. Como respondi na pergunta 1, nas implementações do TCP, UDP e WebSocket, o cliente possui o endereço do servidor escrito no código-fonte. Assim, se o servidor mudasse de máquina, seria necessário alterar o código-fonte dessas implementações, para o cliente conseguir continuar se comunicando com o servidor. Já o multicast sobreviveria, porque o cliente não tem o endereço do servidor escrito no código-fonte, apenas o endereço IP de um grupo. Então, se o servidor mudasse de máquina mas continuasse mandando mensagens para o mesmo endereço IP do grupo e para a mesma porta, o cliente ainda conseguiria receber os avisos. 