# Burocracia e Poesia — Projeto de Redes

Projeto desenvolvido no Cisco Packet Tracer, inspirado em *O Guia do Mochileiro das Galáxias*. A rede simula a infraestrutura do Restaurante no Fim do Universo, com segmentação por VLANs, roteamento inter-VLAN, serviços HTTP e DNS, além de regras de controle de acesso. Para mais detalhes da referência do livro usada neste projeto, acesse o arquivo referencias.txt

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

## Execução

Abra o arquivo `.pkt` no Cisco Packet Tracer. Para acessar os serviços por nome, mantenha `10.42.99.10` configurado como servidor DNS nos PCs autorizados.

