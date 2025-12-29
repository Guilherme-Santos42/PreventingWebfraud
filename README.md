# PreventingWebfraud
An event that required investigation!

Identifiquei um site tentando se passar por um site de sorteios da caixa (site do governo):

Primeiro passo reportar a informação ao google em https://safebrowsing.google.com/safebrowsing/report_phish/?authuser=1:


<img width="1094" height="703" alt="Screenshot 2025-12-28 135747" src="https://github.com/user-attachments/assets/9a9cecf9-579f-4f46-8486-fce7b5e4168e" />

Ao acessar o site recebo esta mensagem, identificando realmente que o site é um esquema de phishing:
<img width="1429" height="533" alt="Screenshot 2025-12-28 135705" src="https://github.com/user-attachments/assets/42b34e8b-b6f6-4564-8676-15cdddfbc8ec" />


Utilizei:
sudo hping3 -S --flood -V -p 80 45.61.157.104
e
slowhttptest -c 1000 -H -g -o slowloris_stats -i 10 -r 200 -t GET -u http://45.61.157.104 -x 24 -p 3
O site caiu temporariamente.... 
<img width="700" height="404" alt="Screenshot 2025-12-28 140531" src="https://github.com/user-attachments/assets/b53eaf2d-4d34-4f97-93d0-f408ff2943fc" />

mas retornou depois de algum tempo! Entendemos que deve ter algum load balancer ou waf realizando algum bloqueio! Então corri atrás de usar o Burp suite e realizei alguns testes com SQL injection, mas nenhum deles com sucesso!

Após isso parti para o esgotamento de recursos da API, já que identifiquei no site que uma api pública era utilizada. E também identifiquei na página de pagamento o link de api e realizei contato com a equipe para desabilitarem a api utilizada no scam!

