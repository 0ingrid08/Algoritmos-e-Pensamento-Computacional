# Desafio: Sistema de Monitoramento de Temperatura

## 1. Identificação
* **Nome do Aluno:** Ingrid Costa Pereira 
* **Disciplina:** Algoritmos e Pensamento Computacional
* **Professora:** Profa. Karla Sartin
* **Título do Projeto:** Sistema Automatizado de Monitoramento de Temperatura Industrial

---

## 2. Objetivo
O programa foi desenvolvido para mitigar riscos de superaquecimento em ambientes industriais. Ele monitora continuamente os valores térmicos enviados por sensores, valida a integridade das entradas, gera estatísticas consolidadas e aciona um desligamento automático de emergência caso detecte anomalias consecutivas perigosas.

---

## 3. Funcionamento do Programa
* **Definição do Limite:** O operador digita o limite de segurança logo no início. O sistema rejeita valores logicamente impossíveis (menores ou iguais ao zero absoluto).
* **Realização das Leituras:** As leituras ocorrem de forma contínua em um ciclo controlado.
* **Tratamento de Valores Inválidos:** Caso o usuário digite letras, caracteres especiais ou valores físicos absurdos, o programa limpa o buffer do teclado, exibe um alerta e desconsidera a entrada nas estatísticas.
* **Identificação de Alarme:** Sempre que uma temperatura válida ultrapassa o limite definido, um alerta visual é impresso em tela.
* **Contagem Consecutiva:** O software incrementa um contador a cada violação consecutiva. Se uma temperatura retornar à faixa de segurança, o contador zera imediatamente.
* **Condição de Encerramento:** O monitoramento é interrompido caso o contador de violações consecutivas atinja **3**, ou se o operador digitar explicitamente o código de parada manual `-999`.

---

## 4. Estruturas de Repetição Utilizadas
O projeto utilizou uma combinação estratégica das duas estruturas exigidas:

1. **`do...while`:** Utilizada na configuração inicial do limite de temperatura. A escolha se justifica porque precisamos que o menu e o comando de entrada executem **pelo menos uma vez** antes de testar se a configuração inserida é válida.
2. **`while`:** Aplicada no loop principal de monitoramento térmico. Justifica-se porque o teste da condição (se o alarme consecutivo atingiu 3) deve ser feito **antes** de aceitar uma nova entrada, garantindo o bloqueio imediato do fluxo se o limite de risco já tiver sido estourado na rodada anterior.

---

## 5. Como Executar
Certifique-se de possuir um compilador GCC configurado no terminal.

1. Navegue até a pasta do projeto:
   ```bash
   cd desafio-monitoramento
   ```
2. Compile o código-fonte:
   ```bash
   gcc monitoramento.c -o monitoramento
   ```
3. Execute o binário gerado:
   ```bash
   ./monitoramento
   ```

---

## 6. Testes Realizados
Os resultados detalhados estão mapeados na pasta `/evidencias`:

* **Teste 1 (Validação de Entradas Inválidas):** Tentativa de inserir letras no limite e valores absurdos no monitoramento. O sistema barrou e manteve o fluxo ativo de forma estável.
* **Teste 2 (Temperaturas Acima do Limite, Não Consecutivas):** Inserção de picos de temperatura intercalados com valores normais. O contador de segurança limpou com sucesso a cada leitura normal e o programa não encerrou de forma precoce.
* **Teste 3 (Três Temperaturas Consecutivas Acima do Limite):** Simulação de falha crítica com 3 registros altos seguidos. O programa interrompeu o loop instantaneamente, exibindo o aviso de emergência e o relatório de cálculos finais perfeitamente consolidados.

---

## 7. Questão Final de Reflexão
**Por que você escolheu while, do...while ou uma combinação das duas estruturas? Em qual parte do algoritmo a diferença entre testar a condição antes ou depois da execução foi importante para sua solução?**

*Resposta:* Optei pela combinação de ambas as estruturas (`do...while` e `while`) para espelhar com precisão o comportamento real de um maquinário de segurança industrial. 

A diferença conceitual de testar a condição antes ou depois foi o fator determinante no design do código:
No bloco de configuração do limite inicial, a abordagem do teste **depois** (`do...while`) foi fundamental. O programa precisa, por obrigação mecânica, solicitar e ler o dado antes de conseguir julgar se o valor faz sentido ou não. Fazer o teste antes nos forçaria a inicializar variáveis com valores fictícios ("gambiarras") apenas para forçar a entrada no bloco.

Por outro lado, no ciclo principal de monitoramento, o teste **antes** (`while`) mostrou-se indispensável para a integridade de segurança. O computador precisa validar se o ecossistema está seguro *antes* de avançar para uma próxima varredura de dados do sensor. Se o limite de 3 falhas consecutivas já foi estourado na rodada anterior, o loop precisa ser impedido de rodar uma quarta vez, preservando a lógica de interrupção instantânea do sistema.
