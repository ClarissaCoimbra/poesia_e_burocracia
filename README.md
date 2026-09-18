
# Poesia e Burocracia — Projeto de Redes em una nave Vogon



Projeto de redes desenvolvido no Cisco Packet Tracer, inspirado em
*O Guia do Mochileiro das Galáxias*. Para entender mais o contexto da referência no projeto, acesse o documento .pdf

## Componentes

- `DEEP_THOUGHT`: roteador inter-VLAN
- `HEART_OF_GOLD`: switch central
- `MARVIN`: servidor HTTP e DNS
- `ARTHUR_DENT`: administração
- `FORD_PREFECT`: restaurante
- `VOGON_POET`: visitante restrito

## VLANs

| VLAN | Nome | Rede |
|---:|---|---|
| 42 | ANSWER | 10.42.42.0/24 |
| 7 | PANGALACTIC | 10.42.7.0/24 |
| 13 | VOGON | 10.42.13.0/24 |
| 99 | MARVIN | 10.42.99.0/24 |

## Regras

A VLAN VOGON não acessa as redes de Administração e da rede geral (punição pela poesia longa e cansativa).
Ela pode consultar o DNS e abrir apenas a página HTTP do servidor
`MARVIN`, disponível em `http://poesia.vogon.local`.
