
# Documentação do Banco de Dados MongoDB

## Estrutura do Banco de Dados

O banco de dados contém quatro coleções principais: `questions`, `categories`, `user_attempts`, e `users`. Abaixo, seguem os detalhes de cada coleção.

---

### 1. **Questions**

Armazena as perguntas do quiz, incluindo informações como o tópico, dificuldade, opções e resposta correta.

#### Exemplo de Documento
```json
{
  "_id": "unique_question_id",
  "topic": "AWS EC2",
  "difficulty": "intermediate",
  "question": "Qual é a finalidade de um grupo de segurança no Amazon EC2?",
  "options": [
    { "label": "A", "text": "Gerenciar o ciclo de vida das instâncias EC2." },
    { "label": "B", "text": "Controlar o tráfego de rede de entrada e saída das instâncias." },
    { "label": "C", "text": "Prover acesso baseado em identidade (IAM)." },
    { "label": "D", "text": "Monitorar logs de aplicação em tempo real." }
  ],
  "correctAnswer": "B",
  "explanation": "Os grupos de segurança atuam como um firewall virtual para controlar o tráfego de rede de entrada e saída das instâncias EC2.",
  "tags": ["EC2", "Networking", "AWS"],
  "createdAt": "2024-11-22T10:00:00.000Z",
  "updatedAt": "2024-11-22T10:00:00.000Z"
}
```

#### Campos
- `_id` (String): ID único da pergunta.
- `topic` (String): Tópico relacionado à pergunta.
- `difficulty` (String): Nível de dificuldade (`easy`, `intermediate`, `advanced`).
- `question` (String): Texto da pergunta.
- `options` (Array): Lista de opções com rótulos e textos.
- `correctAnswer` (String): Resposta correta (rótulo da opção).
- `explanation` (String): Explicação da resposta.
- `tags` (Array): Tags relacionadas à pergunta.
- `createdAt` (Date): Data de criação.
- `updatedAt` (Date): Data da última atualização.

---

### 2. **Categories**

Armazena as categorias (tópicos) relacionadas às perguntas.

#### Exemplo de Documento
```json
{
  "_id": "aws-ec2",
  "name": "AWS EC2",
  "description": "Elastic Compute Cloud da AWS, usado para hospedar máquinas virtuais.",
  "createdAt": "2024-11-22T10:00:00.000Z",
  "updatedAt": "2024-11-22T10:00:00.000Z"
}
```

#### Campos
- `_id` (String): ID único da categoria.
- `name` (String): Nome da categoria.
- `description` (String): Descrição da categoria.
- `createdAt` (Date): Data de criação.
- `updatedAt` (Date): Data da última atualização.

---

### 3. **User Attempts**

Armazena as tentativas dos usuários em responder as perguntas.

#### Exemplo de Documento
```json
{
  "_id": "unique_attempt_id",
  "userId": "user_123",
  "questionId": "unique_question_id",
  "selectedAnswer": "A",
  "isCorrect": false,
  "attemptedAt": "2024-11-22T12:00:00.000Z",
  "timeTaken": 15
}
```

#### Campos
- `_id` (String): ID único da tentativa.
- `userId` (String): Referência ao ID do usuário na coleção `users`.
- `questionId` (String): Referência ao ID da pergunta na coleção `questions`.
- `selectedAnswer` (String): Resposta escolhida pelo usuário (rótulo da opção).
- `isCorrect` (Boolean): Indica se a resposta foi correta.
- `attemptedAt` (Date): Data e hora da tentativa.
- `timeTaken` (Number): Tempo gasto na tentativa (em segundos).

---

### 4. **Users**

Armazena informações dos usuários do sistema.

#### Exemplo de Documento
```json
{
  "_id": "user_123",
  "name": "Vitor Valim",
  "email": "vitor@example.com",
  "createdAt": "2024-11-22T10:00:00.000Z",
  "updatedAt": "2024-11-22T10:00:00.000Z"
}
```

#### Campos
- `_id` (String): ID único do usuário.
- `name` (String): Nome do usuário.
- `email` (String): E-mail do usuário.
- `createdAt` (Date): Data de criação.
- `updatedAt` (Date): Data da última atualização.

---

## Relacionamentos

1. **`questions` ↔ `categories`**: Relacionamento baseado no campo `topic` de `questions`, que corresponde ao campo `_id` de `categories`.
2. **`user_attempts` ↔ `users`**: Relacionamento baseado no campo `userId` de `user_attempts`, que corresponde ao campo `_id` de `users`.
3. **`user_attempts` ↔ `questions`**: Relacionamento baseado no campo `questionId` de `user_attempts`, que corresponde ao campo `_id` de `questions`.

---

## Esquema Visual

Inclui diagramas mostrando as relações entre as coleções (vide esquema visual separado).

---

## Considerações

- **Indexação**: Considere criar índices nos campos `userId` e `questionId` em `user_attempts` para melhorar o desempenho das consultas.
- **Validação de Dados**: Utilize um mecanismo como o Mongoose para validar a estrutura dos documentos antes de armazená-los no banco de dados.
