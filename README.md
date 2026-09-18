# Burocracia e Poesia — Projeto de Redes

Projeto desenvolvido no Cisco Packet Tracer, inspirado em *O Guia do Mochileiro das Galáxias*. A rede simula a infraestrutura de acesso em um contexto de nave Vogon, com segmentação por VLANs, roteamento inter-VLAN, serviços HTTP e DNS, além de regras de controle de acesso. Para mais detalhes da referência do livro usada neste projeto, acesse esse mesmo md, mas depois das descrições técnicas. [|:¬)

## Topologia

O switch `HEART_OF_GOLD` conecta os computadores e servidores ao roteador `DEEP_THOUGHT`. O roteador faz o encaminhamento entre as VLANs por meio de uma ligação trunk. Os serviços de DNS e HTTP públicos ficam no servidor `MARVIN`; a nave Vogon possui um servidor interno próprio.

## Equipamentos

| Equipamento | Nome | Função |
|---|---|---|
| Roteador Cisco 2911 | `DEEP_THOUGHT` | Roteamento inter-VLAN e aplicação de ACLs |
| Switch Cisco 2960 | `HEART_OF_GOLD` | Segmentação e conexão dos dispositivos |
| Servidor | `MARVIN` | DNS e página HTTP pública |
| Servidor | `VOGON_SHIP` | Terminal HTTP interno da nave Vogon |
| PC | `ARTHUR_DENT` | Administração |
| PC | `FORD_PREFECT` | Auxiliar |
| PC | `VOGON_POET` | Nave Vogon |

## VLANs e endereçamento

| VLAN | Nome | Rede | Gateway | Dispositivo principal |
|---:|---|---|---|---|
| 42 | `ANSWER` | `10.42.42.0/24` | `10.42.42.1` | `ARTHUR_DENT` — `10.42.42.10` |
| 7 | `PANGALACTIC` | `10.42.7.0/24` | `10.42.7.1` | `FORD_PREFECT` — `10.42.7.10` |
| 13 | `VOGON` | `10.42.13.0/24` | `10.42.13.1` | `VOGON_POET` — `10.42.13.10` |
| 99 | `MARVIN` | `10.42.99.0/24` | `10.42.99.1` | `MARVIN` — `10.42.99.10` |

O servidor `VOGON_SHIP` pertence à VLAN 13 e utiliza o endereço `10.42.13.20`.

## Serviços e domínios

O servidor `MARVIN` oferece DNS e HTTP.

| Domínio | Endereço | Acesso |
|---|---:|---|
| `poesia.vogon.local` | `10.42.99.10` | Arthur, Ford e Vogon |
| `leia.vogon.local` | `10.42.13.20` | Apenas o usuário Vogon |

`poesia.vogon.local` apresenta o Arquivo Oficial de Poesia Vogon. Já `leia.vogon.local` é o terminal interno da nave Vogon, com normas burocráticas e poesia destinada exclusivamente à tripulação.

## Políticas de acesso

As ACLs configuradas no roteador aplicam as seguintes regras:

- A VLAN `VOGON` não pode acessar as redes `ANSWER` e `PANGALACTIC`.
- O usuário Vogon pode consultar o DNS no servidor `MARVIN` e acessar somente HTTP em `poesia.vogon.local`.
- O usuário Vogon não consegue usar ping no servidor `MARVIN`.
- Arthur e Ford não podem acessar o endereço `10.42.13.20`, que hospeda o terminal interno da nave Vogon.
- Apenas o `VOGON_POET`, por estar na VLAN 13, acessa `leia.vogon.local`.

## Testes realizados

| Teste | Resultado esperado |
|---|---|
| `ARTHUR_DENT` faz ping em `FORD_PREFECT` | Permitido, com roteamento entre VLANs |
| `VOGON_POET` faz ping em `ARTHUR_DENT` | Bloqueado pela ACL |
| `VOGON_POET` faz ping no `MARVIN` | Bloqueado pela ACL |
| `VOGON_POET` abre `http://poesia.vogon.local` | Permitido por DNS e HTTP |
| `VOGON_POET` abre `http://leia.vogon.local` | Permitido, pois está na VLAN 13 |
| Arthur ou Ford tentam acessar `10.42.13.20` | Bloqueado pela ACL |

## Conceitos demonstrados

- Endereçamento IPv4 e gateways padrão;
- Switch e portas de acesso;
- VLANs e trunk 802.1Q;
- Roteamento inter-VLAN com subinterfaces;
- Servidor HTTP e DNS;
- ACLs estendidas para controle de acesso;
- Validação com ping, navegador web e resolução de nomes.









# AH, PORQUÊ ESCOLHER VOGONS??????

> ¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨
>
> **NÃO ENTRE EM PÂNICO!!**
>
> ¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨

## Por que Vogons?

A escolha para este projeto foi uma cena do primeiro livro da série do **Guia do Mochileiro das Galáxias**. Explico o contexto rapidinho abaixo:

Arthur Dent é amigo de Ford Perfect e descobre, em uma situação normal e completamente absurda, que perderá sua casa, seu planeta e que seu amigo é um alienígena.

*(Também pudera, com o nome Ford Perfect!)*

Ambos tentam sair da Terra para não serem obliterados. Os algozes são os **Vogons**, que anunciam, com a voz mais cansada, repetitiva, burocrática e sacripanta possível, que a Terra será demolida para ser feita uma obra na região e que eles já haviam protocolado todo o processo antes.

Se os terráquios não responderam ao protocolo, não era problema Vogon.

Como bons burocratas que adoram criar pontes, prédios e coisas grandes de natureza estranha e não natural, iriam até o fim com a ação.

Assim, para escapar, Arthur conta com os conhecimentos extraterrestres de seu amigo Ford, e este carrega consigo o **Guia do Mochileiro das Galáxias**.

Esse guia é muito útil e tem na capa uma mensagem importante e muito relevante:

> **"NÃO ENTRE EM PÂNICO".**

Depois de algumas confusões de transporte para fora da Terra, Arthur e Ford estão salvos.

Mas...

**Estão dentro da nave Vogon!**

---

## E agora?

Não vou dar mais spoiler da história, caso você queira ler o livro.

Só o que você precisa saber daqui em diante é:

- Os **Vogons amam recitar sua poesia**.
  - Ela é chata.
  - Chatíssima.
  - Cansativa.
  - Repetitiva.
  - E qualquer distúrbio — nem a menor tossezinha é tolerada — faz com que o declamador recomece a leitura.

- Ninguém consegue aguentar a poesia dos Vogons, por isso é um **ato obrigatório**.

- Arthur e Ford sofrem.
  - Infelizmente, eles têm que ouvir a poesia.
  - De camarote ainda por cima.

- Os Vogons amam, além de sua poesia cheia de *rembimbotas da parafuseta*, **burocracia, formulários, processos longos e muita, muita, muita mesmo, espera**.

Então, você imagina em que sinuca de bico os dois protagonistas se encontram.

---

## Onde o projeto entra nisso?

É nesse momento que o esquema que criei está situado.

Os dois estão lá, ouvindo a **poesia Vogon**, como na mensagem que você vê na imagem:

`arthur-dent_access.png`

E estão tentando aguentar toda a tortura.

Os Vogons, como bons burocratas, só se preocupam com suas próprias coisas e, portanto, não só escreveram a mensagem para os viajantes como também têm seu comunicado interno!

Você pode ver a mensagem que eles acessam na imagem:

`vogon_msg.png`

---

## E aqui termina o contexto!

Espero que você ache legal essa referência e, se puder, dê uma lidinha no livro.

Tem menos poesia Vogon do que parece, prometo!!!

**Até!!**

**Clarissa**

## Execução

Abra o arquivo `.pkt` no Cisco Packet Tracer. Para acessar os serviços por nome, mantenha `10.42.99.10` configurado como servidor DNS nos PCs autorizados.

