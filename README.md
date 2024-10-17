# Разведка поддоменов из свободных ресурсов
	
 ФОТОН
    https://spy-soft.net/photon-web-crawler-osint/

    - через DNS
    
    subfinder -d hilton.com
    
    sublist3r -d hilton.com
	--- Поиск доменов по ip
    https://dnsdumpster.com/
# Брут директрий и доменов

Основной словарик каталогов

https://github.com/mxdelta/SecLists/blob/master/Discovery/Web-Content/big.txt

утилиты

wpscan --url://192.168.50.200/wordpress/ --wp-content-dir -at -eu  (все директории и все плагины для вордпресс)

gobuster dir -u http://192.168.50.13 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -e -k -x txt,html,php,css,js,sh,py,cgi,db -t 50

gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -k -u https://watch.streamio.htb/ -x php

gobuster dir -u 10.129.249.156 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t 10 -x php,html,bak,txt

gobuster vhost --append-domain -w /usr/share/amass/wordlists/subdomains-top1mil-5000.txt -u http://thetoppers.htb 

gobuster dns -w word.txt -d hilton.com -i (брут по днс)


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


# WEB

Web Shell

https://github.com/jivoi/pentest/blob/master/shell/insomnia_shell.aspx

# словарик для бурпа - к примеру заголовки

	https://github.com/PortSwigger/param-miner/tree/master

 # Смена IP при запрсе 

 	X-Forwarded-For: 1
  
