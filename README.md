# Overflow Awards — votação

Votação online da premiação "OVERFLOW AWARDS" (retiro da Rede Flow). App
irmão do [TalkFlow 01](https://github.com/filipebiten/talkflow) e do
[Velhos Fins — TalkFlow 02](https://github.com/filipebiten/velhosfins):
mesma arquitetura — single-file HTML, Preact + htm via ESM (esm.sh), sem
build, sem npm — sincronização via Firebase Realtime Database (mesmo
projeto `talkflow-dba50`, path próprio `talkflow/overflowawards` pra não
colidir com os outros apps).

Diferente dos apps anteriores: aqui não tem telão nem painel operados ao
vivo. São duas views sem ligação entre si — a votação acontece sozinha, nos
dias antes do evento, pelo celular de cada pessoa.

## As duas views

- `?view=votar` — é o link que circula no WhatsApp. Cada pessoa abre no
  próprio celular e vota nas 15 categorias.
- `?view=resultado` — acompanhamento privado, só pra mim. Mostra o ranking
  de cada categoria em tempo real, permite moderar e exportar o resultado.

## Deploy no GitHub Pages

1. Commit e push do `index.html` (e este README).
2. Settings → Pages → Deploy from branch.
3. Compartilhe a URL publicada + `?view=votar` no grupo do WhatsApp.

## Como funciona a votação (pra quem vota)

Uma categoria por tela, com barra de progresso. Ao digitar um nome, aparecem
sugestões de nomes já votados naquela categoria — clicar numa sugestão
confirma o voto e avança. Digitar um nome novo e apertar **Confirmar**
também avança; se o nome for parecido com um já existente, o app pergunta
antes de criar um novo ("Nome parecido já votado: ... É essa pessoa?").

Sem login. O celular é identificado por um id salvo no navegador — recarregar
a página retoma de onde parou. **← Voltar** deixa mudar um voto já dado,
desde que a pessoa ainda não tenha terminado a última categoria. Dá pra
**pular** categorias que a pessoa não sabe responder. Ao terminar a 15ª
categoria (ou pular), cai direto na tela de encerramento — o resultado nunca
é mostrado pro votante.

## Como acompanhar o resultado

Abra `?view=resultado`. Atualiza em tempo real conforme as pessoas votam.
Em cada categoria: ranking completo, nº de votos, 1º colocado em destaque.

**Moderação**, por categoria:
- **fundir com…** — se duas grafias são a mesma pessoa, funde os votos num
  nome só (soma as contagens, mantém o nome de destino).
- **-1** — remove um voto daquele nome (correção pontual).
- **✕** — exclui o nome inteiro e todos os votos dele.

**Exportar JSON** gera o arquivo no formato que o app da premiação (telão)
vai ler depois — ranking completo de cada categoria, na ordem das 15
categorias configuradas.

## Como testar sem sujar os dados reais

Adicione `?teste=1` na URL de qualquer view (ex.: `?view=votar&teste=1`).
Isso usa um namespace separado no Firebase. Na view de resultado em modo
teste aparece um botão **Limpar tudo**, que zera esse namespace (só
funciona em modo teste).

## Editar as categorias

Array `CATEGORIAS` no topo do `<script>` do `index.html` — cada item tem
`id`, `premio` e `explicacao`.
