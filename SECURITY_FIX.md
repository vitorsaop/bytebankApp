# 🚨 GUIA DE SEGURANÇA - Resolução de Exposição de API Keys

## ⚠️ PROBLEMA
As API Keys do Firebase foram expostas publicamente no GitHub. Isso representa um risco de segurança crítico.

## 🔧 SOLUÇÃO - Passo a Passo

### 1️⃣ REGENERAR AS API KEYS NO FIREBASE (URGENTE!)

#### Android API Key
1. Acesse: https://console.cloud.google.com/apis/credentials?project=bytebankapp-3fe6b
2. Localize a chave: `AIzaSyAtvZUxMpjITlOtLN-GBjrXFL4iTa7T2uQ`
3. Clique nos 3 pontos (⋮) → **"Regenerate key"**
4. Copie a nova chave gerada

#### iOS API Key
1. Na mesma tela, localize: `AIzaSyCyNJS83JXQSKYbX0feEFGb40UNmg1ArSs`
2. Clique nos 3 pontos (⋮) → **"Regenerate key"**
3. Copie a nova chave gerada

#### Adicionar Restrições às Chaves (Recomendado)
1. Clique em cada chave
2. Em **"Application restrictions"**:
   - Para Android: selecione **"Android apps"** e adicione o package name: `com.grupo30.bytebankapp`
   - Para iOS: selecione **"iOS apps"** e adicione o bundle ID: `com.grupo30.bytebankapp`
3. Em **"API restrictions"**: selecione **"Restrict key"** e habilite apenas:
   - Firebase Authentication API
   - Cloud Firestore API
   - Firebase Storage API
4. Clique em **"Save"**

---

### 2️⃣ ATUALIZAR firebase_options.dart LOCAL

1. Abra `lib/firebase_options.dart`
2. Substitua as API Keys antigas pelas novas geradas
3. **NÃO FAÇA COMMIT** deste arquivo

---

### 3️⃣ REMOVER firebase_options.dart DO GIT

No terminal, execute:

```bash
# Remove do índice do Git (mantém o arquivo localmente)
git rm --cached lib/firebase_options.dart

# Adiciona as mudanças do .gitignore
git add .gitignore lib/firebase_options.dart.example

# Commit
git commit -m "security: Remove firebase_options.dart e adiciona template"
```

---

### 4️⃣ LIMPAR O HISTÓRICO DO GIT (Remover chaves antigas)

⚠️ **ATENÇÃO**: Isso reescreverá o histórico do Git!

#### Opção A: Usar filter-repo (Recomendado)

```bash
# Instalar git-filter-repo
brew install git-filter-repo  # macOS
# ou
pip install git-filter-repo    # Python

# Remover o arquivo do histórico
git filter-repo --path lib/firebase_options.dart --invert-paths

# Force push (reescreve o histórico remoto)
git push origin --force --all
```

#### Opção B: Usar BFG Repo-Cleaner (Alternativa)

```bash
# Instalar BFG
brew install bfg  # macOS

# Fazer backup
git clone --mirror https://github.com/vitorsaop/bytebankApp.git

# Remover o arquivo
bfg --delete-files firebase_options.dart bytebankApp.git

# Limpar e push
cd bytebankApp.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push --force
```

---

### 5️⃣ REVOGAR ACESSO AO GITHUB (Se necessário)

Se você usou Personal Access Tokens ou Deploy Keys comprometidos:

1. Acesse: https://github.com/settings/tokens
2. Revogue tokens antigos
3. Gere novos tokens se necessário

---

### 6️⃣ MONITORAR ATIVIDADE NO FIREBASE

1. Acesse: https://console.firebase.google.com/project/bytebankapp-3fe6b
2. Verifique:
   - **Authentication** → Usuários criados recentemente
   - **Firestore** → Documentos criados/modificados
   - **Storage** → Arquivos enviados
   - **Usage and billing** → Uso anormal de recursos

---

### 7️⃣ ATUALIZAR O README.md

O README já está configurado para NÃO incluir as chaves. Certifique-se de que está atualizado:

```markdown
### Passo 2: Configuração do Firebase

1. Acesse o Firebase Console
2. Crie um novo projeto ou use o existente
3. Adicione um app Android e/ou iOS
4. Baixe os arquivos de configuração:
   - **Android**: `google-services.json` → coloque em `android/app/`
   - **iOS**: `GoogleService-Info.plist` → coloque em `ios/Runner/`
5. **Copie** `lib/firebase_options.dart.example` para `lib/firebase_options.dart`
6. **Substitua** os valores de placeholder pelas suas configurações reais
```

---

## ✅ VERIFICAÇÃO FINAL

Após seguir todos os passos:

- [ ] API Keys regeneradas no Firebase Console
- [ ] Restrições adicionadas às API Keys
- [ ] `firebase_options.dart` removido do Git
- [ ] `.gitignore` atualizado
- [ ] Histórico do Git limpo (force push feito)
- [ ] Atividade do Firebase monitorada
- [ ] README atualizado com instruções corretas

---

## 🔐 BOAS PRÁTICAS PARA O FUTURO

1. **Nunca commite arquivos de configuração** com credenciais reais
2. **Use variáveis de ambiente** para dados sensíveis em produção
3. **Sempre adicione arquivos de config ao .gitignore** ANTES do primeiro commit
4. **Habilite GitHub Secret Scanning** no seu repositório
5. **Configure Firebase App Check** para proteger suas APIs
6. **Monitore logs regularmente** para detectar uso indevido

---

## 📞 SUPORTE

Se detectar uso indevido ou atividade suspeita:
- Firebase Support: https://firebase.google.com/support
- Google Cloud Support: https://cloud.google.com/support

---

⚠️ **IMPORTANTE**: Execute estes passos IMEDIATAMENTE para minimizar riscos de segurança!
