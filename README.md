Ataque de Brute Force com Medusa
Introdução

Este projeto faz parte do desafio de Cybersegurança da DIO, com o objetivo de documentar um ataque de Brute Force utilizando a ferramenta Medusa, integrante da suíte de ferramentas do Kali Linux.

Para a realização do laboratório, foi utilizado o DVWA (Damn Vulnerable Web Application) hospedado no Metasploitable2, ambos amplamente empregados em ambientes educacionais para o estudo de vulnerabilidades e técnicas de ataque.

O que é Brute Force

O ataque de Brute Force consiste em tentar diversas combinações de usuários e senhas de forma automatizada até encontrar credenciais válidas para acesso a um sistema.

Esse tipo de ataque pode ser aplicado em diferentes cenários, como:

Sistemas Web (páginas de login)

Serviços de rede (SSH, FTP, SMB, etc.)

Servidores corporativos

Painéis administrativos

A eficiência do ataque depende principalmente da qualidade das wordlists utilizadas e das políticas de segurança implementadas no sistema alvo.

Ferramenta Utilizada – Medusa

O Medusa é uma ferramenta de força bruta paralela, amplamente utilizada em testes de intrusão.
Ela suporta diversos módulos de autenticação, incluindo:

SMB

SSH

FTP

HTTP

MySQL, entre outros

Uma característica importante do Medusa é que ele se baseia na mensagem de erro retornada pelo serviço para identificar se uma tentativa de autenticação falhou ou foi bem-sucedida.

Preparação do Ataque

Antes da execução do ataque, é necessário criar dois arquivos:

Um arquivo contendo possíveis usuários

Um arquivo contendo possíveis senhas

Essas listas podem ser obtidas por meio de:

Vazamentos de dados

Senhas padrão

Regras comuns utilizadas por empresas

Informações descobertas durante a fase de reconhecimento

Criação da Wordlist de Usuários
echo -e "user\nmsfadmin\nservice" > smb_users.txt


Esse comando cria o arquivo smb_users.txt, que contém possíveis usuários do sistema alvo.

Criação da Wordlist de Senhas
echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt


Esse comando cria o arquivo senhas_spray.txt, que contém possíveis senhas a serem testadas.

Importância da Mensagem de Erro

Antes de iniciar o ataque, é fundamental observar qual mensagem o sistema retorna quando uma senha incorreta é informada.

Isso é importante porque o Medusa utiliza essa resposta para determinar se:

A tentativa de login falhou, ou

A autenticação foi bem-sucedida

Caso essa etapa não seja analisada corretamente, o ataque pode gerar falsos positivos ou não funcionar como esperado.

Execução do Ataque

Após a criação das wordlists, o seguinte comando foi utilizado para executar o ataque de Brute Force:

medusa -h 192.168.2.113 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 6 -T 50

Descrição dos parâmetros:

-h → Endereço IP do alvo

-U → Arquivo com a lista de usuários

-P → Arquivo com a lista de senhas

-M smbnt → Módulo SMB NT

-t 6 → Número de tentativas paralelas por usuário

-T 50 → Número total de threads

Resultado

Durante a execução do ataque, o Medusa retornou a seguinte mensagem:

2026-02-03 16:12:21 ACCOUNT FOUND:
[smbnt] Host: 192.168.2.113
User: msfadmin
Password: msfadmin
[SUCCESS (ADMIN$ - Access Allowed)]


Esse resultado indica que:

O usuário msfadmin

Possui a senha msfadmin

E foi possível acessar o compartilhamento administrativo (ADMIN$)

Portanto, as credenciais válidas foram encontradas com sucesso com base nas wordlists utilizadas.
