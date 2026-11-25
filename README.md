# Em Busca do Match Perfeito · Workshop de CRM

Dinâmica no estilo Tinder para o workshop de CRM, publicada pelo GitHub Pages.

- Jogo (QR Code dos participantes): https://nicollymcosta26-cyber.github.io/match-perfeito-crm/
- Telão do ranking: https://nicollymcosta26-cyber.github.io/match-perfeito-crm/?tela=ranking
- Área do administrador: https://nicollymcosta26-cyber.github.io/match-perfeito-crm/?tela=admin
- Apresentação do workshop: https://nicollymcosta26-cyber.github.io/match-perfeito-crm/apresentacao/ (F para tela cheia, N para o roteiro do narrador)

O jogo inteiro está no arquivo `index.html`, e a apresentação em `apresentacao/index.html`. Para atualizar, substitua esse arquivo mantendo exatamente esse nome.
Não apague o `index.html`: sem ele, o GitHub Pages não tem página para abrir.

## Regras do jogo

| Situação | Pontos |
|---|---|
| Acerto nos filtros (3) | +100 cada |
| Acerto nos perfis (8) | +100 cada |
| Acerto no chat com a Helena (6) | +200 cada |
| Super like na Helena (só existe 1 super like por partida) | +200 de bônus |
| Like em contato desatualizado, duplicado ou descadastrado | −50 |
| Deixar a Helena passar | Game Over (vai para o fim da lista e não entra no pódio) |

Máximo: 2.500 pontos. Ganham as 3 maiores pontuações. Empate: vence quem decidiu mais rápido. O relógio só corre enquanto a pessoa decide e para durante as explicações e as falas da Helena.
"Mais rápidos" mostra quem terminou com pelo menos 12 acertos, pelo menor tempo de decisão. Só a primeira partida de cada celular conta; as outras são treino.

## Encerrar a rodada

Na área do administrador, "Encerrar rodada" (pede a senha) começa uma contagem de 30 s, 1 min ou 2 min:

1. O telão congela as posições e mostra quem ainda está jogando.
2. Quem está jogando vê a contagem no celular.
3. No zero, a partida de quem não terminou fecha com os pontos que já tem.
4. O telão revela o pódio: 3º, 2º e 1º lugar.

"Zerar ranking" começa uma rodada nova.

## Regras do Firebase

Em Firebase › Realtime Database › Regras, cole o texto abaixo e clique em Publicar. Ele aceita pontuações até 2.500, só deixa encerrar ou zerar a rodada com a senha do admin e não mostra a senha para os participantes. A área do administrador avisa, em vermelho, se as regras publicadas ainda forem antigas.

```json
{
  "rules": {
    "salas": {
      "$sala": {
        "jogadores": {
          ".read": true,
          "$id": {
            ".write": true,
            ".validate": "newData.child('nome').isString() && newData.child('nome').val().length <= 20 && newData.child('pts').isNumber() && newData.child('pts').val() >= 0 && newData.child('pts').val() <= 2500"
          }
        },
        "rodada": {
          ".write": "newData.child('chave').val() === 'crm2026:' + newData.child('valor').val()",
          "valor": { ".read": true },
          "fim": { ".read": true }
        }
      }
    }
  }
}
```

Se trocar a senha do admin, troque também o `crm2026` dentro da regra.
