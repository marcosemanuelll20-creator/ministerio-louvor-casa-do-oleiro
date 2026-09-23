# Acessos do Casa do Oleiro

## Perfis do aplicativo

- `admin`: controla músicas, letras, cifras, tons, cultos, escala, integrantes e perfis de acesso.
- `editor`: edita músicas, repertórios, cultos, escala e cadastro da equipe; não administra usuários ou permissões.
- `viewer`: consulta as informações e registra somente a própria confirmação e observação em cada culto.
- Conta sem perfil ativo: permanece pendente e não lê o repertório compartilhado.

Funções musicais (vocal, teclado, bateria etc.) são diferentes dos perfis de acesso. Cadastre integrantes em `Equipe`; use esses nomes para as atribuições por música e escala.

## Entrada de novos integrantes

1. A pessoa abre o aplicativo e entra com Google ou cria conta com e-mail e senha. Para cadastro por senha, precisa confirmar o e-mail pelo link enviado.
2. O aplicativo cria uma solicitação pendente vinculada ao UID e ao e-mail autenticado.
3. Em `Acessos`, um administrador confere a solicitação, escolhe Visualizador, Editor ou Administrador e aprova ou recusa.
4. Depois da aprovação, a pessoa entra novamente ou atualiza o aplicativo para carregar o perfil.

Este fluxo não envia convite por e-mail. Ele usa aprovação dentro do aplicativo e não exige que o integrante copie o UID para o administrador. A criação de contas com e-mail/senha requer senha com pelo menos seis caracteres. O Firebase Authentication continua sendo a autenticação; não use uma senha compartilhada simples para toda a equipe.

## Participação nos cultos

Na página de cada culto, a área “Minha participação” grava resposta e observação em documento individual no Firestore. O participante não altera a escala compartilhada nem a resposta de outra pessoa. Administradores e editores continuam responsáveis pela escala oficial.

## Integrantes e fotos

O menu `Equipe` mantém o cadastro de integrantes separado das contas de acesso. Fotos existentes são preservadas; o armazenamento permite somente imagens JPEG, PNG ou WebP com menos de 2 MiB. Escrita na pasta de fotos é limitada ao próprio usuário editor, salvo administrador.

## Regras do Firebase

Publique `firestore.rules` no Firestore e `storage.rules` no Firebase Storage antes de ativar os novos fluxos. As regras locais não alteram o projeto Firebase automaticamente. Teste aprovação, papéis, respostas pessoais e fotos antes de liberar para toda a equipe.

## Primeiro administrador

Depois de criar a conta do proprietário no Firebase Authentication, crie no Firestore `members/{UID}` com `email`, `name`, `role: "admin"` e `active: true`. Esse passo inicial continua sendo feito no Firebase Console; depois, administradores podem aprovar solicitações no aplicativo.
