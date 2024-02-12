# Início Rápido

## Instalação

Instale as dependências e inicie o projeto.

```sh
> git clone https://gitlab.com/superteia-dev/expo-sinpeem-app.git
> cd expo-sinpeem-app
> npm i && npm start
```

## Comandos

- `npm start` - Inicia o projeto utilizando Expo Go por padrão.
- `npm run start:clear` - Inicia o projeto enquanto limpa o cache do Expo. Use quando alterar dependências nativas ou modificar as configurações do Expo.
- `npm run build:android` - Compila o aplicativo **localmente** utilizando Android SDK e Java JDK. Utiliza o perfil `preview`.
- `npm run lint` - Realiza uma análise em todos os arquivos utilizando o Eslint.
- `npm run format` - Realiza uma análise em todos os arquivos utilizando o Prettier.
- `npm run test` - Executa testes utilizando o Jest.

## Expo

- `npx expo login` - Entra em sua conta Expo. Necessário para a compilação do aplicativo.
- `npx eas build` - Compila o aplicativo, local ou na nuvem, para múltiplas plataformas.

## Referêcias

[MkDocs](https://www.mkdocs.org): Ferramenta para criação de documentações.  
[Expo](https://docs.expo.dev): Plataforma de desenvolvimento de aplicativos móveis.  
[EAS](https://docs.expo.dev/build/introduction/): Ferramenta para compilação e publicação de aplicativos Expo.