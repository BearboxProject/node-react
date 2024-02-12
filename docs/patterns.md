## Conventional Commits

Os Conventional Commits estabelecem um formato padrão para as mensagens de commit, tornando-as mais legíveis e fornecendo informações precisas sobre o conteúdo das alterações. Essa prática facilita a automação do versionamento semântico, promovendo uma comunicação clara sobre as mudanças realizadas.

A mensagem deve ser estruturada da seguinte forma:

```sh
<tipo>[escopo opcional]: <descrição>

[corpo opcional]

[rodapé opcional]
```

Ela **deve** conter `tipo` e `descrição`. Também pode possuir `corpo` e `rodapé` para prover informação mais detalhada ou em casos especiais.

Ao seguir os Conventional Commits, podemos encontrar uma variedade de tipos e suas respectivas variantes. No entanto, vale ressaltar que muitos desses tipos são altamente situacionais, e alguns demandam esforço extra para memorização, sem agregar um valor substancial. Diante disso, optamos por focar em alguns tipos mais relevantes, destacando seus principais usos.

Abaixo estão alguns dos tipos essenciais que selecionamos para simplificar e tornar mais eficiente o processo de comunicação em nosso fluxo de trabalho:

- **feat**: Descreve a adição de uma nova funcionalidade ou recurso ao projeto. Útil para destacar implementações significativas que agregam valor ao software.

- **refactor**: Indica alterações no código que não adicionam novas funcionalidades, mas melhoram a estrutura, a eficiência ou a legibilidade. Ideal para destacar aprimoramentos internos.

- **docs**: Utilizado quando há modificações na documentação do projeto. Essencial para manter a documentação atualizada e fornecer informações claras sobre o código.

- **chore**: Designado para alterações relacionadas a tarefas de manutenção, configuração ou outras atividades que não se enquadram diretamente nas categorias convencionais. Útil para manter o repositório organizado.

- **test**: Indica alterações ou adições relacionadas a testes, sejam eles unitários, de integração ou outros. Importante para garantir a qualidade e a estabilidade do código.

Existem outros tipos comuns como `styles`, `ci`, `build` que descrevem, respectivamente, mudanças no estilo do código, mudanças nas configurações de CI, e mudanças no processo de build. Esses os quais poderiam ser facilmente descritos como `feat`, `refactor` ou `chore`. Seu uso não é irrelevante, mas em vários casos não é de extrema importância.

## Git Flow

O Git Flow é uma estratégia de branching para o Git que organiza o fluxo de trabalho, simplificando o gerenciamento de features, releases e hotfixes. Ao utilizar branches dedicadas para cada tipo de alteração, o Git Flow promove um desenvolvimento estruturado e permite uma integração contínua mais suave.

No Git Flow, as branches são categorizadas em dois tipos: Principais e de Suporte. As branches principais são onde todo código é consolidado após o desenvolvimento, enquanto as de suporte são destinadas ao desenvolvimento efetivo do código.

Entre as branches principais, temos:

- **main/master**: Onde reside o código pronto para produção.
- **stage/staging**: Contém o código para ambiente de homologação.
- **dev/development**: Principal branch de desenvolvimento, onde são mescladas todas as branches de suporte antes de irem para stage ou main.

Embora originalmente `stage` não faça parte da definição formal do `git flow`, durante o desenvolvimento, consideramos como principais todas aquelas que existem permanentemente.

Quanto às branches de suporte, temos as seguintes:

- **feature/\***: Utilizada para novas implementações.
- **release/\***: Empregada para preparar o código para um lançamento.
- **hotfix/\***: Utilizada para correções de erros.

É importante observar que a branch dev, como um ponto de entrada para as branches de suporte, também serve como base para a criação delas. Isso significa que qualquer branch de suporte pode ser mesclada ou rebasada a partir da dev. Essa estrutura proporciona uma organização clara e eficiente no ciclo de vida do desenvolvimento.

## Versionamento Semântico

O SemVer é um padrão que proporciona uma abordagem clara e consistente para versionar software. Composto por três números (MAJOR, MINOR e PATCH), o SemVer reflete alterações significativas, adições de funcionalidades e correções de bugs, facilitando a compreensão do impacto das atualizações.

Antes de compreender o seu funcionamento, é crucial analisar dois cenários: o primeiro durante o desenvolvimento inicial e o segundo após o lançamento da primeira versão estável do sistema.

Conforme a documentação oficial do SemVer, "a coisa mais simples a se fazer é começar sua versão de desenvolvimento inicial em `0.1.0` e, então, incrementar a uma versão ‘menor’ em cada lançamento subsequente". Isso significa que a cada nova build aumentamos um número.

A partir da versão `1.0.0`, começamos a utilizar o versionamento semântico de fato. Decidir quando atingir essa versão é uma escolha da equipe de desenvolvimento, mas existem alguns critérios que devem ser seguidos:

- A documentação pública deve estar pronta.
- Os testes de unidade e integração cobrem adequadamente o código.
- Não há bugs críticos ou problemas de segurança não resolvidos.
- Todas as dependências externas foram atualizadas para versões estáveis e compatíveis.
- Todas as questões importantes identificadas nas revisões de código foram abordadas.
- A equipe revisou e concordou com a marcação 1.0.0 como um marco significativo.

Tendo isso em mente, ao atingir essa versão, passamos a seguir estritamente as regras do versionamento semântico, sendo alguma delas:

- Um número de versão normal **deve** ter o formato de X.Y.Z. Cada elemento **deve** aumentar numericamente.
- Uma vez que um pacote versionado foi lançado, o conteúdo desta versão **não deve** ser modificado. Qualquer modificação **deve** ser lançado como uma nova versão.
- Versão de Correção Z **deve** ser incrementado apenas se mantiver compatibilidade e introduzir correção de bugs.
- Versão Menor Y **deve** ser incrementada se uma funcionalidade nova e compatível for introduzida na API pública.
- Versão Maior X **deve** ser incrementada se forem introduzidas mudanças incompatíveis na API pública.

Existem diversas outras regras, mas, no geral, essas cinco cobrem a maioria dos casos. Para mais informações, verifique o site oficial [semver.org](https://semver.org/lang/pt-BR/).