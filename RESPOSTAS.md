# Respostas

## Parte A - Transparências em Sistemas Distribuídos

### 4.1

**1. O endereço do servidor (localhost, IP, grupo multicast) está escrito diretamente no código do cliente? Isso favorece ou prejudica a transparência de localização?**

Sim, nas implementações em TCP, UDP e WebSocket, o cliente possui, em seu código, o endereço do servidor, neste caso localhost, e a porta da aplicação. Isso prejudica a transparência de localização, pois, se o servidor mudar de máquina, o cliente não conseguirá mais se comunicar com o servidor. Nesse caso, será necessário alterar diretamente no código do cliente o endereço do servidor e, se necessário, a porta utilizada pela aplicação. Já no multicast, o cliente possui o endereço IP do grupo ao qual deve se inscrever. Ele não apresenta o endereço do servidor escrito no código, o que favorece a transparência de localização.

**2. Para “perguntar uma coisa” ao servidor, o cliente precisa montar uma string de texto manualmente (e o servidor precisa interpretá-la/fazer parsing)? Isso é meio-termo, presença ou ausência de transparência de acesso?**

Sim, nos 4 protocolos implementados o cliente precisa montar manualmente uma string de texto para se comunicar, e o servidor precisa interpretá-la, e isso caracteriza ausência de transparência de acesso. No caso do TCP e do UDP, o cliente monta o texto digitado e o converte para bytes, usando o método .encode no Python e .getBytes() no Java. Do lado do servidor, os bytes recebidos são convertidos de volta para texto com .decode no Python e new String(...) no Java, para então serem interpretados. Já no no Multicast, isso muda um pouco, pois quem monta e converte o texto manualmente é o servidor, que gera avisos periódicos e os converte para bytes antes de enviar ao grupo. Os clientes apenas recebem esses bytes e os convertem de volta para texto para exibi-los. Por fim, no WebSocket, mesmo usando bibliotecas prontas, a websockets em Python e a Java-WebSocket em Java, para lidar com a parte técnica do protocolo, o conteúdo trocado continua sendo texto simples, montado e interpretado à mão. O cliente manda direto o que o usuário digitou, e o servidor só coloca esse texto dentro de uma frase pronta ("Aviso da turma: " + mensagem) antes de mandar para os outros clientes.

**3. O que aconteceria com o cliente se o servidor mudasse de máquina amanhã? Alguma dessas quatro soluções sobreviveria a essa mudança sem alterar o código-fonte do cliente?**

O cliente não iria conseguir se comunicar com o servidor, exceto na implementação do multicast. Como respondi na pergunta 1, nas implementações do TCP, UDP e WebSocket, o cliente possui o endereço do servidor escrito no código-fonte. Assim, se o servidor mudasse de máquina, seria necessário alterar o código-fonte dessas implementações, para o cliente conseguir continuar se comunicando com o servidor. Já o multicast sobreviveria, porque o cliente não tem o endereço do servidor escrito no código-fonte, apenas o endereço IP de um grupo. Então, se o servidor mudasse de máquina mas continuasse mandando mensagens para o mesmo endereço IP do grupo e para a mesma porta, o cliente ainda conseguiria receber os avisos. 

---

### 4.3

**1. Dentre os 8 tipos de transparência listados, qual você diria que é a mais visível para o programador que está usando um serviço remoto (e não construindo a infraestrutura por trás dele)? Justifique.**

A transparência de acesso, porque observei nos exercícios anteriores que, como as implementações não apresentavam essa transparência, o programador precisou escrever manualmente o código de conversão entre texto e bytes, tanto para enviar quanto para receber dados, além de interpretar os comandos no lado do servidor. Essa conversão só é necessária porque o recurso é remoto, pois um recurso local é acessado por uma chamada de função comum. Se houvesse transparência de acesso, essa conversão ficaria a cargo de alguma camada ou biblioteca, e o programador chamaria um método comum, sem perceber a diferença entre acessar um recurso local ou remoto. Por isso, entre os 8 tipos, a transparência de acesso é a que tem mais impacto direto sobre o programador que consome um serviço remoto, pois ele não vê o hardware, a rede ou onde os dados estão replicados. O que ele realmente vê e escreve é a API, o método ou a chamada no código. As demais transparências dizem respeito a como a infraestrutura por trás do serviço está organizada, algo que normalmente fica escondido de quem apenas consome a API.

**2. Transparência total é sempre desejável? Dê um exemplo (pode ser hipotético) de uma situação em que esconder completamente que uma operação é remota atrapalharia mais do que ajudaria (dica: pense em desempenho ou em tratamento de falhas).**

Não, a transparência total nem sempre é desejável, pois ocultar completamente que uma operação é remota pode dificultar o tratamento de problemas de desempenho ou de falhas. Por exemplo, em um e-commerce, durante o pagamento, o processamento pode depender de um serviço remoto de uma empresa de pagamentos. Se o sistema esconder completamente essa dependência, uma demora ou indisponibilidade pode fazer o usuário pensar que o próprio e-commerce está travado e tentar realizar o pagamento várias vezes. Informar que o pagamento está sendo processado por um serviço externo pode permitir um tratamento mais adequado da situação e tornar o comportamento do sistema mais compreensível. 

**3.(Responder depois de concluir as Partes C e D) Comparando o cliente TCP do laboratório anterior com o cliente gRPC que você vai construir agora: qual dos dois exige que você “pense em rede” (sockets, send/receive, parsing de string) e qual permite que você “pense no problema” (chamar uma função e receber um resultado)? A que tipo de transparência isso se relaciona?**


## Parte B - Protocol Buffers e o contrato do serviço
 


