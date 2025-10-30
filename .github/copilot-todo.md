4. Etapas do Trabalho
Etapa 1: Preparação do Ambiente
1. Faça um fork do repositório para sua conta do GitHub.
2. Clone o seu fork para sua máquina local.
3. Instale as dependências do projeto com o comando: npm install.
Etapa 2: Análise Inicial
1. Execute a suíte de testes existente para garantir que tudo está funcionando: npm
test.
2. Gere o relatório de cobertura de código: npm test -- --coverage.
3. Anote o percentual de cobertura de código inicial. Este valor provavelmente será
alto.
Etapa 3: Configuração do StrykerJS
1. Adicione as dependências do Stryker ao projeto:
npm install --save-dev @stryker-mutator/core @stryker-mutator/jest-runner
2. Inicie o Stryker para gerar o arquivo de configuração stryker.conf.json:
npx stryker init
Siga as instruções do assistente. Ele deve detectar automaticamente o Jest e o
JavaScript/TypeScript.
Etapa 4: Primeira Execução e Análise do Relatório
1. Execute a análise de teste de mutação:
npx stryker run
2. Ao final da execução, o Stryker informará a pontuação de mutação e o caminho para
o relatório HTML (geralmente em reports/mutation/mutation.html).
3. Abra o relatório no navegador e analise-o cuidadosamente:
○ Observe a pontuação de mutação. Compare-a com a alta pontuação de
cobertura de código.
○ Navegue pelos arquivos de código e identifique os mutantes que
sobreviveram (Survived).
○ Clique em um mutante sobrevivente. Analise a alteração que o Stryker fez no
código (ex: trocou um + por um -). Pergunte a si mesmo: "Por que meu teste
atual não falhou com essa alteração?".
Etapa 5: Melhoria da Suíte de Testes
1. Seu principal objetivo agora é "matar" os mutantes sobreviventes.
2. Para cada mutante que sobreviveu, escreva um novo teste (ou altere um existente)
que falharia com o código mutado.
○ Exemplo: Se um mutante que troca a > b por a >= b sobreviveu, é provável
que você não tenha um teste para o caso em que a === b. Adicione um
expect(comparar(5, 5)).toBe(false).
3. Execute npm test regularmente para garantir que seus novos testes passam com o
código original.
Etapa 6: Validação Final
1. Após adicionar seus novos testes, execute o Stryker novamente: npx stryker run.
2. Verifique o novo relatório. Sua pontuação de mutação deve ter aumentado
significativamente.
3. Sua meta é alcançar uma pontuação de mutação superior a 98%.
4. Faça o commit e o push das suas alterações (incluindo os novos testes) para o seu
repositório no GitHub.
5. O Que Entregar
1. Link do Repositório GitHub: O link para o seu fork do projeto, contendo a suíte de
testes aprimorada.
2. Relatório Escrito (PDF, 2-4 páginas): Um documento contendo:
○ Capa: Nome da disciplina, nome do trabalho, seu nome completo e
matrícula.
○ Análise Inicial: Apresente a cobertura de código inicial e a pontuação de
mutação inicial. Discuta a discrepância entre os dois valores.
○ Análise de Mutantes Críticos: Escolha 2 ou 3 mutantes sobreviventes
interessantes da primeira execução. Inclua screenshots do relatório do
Stryker, explique o que a mutação fez e por que o teste original foi incapaz de
matá-la.
○ Solução Implementada: Descreva os novos casos de teste que você
escreveu para matar os mutantes analisados. Justifique por que esses novos
testes são eficazes.
○ Resultados Finais: Apresente a pontuação de mutação final, comprovando
a melhoria na qualidade da suíte.
○ Conclusão: Uma breve reflexão sobre a importância do teste de mutação
como ferramenta de avaliação de qualidade de testes.
6. Critérios de Avaliação
● Configuração e Execução (10%): O Stryker foi configurado e executado
corretamente no projeto.
● Qualidade da Análise (35%): A capacidade de interpretar o relatório, identificar as
fraquezas e explicar claramente por que os mutantes sobreviveram.
● Eficácia dos Novos Testes (35%): Os testes adicionados foram eficazes para
"matar" os mutantes e aumentar significativamente a pontuação de mutação.
● Qualidade do Relatório Escrito (20%): Clareza, objetividade, estrutura e
cumprimento de todos os itens solicitados na entrega.