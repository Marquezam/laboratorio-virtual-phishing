# Resultado Sanitizado do Credential Harvester

## Identificação

- Data: 04 de setembro de 2026
- Ambiente: laboratório virtual autorizado
- Origem do teste: WINLAB11 — `173.168.100.1`
- Servidor: Kali Linux — `173.168.100.75`
- Ferramenta: SEToolkit `8.1.3`
- Porta utilizada: TCP `80`

## Resultado observado

O WINLAB11 acessou a página educacional hospedada pelo SEToolkit e enviou um formulário pelo método HTTP POST.

O terminal do SEToolkit apresentou:

    WE GOT A HIT!
    POSSIBLE USERNAME FIELD FOUND: [IDENTIFICADOR FICTÍCIO OMITIDO]
    POSSIBLE PASSWORD FIELD FOUND: [SENHA FICTÍCIA OMITIDA]

Após o registro dos parâmetros, o navegador foi redirecionado para:

    https://www.facebook.com/

## Tratamento dos dados

Os valores submetidos foram criados exclusivamente para o laboratório.

O relatório XML bruto gerado pelo SEToolkit permanece somente no Kali e não será incluído no repositório público.

Nenhuma credencial verdadeira foi utilizada, armazenada no projeto ou enviada ao GitHub.

## Conclusão

O teste comprovou que o Credential Harvester recebeu a requisição POST e identificou os campos de usuário e senha dentro do ambiente autorizado.
