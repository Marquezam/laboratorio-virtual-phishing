# Relatório Técnico — Laboratório Virtual de Phishing

## 1. Identificação

- **Projeto:** Laboratório Virtual de Phishing
- **Formação:** Cibersegurança — DIO
- **Responsável:** Marquezam Xavier Marques
- **Data de execução:** 04 de setembro de 2026
**Finalidade:** exclusivamente educacional e autorizada

---

## 2. Objetivo

O desafio propõe a utilização do Social-Engineer Toolkit — SEToolkit para demonstrar o funcionamento de uma página falsa do Facebook destinada à captura de credenciais.

Os objetivos técnicos foram:

- preparar um laboratório virtual;
- validar a comunicação entre as máquinas;
- executar o Credential Harvester;
- tentar utilizar o Site Cloner com o Facebook;
- analisar as limitações encontradas;
- realizar uma adaptação segura e controlada;
- capturar somente dados fictícios;
- documentar decisões, resultados e medidas preventivas;
- versionar o projeto com Git e publicá-lo no GitHub.

---

## 3. Escopo e autorização

O experimento foi executado exclusivamente em equipamentos próprios e autorizados.

Nenhuma página foi publicada na internet, nenhum link foi enviado a terceiros e nenhuma credencial verdadeira foi utilizada.

### Regras adotadas

- utilização exclusiva das máquinas do laboratório;
- ausência de redirecionamento de portas no roteador;
- ausência de DMZ ou publicação externa;
- utilização de dados fictícios;
- aviso visível na página de treinamento;
- encerramento do servidor após a coleta da evidência;
- não publicação dos relatórios brutos do Harvester;
- não realização de tentativas para contornar proteções do Facebook real.

---

## 4. Ambiente utilizado

| Componente | Configuração |
|---|---|
| Hipervisor | VMware Workstation |
| Máquina de testes | Kali Linux |
| Máquina cliente | Windows 11 Pro — WINLAB11 |
| Máquina física autorizada | Estação do responsável pelo laboratório |
| IP do Kali | `173.168.100.75/24` |
| IP do WINLAB11 | `173.168.100.1/24` |
| IP da máquina física | `173.168.100.100/24` |
| Gateway | `173.168.100.254` |
| Ferramenta | Social-Engineer Toolkit — SEToolkit |
| Versão inicial do SET | `8.0.3` |
| Versão atualizada do SET | `8.1.3` |
| Controle de versão | Git |
| Branch principal | `main` |

---

## 5. Topologia

    Máquina física autorizada
    173.168.100.100
             |
             |
    Rede local do laboratório
       173.168.100.0/24
          /          \
         /            \
    Kali Linux       WINLAB11
    173.168.100.75   173.168.100.1
    SEToolkit        Navegador cliente

As máquinas foram temporariamente configuradas em modo Bridged para permitir a comunicação na mesma rede.

Não houve publicação do serviço na internet.

---

## 6. Preparação do repositório

Foi criado o repositório local:

    /home/marquezam/laboratorio-virtual-phishing

Estrutura inicial:

    laboratorio-virtual-phishing/
    ├── README.md
    ├── docs/
    ├── evidencias/
    ├── images/
    └── lab-site/

A branch inicial foi renomeada de `master` para `main`.

O primeiro commit foi criado com a mensagem:

    docs: cria estrutura inicial do laboratorio

Identificador abreviado:

    70688b4

Após o commit, o estado do repositório apresentou:

    On branch main
    nothing to commit, working tree clean

---

## 7. Validação da conectividade

A comunicação entre o Kali e o WINLAB11 foi testada nos dois sentidos.

### Endereços validados

- Kali: `173.168.100.75`;
- WINLAB11: `173.168.100.1`;
- gateway: `173.168.100.254`.

O teste do Kali para o WINLAB11 apresentou:

- quatro pacotes transmitidos;
- quatro pacotes recebidos;
- perda de `0%`;
- latência média aproximada de `0,422 ms`;
- resposta com `TTL=128`, compatível com Windows.

O Firewall do Windows permaneceu ativo. Foi criada uma regra limitada para permitir o ping originado do Kali.

---

## 8. Preparação do SEToolkit

A presença da ferramenta foi confirmada com:

    command -v setoolkit

Resultado:

    /usr/bin/setoolkit

A porta HTTP foi verificada antes da execução:

    sudo ss -ltnp '( sport = :80 )'

Inicialmente, nenhum processo estava utilizando a porta `80`.

O SEToolkit foi iniciado com privilégio administrativo somente durante sua execução:

    sudo setoolkit

Não foi mantida uma sessão permanente como usuário `root`.

---

## 9. Fluxo original exigido pelo desafio

O procedimento original foi seguido conforme o repositório-base da atividade:

1. `Social-Engineering Attacks`;
2. `Website Attack Vectors`;
3. `Credential Harvester Attack Method`;
4. `Site Cloner`;
5. definição do IP de retorno;
6. informação da URL do Facebook.

O IP indicado para o retorno dos dados foi:

    173.168.100.75

A URL foi informada exatamente como consta no projeto original:

    http://www.facebook.com

---

## 10. Falha do Site Cloner

Durante a tentativa, o Facebook redirecionou a requisição para:

    https://login.facebook.com/login.php

O SEToolkit apresentou:

    Error. Unable to clone this specific site.
    Check your internet connection.

A mensagem sugeria falha de internet, mas testes posteriores comprovaram que a conectividade estava funcionando.

---

## 11. Atualização do SEToolkit

A versão inicialmente instalada era:

    8.0.3+git20241021-0kali1

O catálogo local do APT estava desatualizado e apontava inicialmente para um pacote inexistente, causando:

    404 Not Found

Foi executado:

    sudo apt update

Um repositório adicional do Tor apresentou erro por não possuir uma distribuição chamada `kali-rolling`. Esse erro não impediu a atualização do catálogo oficial do Kali.

Após a atualização do catálogo, a versão candidata passou a ser:

    8.1.3+git20260604-0kali1

A atualização foi realizada com:

    sudo apt install --only-upgrade set

A instalação foi concluída com:

    Configurando set (8.1.3+git20260604-0kali1)

O SEToolkit foi reiniciado e apresentou:

    Version: 8.1.3
    Codename: Maverick

O Site Cloner foi testado novamente com a mesma URL exigida pelo desafio, mas a falha permaneceu.

---

## 12. Diagnóstico da clonagem

A conectividade HTTPS foi testada com:

    curl -I --max-time 15 https://www.facebook.com/

O servidor respondeu:

    HTTP/2 200

Isso comprovou que o Kali possuía:

- conectividade com a internet;
- resolução DNS;
- comunicação HTTPS;
- acesso ao domínio do Facebook.

Entretanto, requisições completas para baixar o conteúdo retornaram:

    HTTP 400 Bad Request

O teste realizado diretamente no endereço de login também retornou:

    400 Bad Request

Foi identificado que o SEToolkit utilizava um User-Agent antigo, correspondente ao Chrome 58. Um User-Agent moderno também foi testado, mas o Facebook continuou recusando a requisição automatizada.

Os endpoints móveis também foram avaliados:

- `mbasic.facebook.com` respondeu `HTTP 200`, mas entregou uma página intitulada “Erro”, sem formulário;
- `m.facebook.com` respondeu `HTTP 400`;
- a página principal respondeu `HTTP 400` em requisições completas automatizadas.

### Conclusão do diagnóstico

A falha não foi causada por:

- erro de endereço IP;
- falta de internet;
- versão desatualizada do SEToolkit;
- erro na sequência dos menus;
- ocupação da porta `80`.

A página atual do Facebook utiliza redirecionamentos, conteúdo dinâmico e proteções contra automação incompatíveis com o mecanismo de clonagem utilizado pelo SEToolkit.

Esse comportamento representa uma proteção defensiva do serviço, e não uma vulnerabilidade a ser contornada.

---

## 13. Análise do código do Site Cloner

O código oficial do SEToolkit demonstra que o Site Cloner utiliza `wget` para obter a página informada.

O processo verifica se foi criado um arquivo chamado:

    index.html

O código também considera a clonagem inválida quando o arquivo não existe ou possui quantidade insuficiente de linhas.

As respostas atuais do Facebook não forneceram ao `wget` uma página de login utilizável, impedindo a conclusão do fluxo original.

---

## 14. Adaptação controlada

Para demonstrar o funcionamento do Credential Harvester sem tentar burlar as proteções do Facebook, foi criada uma página local de treinamento.

Arquivo:

    lab-site/index.html

A página possui:

- referência visual educacional ao Facebook;
- aviso destacado de ambiente de treinamento;
- campo de usuário fictício;
- campo de senha fictícia;
- formulário utilizando o método `POST`;
- ausência de scripts externos;
- ausência de imagens ou recursos carregados do Facebook;
- indicação de que o teste ocorre entre Kali e WINLAB11.

Aviso apresentado:

    AMBIENTE DE TREINAMENTO — NÃO UTILIZE CREDENCIAIS VERDADEIRAS

O arquivo foi validado e possuía `135` linhas.

---

## 15. Pré-visualização local

Antes da importação, a página foi testada somente no próprio Kali:

    python3 -m http.server 8080 --bind 127.0.0.1

A página foi acessada em:

    http://127.0.0.1:8080

O uso de `127.0.0.1` impediu que o servidor de pré-visualização fosse acessado externamente.

Após a validação visual, o servidor temporário foi encerrado.

---

## 16. Importação no Credential Harvester

No SEToolkit, foi utilizado:

1. `Social-Engineering Attacks`;
2. `Website Attack Vectors`;
3. `Credential Harvester Attack Method`;
4. `Custom Import`.

O IP de retorno utilizado foi:

    173.168.100.75

O caminho importado foi:

    /home/marquezam/laboratorio-virtual-phishing/lab-site/

O SEToolkit confirmou:

    Index.html found

Foi escolhida a opção:

    Copy just the index.html

Como URL representada pela página importada, foi informado:

    https://www.facebook.com/

Essa URL foi usada apenas para redirecionar o navegador após o envio do formulário.

---

## 17. Execução do Harvester

O SEToolkit informou:

    Credential Harvester is running on port 80

A porta foi validada com:

    sudo ss -ltnp '( sport = :80 )'

Resultado:

    LISTEN 0.0.0.0:80
    Processo: setoolkit

O WINLAB11 acessou:

    http://173.168.100.75

A página educacional foi exibida corretamente no navegador.

---

## 18. Captura controlada

Foi realizado apenas um teste autorizado com identificadores fictícios.

O SEToolkit registrou:

    WE GOT A HIT!
    POSSIBLE USERNAME FIELD FOUND
    POSSIBLE PASSWORD FIELD FOUND

Os valores não serão reproduzidos neste relatório público.

O resultado demonstra que:

1. o navegador enviou uma requisição `POST`;
2. o Kali recebeu a requisição;
3. o SEToolkit analisou os parâmetros;
4. o campo de usuário foi identificado;
5. o campo de senha foi identificado;
6. o navegador foi redirecionado para o site oficial.

Nenhuma senha verdadeira foi utilizada.

---

## 19. Relatório interno do SEToolkit

Após `Ctrl + C`, o SEToolkit gerou um relatório XML em:

    /root/.set/reports/

O relatório bruto não será adicionado ao GitHub porque contém os valores submetidos durante o teste, mesmo sendo fictícios.

O repositório público apresentará somente resultados sanitizados.

---

## 20. Controle de acesso à porta 80

Durante parte do laboratório, foram utilizadas regras temporárias de firewall para:

- permitir acesso do WINLAB11 à porta `80`;
- bloquear essa porta para outras origens.

As regras utilizaram os comentários:

    LAB-PHISHING-WINLAB11
    LAB-PHISHING-BLOQUEIO

Uma verificação posterior mostrou que essas regras temporárias não permaneceram ativas. Não foi atribuída uma causa sem evidência conclusiva.

Os acessos observados durante o teste vieram somente de:

- `173.168.100.1` — WINLAB11;
- `173.168.100.100` — máquina física autorizada.

Nenhum endereço externo ou equipamento desconhecido acessou a página.

---

## 21. Resultado final

O laboratório alcançou os seguintes resultados:

- conectividade entre Kali e Windows validada;
- repositório Git criado;
- evidências iniciais organizadas;
- SEToolkit atualizado para `8.1.3`;
- fluxo original do Site Cloner executado;
- incompatibilidade atual do Facebook identificada;
- causa investigada com `curl` e `wget`;
- página educacional criada;
- página importada pelo Custom Import;
- Credential Harvester iniciado na porta `80`;
- dados fictícios capturados;
- redirecionamento ao endereço oficial validado;
- relatório XML gerado;
- ambiente encerrado após o teste.

---

## 22. Limitações

- O Site Cloner não conseguiu baixar a página atual do Facebook.
- O conteúdo do Facebook depende de mecanismos modernos incompatíveis com o fluxo antigo do curso.
- O laboratório utilizou temporariamente uma rede Bridged.
- As regras aplicadas diretamente pelo `iptables` eram temporárias.
- A página adaptada não representa uma cópia integral do Facebook.
- O relatório XML bruto não será publicado.

---

## 23. Medidas de prevenção contra phishing

### Para usuários

- conferir cuidadosamente o domínio antes de digitar credenciais;
- desconfiar de páginas abertas por links recebidos;
- utilizar autenticação em dois fatores;
- usar senhas diferentes em cada serviço;
- utilizar um gerenciador de senhas;
- observar avisos de conexão não segura;
- evitar inserir senhas em páginas acessadas por endereço IP;
- comunicar mensagens suspeitas à equipe responsável.

### Para organizações

- realizar treinamentos periódicos de conscientização;
- utilizar filtros de e-mail e proteção contra URLs maliciosas;
- implementar autenticação multifator;
- monitorar domínios semelhantes aos oficiais;
- adotar SPF, DKIM e DMARC;
- restringir serviços desnecessários na rede;
- registrar e analisar eventos de autenticação;
- estabelecer processo de resposta a incidentes;
- executar simulações somente com autorização formal.

---

## 24. Considerações éticas

A capacidade de copiar páginas e capturar formulários demonstra por que técnicas de engenharia social representam risco.

O conhecimento deve ser utilizado para:

- conscientização;
- defesa;
- auditorias autorizadas;
- testes controlados;
- desenvolvimento de controles de segurança.

A execução dessas técnicas contra pessoas, sistemas ou organizações sem autorização pode violar leis, contratos e políticas de uso.

A proteção atual do Facebook não foi contornada. A adaptação ocorreu somente dentro do laboratório, com uma página criada pelo próprio responsável.

---

## 25. Referências

- Projeto-base da DIO:
  https://github.com/cassiano-dio/cibersecurity-desafio-phishing

- Documentação oficial do SET no Kali Linux:
  https://www.kali.org/tools/set/

- Código oficial do Social-Engineer Toolkit:
  https://github.com/trustedsec/social-engineer-toolkit

- Código do Site Cloner:
  https://github.com/trustedsec/social-engineer-toolkit/blob/master/src/webattack/web_clone/cloner.py

- Programa oficial de divulgação responsável da Meta:
  https://business.facebook.com/whitehat

---

## 26. Conclusão

O procedimento original foi reproduzido fielmente até a tentativa de clonagem do Facebook.

A falha permaneceu mesmo após a atualização do SEToolkit, demonstrando que o roteiro original depende de um comportamento antigo do site externo.

Em vez de tentar burlar os mecanismos defensivos atuais, o experimento foi concluído por meio de uma página local, claramente identificada como treinamento e importada no Credential Harvester.

O resultado comprovou tecnicamente o funcionamento da captura de parâmetros `POST`, preservando o caráter educacional, autorizado e ético da atividade.
