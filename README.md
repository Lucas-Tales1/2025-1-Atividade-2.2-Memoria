# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: [Lucas Tales B. Barbosa](https://github.com/Lucas-Tales1)

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit

**Processos:**
- P1 = 20 KB  
- P2 = 15 KB  
- P3 = 25 KB  
- P4 = 10 KB  
- P5 = 18 KB  

**Memória RAM:** 64 KB (endereços de 0 a 63 KB)

**Passo a passo do Best-Fit:**

1. **P1 (20 KB)** → menor espaço livre inicial é de 64 KB, cabe, ocupa **[0–19]**.  
   - RAM livre: [20–63] (44 KB)

2. **P2 (15 KB)** → menor espaço livre que comporta é [20–63] (44 KB), ocupa **[20–34]**.  
   - RAM livre: [35–63] (29 KB)

3. **P3 (25 KB)** → menor espaço livre que comporta é [35–63] (29 KB), ocupa **[35–59]**.  
   - RAM livre: [60–63] (4 KB)

4. **P4 (10 KB)** → menor espaço livre é 4 KB, não cabe, vai para **Memória Virtual**.  

5. **P5 (18 KB)** → menor espaço livre é 4 KB, não cabe, vai para **Memória Virtual**.  

📌 **RAM final após alocação inicial:**

### 2. Simular Memória Virtual (Paginação)

**Tabela de páginas:**

| Processo | Localização | Páginas na RAM | Páginas no Disco |
|----------|-------------|---------------|------------------|
| P1 (20)  | RAM         | 5 páginas (4 KB cada) | 0 |
| P2 (15)  | RAM         | 4 páginas     | 0 |
| P3 (25)  | RAM         | 7 páginas     | 0 |
| P4 (10)  | Disco       | 0             | 3 páginas |
| P5 (18)  | Disco       | 0             | 5 páginas |

*(Assumindo paginação de 4 KB/página)*

### 3. Desfragmentação da RAM

**Antes da desfragmentação:**  
[0–19] → P1
[20–34] → P2
[35–59] → P3
[60–63] → Livre (4 KB)

**Após desfragmentar:**  
[0–19] → P1 (20 KB)
[20–34] → P2 (15 KB)
[35–59] → P3 (25 KB)
[60–63] → Livre (4 KB)

Mesmo após a compactação, ainda não há espaço suficiente para alocar P4 (10 KB) ou P5 (18 KB) sem remover algum processo para o disco.

 ### 4. Questões para Reflexão

 1. **Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**  
   Sim, pois o Best-fit procura o menor espaço livre suficiente, reduzindo a fragmentação interna. No entanto, neste caso específico, o resultado prático foi igual ao First-fit porque a RAM estava inicialmente vazia e os processos foram inseridos sequencialmente.

2. **Como a memória virtual evitou um deadlock?**  
   Sem memória virtual, os processos P4 e P5 não poderiam ser carregados, bloqueando a execução. Com paginação, eles ficam no disco e podem ser trazidos para a RAM quando necessário, evitando que o sistema fique paralisado.

3. **Qual o impacto da desfragmentação no desempenho do sistema?**  
   A desfragmentação melhora a disponibilidade de blocos contíguos, permitindo alocar novos processos e reduzir a fragmentação externa. Porém, ela consome tempo de CPU e pode gerar atraso temporário, já que exige copiar dados dentro da RAM.
