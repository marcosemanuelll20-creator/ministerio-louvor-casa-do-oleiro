# Firebase — Ministério de Louvor Casa do Oleiro

Projeto usado pelo aplicativo: `igreja-casa-do-oleiro-2e2fb`.

## Serviços usados

- Authentication: login por Google e e-mail/senha; novos cadastros por e-mail precisam confirmar o endereço.
- Cloud Firestore: dados do aplicativo, perfis, solicitações de acesso, respostas individuais e logs.
- Cloud Storage: fotos existentes dos integrantes.
- Hospedagem: GitHub Pages; a publicação do site é independente da publicação das regras Firebase.

## Estrutura de dados

- `members/{uid}`: perfil de acesso (`admin`, `editor` ou `viewer`) e estado ativo.
- `accessRequests/{uid}`: solicitação criada pela própria conta autenticada; somente administradores podem listar e aprovar/recusar.
- `appState/main`: músicas, cultos, escalas e cadastro da equipe. Admins e editores podem editar; viewers apenas leem.
- `cultResponses/{cultId}/members/{uid}`: confirmação e observação individuais; cada pessoa altera somente o próprio registro, e a equipe organizadora pode consultar as respostas.
- `auditLogs/{id}`: registro das alterações de conteúdo.

## Configuração inicial

1. Authentication > Sign-in method: habilite Google e e-mail/senha.
2. Firestore Database: crie ou mantenha o banco em modo de produção.
3. Publique o conteúdo de `firestore.rules` nas regras do Firestore.
4. Publique o conteúdo de `storage.rules` nas regras do Storage.
5. Em Authentication > Settings > Authorized domains, confira o domínio de produção do GitHub Pages.
6. Crie no Firestore o perfil inicial `members/{UID}` do proprietário com e-mail correspondente à conta, nome, `role: "admin"` e `active: true`.

Não publique o fluxo novo antes de atualizar as regras: as regras antigas negam as gravações de editores, solicitações de acesso e respostas pessoais. O arquivo local de regras não modifica o Firebase sozinho.

## Segurança e operação

As permissões são verificadas no Firestore, não apenas escondidas na interface. O perfil `viewer` não pode escrever em `appState/main`; pode gravar apenas o próprio documento de resposta no culto. `editor` pode editar conteúdo, mas não perfis. Somente `admin` administra membros e solicitações.

Antes de publicar alterações, teste as regras com Firebase Emulator ou no ambiente de teste: usuário pendente, viewer, editor e admin; resposta própria e de outra conta; tentativa de editar músicas com viewer; gestão de papéis com editor; e atualização das fotos.

O aplicativo mantém músicas, cultos e equipe em um documento agregado `appState/main`. Isso preserva a estrutura existente, mas não oferece edição simultânea sem risco de uma gravação substituir mudanças concorrentes. Para uma equipe pequena, coordene edições simultâneas; uma futura migração para documentos separados reduziria esse risco, mas exige migração e validação próprias.
