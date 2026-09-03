# Laboratório Virtual de Phishing

## Sobre o projeto

Este projeto documenta uma simulação educacional de phishing realizada em um laboratório autorizado, como parte da formação em Cibersegurança da DIO.

O objetivo é compreender o funcionamento desse tipo de ataque, reconhecer seus riscos e estudar medidas de identificação e prevenção.

## Objetivos

- Preparar um ambiente controlado para testes;
- Validar a comunicação entre as máquinas virtuais;
- Estudar o funcionamento do SEToolkit no Kali Linux;
- Executar uma simulação utilizando somente dados fictícios;
- Registrar evidências técnicas do laboratório;
- Apresentar medidas de prevenção contra phishing.

## Ambiente utilizado

| Componente | Configuração |
|---|---|
| Hipervisor | VMware Workstation |
| Máquina de segurança | Kali Linux |
| Máquina cliente | Windows 11 Pro — WINLAB11 |
| IP do Kali | `173.168.100.75` |
| IP do WINLAB11 | `173.168.100.1` |
| Ferramenta estudada | Social-Engineer Toolkit — SEToolkit |
| Controle de versão | Git |

## Topologia

```text
Kali Linux                         WINLAB11
173.168.100.75  <------------->  173.168.100.1

```

A comunicação entre as duas máquinas foi validada nos dois sentidos, sem perda de pacotes.

## Regras de segurança

O experimento possui finalidade exclusivamente educacional e foi executado em equipamentos autorizados.

- Uso exclusivo de dados fictícios;
- Nenhuma credencial verdadeira será utilizada;
- Nenhum terceiro participará do teste;
- Nenhum serviço será publicado na internet;
- Nenhuma página será enviada a pessoas externas;
- O Firewall do Windows permanecerá ativo;
- O ambiente será desativado após o laboratório.

> As máquinas estão temporariamente conectadas à mesma rede local. O experimento será limitado aos equipamentos autorizados e não será exposto externamente.

## Evidências iniciais

### Comunicação do Kali com o WINLAB11

![Teste de conectividade](images/01-conectividade-kali-winlab11.png)

### Regra de firewall para o laboratório

![Liberação controlada de ping](images/02-liberacao-ping-winlab11.png)

### Comunicação do WINLAB11 com o Kali

![Teste do Windows para o Kali](images/03-teste-ping-winlab11-para-kali.png)

### Máquina virtual WINLAB11

![Configuração do WINLAB11](images/04-configuracao-winlab11.png)

## Estrutura do repositório

```text
laboratorio-virtual-phishing/
├── README.md
├── docs/
├── evidencias/
└── images/
```

## Progresso

- [x] Preparação do Kali Linux;
- [x] Preparação do WINLAB11;
- [x] Configuração da comunicação de rede;
- [x] Validação da conectividade;
- [x] Organização das evidências iniciais;
- [x] Inicialização do repositório Git;
- [ ] Execução da simulação educacional;
- [ ] Registro dos resultados;
- [ ] Documentação das medidas preventivas;
- [ ] Publicação do projeto no GitHub.

## Uso responsável

Este projeto não incentiva ataques contra terceiros. Seu propósito é demonstrar, em ambiente autorizado, como o phishing funciona e como usuários e organizações podem reconhecer e evitar essa ameaça.
