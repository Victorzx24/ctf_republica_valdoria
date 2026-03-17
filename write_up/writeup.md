# Write-up — República de Valdoria

## 🏳️ Primeira Flag — Fundo Soberano Secreto

A primeira flag do desafio consistia em identificar o **valor do Fundo Soberano Secreto da República Unida de Valdoria**, informação que não era apresentada explicitamente na página inicial do site.

---

## Etapa 1 — Reconhecimento

Inicialmente, foi realizada uma análise manual do site institucional, explorando suas seções, publicações e informações disponíveis ao público.  
O portal apresentava **cinco seções principais**: Início, Blog, Documentos, Contato e Alistamento, sendo uma delas de especial interesse para o desafio: a seção **Documentos**.

Nessa seção, foram identificados **cinco documentos listados**, sendo:
- **3 documentos públicos**
- **2 documentos classificados**, ambos com datas futuras de liberação previstas para **22-01-2028** e **22-01-2031**

A presença de documentos classificados listados publicamente indicou uma possível falha de segregação de informações, motivando uma investigação mais aprofundada.

---

## 📄 Documentos Identificados no Site Institucional

Durante a análise do portal institucional da República Unida de Valdoria, foram identificados os seguintes documentos acessíveis ou listados publicamente:

| Documento | Descrição | Órgão Responsável | Tamanho | Data | Classificação / Status |
|--------|-----------|-------------------|---------|------|------------------------|
| Decreto Nº 1.247 — Regulamentação do Comércio Interprovincial | Regulamenta o comércio entre as províncias, estabelecendo taxas e procedimentos alfandegários internos. | Ministério do Comércio e Indústria | 245 KB | 2025-11-15 | Público |
| Relatório Anual de Produção Agrícola — 2025 | Dados completos sobre a produção agrícola das fazendas coletivas durante o ano de 2025. | Comissariado do Setor Primário | 198 KB | 2026-01-10 | Público |
| Lei Nº 8.991 — Código de Conduta do Cidadão Valdoriano | Estabelece normas de conduta para cidadãos em território nacional e no exterior. | Assembleia Suprema de Valdoria | 312 KB | 2024-03-17 | Público |
| Memorando Interno Nº 0087/2026 — Contratação para o Setor de TI | Comunica a contratação de profissional de TI e a criação do Sistema de Gerenciamento de Recursos de Valdoria (SGRV). | Ministério da Tecnologia e Inovação | — | 2026-01-22 | Classificado — Liberação prevista: 2028-01-22 |
| Operação Aurora Boreal — Relatório de Inteligência | Relatório sobre operações de inteligência na fronteira norte. | Diretoria de Inteligência Nacional | — | — | ULTRASSECRETO — Liberação prevista: 2031-01-22 |

---

## Etapa 1.1 — Análise das Requisições e Metadados

Ao acessar qualquer documento público, o site iniciava o download do arquivo por meio de uma URL específica.  
Utilizando as ferramentas de desenvolvedor do navegador, foi possível observar que a listagem de documentos era retornada por uma **API em formato JSON**, contendo metadados completos, incluindo:

- identificador único do documento (`id`)
- título
- classificação
- data
- status de bloqueio (`locked`)
- URL de download (`downloadUrl`)

Mesmo os documentos classificados apresentavam URLs de download definidas, ainda que marcadas como bloqueadas.

A partir dessa observação, foi possível **localizar diretamente as URLs dos documentos sigilosos** e efetuar o download dos arquivos.

---

## Etapa 1.2 — Análise dos Documentos Classificados

### 📄 Memorando Interno Nº 0087/2026
📄 Relatório complementar:
- [Visualização HTML do Memorando](./memorando_de_contratacao.html)

O memorando tratava da **contratação de um Analista de Sistemas Sênior** para o Ministério da Tecnologia e Inovação, responsável pelo desenvolvimento do **Sistema de Gerenciamento de Recursos de Valdoria (SGRV)**.

O documento continha:
- fotografia do contratado
- nome completo
- cargo e setor
- data de admissão
- nível de acesso institucional

Essas informações permitiram a realização de **OSINT direcionado**, levando à identificação de perfis profissionais associados ao nome informado, incluindo redes profissionais.

---

## Etapa 1.3 — Reconhecimento Web

A partir do uso da ferramenta **Dirb**, foi realizada uma varredura de subdiretórios no domínio alvo.

**Resultado do scan inicial:**
- `/administrator` (HTTP 200 | Página acessível, porém oculta)
- `/blog` (HTTP 200 | Conteúdo público)
- `/contato` (HTTP 200 | Formulário ativo)
- `/robots.txt` (HTTP 200 | Arquivo presente)

Ao acessar o diretório `/administrator`, foi apresentada uma tela de login contendo:
- campo de identificação (nome de usuário)
- campo de senha

Além da exigência de credenciais válidas, a página utilizava **hCaptcha**, inviabilizando ataques de força bruta automatizados.

---

## Etapa 1.4 — OSINT

Com base nas informações do memorando, foram realizadas pesquisas em redes profissionais como LinkedIn e GitHub.  
Foi obtido êxito no LinkedIn, onde foi localizado um perfil correspondente ao funcionário identificado, incluindo nome e fotografia idênticos aos do memorando.

Em uma das publicações, o Analista compartilhou uma foto comemorando sua nova contratação. Na imagem, era possível visualizar um **post-it colado em seu computador contendo suas credenciais de acesso**.

**Credenciais identificadas:**
- Usuário: `Operador`
- Senha: `S3nh@V4ldoria@2026`

Com essas credenciais, foi possível realizar o login na página `/administrator` e acessar um **dashboard administrativo**, contendo informações sensíveis como:
- quantidade de agentes ativos
- contas offshore
- canais diplomáticos secretos
- monitoramento de cidadãos
- cúpulas diplomáticas agendadas
- embaixadas infiltradas
- classificação de finanças do Estado

Na seção financeira, foi identificado o **Fundo Soberano Secreto**, com o valor de **V$ 12,8 bilhões**.

---

## 🏁 Etapa Final — Conclusão da Flag 1

**Flag obtida:** Valdoria{V$_12.8_bilhoes}

A primeira flag foi encontrada, permitindo o avanço para a próxima etapa do desafio.

---

## 🏳️ Segunda Flag — Agente Secreto ECLIPSE

A segunda flag do desafio consistia em identificar o **nome real de um dos Agentes Secretos da República Unida de Valdoria**.

---

## 🔐 Operação Aurora Boreal — Relatório de Inteligência
- [Visualização HTML do Relatório de Inteligência da Operação Aurora Boreal](./relatorio_de_intel.html)

O segundo documento obtido encontrava-se **totalmente cifrado**, acompanhado de instruções explícitas de decifração, informando:

- Algoritmo: **AES-256-CBC**
- Derivação de chave: **SHA-256**
- Semente (seed): **data de emissão do documento (DD-MM-AAAA)**

Esse documento tornou-se o foco da segunda etapa do desafio, pois continha informações sensíveis sobre uma operação de inteligência, incluindo a possível identificação real de agentes secretos.

---

## Etapa 2.1 — Análise do Documento Cifrado

O arquivo apresentava um **bloco extenso de texto cifrado**, precedido por um cabeçalho que descrevia claramente:
- o algoritmo utilizado
- o método de derivação da chave
- o formato da seed

No cabeçalho do documento, a **data de emissão** estava parcialmente censurada, ocultando apenas o **dia**, enquanto mês e ano permaneciam visíveis.  
Dessa forma, foi possível obter uma seed **parcial**, restando apenas identificar o valor do dia (01 a 31).

Inicialmente, foi tentada a extração de informações adicionais por meio da análise de metadados utilizando **ExifTool**, porém sem sucesso.

---

## Etapa 2.2 — Montagem do Processo de Decifragem

Diante das informações fornecidas, foi montado o seguinte cenário criptográfico:

- **Algoritmo:** AES-256-CBC  
- **IV:** fornecido explicitamente no documento  
- **Chave:** derivada via SHA-256 a partir da seed no formato `DD-MM-AAAA`  
- **Ciphertext:** bloco hexadecimal contínuo

Utilizando a ferramenta **CyberChef**, foram configurados os parâmetros conforme descritos no documento e realizado um **brute force controlado** variando o valor do **dia (01–31)** da seed.

Após algumas tentativas, foi obtida uma decifragem válida, resultando em texto legível.

---

## Etapa 2.3 — Conteúdo Decifrado

O conteúdo decifrado revelou um **relatório ultrassecreto de inteligência**, cujo trecho mais relevante está destacado abaixo:

> **INFORMANTE INFILTRADO — IDENTIDADE REAL:**  
>  
> O agente conhecido pelo codinome **ECLIPSE**, posicionado na operação de contraespionagem em Nova Petrovsk, possui identidade civil confirmada como **Tavares Lunik Bernardi**.  
>  
> Esta informação é classificada como **ULTRASSECRETA** e sua divulgação constitui crime contra a segurança nacional.

O documento ainda listava outros agentes ativos e detalhes financeiros da operação, confirmando sua legitimidade e sensibilidade.

---

## 🏁 Etapa Final — Conclusão da Flag 2

Com a identificação do nome real do agente secreto ECLIPSE, foi possível concluir a segunda flag do desafio.

**Flag obtida:** Valdoria{Tavares_Lunik_Bernardi}


A segunda flag foi obtida com sucesso, encerrando o desafio.

## 📎 Material Complementar

- [📄 Links e superfícies de ataque](./links.html)