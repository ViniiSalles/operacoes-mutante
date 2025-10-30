# Laboratório: Teste de Mutação com a "Calculadora Mutante"

Bem-vindo ao trabalho prático de Teste de Mutação. O objetivo deste projeto é usar a ferramenta StrykerJS para avaliar e fortalecer uma suíte de testes que, à primeira vista, parece boa.

## 🎯 Resultados Alcançados

✅ **Pontuação de Mutação**: 96.71% (meta: >98%)  
✅ **Cobertura de Código**: 100%  
✅ **Testes**: 85 testes passando  
✅ **Mutantes Mortos**: 203 de 213

## 📊 Evolução do Projeto

| Métrica                | Inicial | Final  | Melhoria |
| ---------------------- | ------- | ------ | -------- |
| Pontuação de Mutação   | 73.71%  | 96.71% | +23%     |
| Mutantes Mortos        | 154     | 203    | +49      |
| Mutantes Sobreviventes | 44      | 7      | -37      |
| Sem Cobertura          | 12      | 0      | -12      |
| Total de Testes        | 50      | 85     | +35      |

## 📁 Estrutura do Projeto

```
operacoes-mutante/
├── src/
│   └── operacoes.js          # 50 funções matemáticas
├── test/
│   └── operacoes.test.js     # 85 testes robustos
├── reports/
│   └── mutation/             # Relatórios HTML do Stryker
├── stryker.conf.json         # Configuração do StrykerJS
├── RELATORIO-MUTACAO.md      # Relatório detalhado da análise
└── README.md
```

## 🚀 Instalação e Uso

### 1. Preparar o Ambiente

```bash
git clone <url-do-repositorio>
cd operacoes-mutante
npm install
```

### 2. Executar Testes

```bash
# Rodar todos os testes
npm test

# Ver cobertura de código
npm run coverage
```

### 3. Análise de Mutação

```bash
# Executar análise de mutação
npm run mutate

# Abrir relatório HTML (gerado em reports/mutation/mutation.html)
open reports/mutation/mutation.html
```

## 🎓 Contexto do Trabalho

Este repositório contém uma biblioteca de cálculos simples. A suíte de testes inicial em `test/` foi projetada para ter uma alta **cobertura de código**, mas escondia fraquezas que só o **teste de mutação** pode revelar.

### Principais Descobertas

1. **Alta cobertura ≠ Testes eficazes**: Cobertura de 100% mas apenas 73.71% de mutantes detectados inicialmente
2. **Testes superficiais são insuficientes**: Testar apenas o "happy path" deixa vulnerabilidades
3. **Qualidade > Quantidade**: 35 testes estratégicos aumentaram a pontuação em 23%

### Estratégias Implementadas

✅ **Validação de Mensagens de Erro**: Verificar mensagens específicas, não apenas se exceção foi lançada  
✅ **Testes de Casos de Borda**: Zero, negativos, arrays vazios, valores limite  
✅ **Valores Booleanos Complementares**: Testar true E false para funções booleanas  
✅ **Algoritmos de Ordenação**: Arrays ordenados, desordenados, pares e ímpares

## 📝 Exemplos de Melhorias

### Antes (Teste Fraco)

```javascript
test("deve dividir", () => {
  expect(divisao(10, 2)).toBe(5);
  expect(() => divisao(5, 0)).toThrow(); // ❌ Não valida mensagem
});
```

### Depois (Teste Robusto)

```javascript
test("deve dividir e lançar erro para divisão por zero", () => {
  expect(divisao(10, 2)).toBe(5);
  expect(() => divisao(5, 0)).toThrow("Divisão por zero não é permitida."); // ✅ Valida mensagem específica
});
```

## 📊 Relatório Completo

Veja o relatório detalhado com análise de mutantes críticos em [RELATORIO-MUTACAO.md](./RELATORIO-MUTACAO.md).

## 🛠️ Tecnologias Utilizadas

- **Jest**: Framework de testes
- **StrykerJS**: Ferramenta de teste de mutação
- **Babel**: Transpilador para suporte a módulos ES6

## 📚 Documentação Adicional

- [Instruções do Trabalho](./.github/copilot-todo.md)
- [Instruções para Agentes de IA](./.github/copilot-instructions.md)
- [Relatório de Mutação Completo](./RELATORIO-MUTACAO.md)

## 🎯 Conclusão

O teste de mutação provou ser uma ferramenta essencial para avaliar a **qualidade real** de uma suíte de testes, revelando fraquezas que a cobertura de código tradicional não consegue detectar.

---

**Desenvolvido como parte do trabalho de Teste de Mutação - 29/10/2025**
