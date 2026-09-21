# Acerto de Contas — app compartilhado (Firebase + GitHub Pages)

App de acompanhamento mensal de despesas para dois usuários (Lucas e Dai),
com banco de dados em tempo real. Nenhum dos dois precisa ter conta Claude —
só um navegador no celular. Custo: R$ 0 (dentro do plano gratuito do
Firebase, que é muito maior do que o uso de duas pessoas gera).

## O que você vai fazer (uns 15-20 minutos, uma vez só)

### 1. Criar o projeto Firebase
1. Acesse https://console.firebase.google.com com sua conta Google.
2. "Adicionar projeto" → nome livre, ex: `acerto-de-contas`. Pode desativar
   o Google Analytics (não precisa).
3. Dentro do projeto, no menu lateral: **Build → Firestore Database** →
   "Criar banco de dados" → escolha uma localização (ex: `southamerica-east1`
   se aparecer, ou a mais próxima) → inicie em **modo de produção**.
4. Ainda no menu lateral: **Build → Authentication** → "Vamos começar" →
   na aba "Sign-in method", ative o provedor **Anônimo** (Anonymous).
   É o que deixa você e a Dai usarem o app sem criar login nenhum.

### 2. Aplicar as regras de segurança
1. Em **Firestore Database → Regras**, apague o conteúdo e cole o que está
   no arquivo `firestore.rules` deste pacote.
2. Clique em "Publicar".

### 3. Pegar as credenciais do projeto
1. No ícone de engrenagem (canto superior esquerdo) → "Configurações do projeto".
2. Em "Seus aplicativos", clique no ícone `</>` (Web) para registrar um app.
   Dê um nome (ex: `acerto-web`) e **não** marque Firebase Hosting.
3. Copie o objeto `firebaseConfig` que aparece (apiKey, authDomain,
   projectId, storageBucket, messagingSenderId, appId).
4. Abra o arquivo `index.html` deste pacote, procure o bloco:
   ```js
   var firebaseConfig = {
     apiKey: "COLE_AQUI",
     ...
   };
   ```
   e substitua pelos valores copiados.

### 4. Publicar no GitHub Pages
1. Crie um repositório novo no GitHub (pode ser privado).
2. Suba os arquivos deste pacote (`index.html`, `manifest.json`, este
   `README.md`) para a raiz do repositório.
3. Em **Settings → Pages**, em "Source" escolha a branch `main` e pasta `/root`,
   salve. Em 1-2 minutos o GitHub mostra a URL pública, algo como
   `https://SEU_USUARIO.github.io/SEU_REPOSITORIO/`.
4. Abra essa URL no seu celular e no da Dai. Em "adicionar à tela inicial"
   (Safari/Chrome) ele fica com carinha de app.

### Como os dados chegam já preenchidos
Na primeira vez que o app abrir (em qualquer um dos dois celulares) e o
banco estiver vazio, ele mesmo cadastra as despesas fixas e as parcelas
atuais (os mesmos valores que já estavam no app anterior, com a planilha
atualizada). Não precisa importar nada manualmente.

### Se quiser, eu ajudo a revisar
Se em algum passo aparecer erro (ex: mensagem de "permission-denied" no
console do navegador), me manda a mensagem que eu ajudo a diagnosticar —
geralmente é a regra do Firestore não publicada ou o provedor Anônimo não
ativado.

## Sobre o modelo de dados (caso queira mexer depois)
- `expenses/{id}`: despesa fixa (nome, responsável, valor "atual").
- `debts/{id}`: parcela/dívida (nome, responsável, valor total, nº de
  parcelas, mês inicial, valor "atual" da parcela).
- `months/{AAAA-MM}`: um documento por mês, com a lista de itens daquele
  mês (gerada a partir de `expenses`/`debts` na primeira vez que alguém
  abre aquele mês) e o status de fechamento/confirmação de cada um.
