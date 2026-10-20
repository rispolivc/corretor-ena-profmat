# Corretor de cartões-resposta — ENA/PROFMAT 2027

App web, em um único `index.html`, que corrige os cartões-resposta do ENA a partir de PDFs digitalizados e gera as planilhas de notas e de classificação conforme o edital nº 23/2026.

## Arquivos

| Arquivo | Para que serve |
|---|---|
| `index.html` | o app |
| `gabarito.json` | gabarito oficial (`"1":"B", …`; `X` ou `ANULADA` para questão anulada) |
| `config.json` | nome da prova, nota máxima, rigor da leitura, dpi mínimo e armazenamento (local ou servidor) |
| `tesseract/` | OCR de nome e CPF (opcional; sem a pasta, o app usa a CDN) |
| `ESPECIFICACAO_API.md` | API para salvar cartões e planilhas no servidor |

## Como usar

1. **Carregar PDF de cartões.** Use digitalização em mesa (scanner), com pelo menos 200 dpi. PDFs abaixo de 150 dpi são recusados por inteiro.
2. **Carregar lista de inscritos (.xlsx), opcional.** É a planilha do SCA. O app confere nome e CPF de cada cartão com a lista e lê nascimento, rede pública/privada, quadro permanente, raça/cor e gênero.
3. **Revisar.** Os cartões com alerta aparecem em "Revisar". Clique na alternativa para corrigir e marque como revisado.
4. **Exportar.**

## O que o app lê

- **Nome e CPF:** por OCR, inclusive em faixas de fundo escuro.
- **Respostas A–E:** alinhamento automático por rotação e escala, com tolerância a rasuras e às letras impressas dentro dos balões.
- **Dupla marcação:** inclui rasura, X e traço em outra alternativa. A questão vale 0.
- **Campo cinza da comissão pintado, inteiro ou em parte:** a questão é anulada para o candidato e vale 0.
- **Questão anulada no gabarito:** o ponto vai para todos (item 6.8).
- **Ausente e Eliminado.**

Toda leitura duvidosa gera alerta. Nada é corrigido em silêncio.

## O que o app gera

**Sem lista de inscritos:** planilha `.xlsx` com nome, CPF, acertos, nota, situação, alertas e a resposta de cada questão.

**Com a lista do SCA:** a própria planilha do SCA, com a formatação original, acrescida de:

- **Aba original** com as colunas **Nota**, **Lista** e **Posição**, e as linhas já na ordem de classificação.
- **Aba Resultados**, de uso interno, com lista, posição, nome, CPF mascarado, CPF, nascimento, rede, quadro permanente, raça/cor, gênero, nota, situação, critério de desempate e respostas.

A ordem segue o edital:

1. **Lista 1:** rede pública, aprovados, dentro das vagas (o número vem do Anexo 2 e é confirmado na exportação).
2. **Lista 2:** rede pública, aprovados além das vagas.
3. **Lista 3:** somente rede privada, aprovados.
4. Aprovados sem rede informada (conferir), reprovados (menos de 50%), eliminados e ausentes.

Quem atua nas redes pública e privada entra como rede pública. Dentro de cada lista, a ordem é a nota e, no empate (item 7.1.1): 60 anos ou mais (o mais velho primeiro), depois quadro permanente da rede pública, depois data de nascimento mais antiga.

**Outros arquivos:** PDF com todos os cartões anotados e PDF com gráficos de frequência por questão.

## Atenção

- A aba Resultados tem nome e CPF completo. **Não publique essa aba.**
- A lista de notas do item 6.5 leva só CPF mascarado e nota.
- As cotas de cada instituição não são aplicadas.
- No modo local, os dados ficam só no navegador. Exporte antes de limpar o navegador.
- Não commite planilhas nem PDFs de candidatos no repositório.
