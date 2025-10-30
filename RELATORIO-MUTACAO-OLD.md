# Relatório de Teste de Mutação - Calculadora Mutante

## Informações do Projeto

- **Repositório**: operacoes-mutante
- **Data de Análise**: 29 de outubro de 2025
- **Ferramenta**: StrykerJS v9.3.0 com Jest

## 1. Análise Inicial

### Cobertura de Código Inicial

- **Testes Iniciais**: 50 testes (1 por função)
- **Cobertura**: ~100% de linhas (testes superficiais)
- **Característica**: Testes fracos focados apenas em "happy path"

### Pontuação de Mutação Inicial

- **Score**: 73.71%
- **Mutantes Totais**: 213
- **Mutantes Mortos**: 154
- **Mutantes Sobreviventes**: 44
- **Mutantes Sem Cobertura**: 12
- **Timeouts**: 3

### Discrepância Observada

Embora a cobertura de código fosse alta (~100%), a pontuação de mutação revelou que **44 mutantes sobreviveram**, indicando que os testes não eram suficientemente robustos para detectar alterações sutis no código.

## 2. Análise de Mutantes Críticos

### Mutante #1: Validação de Mensagens de Erro (Divisão por Zero)

**Localização**: `src/operacoes.js:8`

**Mutação**:

```javascript
// Original
if (b === 0) throw new Error("Divisão por zero não é permitida.");

// Mutante
if (b === 0) throw new Error("");
```

**Por que sobreviveu?**
O teste original apenas verificava se uma exceção era lançada (`expect(() => divisao(5, 0)).toThrow()`), mas não verificava a **mensagem específica do erro**.

**Solução Implementada**:

```javascript
test("4. deve dividir e lançar erro para divisão por zero", () => {
  expect(divisao(10, 2)).toBe(5);
  expect(() => divisao(5, 0)).toThrow("Divisão por zero não é permitida.");
});
```

**Resultado**: Mutante morto ✅

---

### Mutante #2: Lógica de Condições (isPrimo)

**Localização**: `src/operacoes.js:73-76`

**Mutação**:

```javascript
// Original
function isPrimo(n) {
  if (n <= 1) return false;
  for (let i = 2; i < n; i++) {
    if (n % i === 0) return false;
  }
  return true;
}

// Mutantes:
// 1. n <= 1 → n < 1
// 2. i < n → i >= n
// 3. return false → return true
```

**Por que sobreviveram?**
O teste original apenas verificava um caso positivo (`isPrimo(7) === true`), não testando:

- Caso de borda: n = 0, n = 1
- Número primo menor: n = 2
- Números não primos: n = 4, n = 6

**Soluções Implementadas**:

```javascript
test("33b. deve retornar false para número não primo", () => {
  expect(isPrimo(4)).toBe(false);
});
test("33c. deve retornar false para 0", () => {
  expect(isPrimo(0)).toBe(false);
});
test("33d. deve retornar false para 1", () => {
  expect(isPrimo(1)).toBe(false);
});
test("33e. deve retornar true para 2", () => {
  expect(isPrimo(2)).toBe(true);
});
```

**Resultado**: 6 mutantes mortos ✅

---

### Mutante #3: Operadores de Comparação (clamp)

**Localização**: `src/operacoes.js:88-89`

**Mutação**:

```javascript
// Original
function clamp(valor, min, max) {
  if (valor < min) return min;
  if (valor > max) return max;
  return valor;
}

// Mutantes:
// 1. valor < min → valor <= min
// 2. valor > max → valor >= max
```

**Por que sobreviveram?**
O teste original apenas verificava um valor dentro do intervalo (`clamp(5, 0, 10) === 5`), não testando:

- Valor igual ao mínimo: `clamp(0, 0, 10)`
- Valor igual ao máximo: `clamp(10, 0, 10)`
- Valor abaixo do mínimo: `clamp(-5, 0, 10)`
- Valor acima do máximo: `clamp(15, 0, 10)`

**Soluções Implementadas**:

```javascript
test("36b. deve retornar min quando valor é menor que min", () => {
  expect(clamp(-5, 0, 10)).toBe(0);
});
test("36c. deve retornar max quando valor é maior que max", () => {
  expect(clamp(15, 0, 10)).toBe(10);
});
test("36d. deve retornar min quando valor igual a min", () => {
  expect(clamp(0, 0, 10)).toBe(0);
});
test("36e. deve retornar max quando valor igual a max", () => {
  expect(clamp(10, 0, 10)).toBe(10);
});
```

**Resultado**: 2 de 4 mutantes mortos (2 ainda sobrevivem - equivalentes) ⚠️

---

## 3. Estratégias de Melhoria Implementadas

### 3.1 Validação de Mensagens de Erro

- Alteramos todos os testes que verificam exceções para validar a **mensagem específica**
- Total: 6 funções melhoradas (divisao, raizQuadrada, fatorial, inverso, maximoArray, minimoArray, medianaArray)

### 3.2 Testes de Casos de Borda

- Adicionamos testes para valores limite:
  - Zero (raizQuadrada(0), fatorial(0), mediaArray([]))
  - Negativos (raizQuadrada(-1), fatorial(-1))
  - Arrays vazios (maximoArray([]), produtoArray([]))

### 3.3 Testes de Valores Booleanos Complementares

- Para funções booleanas, adicionamos testes para ambos os casos:
  - isPar: teste para par **E** para ímpar
  - isImpar: teste para ímpar **E** para par
  - isMaiorQue, isMenorQue, isEqual: testes para casos verdadeiros e falsos

### 3.4 Testes de Algoritmos de Ordenação

- medianaArray: testamos com arrays ordenados, desordenados, pares e ímpares
- Total: 4 testes adicionais cobrindo diferentes cenários

## 4. Resultados Finais

### Pontuação de Mutação Final

- **Score**: 96.71% ✅ (Aumento de 23%)
- **Mutantes Totais**: 213
- **Mutantes Mortos**: 203 (+49)
- **Mutantes Sobreviventes**: 7 (-37)
- **Mutantes Sem Cobertura**: 0 (-12)
- **Timeouts**: 3
- **Total de Testes**: 85 (+35 novos testes)

### Evolução da Pontuação

| Métrica         | Inicial | Final  | Melhoria |
| --------------- | ------- | ------ | -------- |
| Score           | 73.71%  | 96.71% | +23%     |
| Mutantes Mortos | 154     | 203    | +49      |
| Sobreviventes   | 44      | 7      | -37      |
| Sem Cobertura   | 12      | 0      | -12      |
| Total de Testes | 50      | 85     | +35      |

### Gráfico de Melhoria

```
Pontuação de Mutação:
Inicial: ███████████████████████░░░░░░ 73.71%
Final:   ████████████████████████████░░ 96.71%
```

## 5. Mutantes Sobreviventes Remanescentes

Dos 7 mutantes que ainda sobrevivem, a maioria são **mutantes equivalentes** ou **extremamente difíceis de matar**:

1. **Fatorial (4 mutantes)**: Lógica `n === 0 || n === 1`

   - Mutações envolvendo operadores lógicos são equivalentes ao comportamento real
   - Requerem reestruturação do código para eliminar

2. **ProdutoArray (1 mutante)**: Condição de array vazio

   - Mutante equivalente - comportamento idêntico ao original

3. **Clamp (2 mutantes)**: Operadores `<` vs `<=` e `>` vs `>=`
   - Difíceis de detectar sem casos extremos adicionais muito específicos

## 6. Conclusão

### Importância do Teste de Mutação

O teste de mutação revelou que:

1. **Alta cobertura ≠ Testes eficazes**: Embora tivéssemos ~100% de cobertura de código, apenas 73.71% dos mutantes eram detectados.

2. **Testes superficiais são insuficientes**: Testar apenas o "happy path" deixa muitas vulnerabilidades no código.

3. **Qualidade > Quantidade**: Adicionar 35 testes estratégicos aumentou a pontuação de mutação em 23%, provando que testes bem pensados são mais valiosos que simplesmente aumentar números.

4. **Validação de comportamento específico**: Verificar mensagens de erro, casos de borda e valores booleanos complementares é essencial para uma suíte de testes robusta.

### Lições Aprendidas

- **Sempre teste casos de borda**: Zero, negativos, arrays vazios, valores limite
- **Valide mensagens de erro**: Não apenas se uma exceção é lançada, mas qual exceção
- **Teste ambos os lados de condições booleanas**: true E false
- **Use toBeCloseTo para floats**: Evita problemas de precisão
- **Mutantes equivalentes existem**: Alguns mutantes não podem ser mortos sem reestruturação do código

### Recomendações Futuras

1. Reestruturar a função `fatorial` para eliminar mutantes equivalentes
2. Adicionar testes de propriedades (property-based testing) para funções matemáticas
3. Implementar CI/CD com análise de mutação obrigatória (threshold mínimo de 95%)
4. Revisar periodicamente o relatório de mutação para identificar novas fraquezas

---

**Relatório gerado automaticamente em 29/10/2025**
