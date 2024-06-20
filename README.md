# MERN-Criando-os-Pilares-de-uma-Carteira-Digital-com-Node.js-Express-e-MongoDB

Neste projeto, vamos criar os pilares fundamentais de uma Carteira Digital utilizando Node.js, Express e MongoDB. Essas tecnologias são poderosas e bem adequadas para o desenvolvimento de aplicações robustas, seguras e escaláveis, ideais para uma carteira digital.

#### Tecnologias Utilizadas:
- **Node.js**: Ambiente de execução JavaScript que permite criar aplicações de servidor.
- **Express**: Framework web para Node.js que facilita a criação de APIs e aplicações web.
- **MongoDB**: Banco de dados NoSQL orientado a documentos, escalável e flexível.

#### Funcionalidades da Carteira Digital:
1. **Autenticação de Usuário**: Implementaremos um sistema de autenticação seguro usando JSON Web Tokens (JWT).

2. **Gerenciamento de Saldo**: Permitirá aos usuários verificar seu saldo, adicionar fundos e realizar transações.

3. **Histórico de Transações**: Registro detalhado das transações realizadas na carteira.

4. **Segurança**: Implementaremos práticas de segurança como criptografia de senhas, validação de entrada e proteção contra ataques comuns.

#### Estrutura do Projeto:
Vamos organizar o projeto em módulos para manter o código limpo e bem estruturado:

1. **Configuração do Ambiente**:
   - Configuração inicial do ambiente de desenvolvimento com Node.js e MongoDB.
   - Instalação das dependências necessárias (Express, Mongoose, JWT, etc.).

2. **Modelos de Dados**:
   - Definiremos os modelos de dados usando Mongoose para representar usuários, transações e outras entidades relacionadas à carteira.

   Exemplo de modelo de usuário (`User`):
   ```javascript
   const mongoose = require('mongoose');
   const Schema = mongoose.Schema;

   const UserSchema = new Schema({
       username: { type: String, required: true, unique: true },
       password: { type: String, required: true },
       balance: { type: Number, default: 0 },
       transactions: [{ type: Schema.Types.ObjectId, ref: 'Transaction' }]
   });

   module.exports = mongoose.model('User', UserSchema);
   ```

3. **Autenticação**:
   - Implementação do sistema de autenticação usando JWT.
   - Rotas para registro (`signup`), login (`signin`) e proteção de rotas privadas.

   Exemplo de rota de login (`signin`):
   ```javascript
   const express = require('express');
   const router = express.Router();
   const jwt = require('jsonwebtoken');
   const bcrypt = require('bcryptjs');
   const User = require('../models/User');

   router.post('/signin', async (req, res) => {
       const { username, password } = req.body;
       try {
           const user = await User.findOne({ username });
           if (!user) {
               return res.status(404).json({ error: 'Usuário não encontrado.' });
           }
           const isMatch = await bcrypt.compare(password, user.password);
           if (!isMatch) {
               return res.status(401).json({ error: 'Credenciais inválidas.' });
           }
           const token = jwt.sign({ userId: user._id }, 'secreto', { expiresIn: '1h' });
           res.json({ token });
       } catch (err) {
           console.error(err);
           res.status(500).send('Erro no servidor.');
       }
   });

   module.exports = router;
   ```

4. **Gerenciamento de Saldo e Transações**:
   - Rotas para consultar saldo, adicionar fundos e realizar transações.
   - Controle de saldo do usuário e atualização do registro de transações.

   Exemplo de rota para adicionar fundos (`add-funds`):
   ```javascript
   router.post('/add-funds', authenticateUser, async (req, res) => {
       try {
           const { amount } = req.body;
           if (amount <= 0) {
               return res.status(400).json({ error: 'O valor deve ser maior que zero.' });
           }
           const user = req.user; // Obtém o usuário autenticado
           user.balance += amount;
           await user.save();
           res.json({ message: 'Fundos adicionados com sucesso.' });
       } catch (err) {
           console.error(err);
           res.status(500).send('Erro no servidor.');
       }
   });
   ```

5. **Testes e Documentação**:
   - Implementação de testes unitários e de integração para garantir a robustez da aplicação.
   - Documentação clara das APIs utilizando ferramentas como Swagger ou Postman.

#### Conclusão:
Este projeto fornecerá uma base sólida para o desenvolvimento de uma carteira digital utilizando Node.js, Express e MongoDB. Ao seguir estas etapas e exemplos, estaremos capacitado a construir uma aplicação segura, eficiente e escalável para gestão de finanças pessoais ou qualquer aplicação que envolva transações monetárias.
