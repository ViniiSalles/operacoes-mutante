## Objetivo rápido

Este repositório é um laboratório simples para teste de mutação ("Calculadora Mutante"). Contém 50 funções matemáticas em `src/operacoes.js` e uma suíte de testes em `test/operacoes.test.js`. A principal ferramenta de CI/developer é Jest; a atividade principal esperada é melhorar testes para sobreviver à análise de mutação (StrykerJS).

## O que você precisa saber imediatamente

- Implementação central: `src/operacoes.js` — 50 funções exportadas via `module.exports` (todas nomeadas). O arquivo está organizado em blocos lógicos (Operações básicas, Arrays, Trig, Teoria dos números, Geometria).
- Testes: `test/operacoes.test.js` importa funções por nome e contém 50 testes "de superfície" (cada função tem ao menos um teste). Esses testes tendem a afirmar apenas um caso feliz por função — por isso muitos mutantes sobrevivem.
- Scripts úteis (em `package.json`):
  - `npm test` -> executa Jest
  - `npm run coverage` -> roda Jest com coverage

Nota rápida: `package.json` declara "type": "module" e o código usa `module.exports` + `require` no teste — Jest + `babel-jest` já está configurado no projeto para transformar os arquivos, por isso os imports/exports funcionam no ambiente de teste atual.

## Arquitetura e convensões específicas do projeto

- Design: código monolítico de utilitários matemáticos (único arquivo de implementação). Não há camadas separadas — foco é didático para testes.
- Convenção de nomes: funções usam nomes em português (ex.: `soma`, `raizQuadrada`, `isPrimo`). Use esses mesmos nomes ao escrever testes ou ao referenciar funções.
- Organização de testes: todos os testes estão em `test/operacoes.test.js` (não em `__tests__`). Ao adicionar novos testes, mantenha-os no mesmo diretório e arquivo ou crie arquivos adicionais em `test/` seguindo o mesmo padrão de import.
- Erros/validações: várias funções lançam erros em condições específicas (ex.: `divisao(..., 0)`, `raizQuadrada(-1)`, `fatorial(-1)`, `inverso(0)`, arrays vazios para `maximoArray`). Escrever testes que verifiquem mensagens de erro específicas melhora a força dos testes contra mutantes.

## Fluxo de trabalho recomendado (dev/CI)

1. Instalar dependências: `npm install`.
2. Executar testes rápidos: `npm test`.
3. Ver cobertura: `npm run coverage` e inspecione quais branches/linhas não estão sendo exercitados.
4. Configurar Stryker (se ainda não existir): `npx stryker init` e escolha um preset adequado (Jest/Babel).
5. Rodar análise de mutação: `npx stryker run` (ou `npm run mutate` se você adicionar esse script). Abra o relatório HTML gerado e observe mutantes sobreviventes.
6. Para cada mutante sobrevivente: localizar o trecho em `src/operacoes.js`, entender por que o teste atual não o detecta e escrever testes adicionais em `test/` cobrindo:
   - valores de fronteira (ex.: zeros, negativos, arrays vazios)
   - erros lançados e mensagens (use toThrow/ toThrowErrorMatching)
   - múltiplas entradas (valores negativos, floats, grandes)

## Exemplos práticos (concisos)

- Cobertura de erro: para `divisao`, além de assertar o resultado esperado, escreva um teste específico para a divisão por zero e, se aplicável, verifique a mensagem do erro:

- Para funções de array como `maximoArray`/`minimoArray`, crie testes para arrays vazios e arrays com valores negativos e duplicados.

- Para `fibonacci` e `fatorial`, adicione casos base e limites (0,1, valores maiores que 10) e testes que confirmem comportamento recursivo/explosão de tempo quando aplicável.

## Padrões e armadilhas observadas aqui

- Testes atuais tendem a checar apenas "happy path"; focar em asserts mais específicas mata muitos mutantes (ex.: usar toBeCloseTo para números de ponto flutuante, checar exceções específicas, checar comportamento em arrays vazios).
- `fibonacci` é implementado recursivamente sem memoização — testes grandes podem ser lentos; prefira limites razoáveis (ex.: n <= 20) nas assertivas de desempenho.
- Mistura ESM/CJS declarada em `package.json` é mascarada por Babel/Jest; não altere isso sem entender o efeito em CI.

## Integrações e dependências externas

- DevDependencies principais: `jest`, `babel-jest`, `@babel/preset-env`. Nenhuma integração externa (DB, API) está presente — ambiente local é suficiente.
- Ferramenta recomendada para análise de mutação: StrykerJS (configurar via `npx stryker init`). Relatórios HTML são a principal fonte para priorizar testes a adicionar.

## O que perguntar ao mantenedor (quando incerto)

- Deseja manter `type: "module"` no package.json ou migrar para CommonJS puro? (pode afetar execução fora do Jest)
- Limites de performance aceitáveis para testes de mutação (tempo por run e número de mutantes a tolerar).

## Como validar que você fez um bom trabalho

- `npm test` continua verde.
- Cobertura (`npm run coverage`) não precisa ser 100% mas deve subir quando novos testes tiverem foco em caminhos não testados.
- Pontuação de mutação (Stryker) melhora — objetivo didático: matar a maioria dos mutantes sobreviventes e explicar por que os restantes são difíceis de testar.

Se algo não estiver claro ou você quiser que eu adicione exemplos de testes concretos (ex.: um teste adicional para `divisao` ou `maximoArray`), diga qual função você quer que eu cubra e eu adiciono o teste de exemplo aqui ou diretamente em `test/`.
