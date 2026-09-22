# Acessos do Casa do Oleiro

## Perfis

- `admin`: administra integrantes, permissões, músicas, cultos e escalas.
- `editor`: edita músicas, letras, cultos e escalas.
- `viewer`: consulta e copia informações, sem alterar dados.
- ausência de perfil: usuário pendente, sem acesso aos dados sincronizados.

## Integrantes e fotos

O menu `Equipe` mantém um cadastro separado de integrantes. A escala usa o nome cadastrado e mostra a foto circular; quando não há foto, o aplicativo exibe as iniciais. Fotos são comprimidas no navegador e, quando o Storage está disponível, enviadas para `teamPhotos/{uid}/`.

## Publicação das regras

Publique `firestore.rules` no Firestore e `storage.rules` no Firebase Storage antes de liberar o uso para a equipe. As regras não permitem que um usuário se promova sozinho a administrador.

## Primeiro administrador

Depois de criar a conta do proprietário no Firebase Authentication, crie no Firestore:

- coleção: `members`
- documento: o UID do usuário autenticado
- campos:
  - `email`: e-mail da conta
  - `name`: nome do usuário
  - `role`: `admin`
  - `active`: `true`
  - `createdAt`: data/hora atual

Esse é o único passo inicial que precisa ser feito no Firebase Console. Depois disso, o administrador poderá cadastrar e aprovar os demais integrantes pelo próprio aplicativo.
